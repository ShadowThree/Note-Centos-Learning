[toc]

### 基本信息

|  笔记本型号  |     ASUS X550VC     |
| :----------: | :-----------------: |
| **系统版本** | **Centos 7.9.2009** |



### 安装Centos

直接按照`Centos`官网的按照指导安装。

不过，我的电脑安装时无线选项一直打不开，我觉得可能是驱动的问题。

通过 `lspci | grep Ethernet` 和 `lspci | grep Network` 两条命令可分别查看电脑的网卡以及无线网卡的型号。弄了半天也没成功安装到可用的驱动，但是取意外发现了一篇文章：[笔记本最小化安装 CentOS 启动之后无法启动无线网卡的解决方法 - SegmentFault 思否](https://segmentfault.com/a/1190000021652252) 

文章中通过以下命令配置了一些神奇了选项，我也不懂啥意思。。。

```
# 以下命令执行任意一条就可以，本人用的是第一条，亲测有用，但是文章中使用的是第二条
echo "options asus_nb_wmi wapf=4" | sudo tee /etc/modprobe.d/asus_nb_wmi.conf
echo "options asus_nb_wmi wapf=4" > /etc/modprobe.d/asus.conf
```

然后通过 `reboot` 命令重启计算机，再通过 `rfkill list` 命令查看无线网卡的状态。我的状态以及操作如下：

（`Soft blocked: yes` 表示设备是从软件层关闭的，所以直接通过命令 `rfkill unblock all` 将软件层打开，然后尽然就可以了。。。可知道为了这个问题我重装了系统，还鏖战到天明啊！ 不过文章中是因为硬件原因关闭的网卡，所以还有进行其他的操作才行，具体请参考上文的链接）

![](/home/shadow3d/Desktop/Centos7_Learning/KeepIt/enable_ASUS_network.png)


### install Typora
1. download from typora home page

2. unzip by command `tar -xf Typora-linux-x64.tar.gz`, and move the directory to `/sur/bin`

3. entry the path where have Typpra executable file and run it

4. found some error when run Typora: `error while loading shared libraries: libXss.so.1`

5. check if is the lib really miss: `ldd Typora | grep "not found"

6. if yes, install the miss lib: `sudo yum install libXScrnSaver`

7. some error about chrome-sandbox:
	`sudo chown root chrome-sandbox`
	`sudo chmod 4755 chrome-sandbox`
	
8. some error about libstdc++.so.6:

  refer: https://www.jianshu.com/p/3f2ef579c1d3

  First, download `libstdc++.so.6.0.26` from internet. 

  Then,  check the path of libstdc++ by `sudo find / -name libstdc++.so.6*`

  ![libstdc++_path](/home/shadow3d/Pictures/KeepIt/libstdc++_path.png)

  Then, copy `libstdc++.so.6.0.26` to `/usr/lib/` and `/usr/lib64/`, and add executable property by `chmod +x libstdc++.so.6.0.26` in both related PATH.

  Last, use Typora at any path by `sudo ln -s /usr/bin/Typora-linux-x64/Typora /usr/bin/Typora`

  follwing is some commands maybe need use

  `strings /usr/lib64/libstdc++.so.6 | grep CXXABI`	# check CXXABI version that we already have
  `sudo yum install -y gmp-devel mpfr-devel libmpc-devel`


### change icon size
	https://www.yanning.wang/archives/774.html
### chainese input
	https://blog.csdn.net/weixin_30699741/article/details/94765012		# 

### time sync with network time protocol

refer: http://woshub.com/centos-set-date-time-timezone-ntp/

```
yum install ntp -y
systemctl start ntpd.service
systemctl enable ntpd.service
service ntpd status
```

### change whether from F to C

```
# terminal command
gsettings set org.gnome.GWeather temperature-unit "'centigrade'"
```



### change bash version

	scl enable devtoolset-9 bash		# change gcc version to 9, 'exit' to quit
	sudo ln -s /usr/lib64/libstdc++.so.6.0.26 libstdc++.so.6	# create soft link
	sudo find / -name libstdc++.so.6*		# find
	# check CXXABI version that we already have
	strings /usr/lib64/libstdc++.so.6 | grep CXXABI	 
	# yum install local rpm parkeg
	sudo yum localinstall libstdc++-4.8.5-44.el7.i686.rpm

### set mail
```
# add below setting in /etc/mail.rc
set from=mailAddr@qq.com
set smtp=smtp.qq.com
set smtp-auth-user=[same with you mail addr]
set smtp-auth-password=[get from the mail setting]
set smtp-auth=login
```
<<<<<<< HEAD
then send mail by following command:
```
mail -s 'subject' mailAddr@qq.com < fileName
```
=======
>>>>>>> 3b38a7ed3c84dbc43f824aa1f4fb3efab5cffd66















