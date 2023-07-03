# fujungit.github.io
个人主页
---

## 1、准备工作（服务器开放端口&域名解析）

> 1、VPS 一台重置好主流的操作系统 （CentOS 7+、Ubuntu 16+、Debian 8+），[美国AWS亚马逊网站](https://aws.amazon.com/)绑定1张银联卡(开启服务器后可以更换1张不用的信用卡，避免乱扣费)，选择EC2，跟着免费的指引走(创建密钥下载并保存、及安全组出入站规则选择全部流量和端口) 
>
> 2、记录IPv4公网ip和默认的用户名(Ubuntu是ubuntu，Debian是admin，CentOS是centos)
>
> 3、域名一个，做好相关的解析，若是需要套用 CDN，请托管域名到 [Cloudflare](https://dash.cloudflare.com/) ，域名可在[NameSilo购买](https://www.namesilo.com/domain/search-domains)，有免费的隐私保护

### 1.1、**开通**aws的服务器**后**设置安全组（其他平台服务器同理）

![img](https://secure2.wostatic.cn/static/jQaffT1x1xSZx6VknVE4oV/image.png?auth_key=1688413853-cbud49HH7WG88vp6SE2UiR-0-7c22a27e1b2d97e8d91bf2d576aafc23)

### 1.2、购买域名后去[Cloudflare](https://dash.cloudflare.com/)，将在NameSilo买的域名托管到Cloudflare，并在Cloudflare做域名解析

![img](https://secure2.wostatic.cn/static/4eMgzMbduvCTH2fbSH3yzN/image.png?auth_key=1688413853-fczZWn9qv7xhkcSxLBxBuP-0-03606a197cdd8cabccfdf13b1c1f5bff)

![img](https://secure2.wostatic.cn/static/pqicYdLj5HPdf2Q6TsrHHc/image.png?auth_key=1688413853-7bT6sGuB7Ss93FhvsgU2ew-0-d938f85a3f166ab3b0154adbcc46f7fa)

#### 1.2.1、**复制Cloudflare的提供的服务器**

![img](https://secure2.wostatic.cn/static/skwkgp4xMAHKsq4EwZ5UBa/image.png?auth_key=1688413853-s3CYP85W9fLThrRKnuU2H9-0-83252e2bb3c5cbcc210f6e82c7bbd52b)

#### 1.2.2、**去NameSilo域名管理页面点击DNS管理图标进去**，**删除NameSilo的默认服务器，替换为CloudFlare提供的服务器**

![img](https://secure2.wostatic.cn/static/hCA4kuuJC4sj4fyJSXJ2mi/image.png?auth_key=1688413853-iGtvwN4RNTFYGWDG9i5cm4-0-3f351e3c1a817920817cd9bf67a0ee0c)

![img](https://secure2.wostatic.cn/static/w31TQiXo1egobP92rdffMo/image.png?auth_key=1688413853-eYvokoaw1Ap9dRXtWEdVsL-0-8a37ed67bbd1b37d60f71043dbf5cd17)

#### 1.2.3、回到Cloudflare点击添加记录解析

![img](https://secure2.wostatic.cn/static/nW9dmG11hLEQwcNrC7ZZyR/image.png?auth_key=1688413853-qPCHwNcxnPNuhF18t9ALNT-0-25af1aed6559e2fcfcb883590c10850a)

#### 1.2.4、**类型选择A类型，名字作为二级域名随便起，IP地址为服务器的公网IPv4地址，关闭代理**

(可以添加多个二级域名解析，@就是默认的主域名解析)

![img](https://secure2.wostatic.cn/static/n7Jj4ofCLiACDzyrTcT5A4/image.png?auth_key=1688413853-6QDRpGEZqCargvD2apjZRh-0-00d92b123585aeb331c2c628d02716ef)

#### 1.2.5、用cmd或 [站长工具](https://ping.chinaz.com/)ping下域名检查解析是否生效

```Bash
ping vps.emjj.top -t
```

## 2、**通过ssh客户端FinalShell软件连接AWS的EC2服务器**并设置root用户+密码的权限登陆

### 2.1、打开ssh工具FinalShell客服端，首次用默认用户+端口(一般是22)+密钥文件登陆进入

![img](https://secure2.wostatic.cn/static/7pR26LrteKzhKmphJKBtL8/image.png?auth_key=1688413852-3hRJLVjKdu3XhbGoxafTpD-0-341de53a08d6c180e036c84b8677c913)

![img](https://secure2.wostatic.cn/static/8NG9pWwwjSUEmSUHLGYkxo/image.png?auth_key=1688413852-jXKefxLWKey6sQhboy7Fei-0-e075436d14679b069dd5cfd83ab028b3)

![img](https://secure2.wostatic.cn/static/ebM9hvvs2t7q26aU8MES3y/image.png?auth_key=1688413853-oZ59h6uqNoNBFGLmF6aaHi-0-d2e21166bc14d08fd3b1a65b4f0f2319)

### 2.2、修改成通过root+密码方式连接登陆

如果每次登录通过密钥文件登陆的话，不方便，如果你要部署自己的服务，就涉及到上传文件，用二合一功能的[FinalShell](https://so.csdn.net/so/search?q=FinalShell&spm=1001.2101.3001.7020)的话可能就不行了，所以，还是要改成用户名密码登录，一劳永逸。步骤如下：

### 2.3、设置root用户的密码 ,需设置2次密码（若后续root账户登不上，只需重复这一步即可）

```Bash
sudo passwd root
```

### 2.4、切换为root用户，会再次验证密码

```Bash
su root
```

### 2.5、进入ssh配置文件并修改允许密码登陆

查找PasswordAuthentication no，把no改为yes

查找PermitRootLogin，改成PermitRootLogin yes（如果前面有#号需要去掉）

```Bash
vim /etc/ssh/sshd_config
```

(可使用vim的查找功能`/查找的内容`即`/PermitRootLogin`

找到后输入`i`进入修改模式，改好之后按`Esc`返回命令模式并输入`:wq`保存并退出 )

如果出现E325: ATTENTION错误，按下图即可解决

![img](https://secure2.wostatic.cn/static/4n7wfP768xXeEMVQvYNqYd/image.png?auth_key=1688413852-3xDn5L4GViB5Q62Rgjbb8Y-0-1529db202f2eeb1d2425f73b979bedf7)

### 2.6、重启sshd服务

```Bash
sudo service sshd restart或sudo /sbin/service sshd restart
```

### 2.7、给ubuntu用户设置密码(Debian是admin)

```Bash
passwd ubuntu
```

### 2.8、现在可以重新用root用户登陆了

![img](https://secure2.wostatic.cn/static/i5WiYFWGJt7YPQqHUWbRjb/image.png?auth_key=1688413852-4jkRkm1d8iTXqxTBBtw9bx-0-5609447e999168f1451145aeacfb163c)

## 3、开启一下服务器的防火墙(如连接正常的话可跳过)

虽然已经在控制台调整了防火墙配置，但云服务器系统也会自带防火墙，经常会发生我们对服务器进行一些操作后，莫名其妙就无法连接，SSH被防火墙拒绝，无法远程操作的尴尬情况，当出现这种情况的时候，我们就会一点办法都没有，而且云服务器商也没有好的办法解决，所以为了安全起见，我们对系统自带的防火墙进行配置。

### 3.1、输入以下五条指令：

首先将输入流量的默认策略设置为接受，允许所有传入的流量进入系统

```Bash
iptables -P INPUT ACCEPT
```

将输入流量的默认策略设置为接受，允许所有传入的流量进入系统

```Bash
iptables -P FORWARD ACCEPT
```

将输出流量的默认策略设置为接受，允许系统中的所有流量离开

```Bash
iptables -P OUTPUT ACCEPT
```

清除当前所有的防火墙规则

```Bash
iptables -F
```

是用于从系统中彻底移除 nonfilter-persistent 软件包，这个软件包在 Debian/Ubuntu 系统上持久化iptables 规则。

```Bash
apt-get purge netfilter-persistent
```

![img](https://secure2.wostatic.cn/static/wc1BUw49CD2teajbHGeftq/image.png?auth_key=1688413853-xtvn4xqg1345kVxBh2cALi-0-1d29155f76b0cf0912cbab2eacd82723)

## 4、申请 SSL 证书

> 若是不明白 SSL 证书为何物，请看这里：[点击观看](https://v2rayssr.com/ssl.html) 若是不明白怎么申请 SSL 证书，请看 [本期视频](https://v2rayssr.com/go?url=https://youtu.be/6ztPETEiY8M) 若你不明白我在说什么，请直接看这里：[点击观看](https://v2rayssr.com/teach-vless.html)

### 4.1、切换到ssh工具，安装 Acme 脚本

```Bash
curl [https://get.acme.sh](https://get.acme.sh) | sh
```

### 4.2、80 端口空闲的证书申请方式

自行更换代码中的域名、邮箱为你解析的域名及邮箱

```Bash
~/.acme.sh/acme.sh --register-account -m 你的邮箱
~/.acme.sh/acme.sh --issue -d 你的域名 --standalone
```

### 4.3、安装证书到指定文件夹

自行更换代码中的域名为你解析的域名

```Bash
~/.acme.sh/acme.sh --installcert -d 你的域名 --key-file /root/private.key --fullchain-file /root/cert.crt
```

**公钥路径：/root/cert.crt**

**私钥路径：/root/private.key**

## 5、安装 X-ui 面板

### 5.1、更新及安装组件

根据自己的系统选择命令安装就好了。

Debian/Ubuntu 系统命令

```Bash
apt update -y
apt install -y curl socat wget
```

CentOS 系统命令

```Bash
yum update -y 
yum install -y curl socat wget
```

### 5.2、安装X-ui

> **有以下的两个安装命令，二选一，自由选择，都可以正常的使用**： 第一个为官方原版，但是2021年8月就没有更新了。 第二个为改版X-ui，更新速度值得点赞，加入了很多新功能，包括最新的 Reality 协议、电报群通知等等功能。

- **官方原版一键\**\*\*安装及升级\*\**\*代码**

```Bash
bash <(curl -Ls [https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh](https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh))
```

- **改版X-ui一键安装代码（项目地址：**[**点击访问**](https://github.com/FranzKafkaYu/x-ui)**）**

```Bash
bash <(curl -Ls [https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install.sh](https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install.sh))
```

然后输入y回车，设置自己的用户名 密码 端口

![img](https://secure2.wostatic.cn/static/w9R5zjKG1pS8qUgesUfSFT/image.png?auth_key=1688413853-2U6yENkKm1qBHpsUXM8T7V-0-4e52647db0cbac78337a209df3feb884)

X-ui安装完成

![img](https://secure2.wostatic.cn/static/fE15HPF8PuzS6XERkCjuX5/image.png?auth_key=1688413853-7JvXoRw7HaJU1e6J9vaKQY-0-ba2cdcc539945285192d3deb3d81655f)

## 6、安装并开启BBRplus加速

### 6.1、一键安装BBR/暴力BBR/魔改BBR/BBRplus/锐速 (Lotserver)四合一的脚本如下：

```Bash
wget -N --no-check-certificate "[https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh](https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh)" && chmod +x [tcp.sh](http://tcp.sh) && ./tcp.sh
```

### 6.2、输入2回车安装

![img](https://secure2.wostatic.cn/static/qhXoFPxXYNzGY6sKbaVSSz/image.png?auth_key=1688413852-2H87stGyfFQpUSvbtZQ37Z-0-16c706d738b94227279ca87e68312bd9)

### 6.3、如出现下图，选择No回车

![img](https://secure2.wostatic.cn/static/ovNsb3G1dFC1JUcdXyLQQ5/image.png?auth_key=1688413852-uEMiGKnHXkLyQCmULqC6Le-0-d2f74bf2eb24dc4d93b01f7dd2974f3d)

### 6.4、安装完成输入Y重启后，再运行

```Bash
./tcp.sh
```

### 6.5、输入7启动BBRplus加速

![img](https://secure2.wostatic.cn/static/mgTjaX2CRZXh5bqBMrdVHH/image.png?auth_key=1688413853-92xaNR9LBDmGETPPT4GDi7-0-127577c53e6730f35a598c3312b998d3)

### 6.6、再次运行./tcp.sh检查是否成功

![img](https://secure2.wostatic.cn/static/w3GA2Xc3kaeXG7dCuqQ5wJ/image.png?auth_key=1688413853-8gydEzGJfiDjBzvcYomc4a-0-4392722161af47a8a45617ec65631a11)

**至此加速成功**

## 7、登录X-ui面板配置

安装完毕后用`域名:端口`访问x-ui后台，检查新版本并切换，并配置证书的公钥和密钥的路径，然后重启面板，用`https://域名:端口`就可访问x-ui后台了，节点配置及功能方面，可看看这 [视频教程](https://v2rayssr.com/go?url=https://youtu.be/6ztPETEiY8M)

![img](https://secure2.wostatic.cn/static/5ZCK6arUwcbEYXFYEvw2Qj/image.png?auth_key=1688413853-q4eqJwnxDkmukE1LbW5jdt-0-59394828c10e2b8a8fa3bd91397624da)

![img](https://secure2.wostatic.cn/static/7DeCYZKQJtYLzdD2o3Gix2/image.png?auth_key=1688413853-rZpDxYiSi14wyN5B6MdBxj-0-ea64f5dae2a483c9dc957cd0237d8357)

或者用下面这两个也可以

![img](https://secure2.wostatic.cn/static/smi3qnD9sr3emTH4XRcWCJ/image.png?auth_key=1688413933-p7vZruvd9Lxgf4wvM7Jy2b-0-cdeea664589672c2fe371ca5a6c15035)

### 7.1、添加节点

vless

![img](https://secure2.wostatic.cn/static/6GqREFXRFMGLzBGM1EDCi8/image.png?auth_key=1688413933-wFtwdLHo3bEKTrctBotTHu-0-af454419753c66fc0494c9d17cde5455)

有的版本是这种

![img](https://secure2.wostatic.cn/static/tb2TVoUNtMPXbTLvd43fw3/image.png?auth_key=1688413935-7Yn3wD4UVQRZkZd7kNKoEL-0-9994bf733919a3131df3abaef84d461f)

![img](https://secure2.wostatic.cn/static/dx4V1AYwEXZwAKWyaNYecB/image.png?auth_key=1688413935-9djRSjFiqYipqMcKynipmv-0-e65729022effe92a0a4cee3e725c22de)

### 7.2、复制节点到代理工具使用

![img](https://secure2.wostatic.cn/static/hpyDQy4aufUpsEFknt7bPy/image.png?auth_key=1688413935-mCUc7pLhoEm7ndFoSGPmyt-0-e76f35085bb42f6c7cc0c84a025d8cf9)

## 8、**客户端下载**

[V2Ray官网对于全平台客户端的总结和一览](https://www.v2ray.com/awesome/tools.html)

Windows：[v2rayN下载](https://github.com/2dust/v2rayN/releases)

IOS：App Store登录非国区账号，安装Shadowrocket小火箭

安卓：[V2RayNG](https://github.com/2dust/v2rayNG/releases)

苹果MAC OS：[V2RayU下载](https://github.com/yanue/V2rayU/releases)

------

**至此教程结束**

------

备注

其他另一套搭建trojan节点命令，装CentOS_7.8x64版本的系统并解析好域名后，依次运行：

```PowerShell
yum update -y  #CentOS
yum install -y curl  #CentOS
source <(curl -sL [https://git.io/trojan-install](https://git.io/trojan-install))
```

出现选择时一直选1就行了，到下面这一步直接回车

![img](https://secure2.wostatic.cn/static/gKYUdg1SuSb5Qk3Aut9tkM/image.png?auth_key=1688413935-nCa3iX48kxDgVwV57nHCUk-0-7e20fb30149e57346f1ff3b050e8a36f)

BBRplus加速：

```PowerShell
wget -N --no-check-certificate "[https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh](https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh)" && chmod +x [tcp.sh](http://tcp.sh) && ./tcp.sh
```

进去后台设置密码（记好）并登录，用户名是admin

![img](https://secure2.wostatic.cn/static/cnSYdVtTC1BWCS1LC7V8DQ/image.png?auth_key=1688413935-x8JHWhbJ7gZpftQ4CP9fWW-0-63cc711d9ea0f03a9ba4552902a2d2d3)

**搭建结束**

可参考视频：https://www.youtube.com/watch?v=YZyJKt0zAYU



