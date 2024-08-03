# VIM

安装 VIM

```shell
sudo apt-get install vim
```

配置 VIM

```shell
sudo vim /etc/vim/vimrc
```

末尾添加下面的代码
```shell
set ts=4
set nu
set noexpandtab
```





# FTP 服务(文件互传)

安装 FTP 服务

```shell
sudo apt-get install vsftpd
```

配置 FTP 服务

```shell
sudo vim /etc/vsftpd.conf
```

删除 write_enable=YES 前面的的 '#'

重启FTP 服务

```shell
sudo /etc/init.d/vsftpd restart  
```





# SSH 服务开启  

```shell
sudo apt-get install openssh-server
```





# 交叉编译器安装

将 [gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf.tar.xz](主文件夹\linux\tool\gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf.tar.xz) 传给Ubuntu

Ubuntu 中创建目录

```shell
sudo mkdir /usr/local/arm
```



将交叉编译器复制到/usr/local/arm 中

```shell
sudo cp gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf.tar.xz /usr/local/arm/ -f  
```

```shell
cd /usr/local/arm
```



解压交叉编译器

```shell
sudo tar -vxf gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf.tar.xz
```



修改环境变量，使用 VIM 打开/etc/profile 文件

```shell
sudo vim /etc/profile
```



在末尾输入如下内容

```shell
export PATH=$PATH:/usr/local/arm/gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf/bin
```



保存退出，重启 Ubuntu 系统



在使用交叉编译器之前需要安装一下其它的库???

```shell
sudo apt-get install lsb-core lib32stdc++6
```



交叉编译器验证

```shell
arm-linux-gnueabihf-gcc -v
```



# 应用无法升级更新

```shell
sudo snap refresh snap-store
```



# 安装第三方软件包管理器

 ```shell
 sudo apt install gdebi
 ```



# clash依赖关系不满足: libwebkit2gtk-4.0-37

 安装 [libjavascriptcoregtk-4.0-18_2.43.3-1_amd64.deb](其他重要文件\libjavascriptcoregtk-4.0-18_2.43.3-1_amd64.deb)  [libwebkit2gtk-4.0-37_2.43.3-1_amd64.deb](其他重要文件\libwebkit2gtk-4.0-37_2.43.3-1_amd64.deb) 即可

```shell
sudo apt install ./libwebkit2gtk-4.0-37_2.43.3-1_amd64.deb ./libjavascriptcoregtk-4.0-18_2.43.3-1_amd64.deb
```



# 查询系统存储设备和烧录程序

```shell
ls /dev/sd* #显示存储设备

chmod 777 imxdownload
./imxdownload *.bin /dev/sdb #烧录代码

sudo rm -rf /dev/sdb #删除sdb节点
```



# load.imx 在 DDR 中的位置

![load.imx在DDR中的位置](picture/load.imx在DDR中的位置.jpg)

# U-Boot

## 编译 U-Boot

创建脚本文件 mx6ull_alientek_emmc.sh

在里面输入如下内容: 

```shell
#!/bin/bash
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- distclean
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- mx6ull_14x14_ddr512_emmc_defconfig
make V=1 ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j6
```

给予 mx6ull_alientek_emmc.sh 文件可执行权限，然后就可以使用这个 shell 脚本文件来编译uboot

```shell
chmod 777 mx6ull_alientek_emmc.sh
./mx6ull_alientek_emmc.sh
```

## U-Boot 烧写

使用 imxdownload 软件烧写，命令如 下: 

```shell
chmod 777 imxdownload
./imxdownload u-boot.bin /dev/sdb #烧录代码
```

# 搭建TFTP

安装 tftp-hpa 和tftpd-hpa，命令如下：

```shell
sudo apt-get install tftp-hpa tftpd-hpa
sudo apt-get install xinetd
```

创建一个文件夹

```shell
mkdir /home/zuozhongkai/linux/tftpboot
chmod 777 /home/zuozhongkai/linux/tftpboot
```

配置tftp

```shell
sudo vim /etc/xinetd.d/tftp
```

输入如下内容

```c
server tftp
{
	socket_type = dgram
	protocol = udp
	wait = yes
	user = root
	server = /usr/sbin/in.tftpd
	server_args = -s /home/zuozhongkai/linux/tftpboot/
	disable = no
	per_source = 11
	cps = 100 2
	flags = IPv4
}
```

 启动 tftp 服务，命令如下： 

```shell
sudo service tftpd-hpa start
```

打开/etc/default/tftpd-hpa 文件

```shell
sudo vim /etc/default/tftpd-hpa
```

替换为以下内容

```shell
# /etc/default/tftpd-hpa

TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/home/zuozhongkai/linux/tftpboot"
TFTP_ADDRESS=":69" 
TFTP_OPTIONS="-l -c -s"
```

重启 tftp 服务器

```shell
sudo service tftpd-hpa restart
```

指令格式

```shell
tftp 80800000 192.168.x.x:zImage
```









