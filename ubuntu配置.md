---
title: ubuntu配置
copyright: true
date: 2017-10-29 21:55:50
tags: [ubuntu, 教程]
categories: ubuntu 安装
---

# ubuntu16.04安装后配置

<!--more-->
## 0.更新源

更换阿里云的源
``` bash
# deb cdrom:[Ubuntu 16.04.3 LTS _Xenial Xerus_ - Release amd64 (20170801)]/ xenial main restricted

# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
# newer versions of the distribution.
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial main restricted

## Major bug fix updates produced after the final release of the
## distribution.
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-updates main restricted

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team. Also, please note that software in universe WILL NOT receive any
## review or updates from the Ubuntu security team.
deb http://mirrors.aliyun.com/ubuntu/ xenial universe
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-updates universe

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team, and may not be under a free licence. Please satisfy yourself as to
## your rights to use the software. Also, please note that software in
## multiverse WILL NOT receive any review or updates from the Ubuntu
## security team.
deb http://mirrors.aliyun.com/ubuntu/ xenial multiverse
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiverse
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-updates multiverse

## N.B. software from this repository may not have been tested as
## extensively as that contained in the main release, although it includes
## newer versions of some applications which may provide useful features.
## Also, please note that software in backports WILL NOT receive any review
## or updates from the Ubuntu security team.
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse

## Uncomment the following two lines to add software from Canonical's
## 'partner' repository.
## This software is not part of Ubuntu, but is offered by Canonical and the
## respective vendors as a service to Ubuntu users.
# deb http://archive.canonical.com/ubuntu xenial partner
# deb-src http://archive.canonical.com/ubuntu xenial partner

deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted
# deb-src http://security.ubuntu.com/ubuntu xenial-security main restricted
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
# deb-src http://security.ubuntu.com/ubuntu xenial-security universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-security multiverse
# deb-src http://security.ubuntu.com/ubuntu xenial-security multiverse
```
list文件在`/etc/apt/sources.list`下

然后更新源和软件
```bash
sudo apt-get update
sudo apt-get dist-upgrade
```
## 1.更换显卡驱动
![显卡更换](/photo/驱动01.png)

ubuntu上有nvidia的驱动

## 2.同步时间
使用双系统时间不同，故要使用
```bash
sudo timedatectl set-local-rtc 1
```

## 3.安装软件
### 3.1 删除亚马逊链接
```bash
  sudo apt-get remove unity-webapps-common
```
### 3.2 安装vim
    sudo apt-get install vim
### 3.3 安装git和vpnc
    sudo apt-get install vpnc git

### 3.4 安装ExFat文件系统驱动
Ubuntu默认不支持exFat文件系统的挂载，需要手动安装exfat的支持

    sudo apt-get install exfat-fuse

### 3.5 修复分区
    sudo ntfsfix /dev/sda8

### 3.6安装atom
官网下载最新版本 [atom](https://atom.io/)

### 3.7 uget+aria2下载工具
安装uget和aria2
```bash
sudo apt-get install uget  aria2
```
配置

 a.打开uget。

 b.打开界面的编辑—>设置—>插件，插件匹配顺序：aria2 。

 c.打开界面的分类—>默认一般设置。调整最大连接数（建议在5)。设置一下下载文件夹。

在火狐中使用Flashgot扩展就可以。
### 3.8 安装shadowsocks-qt5
* 添加源安装

```bash
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```

### 3.9 安装wine/TIM
使用 wine staging （ 开发版本的 wine ）安装详细教程 https://wine-staging.com/installation.html
```bash
sudo dpkg --add-architecture i386
#add the repository:
wget -nc https://dl.winehq.org/wine-builds/Release.key
sudo apt-key add Release.key
sudo apt-add-repository https://dl.winehq.org/wine-builds/ubuntu/
#install
sudo apt-get update
sudo apt-get install --install-recommends winehq-staging
```
去qq官网下载[TIM](http://office.qq.com/) 我使用的是TIM1.2。

安装wine后 ，命令执行`winecfg`然后会安装一些插件，选择安装它们。然后拷贝windows字体（不安装字体会使一些字无法查看）。

windows字体在c：\windows\fonts

把这个目录下字体全部复制到wine下的设置目录下：

~/.wine/drive_c/windows/Fonts/

重载所有wine的配置`wineboot`
再次打开wine `winecfg`配置如图

![wine 设置](/photo/wine设置.png)

然后重启一下wine `wineboot` 。安装tim时，使用Wine Windows Program Loader 打开TIM。然后就是windows下安装程序——点点点。

备注：此方法有个bug 当TIM打开讨论组时会是TIM崩溃。

建立桌面快捷方式
```bash
[Desktop Entry]
Encoding=UTF-8
Version=1.2
Name[en_US]=TIM
Name[zh_CN]=腾讯TIM
Exec=env LC_ALL=zh_CN.utf8 wine /home/h/.wine/drive_c/Program\ Files\ \(x86\)/Tencent/TIM/Bin/TIM.exe
Icon=/home/h/we/electronic-wechat-linux-x64/photo/TIM.ico
Terminal=false
NoDisplay=false
StartupNotify=true
Type=Application
Categories=Network;InstantMessaging;
```


### 3.10  安装electronic-wechat
项目在 [github](https://github.com/geeeeeeeeek/electronic-wechat) 上 。
我使用源码安装失败了，npm总是出错，不知为什么。（如果你使用源码安装成功，请求教）最后使用安装版，见此 https://github.com/geeeeeeeeek/electronic-wechat/releases

在桌面上建立图标
```bash
[Desktop Entry]
Encoding=UTF-8
Name=微信
Comment=electronic-wechat
Exec=/home/h/we/electronic-wechat-linux-x64/electronic-wechat
Icon=/home/h/we/electronic-wechat-linux-x64/photo/icon.png
Terminal=false
StartupNotify=true
Type=Application
Categories=Application
```
### 3.11 安装最新的python3
首先下载最新的python版本，这个在python官网上下载[python官网](https://www.python.org/downloads/)

解压 进入其目录
```bash
./configure
make
make install
```
安装完后检查二进制文件的位置
```ruby
which python3
python3 -V #V是大写的
```
### 3.12安装最新版本的npm 和 nodejs
首先安装npm和 nodejs
```ruby
sudo apt-get install nodejs-legacy
suao apt-get install npm
```
ubuntu上安装时npm和nodejs都是低版本。
- 升级npm
```bash
sudo npm install npm -g
```
- 升级node.js
```bash
sudo npm install -g n
sudo n stable
```
## 4.gnome3的安装和配置
- 安装gnome
```bash
sudo apt-get install gnome
```
- arc主题
在github上的项目 [horst3180/arc-theme](https://github.com/horst3180/arc-theme)

- Papirus图标
github上项目[PapirusDevelopmentTeam/papirus-icon-theme](https://github.com/PapirusDevelopmentTeam/papirus-icon-theme)
```bash
sudo add-apt-repository ppa:papirus/papirus
sudo apt-get update
sudo apt-get install papirus-icon-theme
```
