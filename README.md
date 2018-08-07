# SS-kcptun-xshell
1.**购买一个VPS**。一个月几美元。
拿到VPS对应的IP地址和端口号，注意可以ping下这个端口号是不是可以访问，不能在国内访问就得换个ip或者端口。

2.**下载xshell**。版本高一点就行比如6.
打开xshell然后新建会话然后输入VPS的ip和端口号，然后还有登陆的账号和密码，如root和xyewrlvobtnldm这种VPS官方发你的账号和密码。

3.**VPS远程配置SSR**。

`wget --no-check-certificate	 https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh
chmod +x shadowsocksR.sh
./shadowsocksR.sh 2>&1 | tee shadowsocksR.log`
如果没问题就可以输入ss密码了。
然后输入端口，输入一个大于1024的任意端口。
然后输入加密，随便选，第一个aes就行。
后面都选择第一个。
一直到出现Enjoy it！

注意以上配置的东西随便填，忘了也没事，后面随时可以查询。

4.**本机（windows平台）下载SSR**，注意有些版本过老可能会出现问题，我用的4.6版本的SSR。
	打开exe，然后右键桌面右下角出现的小飞机，导入我们第三步拿到的SSRkey，自动配置。
	
此时不出意外就能顺利访问外网了。

5.**VPS远程配置kcptun。kcptun是用来加速我们访问外网速度用的。不是必须装**，但是安装后能普遍加速30%以上。
顺序在远程命令行中敲入下面3行代码等安装完成。
```
    wget https://raw.githubusercontent.com/kuoruan/kcptun_installer/master/kcptun.sh
    chmod +x ./kcptun.sh
    ./kcptun.sh
```
	然后经过若干步骤，配置好kcptun,注意kcptun版本号很重要，我们要是在本地安装kcptun客户端，需要版本号能对应上。
	当前安装的 Kcptun 版本为: 20180316

可使用的客户端配置文件为:
{
  "localaddr": ":2333",
  "remoteaddr": "195.120.118.8:29900",
  "key": "kcpttttt",
  "crypt": "aes",
  "mode": "fast",
  "mtu": 1350,
  "sndwnd": 512,
  "rcvwnd": 512,
  "datashard": 10,
  "parityshard": 3,
  "dscp": 0,
  "nocomp": false,
  "quiet": false
}

我们会见到上述文本类似的东西， 复制括号内的所有东西包括{}括号下来然后保存在我们本地，后缀为json。

本地下载好kcptun-windows-386-20180316.tar.gz解压。然后下载kcptun_gclientv.1.1.3.zip解压。
kcptun_gclientv.1.1.3.zip里面打开kcptun界面，导入kcptun_client.exe，然后使用导入配置，也就是我们上面保存的后缀为json的文件。

如果json文件报错，那么就手动输入吧，缺少的选项就缺失没事。
这时候点启动，然后加速就生效了。
然后还得修改下桌面右下角SSR小飞机的“服务器IP”，改成127.0.0.1就行了，其他东西不用动。

需要用到的Kcptun、Kcptun窗口界面、SSR的下载地址都在下面的链接里。
https://github.com/deadwin/SS-kcptun-xshell/releases

=========================================
如果我们需要和他人共用：
VPS支持多个用户共享。给ssr编辑多端口：
root登陆之后输入：
	
vi /etc/shadowsocks.json

如果找不到上面路径，那么慢慢找吧或者网上搜下。
但是文件位置大同小异。

按i编辑，esc退出编辑，然后按Shift+Q编辑状态输入:wq保存

示例如下,请对照自己配置进行修改：
{
"server": "0.0.0.0",
"server_ipv6": "::",
"local_address": "127.0.0.1",
"local_port":1080,
"port_password":{    //添加这个东西，和里面内容。前面是端口，后面是密码。
"2000":"password1",  
"20001":"password2",
"13228":"password3",
"8085":"password4",
"4430":"password1"
},				//到这截止，注意逗号
"timeout": 300,
"udp_timeout": 60,
"method": "chacha20",
"protocol": "auth_sha1",
"protocol_param": "",
"obfs": "http_simple",
"obfs_param": "",
"dns_ipv6": false,
"connect_verbose_info": 0,
"redirect": "",
"fast_open": false,
"workers": 1
}

简单说也就是把你设置的一个端口改成包含多个端口和密码罢了。这时候我们的VPS的防火墙可能还会限制，所以再去配置下防火墙。

以下是抄来的（https://www.cnblogs.com/gne-hwz/p/6662000.html）：
多用户加进去了，但还是只会有一个用户可以使用，因为防火墙规则还是会阻止端口连接。

接下来就是修改防火墙规则了

这里是使用firewalld，不使用iptable

状态：systemctl status firewalld.service

停止防火墙：systemctl stop firewalld

开启防火墙：systemctl start firewalld

停止firefalld

systemctl stop firewalld

命令行下

编辑public.xml文件

vi /etc/firewalld/zones/public.xml

上面添加你想要的端口就行了，一样：wq!

到这里就可以实现多端口访问了。

 最后别忘记启动防火墙和ssr

systemctl start firewalld
/etc/init.d/shadowsocks start



OK搞定。


