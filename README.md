## 1、准备工作（服务器开放端口&域名解析）

  ### 1.1、购买好服务器 ，或者免费的

  - 服务器版本：CentOS 7+、Ubuntu 16+、Debian 8+
  - [亚马逊WAS](https://aws.amazon.com/)可白嫖。注册后绑定1张银联卡(开启服务器后,可以更换1张不用的信用卡,避免乱扣费)，选择EC2，跟着免费的指引走(创建密钥下载并保存、及安全组出入站规则选择全部流量和端口)。**开通**aws的服务器**后**设置安全组（其他平台服务器同理）

      ![](https://secure2.wostatic.cn/static/jQaffT1x1xSZx6VknVE4oV/image.png?auth_key=1688902140-idZocJpuTQ6hXPVNPTkoq7-0-37a45daf65a49f37e015f3cb9ce658bd)
  - 亚马逊的如果ip被墙，可以通过操作【停止实例-启动现有实例】的方法即可更换ip恢复，系统不会被重置，只需重新把新的ip解析过来就好
  - **记录IPv4公网ip和默认的用户名以及密码，如果你服务器支持默认root+密码登陆可直接跳至第4步开始，具体以你服务器给出的为主**
      - aws默认只能用创建的密钥文件登陆，后面步骤会开启root账号和密码登陆
      - Ubuntu是ubuntu
      - Debian是admin
      - CentOS是centos

  ### 1.2、购买好域名，可去[NameSilo](https://www.namesilo.com/login?redirect=/account_domains.php)购买，有免费的隐私保护，买好后去[Cloudflare](https://dash.cloudflare.com/)，将域名托管到Cloudflare，并在Cloudflare做域名解析

    #### 1.2.1、进Cloudflare添加域名

      ![](https://secure2.wostatic.cn/static/4eMgzMbduvCTH2fbSH3yzN/image.png?auth_key=1688902140-eRgigM9NPAhcdqyAEXfZGX-0-2d9e6eff3fe37c604fc4f958f2132d31)

    #### 1.2.2、**复制Cloudflare的提供的服务器**

      ![](https://secure2.wostatic.cn/static/fN7XEGJuxEEd8QkamqS1AF/image.png?auth_key=1688902140-r3Jg33E2Zg8H9BZpE7NbbK-0-0191dd20dc894de769c69d090e33070d)

    #### 1.2.3、**去NameSilo域名管理页面点击DNS管理图标进去**，**删除NameSilo的默认服务器，替换为CloudFlare提供的服务器**

      ![](https://secure2.wostatic.cn/static/hCA4kuuJC4sj4fyJSXJ2mi/image.png?auth_key=1688902139-wVPqroqZgNHuH9bBcX1FxB-0-f1a1ad991e4a1d0929538d7cf2c37362)

      ![](https://secure2.wostatic.cn/static/8jyEvR6NkgPqWjpwWboASb/image.png?auth_key=1688902139-pRuQzdWNPwUt8mGACTLaBA-0-81aefed0020b54c84492c1a87acbbbaa)

    

    #### 1.2.4、回到Cloudflare查看是否添加成功

      ![](https://secure2.wostatic.cn/static/eborJexYUxFXZxajxGiEn7/image.png?auth_key=1688902139-neYsqHR2TBBgXYSyESUjMH-0-172c22ba5de55a0c4ed247e146b992e4)

    

    #### 1.2.5、添加成功后点击添加记录解析

      ![](https://secure2.wostatic.cn/static/nW9dmG11hLEQwcNrC7ZZyR/image.png?auth_key=1688902140-bamGRXj82Nqky6VveaenfT-0-53ad490b61a3fcfaf1fb4484308af870)

    

    #### 1.2.6、**类型选择A类型，名字作为二级域名随便起，IP地址为服务器的公网IPv4地址，关闭代理**

    (可以添加多个二级域名解析，@就是默认的主域名解析)

      ![](https://secure2.wostatic.cn/static/n7Jj4ofCLiACDzyrTcT5A4/image.png?auth_key=1688902140-sFzzn6sUp8Ryaf9wRqgsdg-0-5e01fe486c5ffea098790ff7aec726c2)

    

    #### 1.2.7、用cmd或 [站长工具](https://ping.chinaz.com/)ping下域名检查解析是否生效

    ping 解析的域名 -t

```Bash
ping vps.emjj.top -t
```

## 2、设置root用户+密码的权限登陆。这一步是aws服务器的登陆设置，如果你其他平台的服务器能正常直接root+密码登陆可以跳过，直接从第4步开始，或者也可以直接通过`sudo -i`使用root的权限

  ### 2.1、修改成通过root+密码方式连接登陆，如果每次登录通过密钥文件登陆的话，不方便，如果你要部署自己的服务，就涉及到上传文件，用二合一功能的[FinalShell](https://so.csdn.net/so/search?q=FinalShell&spm=1001.2101.3001.7020)的话可能就不行了，所以，还是要改成用户名密码登录，一劳永逸。

  - 打开ssh工具FinalShell客服端，首次用默认用户+端口(一般是22)+密钥文件登陆进入

      ![](https://secure2.wostatic.cn/static/7pR26LrteKzhKmphJKBtL8/image.png?auth_key=1688902139-nUBCDsZazeu3F6aShzHf9u-0-98ff3cefc22e84e72cd960c82af0b108)

      ![](https://secure2.wostatic.cn/static/8NG9pWwwjSUEmSUHLGYkxo/image.png?auth_key=1688902140-c2QkTG2rYJceLsDf4DL5tY-0-8e193c2fe69e3591d4a081cec2e26777)

      ![](https://secure2.wostatic.cn/static/ebM9hvvs2t7q26aU8MES3y/image.png?auth_key=1688902139-d2otUuZUQogxTGUbmafLJN-0-22bcb1c0d8a9e8b0b62981ff150ee055)

  ### 2.2、设置root用户的密码 ,需设置2次密码（若后续root账户登不上，只需重复这1步即可）

```Bash
sudo passwd root
```

  ### 2.3、切换为root用户，会再次验证密码

```Bash
su - root
或
sudo -i
```

  ### 2.4、进入ssh配置文件并修改允许密码登陆

  查找PasswordAuthentication no，把no改为yes

  查找PermitRootLogin，改成PermitRootLogin yes（如果前面有#号需要去掉）

```Bash
vim /etc/ssh/sshd_config
```

  (可使用vim的查找功能`/查找的内容`即`/PermitRootLogin`

  找到后输入`i`进入修改模式，改好之后按`Esc`返回命令模式并输入`:wq`保存并退出 )

  如果出现E325: ATTENTION错误，按下图即可解决

    ![](https://secure2.wostatic.cn/static/4n7wfP768xXeEMVQvYNqYd/image.png?auth_key=1688902139-3t64CndYrQs5sZCSBUeMtZ-0-e0f76c2cbd329a7025364fc0a18ad686)

  ### 2.5、重启sshd服务

```Bash
service ssh restart或
sudo service sshd restart或
sudo /sbin/service sshd restart
```

  sudo service sshd restar

  ### 2.6、给ubuntu用户设置密码(Debian是admin、CentOS是centos)

```Bash
passwd ubuntu
```

  - 现在就可以用root+密码重新登陆了

      ![](https://secure2.wostatic.cn/static/i5WiYFWGJt7YPQqHUWbRjb/image.png?auth_key=1688902139-sjgGS9es2cBdR1cVBPNjth-0-72fd1213faf36f9bd67c22e705f8728e)

## 3、开启一下服务器的防火墙（如连接正常的话可跳过）

> 虽然已经在控制台调整了防火墙配置，但云服务器系统也会自带防火墙，经常会发生我们对服务器进行一些操作后，莫名其妙就无法连接，SSH被防火墙拒绝，无法远程操作的尴尬情况，当出现这种情况的时候，我们就会一点办法都没有，而且云服务器商也没有好的办法解决，所以为了安全起见，我们对系统自带的防火墙进行配置。

  #### 3.1、输入以下五条指令：

  首先将输入流量的默认策略设置为接受，允许所有传入的流量进入系统

```Bash
iptables -P INPUT ACCEPT
```

```Bash
iptables -P FORWARD ACCEPT
```

```Bash
iptables -P OUTPUT ACCEPT
```

```Bash
iptables -F
```

```Bash
apt-get purge netfilter-persistent

```

  ![](https://secure2.wostatic.cn/static/wc1BUw49CD2teajbHGeftq/image.png?auth_key=1688902140-dxaZHsLaDVvM6aXz5X3MU9-0-ddc8b02a19a3ad0f600b86f72998cacc)

  是用于从系统中彻底移除 nonfilter-persistent 软件包，这个软件包在 Debian/Ubuntu 系统上持久化iptables 规则。

  清除当前所有的防火墙规则

  将输出流量的默认策略设置为接受，允许系统中的所有流量离开

  将输入流量的默认策略设置为接受，允许所有传入的流量进入系统

## 4、申请 SSL 证书
  - 若是不明白 SSL 证书为何物，请看这里：[点击观看](https://v2rayssr.com/ssl.html)
  - 若是不明白怎么申请 SSL 证书，请看 [本期视频](https://v2rayssr.com/go?url=https://youtu.be/6ztPETEiY8M)
  - 若你不明白我在说什么，请直接看这里：[点击观看](https://v2rayssr.com/teach-vless.html)

  ### 4.1、用root用户

    ![](https://secure2.wostatic.cn/static/i5WiYFWGJt7YPQqHUWbRjb/image.png?auth_key=1688902139-uarU53wGYviw9pBgNrBKG8-0-9b026f49534a286ff55f864816cd9304)

### 4.2、安装 Acme 脚本

```Bash
curl [https://get.acme.sh](https://get.acme.sh) | sh
```

### 4.3、80 端口空闲的证书申请方式

自行更换代码中的域名、邮箱为你解析的域名及邮箱

```Bash
~/.acme.sh/acme.sh --register-account -m 你的邮箱
~/.acme.sh/acme.sh --issue -d 你的域名 --standalone

```

### 4.4、安装证书到指定文件夹

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

  > **有以下的两个安装命令，二选一，自由选择，都可以正常的使用**：
第一个为官方原版，但是2021年8月就没有更新了。
第二个为改版X-ui，更新速度值得点赞，加入了很多新功能，包括最新的 Reality 协议、电报群通知等等功能。

  - **官方原版一键****安装及升级****代码**

```Bash
bash <(curl -Ls [https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh](https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh))
```
  - **改版X-ui一键安装代码（项目地址：**[**点击访问**](https://github.com/FranzKafkaYu/x-ui)**）**

```Bash
bash <(curl -Ls [https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install.sh](https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install.sh))
```

  然后输入y回车，设置自己的用户名 密码 端口

    ![](https://secure2.wostatic.cn/static/w9R5zjKG1pS8qUgesUfSFT/image.png?auth_key=1688902140-nGD7Hag5agJxfLJ4PVCMNV-0-c0b2a76cd1a6450eb8076cac5ce333df)

  X-ui安装完成

    ![](https://secure2.wostatic.cn/static/fE15HPF8PuzS6XERkCjuX5/image.png?auth_key=1688902140-nG8Y8G6iM4CX9ziJWpCuFb-0-869d3e015b2b4814d6ec1ba6a667ac3e)

### 5.3、放行端口（服务器开放了所有端口的可跳过）

```Bash
iptables -I INPUT -p tcp --dport 443 -j ACCEPT
iptables -I INPUT -p tcp --dport 5000 -j ACCEPT
```

## 6、安装并开启BBRplus加速

  ### 6.1、一键安装BBR/暴力BBR/魔改BBR/BBRplus/锐速 (Lotserver)四合一的脚本如下：

```Bash
wget -N --no-check-certificate "[https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh](https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh)" && chmod +x [tcp.sh](http://tcp.sh) && ./tcp.sh
```

  ### 6.2、输入2回车安装

    ![](https://secure2.wostatic.cn/static/qhXoFPxXYNzGY6sKbaVSSz/image.png?auth_key=1688902140-mNCSxVpKSLPRBFVp6rVSG6-0-30c43287f31bb7698678f78757b266e5)

  ### 6.3、如出现下图，选择No回车

    ![](https://secure2.wostatic.cn/static/ovNsb3G1dFC1JUcdXyLQQ5/image.png?auth_key=1688902140-8uMScqf1VNNW4jFw1FM5xn-0-38d28e86d6fe3e9f349c7c17190e3476)

  ### 6.4、安装完成输入Y重启后，再运行

```Bash
./tcp.sh
```

  ### 6.5、输入7启动BBRplus加速

    ![](https://secure2.wostatic.cn/static/mgTjaX2CRZXh5bqBMrdVHH/image.png?auth_key=1688902140-afTRN1X546V7j8pF4TkikV-0-21d7a42456f15a56da0df60d921c2efa)

  ### 6.6、再次运行./tcp.sh检查是否成功

    ![](https://secure2.wostatic.cn/static/w3GA2Xc3kaeXG7dCuqQ5wJ/image.png?auth_key=1688902140-nRPYK7mfnkkAu8soygmFzX-0-24a4e4e741b9049a1b76678044718af5)

  **至此加速成功**



## 7、登录X-ui面板配置&添加节点

  ### 7.1、面板配置

  安装完毕后用`域名:端口`访问x-ui后台，检查新版本并切换，配置证书的公钥和密钥的路径，然后重启面板，用`https://域名:端口`就可访问x-ui后台了，节点配置及功能方面，可看看这 [视频教程](https://v2rayssr.com/go?url=https://youtu.be/6ztPETEiY8M)

    ![](https://secure2.wostatic.cn/static/5ZCK6arUwcbEYXFYEvw2Qj/image.png)

    ![](https://secure2.wostatic.cn/static/7DeCYZKQJtYLzdD2o3Gix2/image.png)

  或者用下面这两个也可以

    ![](https://secure2.wostatic.cn/static/smi3qnD9sr3emTH4XRcWCJ/image.png)

  ### 7.1、添加节点

  - vless

      ![](https://secure2.wostatic.cn/static/6GqREFXRFMGLzBGM1EDCi8/image.png)

      

      有的版本是这种

        ![](https://secure2.wostatic.cn/static/tb2TVoUNtMPXbTLvd43fw3/image.png)

        ![](https://secure2.wostatic.cn/static/dx4V1AYwEXZwAKWyaNYecB/image.png)

  ### 7.2、复制节点到代理工具使用

    ![](https://secure2.wostatic.cn/static/hpyDQy4aufUpsEFknt7bPy/image.png)

---

**至此教程结束**

---



## 8、结尾其他

  ### 8.1、**客户端下载**

  - [V2Ray官网对于全平台客户端的总结和一览](https://www.v2ray.com/awesome/tools.html)  
  - Windows：[v2rayN下载](https://github.com/2dust/v2rayN/releases)
  - IOS：App Store登录非国区账号，安装Shadowrocket小火箭
  - 安卓：[V2RayNG](https://github.com/2dust/v2rayNG/releases)
  - 苹果MAC OS：[V2RayU下载](https://github.com/yanue/V2rayU/releases)

  

  ### 8.2、备注

  其他另一套搭建trojan节点命令，装CentOS选择7.8x64版本更稳定，依次运行：

  - Debian/Ubutu

```PowerShell
apt update -y  #Debian
apt install -y curl  #Debian
source <(curl -sL [https://git.io/trojan-install](https://git.io/trojan-install))
```
  - CentOS

```PowerShell
yum update -y  #CentOS
yum install -y curl  #CentOS
source <(curl -sL [https://git.io/trojan-install](https://git.io/trojan-install))
```

  出现选择时一直选1就行了，到下面这一步直接回车

    ![](https://secure2.wostatic.cn/static/gKYUdg1SuSb5Qk3Aut9tkM/image.png)

  直接回车

    ![](https://secure2.wostatic.cn/static/dCoKFecHj9JASsSL4KzDxf/image.png)

  

  再直接回车

    ![](https://secure2.wostatic.cn/static/8zqJw8hi1Kr1TyU4jFFjCH/image.png)

  BBRplus加速：

```PowerShell
wget -N --no-check-certificate "[https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh](https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh)" && chmod +x [tcp.sh](http://tcp.sh) && ./tcp.sh
```

  进去后台设置密码（记好）并登录，用户名是admin

    ![](https://secure2.wostatic.cn/static/cnSYdVtTC1BWCS1LC7V8DQ/image.png)

  **搭建结束**

  可参考视频：[https://www.youtube.com/watch?v=YZyJKt0zAYU](https://www.youtube.com/watch?v=YZyJKt0zAYU)

  

