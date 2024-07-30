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

./imxdownload *.bin /dev/sdb #烧录代码

sudo rm -rf /dev/sdb #删除sdb节点
```



