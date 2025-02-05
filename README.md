# 一键脚本搭建科学上网Trojan梯子(自建VPN)详细教程,实测，应该是全网最简版

需要的背景知识：
1、你会用linux命令，
2、你会ssh登录，
3、你会放行防火墙端口80和443
4、你知道网站需要个证书，CA申请免费证书，为啥用CA，我感觉方便，一次成功了。用命令注册一下
5、云服务上安装个服务端，你自己的电脑和手机用客服端v2ray（手机端名称叫v2rayNG，电脑端叫V2rayN）。

原理:用一个网站来伪装，有脚本搞定，看了一圈，反正感觉是Trajon比较隐秘难发现

整体步骤
  1、买海外云服务器，
  2.买域名（国内国外均可）并可以设置好解析
  3、按照服务端和客户端使用


## Trojan简介

简单理解为就是v2ray+ws+TLS的lite版吧，也是走https，安全稳定性比较高。在某些设备上表现比v2ray+ws+TLS好一些。

## 1.购买VPS

首先第一步，你还是得有个vps（云服务器），我现在用的[vultr](https://www.vultr.com/?ref=9711005)，首要原因可以随时删除服务器重建服务器，以达到换ip的效果，然后就是可以支付宝付款，很方便了。

###  购买性价比高的墙外服务器？

在搭建之前需要一台境外的云服务器，而 [vultr](https://www.vultr.com/?ref=9711005) 服务商比较稳定，安全，相当于境内的阿里云。

值得说的一点是， [vultr](https://www.vultr.com/?ref=9711005) 给新用户的福利相当给力，充值 10 美元就可以获取 100 美元，你可以点击 [vultr 专属赠送新用户 100 美元](https://www.vultr.com/?ref=9711005) 进去抢先注册。

现在你只要选择支付宝充值 10 美元以上，就可以获得额外赠送的 100 美元。


接下来就可以在这个平台选购云服务器了。

服务器的位置，可以选择美国地区，比如纽约：

服务器类型，选：centos 7 x64 系统：

选择最便宜的完全够用，而且性价比高，注意不要选择IPv6 ONLY的，要不然无法搭建使用。


## 2.购买域名（买一个吧，IP我也试过用的ss，被封了。国外国内都行，国内的域名要去cloudflare搞个域名DNS）

Trojan和v2ray+ws+TLS一样，也是走https，所以也需要有一个自己的域名为我们做掩护。

买域名的地方也很多，国内主要是腾讯云和阿里云，域名不需要在国外供应商买，哪里买都一样。腾讯阿里都有新人优惠，第一次购买域名，像.club, .xyz之类的小众域名基本只要1块一年，很便宜了。我用的是腾讯云，这里也以腾讯云为例，其他平台的流程也基本一致。

在vps上用脚本安装Trojan服务端。

## 3.脚本安装v2ray服务端

首先下载一个ssh软件，比如FinalShell,Ssh登录来用

```
curl -O https://raw.githubusercontent.com/luyiming1016/ladderbackup/master/trojan_centos7.sh && chmod +x trojan_centos7.sh && ./trojan_centos7.sh
```


之后选1，回车，
提示需要输入域名，输入解析到本VPS的域名，然后回车。（提示，如果域名不正常解析IP，那么就会报错）
提示安装一个工具socat来验证请求
```
# 对于基于 Debian/Ubuntu 的系统
apt-get install socat

# 对于基于 CentOS/RHEL 的系统
yum install socat
```
其次还会有证书的申请
```
acme.sh --set-default-ca --server letsencrypt
acme.sh --register-account -m your-email@example.com
```
客户端的用户名和密码是自动生成的：
  安装完成的信息，是一个指向你域名下的一个下载链接，右键选中复制下来，然后粘贴进浏览器下载，下载下来是一个trojan-cil压缩包，解压点开文件夹下usr->src->trojan-cil->config.json，记下remote_addr, remote_addr, password三项。

验证伪装的网站是否正常：
  用任意一个浏览器输入你刚才注册的域名，应该就可以看到一个这样的网站（如果你到这看不到网页，就别往下做了，有两个原因，第一可能你买域名的时候解析地址填错了，你的海外云服务器把80和443端口防火墙放行，）。理解为它就是我们翻墙的掩护就好。




之后回到我们的ssh界面，输入下面的代码安装BBR加速，安了会让科学上网速度提升很多。

```
cd /usr/src && wget -N --no-check-certificate "https://raw.githubusercontent.com/luyiming1016/ladderbackup/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```

之后显示BBR启动成功，这样我们服务端架设的步骤就完全完成了。

这时候注意最好在命令行输入service v2ray start，确保v2ray起来了，因为有时会发生系统重启v2ray进程起不来，会发生伪装网站可以进但配好不好用的状况

# 客户端使用
## window使用Trojan梯子
客户端下载地址：
https://github.com/2dust/v2rayN/releases

windows下现在体验最好的还是这个带Trojan版本的v2ray客户端。

- 双击打开trojan文件夹下的 v2rayN.exe 文件
- 如客户端运行Trojan报错，请尝试下载以下软件，安装完毕再运行Trojan
- [isual C++ 2015 (x64、x86均安装)](https://www.microsoft.com/zh-CN/download/details.aspx?id=48145)
- [Windows.NET Framework 4.6.2](https://support.microsoft.com/zh-cn/help/3151800/the-net-framework-4-6-2-offline-installer-for-windows)
- 桌面右下角会出现1个小图标，再次双击，打开 Trojan 设置面板
- 点击「服务器」-「Add [Trojan] server」来添加Trojan节点
- 打开v2rayN.exe,右下角出现如下小图标：




双击左键点开，选择服务器，添加Trojan服务器。将之前在ssh客户端中记下的参数(地址，端口，密码)输入这里，然后点击确定。

之后就配置成功了，建议在配好的服务器上单击右键选择“批量导出分享URL至粘贴板”，然后在记事本中保存，之后会用在配置手机客户端里。

之后点右上角×关闭主界面，在右下角小图标单击右键。在服务器一栏勾选上已经配好的服务器，点击启用Http代理，代理模式选pac模式。之后就可以开心的科学上网了。提醒一下记得不用的时候，按顺序先把启用http代理勾选取消，之后退出客户端，最后关电脑。如果有没有按顺序操作，再次开机发现上不了网了，请在这里的小电脑点右键，打开“网络和Internet设置”→代理→手动设置代理，将使用代理服务器一栏关闭。

##  ios使用Trojan梯子（没验证过，你们自己试试，支撑Trajon协议的就可以）
新版本的Shadowrocket（简称小火箭）也支持Trojan的配置，虽然小火箭也要钱，不能在大陆App store下载，但是网上共享账号很多，大家可以多搜一搜，关键词“Shadowrocket/小火箭 ios 共享账号”。我这里提供两组账号，大家试一试，不保证好用，因为这种多人共享的账号很容易被苹果封了，账号时效性很强，不好用大家就自己找找了。

app store账号：hadadreammm@gmail.com
密码：As778899

注意下完小火箭后一定马上把账号退了，换回自己的账号。

之后把刚才在电脑端导出的链接复制到手机里，在手机上直接复制，然后打开小火箭，小火箭就会自动识别到已经复制好的链接，将服务器添加进来，之后把最上面的链接打开就好了。（软件会要求使用手机vpn，请允许）

若导出链接在小火箭里不好用，就在小火箭手动添加Trojan节点，输入地址，端口，密码，

## Android使用Trojan梯子
搜一个v2rayNG版本下载安装 地址，端口，密码三项信息就可以了。
