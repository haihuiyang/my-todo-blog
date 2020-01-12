MBP 是在 PDD 上买的，要问为什么，因为便宜呀！

买的时候犹豫了挺久的，因为害怕买到翻新机或者不是正版的。但是又眼馋 PDD 的价格，最终狠下心来，决定上车。

拼单成功之后，过了一天多才发的货，是用顺丰快递寄的。大概过了三四天到的，取的时候，当面验货，主要检查了包装有没有损坏。

到手之后，慎重起见，又在网上搜了一下应该如何检查 Mac 电脑有没有问题。三码合一、通过序列号在官网查询保修期、电池循环计数等等，里里外外都检查了一遍，貌似也没发现什么问题。算是成功下车了吧？真香，其实内心是慌得一批。。。

其实自己平时工作用的也是 MBP，不过那是公司配的。这个才算是真正的属于自己的 MBP，激动、开森。^_^

---

新电脑，除了系统自带的一些软件之外，什么都没有。不过自己工作也是一直用的 Mac，所以需要哪些软件及环境，还是比较明了的。对照着现有的电脑，弄了一下环境，不得不说，还是花了不少时间的。

为了方便以后新电脑环境的安装，所以就整理了一下。主要分为两类：

- 普通软件
- 开发环境
- Mac 的一些设置

### 一、普通软件

#### 1、[Chrome 浏览器](https://www.google.cn/chrome/index.html)

