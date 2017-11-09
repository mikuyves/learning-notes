# 配置开发环境 Setting Dev Environment

配置开发环境事件是一件苦差，所以一直都不想重装电脑，或者或新的机子。
这次换新机，逼于无奈要重新配置一次。由于过程太折腾，怕以后忘记了，所以一定要记录下来，也可以帮助有需要的朋友。

之前使用的 Macbook Air，是刚刚开始学习编程的时候买的，所以配置的时候安装了一大堆东西。
原生的vim 也是崩溃了，python 的版本混乱不堪。
eclipse, vim, subtext, pycharm, wingide 各种编辑器。乱！

刚不久前把工作环境从 Mac 迁移到 Windows 中，摸索到一个比较方便的配置方法。
但是在新的 Mac 上，居然还是要折腾一番！
首先是屎冲冲地升级到 High Sierra，才发现 Pycharm 的 Terminal 居然中文乱码，而网上没有解决办法。
另外，小程序开发者工具也有一些莫名的 bugs，唉，都怪自己手贱。
最后只能降级会 El Capitan。


## 重装 Mac 系统
#### 1. 下载镜像文件

如果需要全新的系统，网上都推荐用 USB 安装。
制作 USB 的方法其实十分简单，从 App Store 下载镜像文件，文件会保存在 /Applications/ 里面。
但是 App Store 只提供最新版本的 MacOs。如果需要滚回就系统，那就只能从网上下载其他镜像。
注意，一定要选原版镜像，不要选择所谓的懒人包。
下载后，如果是 dmg 文件，把解压出来的镜像文件拷贝到 /Applications/ 里面。

#### 2. 准备 USB
USB3.0: SIZE >= 8GB

打开 “应用程序 → 实用工具 → 磁盘工具”，将U盘「抹掉」(格式化) 成「Mac OS X 扩展（日志式）」格式、GUID 分区图，并将U盘命名为「MyVolume」。(注意：这个盘符名称将会与后面的命令一一对应，如果你改了这盘符的名字，必须保证后面的命令里的名称也要一致。)

#### 3. 制作安装盘
敲代码了!
* High Sierra

`sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

* El Capitan

`sudo /Applications/Install\ OS\ X\ El\ Capitan.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ OS\ X\ El\ Capitan.app`

参考
[How to create a bootable installer for macOS
](https://support.apple.com/en-us/HT201372)

## 安装系统
1. 抹掉整个磁盘，High Sierra 是APFS格式，此前版本全部都是「Mac OS X 扩展（日志式）」。
2. 安装 Mac Os。

## 配置工具
* Chrome (无乱如何，先把这个装了！)
* Anaconda
* Homebrew
* PyCharm
* WebStorm
* nvm
* node npm
* wepy wepy-cli
* 微信小程序开发者工具


## 配置开发环境步骤

#### 0. 设置 Terminal
设置好终端的颜色能让之后的步骤更清晰。
在 ~/.bash_profile 添加如下代码：
```bash
# Style
export CLICOLOR=1
export LSCOLOR=gxfxaxdxcxegedabagacad
export PS1='\[\033[31m\]\u@MacBookAir\[\033[00m\]:\[\033[34m\]\W\[\033[00m\]\$ '

# Shortcuts
alias ll="ls -l"
alias settings="open -a 'System Preferences.app'"
alias chrome="open -a 'Google Chrome.app'"

# Python version
alias python='/usr/bin/python'
alias python3='/Users/<username>/anaconda3/bin/python'

# Show all files including the invisible files.
alias showAllFiles='defaults write com.apple.finder AppleShowAllFiles YES'

# Set terminal input mode to vi.
set -o vi

```

#### 1. Anaconda
多版本 Python 神器，避免影响系统的 python，但是在 Mac 中安装后，跟 homebrew 有冲突。

参考
[Keep Anaconda from Constricting Your Homebrew Installs](https://hashrocket.com/blog/posts/keep-anaconda-from-constricting-your-homebrew-installs)
提供了非常有效的解决方法。

在 .bash_profile 中加上这段代码即可。
```bash
export SANS_ANACONDA="/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"

# added by Anaconda3 4.4.0 installer
export PATH="/Applications/anaconda/bin:$SANS_ANACONDA"

alias perseus="export PATH="\$SANS_ANACONDA" && echo Medusa decapitated."
alias medusa="export PATH="/Applications/anaconda/bin:\$SANS_ANACONDA" && echo Perseus defeated."

brew () {
  perseus
  command brew "$@"
  medusa
}
```

#### 2. Homebrew
https://brew.sh

#### 3. PyCharm and WebStorm
从官网上下载并安装后，选择从服务器注册。

1. 注册服务器： `http://xidea.online`
2. 安装 vimidea 插件。
3. 安装 Markdown Navigatior。

#### 4. nvm
```bash
$ git clone https://github.com/creationix/nvm.git ~/.nvm
$ source ~/.nvm/nvm.sh

```
参考 [版本管理工具nvm](http://javascript.ruanyifeng.com/nodejs/basic.html#toc2)


#### 5. node npm
```bash
# 安装最新版本
$ nvm install node

# 安装指定版本 (wepy指定版本为 7.8.0)
$ nvm install 7.8.0

# 使用已安装的最新版本
$ nvm use node

# 使用指定版本的node
$ nvm use 7.8.0
```
* 参考 [版本管理工具nvm](http://javascript.ruanyifeng.com/nodejs/basic.html#toc2)
* npm 使用方法参考 [npm模块管理器](http://javascript.ruanyifeng.com/nodejs/npm.html)

#### 6. wepy wepy-cli
 安装
```bash
# 全局安装, 稳定版本为 1.5.7
$ npm install wepy-cli@1.5.7 -g

# 项目安装
<project path>$ npm install wepy@1.5.7

# 测试
<project path>$ npm run build
```
配置 WebStorm
1. 打开Settings，搜索Plugins，搜索Vue.js插件并安装。

2. 打开Settings，搜索File Types，找到Vue.js Template，在Registered Patterns添加*.wpy，即可高亮。

版本问题：
* 使用 1.6.0 bug 列表页面不能渲染。
* 使用 1.5.9 bug 传参为事件。
* 使用 1.5.8 bug scss 编译报错。


* 参考 [Wepy官网](https://wepyjs.github.io/wepy) 文档太烂


#### 7. 微信小程序开发者工具
安装后配合 wepy 进行测试。
1. 使用微信开发者工具-->添加项目，项目目录请选择dist目录。
2. 微信开发者工具-->项目-->关闭ES6转ES5。 重要：漏掉此项会运行报错。
3. 微信开发者工具-->项目-->关闭上传代码时样式自动补全。 重要：某些情况下漏掉此项也会运行报错。
4. 微信开发者工具-->项目-->关闭代码压缩上传。 重要：开启后，会导致真机computed, props.sync 等等属性失效。（注：压缩功能可使用WePY提供的build指令代替，详见后文相关介绍以及Demo项目根目录中的wepy.config.js和package.json文件。）
5. 本地项目根目录运行wepy build --watch，开启实时编译。（注：如果同时在微信开发者工具-->设置-->编辑器中勾选了文件保存时自动编译小程序，将可以实时预览，非常方便。）


