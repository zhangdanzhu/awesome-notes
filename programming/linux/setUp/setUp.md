
* [Linux SetUp](#linux-setup)
	* [Lang](#lang)
	* [Purge](#purge)
	* [Set Up](#set-up)
		* [Config](#config)
		* [PPA](#ppa)
		* [Download](#download)
		* [Office/Docs](#officedocs)
		* [Entertainment](#entertainment)
			* [music](#music)
			* [game](#game)
				* [playonlinux](#playonlinux)
				* [emulator](#emulator)
		* [ fonts](#-fonts)
		* [tools](#tools)
		* [GDB Hacker UI](#gdb-hacker-ui)
			* [GDB-DashBoard](#gdb-dashboard)
			* [Voltron](#voltron)
				* [pip3](#pip3)
				* [install](#install)
				* [config](#config-1)
	* [IDE](#ide)
		* [ Android Studio](#-android-studio)
		* [ WebStorm](#-webstorm)
	* [Text Editor](#text-editor)
		* [Vim(Github)](#vimgithub)
		* [Sublime Text](#sublime-text)
		* [Atom](#atom)
		* [Visual Studio Code](#visual-studio-code)
	* [Software](#software)
		* [Desktop](#desktop)
			* [gnome](#gnome)
		* [ZealDocs](#zealdocs)
		* [[N1(E-Mail)](https://invite.nylas.com/download/)](#n1e-mailhttpsinvitenylascomdownload)
	* [Shell Tools](#shell-tools)
		* [f-irc/irssi (irc client)](#f-ircirssi-irc-client)
		* [wifi access point](#wifi-access-point)
		* [pppoeconf/speedtest-cli](#pppoeconfspeedtest-cli)
		* [Terminal](#terminal)
		* [zsh](#zsh)
	* [Platform](#platform)
		* [Nodejs](#nodejs)
			* [Npm](#npm)
			* [Bower](#bower)
	* [firefox extensions](#firefox-extensions)
	* [Chrome Extensions](#chrome-extensions)
	* [GFW](#gfw)
		* [Hosts](#hosts)
		* [XX-Net](#xx-net)
		* [Lantern](#lantern)
		* [Shadowsocks](#shadowsocks)
		* [sshuttle](#sshuttle)
		* [Proxychains(Global Proxy)](#proxychainsglobal-proxy)
		* [OpenVPN && vpngate/vpnbook](#openvpn--vpngatevpnbook)
		* [Docker VPN](#docker-vpn)
	* [Windows](#windows)
		* [Imitate Linux](#imitate-linux)
		* [Software](#software-1)

# Linux SetUp

> 建立一个软件repo，加快装机速度，是程序员的必备修养 -- 尼采

/home /usr /opt /lib /var /etc

------

## Lang

```shell
export LANG=en_US
xdg-user-dirs-gtk-update
export LANG=zh_CN
```

```shell
sudo locale-gen zh_CN.GBK
sudo locale-gen zh_CN.GB2312
sudo locale-gen zh_CN.GB18030
```

```shell
sudo vim /var/lib/locales/supported.d/local
zh_CN.GBK GBK
zh_CN.GB2312 GB2312
zh_CN.GB18030 GB18030
sudo dpkg-reconfigure locales
```

## Purge

```shell
sudo apt-get remove libreoffice-common
sudo apt-get remove unity-webapps-common
```

## Set Up

### Config

```shell
sudo vim /etc/dhcp/dhclient.conf

prepend domain-name-servers 127.0.0.1;下一行

prepend domain-name-servers 114.114.114.114;
prepend domain-name-servers 223.5.5.5;
```

### PPA

```shell
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
sudo vim /etc/apt/sources.list < deb http://dl.google.com/linux/deb/ stable main
sudo add-apt-repository ppa:nilarimogard/webupd8
sudo add-apt-repository ppa:webupd8team/sublime-text-3
sudo add-apt-repository ppa:webupd8team/atom
sudo add-apt-repository ppa:numix/ppa
sudo add-apt-repository ppa:zeal-developers/ppa
sudo add-apt-repository ppa:ubuntu-desktop/ubuntu-make
sudo add-apt-repository ppa:ubuntu-wine/ppa
sudo add-apt-repository ppa:docky-core/stable
sudo add-apt-repository ppa:noobslab/deepin-sc
sudo add-apt-repository ppa:noobslab/themes
sudo add-apt-repository ppa:noobslab/macbuntu
sudo apt-get update
```

### Download

```shell
sudo apt-get install aria2
mkdir -p ~/.aria2
sudo vim ~/.aria2/aria2.conf
firefox-addons: baiduexporter send-to-aria2
```

```conf
#用户名
#rpc-user=user
#密码
#rpc-passwd=passwd
#设置加密的密钥
#rpc-secret=secret
#允许rpc
enable-rpc=true
#允许所有来源, web界面跨域权限需要
rpc-allow-origin-all=true
#是否启用https加密，启用之后要设置公钥,私钥的文件路径
#rpc-secure=true
#启用加密设置公钥
#rpc-certificate=/home/acgotaku/.config/aria2/example.crt
#启用加密设置私钥
#rpc-private-key=/home/acgotaku/.config/aria2/example.key
#允许外部访问，false的话只监听本地端口
rpc-listen-all=true
#RPC端口, 仅当默认端口被占用时修改
#rpc-listen-port=6800
#最大同时下载数(任务数), 路由建议值: 3
max-concurrent-downloads=5
#断点续传
continue=true
#同服务器连接数
max-connection-per-server=5
#最小文件分片大小, 下载线程数上限取决于能分出多少片, 对于小文件重要
min-split-size=10M
#单文件最大线程数, 路由建议值: 5
split=10
#下载速度限制
max-overall-download-limit=0
#单文件速度限制
max-download-limit=0
#上传速度限制
max-overall-upload-limit=0
#单文件速度限制
max-upload-limit=0
#断开速度过慢的连接
#lowest-speed-limit=0
#验证用，需要1.16.1之后的release版本
#referer=*
#文件保存路径, 默认为当前启动位置
dir=D:\Downloads
#文件缓存, 使用内置的文件缓存, 如果你不相信Linux内核文件缓存和磁盘内置缓存时使用, 需要1.16及以上版本
#disk-cache=0
#另一种Linux文件缓存方式, 使用前确保您使用的内核支持此选项, 需要1.15及以上版本(?)
#enable-mmap=true
#文件预分配, 能有效降低文件碎片, 提高磁盘性能. 缺点是预分配时间较长
#所需时间 none < falloc ? trunc << prealloc, falloc和trunc需要文件系统和内核支持
file-allocation=prealloc
#不进行证书校验
check-certificate=false
```

### Office/Docs

-   [Sougou Pinyin](http://pinyin.sogou.com/linux/)
-   [WPS](http://community.wps.cn/download/)
-   [Chartsflow Editor](http://www.yworks.com/products/yed/download)
-   [GeoGebra - 几何图形](http://www.geogebra.org/download)

```shell
sudo apt-get install ipython ipython3 python-pip python3-pip 
sudo apt-get install xpad xchm
```

```shell
sudo apt-get install fcitx-sogoupinyin
sudo apt-get install wps-office
```

### Entertainment

#### music

*   [Netease Music Official](http://music.163.com/#/download)

```sh
$ sudo -H pip install requests
$ sudo -H pip install pycrypto
$ sudo apt-get install deepin-music
$ git clone https://github.com/wu-nerd/dmusic-plugin-NeteaseCloudMusic.git
$ python dmusic-plugin-NeteaseCloudMusic/install.py
```

#### game

##### playonlinux

```shell
sudo apt-get install build-essential libgl1-mesa-dev libglu1-mesa-dev libglut-dev freeglut3-dev
```

```shell
wget -q "http://deb.playonlinux.com/public.gpg" -O- | sudo apt-key add -
sudo wget http://deb.playonlinux.com/playonlinux_trusty.list -O /etc/apt/sources.list.d/playonlinux.list
sudo apt-get update
sudo apt-get install playonlinux
```

##### emulator

```shell
sudo apt-get install vbaexpress 
sudo apt-get install visualboyadvance 
sudo apt-get install visualboyadvance-gtk
sudo apt-get install desmume
```

###  fonts

-   /usr/share/fonts/chinese/TrueType/
-   [Windows Fonts Download](http://pan.baidu.com/s/1jGRz7ue)

```shell
mkfontscale
mkfontdir
fc-cache
```

### tools

```shell
sudo apt-get install git unrar
sudo apt-get install screen ssh axel
sudo apt-get install lnav exfat-fuse manpages-zh
sudo apt-get install vpnc network-manager-vpnc 
sudo apt-get install hardinfo
sudo apt-get install gconf-editor
sudo apt-get install nautilus-actions -y
sudo apt-get install htop
```

### GDB Hacker UI

#### GDB-DashBoard

```shell
wget -P ~ git.io/.gdbinit
(gdb) source ~/.gdbinit
```

```shell
git clone https://github.com/cyrus-and/gdb-dashboard.git
cp -fr ./gdb-dashboard/.gdbinit ~/
rm -fr ./gdb-dashboard
(gdb) source ~/.gdbinit
```

#### Voltron

##### pip3

```shell
$ sudo easy_install3 pip
```

##### install

```shell
$ git clone https://github.com/snare/voltron
$ sudo apt-get install libreadline6-dev python3-dev python3-setuptools python3-yaml
$ cd voltron
$ sudo python3 setup.py install
```

##### config

-   .gdbinit

```shell
source /usr/local/lib/python3.4/dist-packages/voltron-0.1.2-py3.4.egg/voltron/entry.py
voltron init
set disassembly-flavor att

(gdb) source .gdbinit
```

-   view (in another terminal)

```shell
$ voltron view register stack disasm backtrace
```

## IDE

###  Android Studio

###  WebStorm

## Text Editor

### Vim(Github)

```shell
sudo apt-get install vim
```

-   basic version

```shell
curl https://raw.githubusercontent.com/wklken/vim-for-server/master/vimrc > ~/.vimrc
```

-   advanced version

```shell
curl https://j.mp/spf13-vim3 -L > spf13-vim.sh && sh spf13-vim.sh
wget -qO- https://raw.github.com/ma6174/vim/master/setup.sh | sh -x
```

### Sublime Text

```shell
sudo apt-get install sublime-text-installer
```

```shell
git clone https://github.com/lyfeyaj/sublime-text-imfix.git
cd sublime-text-imfix
./sublime-imfix
```

```markdown
—– BEGIN LICENSE —–
Nicolas Hennion
Single User License
EA7E-866075
8A01AA83 1D668D24 4484AEBC 3B04512C
827B0DE5 69E9B07A A39ACCC0 F95F5410
729D5639 4C37CECB B2522FB3 8D37FDC1
72899363 BBA441AC A5F47F08 6CD3B3FE
CEFB3783 B2E1BA96 71AAF7B4 AFB61B1D
0CC513E7 52FF2333 9F726D2C CDE53B4A
810C0D4F E1F419A3 CDA0832B 8440565A
35BF00F6 4CA9F869 ED10E245 469C233E
—— END LICENSE ——
```

```python
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

```shell
git clone https://github.com/sabertazimi/Awesome-Sublime
cd Awesome-Sublime
cp -fr User ~/.config/sublime-text-3/Packages/
```

### Atom

```shell
sudo apt-get install atom
```

-   firewall config

```
 $ touch ~/.atom/.apmrc && echo 'strict-ssl = false' > ~/.atom/.apmrc
```

-   plugins: sync-SETTINGS

e95d69c522d199322429

```javascript
  "activate-power-mode":
    screenShake:
      enabled: false
```

### Visual Studio Code

```Shell
sudo apt-get install ubuntu-make
umake web visual-studio-code
umake web visual-studio-code --remove
sudo ln -s /home/hustlyl/.local/share/umake/web/visual-studio-code/Code /usr/local/bin/code
```

## Software

### [Appearence](https://blog.microideation.com/2016/08/30/customizing-ubuntu-system/)

```shell
sudo apt-get install docky
sudo apt-get install numix-gtk-theme numix-icon-theme numix-icon-theme-circle
sudo apt-get install mac-ithemes-v3 mac-icons-v3
sudo apt-get install macbuntu-os-ithemes-lts-v7 macbuntu-os-icons-lts-v7 
```

#### gnome

```shell
sudo apt-get install gnome gnome-shell gnome-panel gnome-menus gnome-session gnome-tweak-tool gdm
```

### ZealDocs

```shell
$ sudo apt-get install zeal
$ sudo apt-get remove appmenu-qt5
```

### [N1(E-Mail)](https://invite.nylas.com/download/)

Awesome Open Source E-Mail Desktop App

## Shell Tools

[Futher List](http://www.codeceo.com/article/linux-terminal-guide.html)

### f-irc/irssi (irc client)

### wifi access point

`https://github.com/oblique/create_ap`

### pppoeconf/speedtest-cli

```shell
sudo pppoeconf
sudo pon dsl-provider
sudo poof
ifconfig ppp0
```

### Terminal

-   Font - DejaVu Sans Mono Book 12
-   Ctrl + Shift + E/O
-   Alt + Arrow

```shell
$ sudo apt-get install terminator
$ sudo update-alternatives --config x-terminal-emulator
```

### zsh

```shell
sudo apt-get install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
sudo vim /etc/passwd
```

-   theme : rkj-repos candy ys
-   plugins : git node ruby meteor bundler osx rake sublime

```shell
vim ~/.zshrc
```

## Platform

### Nodejs

```shell
sudo apt-get install nodejs nodejs-dbg
sudo apt-get install npm
```

#### Npm

```shell
sudo npm install -g n
sudo n stable
```

-   global awesome packages

```shell
sudo npm install -g bower gulp gh-pages tooling antd-init npm-check standard jslint jshint nodemon express express-generator speed-test babel-cli lebab
```

#### Bower

```shell
bower init
bower install bootstrap -S
bower install font-awesome -S
```

## firefox extensions

```shell
nohup ~/XX-Net-2.9.4/start.sh & >/dev/null 2>&1
```

-   AdblockPlus
-   Alexa
-   BetterSearch
-   ChatZilla
-   CLEO
-   Disconnect Search
-   DownThemeAll
-   ExtendedStatusBar
-   FEBE
-   Firebug
-   Flagfox
-   Greasemonkey
-   Markdown Here
-   Markdown Viewer
-   Octotree
-   Pan
-   SeleniumIDE
-   Stylish
-   TabMixPlus
-   User Agent Switcher
-   VimFx
-   Web Search Pro
-   xThundedr

## Chrome

### Installation

```sh
$ sudo apt install google-chrome-stable
```

### Extensions

-   Adblock Plus
-   AutoPager Chrome
-   Flag for Chrome
-   GitHub Awesome Autocomplete
-   GitHub Hovercard
-   GitHub improved
-   Gmail Notifier +
-   Google Search
-   Hacker News
-   Hacker News UX
-   IE Tab
-   Isometric Contributions
-   JSONView
-   Wikipedia
-   Momentum
-   Octo Mate
-   Octotree
-   Page load time
-   Proxy SwitchyOmega
-   Quora Extender
-   React Developer Tools
-   Search the current site
-   Show me the React
-   Stylish
-   Tampermonkey
-   Thunder
-   User-Agent Switcher
-   Vimium
-   Wappalyzer
-   Web Developer
-   Zhihu-showL
-   translate
-   vocabulary
-   bilibili
-   dida
-   zhihu
-   163

## GFW

### Hosts

https://github.com/racaljk/hosts

### XX-Net

```shell
sudo apt-get install python-vte  python-OpenSSL  python-gtk2 libffi-dev python-appindicator libnss3-tools
```

-   download

https://github.com/XX-net/XX-Net/wiki/%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95

-   firefox config: pan add-on

### Lantern

-   https://github.com/getlantern/lantern

### Shadowsocks

-   https://github.com/shadowsocks/shadowsocks
-   https://github.com/breakwa11/shadowsocks-rss
-   https://github.com/yangyangwithgnu/autoshadower

### sshuttle

-   https://github.com/apenwarr/sshuttle

### Proxychains(Global Proxy)

-   https://github.com/rofl0r/proxychains-ng

### OpenVPN && vpngate/vpnbook

-   https://github.com/OpenVPN/openvpn
-   https://github.com/waylau/vpngate-mirrors

```shell
sudo apt-get install openssl
sudo apt-get install pam-devel
```

```shell
sudo wget http://www.oberhumer.com/opensource/lzo/download/lzo-2.06.tar.gz
tar –zxvf lzo-2.06.tar.gz –C /usr/src/
cd /usr/src/lzo-2.06
sudo  ./configure --prefix=/usr/local
sudo make
sudo make install
```

```shell
sudo unzip openvpn-2.3.10.zip
cd openvpn-2.3.2
sudo ./.configure --prefix=/opt/openvpn-2.3.10
sudo make
sudo make install
```

```shell
sudo apt-get install easy-rsa
sudo apt-get install openvpn
```

### Docker VPN

https://github.com/hwdsl2/docker-ipsec-vpn-server

## Windows

### Imitate Linux

-   Wox
-   Cygwin

### Software

-   360 safe
-   qq browser
-   sougoupinyin
-   haozip
-   qq
-   baiduyun
-   thunder
-   netease music
-   office 2007
-   nodejs: cash