第一个安装的就是 Chrome 浏览器，有了它，下载其他的软件就都没有问题了。虽然 Mac 自带的 Safari 也不错，但是 Chrome 有很多好用的插件。比如：Chrome 神器 [Vimium](https://github.com/philc/vimium)，有了它，浏览网页几乎可以做到脱离鼠标。

#### 2、微信、QQ 和 钉钉

聊天工具，平时主要用微信和钉钉，QQ 很少用，不过也给安装上了。

#### 3、网易云音乐 和 QQ音乐

听歌软件，这两个我都有在用，有些时候想听一些电台，比较适合在 QQ 音乐听。

#### 4、腾讯视频

因为有腾讯视频会员。。。也可以把爱奇艺、优酷视频也装上。

#### 5、[百度网盘](https://www.maczd.com/post/7.html) 和 [迅雷](http://mac.xunlei.com/)

#### 6、[TeamView](https://www.teamviewer.cn/cn/)

用于远程桌面控制，也可以考虑一下向日葵。

#### 7、[Sublime Text 3](https://www.sublimetext.com/3) 和 [Visual Studio Code](https://code.visualstudio.com/)

两款好用的编辑器，其实我只用过 Sublime Text 3，确实挺不错的；不过听说 Visual Studio Code 也不错，所以也下下来尝试一下。

#### 8、[Typora](https://www.typora.io/)

一款好用的 Markdown 编辑器，主要用于写博客的。之前一直用的 MacDown，不过自从知道了 Typora 之后，就抛弃了 MacDown。。。

#### 9、[calibre](https://calibre-ebook.com/download_osx)

一个文件格式转换工具，个人觉得非常好用，支持各种格式相互转换。

#### 10、[WPS Office](wps.com/phone-mac/)

也是听说 WPS Office 目前功能也挺强大的，像脑图、流程图之类的都支持，也准备尝试一下。

### 二、开发环境

#### 1、[iTerm2](https://iterm2.com/)、Git、[zsh](https://ohmyz.sh/) 和 [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md)

Git 安装：直接在 iTerm2 中执行 git --version 会提示，选择安装就好了。

（1）安装顺序为：**iTerm2 => Git => zsh => zsh-autosuggestions**

（2）iTerm2 ，Mac 上的 shell 终端神器，配合 zsh 真的特别好用，然后 zsh-autosuggestions 是代码提示自动补全。

（3）通过 `ssh-keygen` 命令生成 `~/.ssh`


```bash
ssh-keygen -t rsa -C "happyfeet"
```

（4）配置心跳文件 `~/.ssh/config`


```bash
Host dev
        HostName                ip
        User                    username
        ServerAliveInterval     60
```

#### 2、[IntelliJ IDEA](https://www.jetbrains.com/idea/)、[PyCharm](https://www.jetbrains.com/pycharm/download/#section=mac) 和 [WebStorm](https://www.jetbrains.com/webstorm/download/#section=mac)

- IntelliJ IDEA：Java、Scala 等

  几款实用的插件：

  - Alibaba Java Coding Guidelines：阿里规范提示
  - CodeGlance
  - Grep Console：给输出日志添加颜色
  - JProfiler：JVM 调试工具
  - Key promoter X：提示快捷键相关
  - Lombok：@Data、@Getter 等注解
  - Presentation Assistant ：提示快捷键（友好型）
  - SonarLint：代码检查工具
  - String Manipulation：字符串转换
  - Python
  - Scala

- PyCharm：Python

- WebStorm：前端

#### 3、[Java](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)、[Python](https://www.python.org/downloads/release/python-381/)、[Mysql](https://dev.mysql.com/downloads/mysql/) 和 [nodejs](https://nodejs.org/en/download/)


这几个都是安装包，下下来之间安装就可以了。

Mysql 安装完成之后，需要 initial database 才能用。

Java 需要配置环境变量，待会和 Gradle 、Maven 一起。

#### 4、[Gradle 和 Maven](http://maven.apache.org/download.cgi)

（1）Gradle

```bash
Unzip -d ~/tools/gradle gradle-5.0-bin.zip
```

（2）Maven

```bash
tar -zxvf apache-maven-3.6.3-bin.tar.gz -C ~/tools/maven/
```

Gradle 和 Maven 都放在 ~/tools 目录下，便于管理，减压之后需要配置环境变量，修改 ~/.zshrc 文件，大致是这个样子的，包含了 JAVA_HOME 的配置：

```bash
JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_231.jdk/Contents/Home
GRADLE_HOME=/Users/happyfeet/tools/gradle/gradle-5.0
#GRADLE_HOME=/Users/happyfeet/tools/gradle/gradle-4.10
#GRADLE_HOME=/Users/happyfeet/tools/gradle/gradle-6.0.1
MAVEN_HOME=/Users/happyfeet/tools/maven/apache-maven-3.6.3
MYSQL_HOME=/usr/local/mysql

export PATH=$HOME/bin:/usr/local/bin:$PATH:$JAVA_HOME/bin:$GRADLE_HOME/bin:$MAVEN_HOME/bin:$MYSQL_HOME/bin

export JAVA_HOME
export GRADLE_HOME
export MAVEN_HOME
export MYSQL_HOME
```

#### 5、zsh 的一些别名配置

```bash
alias ll="ls -al -h"
alias lt="ls -alt -h"
alias ltr="ls -altr -h"


# git alias 配置
# 提交代码相关
alias ga='git add '
alias gci='git commit -m '
alias gst='git status'
alias gpl='git pull'
alias gps='git push'

# 查看提交记录
alias glg="git log --color --graph --pretty=format:'%Cred%h%Creset %Cgreen%ad -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>' --date=format:'%F %T' --decorate=short"

# 切换分支相关
alias gcb='git checkout -b '
alias gco='git checkout '
alias gcm='git checkout master'

# 获取、删除分支
alias gfa='git fetch --all'
alias gfp='git fetch --prune'
alias gbr='git branch '
alias gba='git branch -a'
alias gbd='git branch -d'
alias gbD='git branch -D'

# stash 相关
alias gsts='git stash save '
alias gstc='git stash clear'
alias gstd='git stash drop'
alias gstl='git stash list'
alias gstp='git stash pop'

# 加强版的 glg （实际上只是把 commit id 全部显示出来而已）
alias glga="git log --color --graph --pretty=format:'%Cred%H%Creset %Cgreen%ad -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>' --date=format:'%F %T' --decorate=short"

# ssh alias
alias s160='ssh 160'
alias s169='ssh 169'
alias so160='ssh out160'
alias s167='ssh 167'
alias so167='ssh out167'

# others
alias ip="ifconfig en0 | grep 'inet ' | sed 's/inet //g' | sed 's/ netmask.*//g'"
```

### 三、Mac 的一些设置

时钟屏保

焦点移动速度

三指拖动设置

F1-F12 功能显示设置

Dock 设置

Touch ID



登录 Chrome 账号同步书签、iTerm2 设置缓存行数

IDEA 设置：File Header，todo 设置

结语：大致就这么多了，可能还有一些忘记了