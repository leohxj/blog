---
title: "2021年设置你的 Mac 环境"
date: 2021-08-14T21:39:29+08:00
tags: []
categories: []
---

最近在考虑添加一个苹果主机, 在 M1 Mac Mini 和 NUC8I5BEK(黑苹果) 之间犹豫不定, 最后还是看重了性价比, 放弃了 M1, 购买了硬改黑苹果, 不到 4k 的价格, 享受 32G 内存 1T SSD 的快感 :)

本文不是最佳实践, 只是个人的安装实录.

## 黑苹果の坑

很早之前就在公司用的 NUC8i5BEH (厚版), 只是没有硬改, 蓝牙/wifi 稍微会有点不稳定, 其他体验还是非常舒心, 加上最近老刷到一些相关的视频, 比如 [这台迷你电脑竟然可以“完美”黑苹果，而且还不贵？【开箱翼闻录 第 26 期】 - YouTube](https://www.youtube.com/watch?v=d1olPsGzfGo), 加强了我想购买一台 NUC 的想法.

纵使虽然 NUC8 的兼容性非常好了, 配合 OC 引导 MacOS, 但还是会有一些些小问题, 这里我也记录一下我遇到的坑:

### 双显示器连接问题

对于双显示器的用户, 需要一台连接 HDMI, 一台连接 Type-c.

但是开机的时候, 可能会遇到黑屏,花屏, 蓝牙断掉等不稳定的情况, 需要我们进入 BIOS, 找到 Device 设置, 修改主屏信号为 HDMI, 副屏保持 None, 开机的时候, 让 NUC8 只显示一个信号.

### 系统升级问题

OC 引导的系统, 只有大版本不变化, 几乎升级不受影响, 体验还是很好的, 这块自己就不折腾了, 有问题找卖家客户远程升级.

---

**回归主线**, 下面说说 Mac 系统, 我都做了什么设置, 让它使用起来更加舒服~

## 更换账号

通常卖家寄过来的电脑中, 是有一个管理员用户的, 可能叫做 admin/mac 之类的, 我们自己的电脑肯定想要改一下了.

两种方法:

1. 在账户中,右击修改替换账户名称和对应的主目录, 直接修改这个管理员名称
2. 新建一个管理员, 然后切换到这个管理员, 再进入账户删除之前的那个管理员.

我选择的是方式 2. 接下来我们可以选择登录 iClould 账号, 同步账号.

## 系统基本设置

接下来, 我们做一些基础的系统设置, 让自己找到熟悉的操作感觉:

- 键盘设置, 如果没有使用 Karabiner-Elements 的习惯, 可以用系统设置->键盘->修改按键:
  - CAPS LOCK -> Left Ctrl
  - 依据键盘的情况, 互换 opts, cmd 按键
- 触控板
  - 提高一些灵敏度
  - 可访问性 -> 修改 Pointer Control -> Trackpad Options -> 开启三指拖拽窗口
- 网络设置
  - 如果连接了有线网卡, wifi 可以选择打开(为了 AirDrop), 但不自动加入网络环境
- 输入法切换
  - 修改添加双拼输入法
  - 默认切换是 `ctrl + space`
- 蓝牙设置
  - 连接好键鼠, 默认就是可以唤起机器的
- 桌面设置
  - 清理 Dock 上面 App 不用的全部取消固定
  - Dock 放在左侧, 默认自动隐藏
  - 顶部菜单自动隐藏
  - 设置 Hot Trigger, 添加右上角关闭显示器
- 修改主机名称, HostName 是在终端下展示的名称, Computer Name 是共享是否显示的名称
  - sudo scutil --set HostName 新的计算机名
  - sudo scutil --set ComputerName 新的计算机名
- Finder 设置
  - 默认打开 Downloads 目录
  - 清理 Sidebar 的 Favorites
- 添加快捷键
  - 设置 -> 键盘 -> 快捷键
  - disable 一些用不到的
  - 添加桌面切换, ctrl + 1/2/3

## 从其他设备传输文件/文字

我希望传输一些文件(主要是安装包)到新的机器上, 或者一些文字(密码,配置数据)的话, 可以通过下面几种方式:

- 如果都是 Mac, 并且保持 Wifi 开启 (不需要在同一个局域网), 即可通过 AirDrop 传输.
- 如果在同一个局域网内, 打开 https://snapdrop.net 即可发送文件或文字
- 如果登录了 iclould, 也可以利用备忘录同步一些文字.
- 可以使用奶牛快传打包传输安装包

## 安装 clash

首先我们要把网络环境搞起来, 我使用的是 `clash-for-windows` 在 Mac 的客户端, 名字很绕, 但它是全平台的.

可以先安装 TUN 模式, 让系统能做透明代理, 就是让那些不支持读取 http_proxy 之类的软件, 也能走到代理中的意思, 开启之后, 还是需要做一些 mixed 的配置设置: [TUN 模式 | Clash for Windows](https://docs.cfw.lbyczf.com/contents/tun.html) .

导入自己的机场 clash 配置, 让节点生效, 我一般访问看下 youtube 是否正常.

开启 mixed 模式, 添加设置 http, sock 代理, 也可以保留 mixed port.

如果我们需要使用 VSC 作为配置修改的编辑器的话, 需要在 VSC 中将 `code` 命令安装到 PATH 下.

## 安装 Homebrew

首先下载 [The Missing Package Manager for macOS (or Linux) — Homebrew](https://brew.sh/) , 它是 Mac 下的一个软件包管理器, 大部分软件都可以从这里下载. brew 的网络需要代理,所以建议先设置好 clash 开启全局代理或者透明代理, 但我是先配置了 Profixier, 而没有有 Clash 的透明代理

安装 `brew` 之前, 我们还需要检查一下[前置条件 ](https://docs.brew.sh/Installation) :

```
- A 64-bit Intel CPU or Apple Silicon CPU 1
- macOS Mojave (10.14) (or higher) 2
- Command Line Tools (CLT) for Xcode: xcode-select --install, developer.apple.com/downloads or Xcode 3
- A Bourne-compatible shell for installation (e.g. bash or zsh) 4
```

安装成功后, 我们可以执行 `brew help` 或者 `brew install neofetch` 测试一下是否正常. `neofetch` 是我非常喜欢的 cli 工具, 值得一试.

## 安装同步盘

先安装网盘应用, 是因为我很多配置数据在网盘之中, 我目前的主力网盘是 Dropbox.

- dropbox: 用于同步盘, **仅限 3 台设备**, 包括手机, 一旦超出设备限制, 付费额度还是挺贵的, 起步 9.9$/month. 我用它存放了:

  - 1password 的数据库 (可替换自建 bitwarden)
  - dotfiles, 软件基本设置都在里面 (可替换存储在坚果云)
  - 其他第三方软件做 IFTTT 的联动,比如自动存储我 Instagram 上的照片.(基于网络, 不用替换)
  - 一些插件/APP 的同步, 比如 snippetsLab, Keyboard Maestro, Stylus, uBlacklist, 有些时候是因为软件只提供了通过 dropbox 同步的方式,没有办法直接选择本地文件 (看文件类型,如果是读取本地文件的, 也可以转移到坚果云中)

- Google Drive: 最近新出的客户端, 主要用于:

  - google Photos
  - 一些大的软件安装包
  - Eagle 的资料库
  - snipaste 保存的截图

- iClould: 登录了账号之后, 自动就开启了

如果需要一个替代 Dropbox 的同步盘的话,我推荐用坚果云, 毕竟 Dropbox 在多设备上登录的限制, 太贵了, 可以这么使用:

- 一种方式完全使用坚果云同步, 把 dropbox 中的一些文件迁移过来, dropbox 以后只用于云同步, 不登录设备
- 另外一种方式, 把坚果云当做只读镜像, 在一台主设备上安装, 去同步 dropbox 的目录, 让它自动检测更新

## 下载基础软件

- Alfred: 快捷启动, 数据同步在 Dropbox 中
- manico 快速启动
- Proxifier: 设置透明代理
- Karabiner-Elements: 取消系统的键盘映射, 直接在软件里面配置.
- startupizer 2: 管理开机启动
- Shiftit: 快捷窗口调整
- 搜狗输入法: 安装后, 删除之前的系统双拼
- Google Chrome
- VS code: 编辑器, 并安装 code 到环境变量
- HyperSwitch: 管理切换的窗口预览

## 设置字体

很多配置依赖了某些字体,所以这是前置的条件, 需要先进行安装, 我主要用到了 [Nerd Fonts - Iconic font aggregator, glyphs/icons collection, & fonts patcher](https://www.nerdfonts.com/) 中的 `FiraCode Nerd Font`, Nerd Fonts 是为常见字体添加了很多图标的一种字体, 如果没有图标需求, 可以使用 [tonsky/FiraCode: Free monospaced font with programming ligatures](https://github.com/tonsky/FiraCode).

字体的:[安装方式](https://github.com/ryanoasis/nerd-fonts/blob/master/readme.md#font-installation) 比较多样, 我选择使用 `homebrew` 安装:

```
brew tap homebrew/cask-fonts
brew install font-fira-code-nerd-font
brew install font-fira-code
```

这类字体还有个特性叫做 `ligatures`, 让符号显示的更加显著, 但需要软件进行开启, 可参考: [Home · tonsky/FiraCode Wiki](https://github.com/tonsky/FiraCode/wiki)

## 让系统更好用一点的软件

### 基础软件

- totalFinder
- bartender 3
- finicky
- snipaste
- NeatDownloadManager.app
- Free Download Manager
- monitorControl
- speedtest-cli
- typora
- DockTime
- https://fliqlo.com/
- AutoSwitchInput Lite
- tencent lemon 清理工具

### 效率工具

- Vivaldi: 我主力浏览器, 功能很多
- 滴答清单
- Obsidian
- rescueTime
- BetterTouchTool
- keyboard Maestro
- Eye monitor

---

基础的设置完后, 我就要为 **工作环境(编码)** 做一些设置了.

## 安装开发工具

开发需要的工具:

- hyper
- VSCode
- utools
- switchHosts
- Eagle
- snippetsLab
- Docker For Mac

### node 环境

[Volta - The Hassle-Free JavaScript Tool Manager](https://volta.sh/) , 这是一个 Rust 写的 node 工具安装包,可以在项目中固定 node 版本, 全局 node_modules 的安装也不需要关联到 yarn/npm 中

安装后,需要安装:

- volta install node
- volta install yarn
- 安装一些常用 node_modules
  - rimraf
  - serve
  - qnm
  - autarky
  - are-you-es5

### git cz 配置

Volta 安装:

- commitizen
- cz-customizable

然后设置 .cz-config.js, `.czrc`

### cli 工具

- [buo/homebrew-cask-upgrade: A command line tool for upgrading every outdated app installed by Homebrew Cask](https://github.com/buo/homebrew-cask-upgrade)
- mas
- bat
- delta
- tig
- fzf
- lazygit
- gitui
- lsd
- hstr
- golang
- kubectl
- kubecm
- k3d
- zoxide
- starship
- thefuck
- mcfly
  - `brew tap cantino/mcfly`
  - `brew install mcfly`
- broot
  - 安装后,需要执行一次 `broot`
- thefuck
- fortune
- cowthink
- dua / duf /duck /dust / gdu
- dog
- hugo

### oh-my-zsh

环境需要依赖 zsh > 5.0.8, git > 2.4.11, curl/wget 存在即可执行:

`sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

### omz 插件

全部通过 git 方式 clone 到 `~/.oh-my-zsh/custom/plugins`:

- [wfxr/forgit: A utility tool powered by fzf for using git interactively.](https://github.com/wfxr/forgit)

- [zdharma/fast-syntax-highlighting: (Short name F-Sy-H). Syntax-highlighting for Zshell – fine granularity, number of features and multiple shipped themes.](https://github.com/zdharma/fast-syntax-highlighting)
- [Aloxaf/fzf-tab: Replace zsh's default completion selection menu with fzf!](https://github.com/Aloxaf/fzf-tab)
- [g-plane/zsh-yarn-autocompletions: Zsh plugin for Yarn autocompletions.](https://github.com/g-plane/zsh-yarn-autocompletions)
  - 需要下载下来放在指定目录
- [zsh-users/zsh-autosuggestions: Fish-like autosuggestions for zsh](https://github.com/zsh-users/zsh-autosuggestions)
- [lukechilds/zsh-better-npm-completion: Better completion for npm](https://github.com/lukechilds/zsh-better-npm-completion)
- [zsh-users/zsh-completions: Additional completion definitions for Zsh.](https://github.com/zsh-users/zsh-completions)
- [zsh-users/zsh-history-substring-search: 🐠 ZSH port of Fish history search (up arrow)](https://github.com/zsh-users/zsh-history-substring-search)

### prompt: starship

[starship/starship: ☄🌌️ The minimal, blazing-fast, and infinitely customizable prompt for any shell!](https://github.com/starship/starship)

`brew install starship`

不需要设置 oh-my-zsh

### tmux

强烈推荐配合 [tmuxinator/tmuxinator: Manage complex tmux sessions easily](https://github.com/tmuxinator/tmuxinator) 使用, 更加方便的管理你的启动项目:

```bash
brew install tmux
brew install tmuxinator
```

使用了 [gpakosz/.tmux: 🇫🇷 Oh my tmux! My self-contained, pretty & versatile tmux configuration made with ❤️](https://github.com/gpakosz/.tmux) 的配置, 需要在 `~` 目录下安装:

```
cd
git clone https://github.com/gpakosz/.tmux.git
ln -s -f .tmux/.tmux.conf
cp .tmux/.tmux.conf.local .
```

重启配置 `tmux source-file ~/.tmux.conf` 生效.

### SSH 设置

主要是生成和管理 ssh config 文件

### Docker Desktop

如果想要体验 Docker Desktop For Mac 提供的 k8s, 需要吧 `gcr.io` 设置到透明代理中, 不然镜像拉取大概率会失败,导致启动一致在 `starting...`, 另外一个选择就是 [AliyunContainerService/k8s-for-docker-desktop: 为 Docker Desktop for Mac/Windows 开启 Kubernetes 和 Istio。](https://github.com/AliyunContainerService/k8s-for-docker-desktop).

## 同步 dotfiles

我的 dotfiles 是放在 dropbox 上, 自己有个安装脚本, 检查一下依赖是否都安装, 即可执行.通过软连接使用存在在 Dropbox 中的配置文件.

## 配置开机启动的服务

brew service 管理. 比如 nginx, code-server 之类的本机安装的应用, 项目的大部分会走 Docker 安装.

## 配置远程访问

小主机放在家里肯定就 24h 开机了, 不如就当做服务器,开通下外部访问. 平时只有关闭显示器即可.

### 阻止系统休眠

MAS 安装 `amphetamine`, 免费好使.

### 设置远程桌面

设置 -> "共享" -> 开启 屏幕共享, 即可使用 5900 端口访问

### 开通 SSH 访问

设置 -> "共享" -> 开启远程访问, 即可通过 22 端口连接, 在客户端机器上再生成 ssh-key 即可免密码登录.

### 内网固定 ip, 并设置转发

最后, 我们想要在外网访问的话, 还需要再路由器上固定一下这台设备的 ip, 并设置好转发的端口, 比如我会使用:

- 6022 => 内网机器:22
- 6059 => 内网机器:5900

### 设置免密登录

原理是生成 ssh_key, 可以在 `~/.ssh` 目录下执行:

#### 客户端执行 ssh-keygen 生成 key

`ssh-keygen -t rsa -f ~/.ssh/xxx_rsa`
生成 `~/.ssh/xxx_rsa` 和 `~/.ssh/xxx_rsa.pub` 两个文件

#### 客户端执行 ssh-copy-id 同步到服务端

不用这个的话，就是 scp 拷贝，会麻烦一点。

`ssh-copy-id -i ./xxx_rsa.pub username@ip_addr` 将公钥上传.

相当于执行 copy , 登录远程服务器，写入远程服务器的 ~/.ssh/authorized_keys .

注意: ==远程服务器登录的用户要一致==，写入到需要登录的用户目录，==而不是 root 哦==

如果是不同的端口的情况, 执行:

`ssh-copy-id -i ./xxx_rsa.pub -p 1234 user@hostname`

---

最后记录一些新 "Bug Sur" 遇到的问题. 目前为止( 20210815), 系统兼容性感觉比半年前强多了.

## Mac Big Sur 不兼容的一些 App

我目前使用的还是 Mac 10.15, 没有升级到 Big Sur, 因为苹果现在新版本 Bug 太多了, 就好像是我开发的一样, 我在这里记录一下我用的 app 有哪些在新版本上无法运行了的.

- profixier: 需要升级到 V3
  - 可以导入 v2 的配置
  - 需要系统授权网络过滤
