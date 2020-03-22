> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)





---





1、`openjdk/jdk/src/share/native/java/lang/Thread.c`

注册了一堆 native 方法，其中，start0 对应的是 JVM_StartThread 方法

2、`openjdk/hotspot/src/share/vm/prims/jvm.cpp`

找到 JVM_ENTRY 为 JVM_StartThread 方法，里面实现了从创建线程到执行 run 方法的逻辑

（1）先创建了一个 JavaThread：`native_thread = new JavaThread(&thread_entry, sz);`

主要看这里的第二个参数：thread_entry

```c++
static void thread_entry(JavaThread* thread, TRAPS) {
  HandleMark hm(THREAD);
  Handle obj(THREAD, thread->threadObj());
  JavaValue result(T_VOID);
  JavaCalls::call_virtual(&result,
                          obj,
                          KlassHandle(THREAD, SystemDictionary::Thread_klass()),
                          vmSymbols::run_method_name(),
                          vmSymbols::void_method_signature(),
                          THREAD);
}
```



可以从 `openjdk/hotspot/src/share/vm/classfile` 找到 

`template(run_method_name,                           "run")`

即：`vmSymbols::run_method_name()`就是 `run` 方法。



（2）中间做了一些 prepare，里面的逻辑不盘：native_thread->prepare(jthread);

`openjdk/hotspot/src/share/vm/runtime/thread.cpp`

（3）Thread::start(native_thread);



2、

```c++
JavaThread::JavaThread(ThreadFunction entry_point, size_t stack_sz) :
  Thread()
#if INCLUDE_ALL_GCS
  , _satb_mark_queue(&_satb_mark_queue_set),
  _dirty_card_queue(&_dirty_card_queue_set)
#endif // INCLUDE_ALL_GCS
{
  if (TraceThreadEvents) {
    tty->print_cr("creating thread %p", this);
  }
  initialize();
  _jni_attach_state = _not_attaching_via_jni;
  set_entry_point(entry_point);
  // Create the native thread itself.
  // %note runtime_23
  os::ThreadType thr_type = os::java_thread;
  thr_type = entry_point == &compiler_thread_entry ? os::compiler_thread :
                                                     os::java_thread;
  os::create_thread(this, thr_type, stack_sz);
}
```

`set_entry_point(entry_point)` // 设置线程运行函数的地址



3、委托给 `openjdk/hotspot/src/os/linux/vm/os_linux.cpp` 创建

`os::create_thread(this, thr_type, stack_sz);`

```c++
bool os::create_thread(Thread* thread, ThreadType thr_type, size_t stack_size) {
  
  // Allocate the OSThread object
  OSThread* osthread = new OSThread(NULL, NULL);
  if (osthread == NULL) {
    return false;
  }

  // set the correct thread state
  osthread->set_thread_type(thr_type);

  // Initial state is ALLOCATED but not INITIALIZED
  osthread->set_state(ALLOCATED);

  thread->set_osthread(osthread);

  ThreadState state;

  {
    // Serialize thread creation if we are running with fixed stack LinuxThreads
    bool lock = os::Linux::is_LinuxThreads() && !os::Linux::is_floating_stack();
    if (lock) {
      os::Linux::createThread_lock()->lock_without_safepoint_check();
    }

    pthread_t tid;
    int ret = pthread_create(&tid, &attr, (void* (*)(void*)) java_start, thread);

    pthread_attr_destroy(&attr);

    if (ret != 0) {
      if (PrintMiscellaneous && (Verbose || WizardMode)) {
        perror("pthread_create()");
      }
      // Need to clean up stuff we've allocated so far
      thread->set_osthread(NULL);
      delete osthread;
      if (lock) os::Linux::createThread_lock()->unlock();
      return false;
    }

    // Store pthread info into the OSThread
    osthread->set_pthread_id(tid);

    // Wait until child thread is either initialized or aborted
    {
      Monitor* sync_with_child = osthread->startThread_lock();
      MutexLockerEx ml(sync_with_child, Mutex::_no_safepoint_check_flag);
      while ((state = osthread->get_state()) == ALLOCATED) {
        sync_with_child->wait(Mutex::_no_safepoint_check_flag);
      }
    }

    if (lock) {
      os::Linux::createThread_lock()->unlock();
    }
  }

  // Aborted due to thread limit being reached
  if (state == ZOMBIE) {
      thread->set_osthread(NULL);
      delete osthread;
      return false;
  }

  // The thread is returned suspended (in state INITIALIZED),
  // and is started higher up in the call chain
  assert(state == INITIALIZED, "race condition");
  return true;
}
```

`int ret = pthread_create(&tid, &attr, (void* (*)(void*)) java_start, thread);` pthread_create 是 linux 创建函数，这里的 thread 就是最终要创建的函数。

这里是创建了一个 pthread，然后调用 java_start(thread) 方法。

```c++
    // Wait until child thread is either initialized or aborted
    {
      Monitor* sync_with_child = osthread->startThread_lock();
      MutexLockerEx ml(sync_with_child, Mutex::_no_safepoint_check_flag);
      while ((state = osthread->get_state()) == ALLOCATED) {
        sync_with_child->wait(Mutex::_no_safepoint_check_flag);
      }
    }
```

pthread 等待 thread 初始化完成，或者 aborted，在调用 java_start(thread) 方法的时候，`osthread` 的 state 会被更新。

`thread->run();`

`JavaThread::run()`

`thread_main_inner();`

```c++
void JavaThread::thread_main_inner() {
  assert(JavaThread::current() == this, "sanity check");
  assert(this->threadObj() != NULL, "just checking");

  // Execute thread entry point unless this thread has a pending exception
  // or has been stopped before starting.
  // Note: Due to JVM_StopThread we can have pending exceptions already!
  if (!this->has_pending_exception() &&
      !java_lang_Thread::is_stillborn(this->threadObj())) {
    {
      ResourceMark rm(this);
      this->set_native_thread_name(this->get_thread_name());
    }
    HandleMark hm(this);
    this->entry_point()(this, this);
  }

  DTRACE_THREAD_PROBE(stop, this);

  this->exit(false);
  delete this;
}
```

`this->entry_point()(this, this);` 调用 entry_point() 方法。

`this->exit(false);` 退出并释放空间
`delete this;` 释放资源



所以线程在执行完 run() 方法之后，就会退出！可以思考一下线程池里面如何做到 core 线程一直 alive 的！



Java 的线程是通过映射到系统的原生线程上来实现的。





尾巴：



参考资料：

（1）

（2）