## iMX6 Android 源码编译过程总结
* 说明

>使用的是迅为官网的板子, iTOP-iMX6 开发板；采用迅为提供的开发环境： Ubuntu12.04.2、镜像采用最新的 `iTOP-iMX6_android4.4.2_20161018.tar.gz` 链接 ： `http://pan.baidu.com/share/init?shareid=3641650722&uk=3091391143` 密码: `gicu`

首先需要将虚拟机的配置, 在迅为的基础上把内存改为 2G 或者以上，迅为给的时候是 1G，如果不修改在组后编译的时候会有东西被莫名的 kill 掉。

## 编译过程出错
回到手册的 Android 源码编译 5.3 章节编译源码的时候, 把源码 `iTOP-iMX6_android4.4.2_20161018.tar.gz` 解压到新建的目录 imx6 目录下先不要编译, 进行下面的这个操作。 
执行命令： `free -m` 查看一下 Swap 的大小，如下图所示。

![01](https://github.com/AlvinMi/Linux-iMX6/blob/master/iMX6Q/03_Android/Picture/01.png)

如上图所示没有达到接近 2000 的大小, 那么就进行一下的操作, 增加 Swap 的大小。
参考：http://blog.csdn.net/yellow_hill/article/details/38894317

* 先创建一个文件: `mkdir swap`
* 然后进入 swap 文件夹: `cd swap`
* 在文件夹里执行这个命令: `sudo dd if=/dev/zero of=swapfile bs=1024 count=2000000`
* 等待一会完成后, 输入命令: `sudo mkswap swapfile`
* 最后再执行命令: `sudo swapon swapfile`
* 然后可以在看一下: `free -m` 是否符合要求。

![02](https://github.com/AlvinMi/Linux-iMX6/blob/master/iMX6Q/03_Android/Picture/02.png)

* 符合要求则回到目录: `/home/imx6/iTOP-iMX6_android4.4.2` 下， 执行 `./create.sh` 编译源码。

## 错误问题解决

#### 01 关于 uuid/uuid.h 的问题
如下图：

![03](https://github.com/AlvinMi/Linux-iMX6/blob/master/iMX6Q/03_Android/Picture/03.png)

原因： 缺少文件库所导致。	解决办法: 安装对应的包 `apt-get install uuid-dev` 安装过程需要确认, 输入 y 即可。安装完成后继续执行命令 `./create.sh` 进行编译。

#### 02 关于 lzo/lzo1x.h 问题 
如下图：

![04](https://github.com/AlvinMi/Linux-iMX6/blob/master/iMX6Q/03_Android/Picture/04.png)

原因： 缺少文件所导致。 解决办法： 安装对应的包 `apt-get install liblzo2-dev` 安装过程如果需要确认, 输入 y 即可。 安装完成继续执行命令 `./create.sh` 进行编译。
如：

![05](https://github.com/AlvinMi/Linux-iMX6/blob/master/iMX6Q/03_Android/Picture/05.png)

>关于usr/bin/ld: cannot find -lxxx问题总结：
http://eminzhang.blog.51cto.com/5292425/1285705

#### 03 关于 usr/bin/ld: connot find -lz 问题
如下图：

![06](https://github.com/AlvinMi/Linux-iMX6/blob/master/iMX6Q/03_Android/Picture/06.png)

原因: 缺少文件库所导致。 解决办法： 安转包 `lib32z1-dev`, 解决办法是安装包: `apt-get install lib32z1-dev`, 安装完成后继续输入命令 `./create.sh` 进行编译。
如：

![07](https://github.com/AlvinMi/Linux-iMX6/blob/master/iMX6Q/03_Android/Picture/07.png)

不出意外就能通过了, 因为我就这么编译过了。 都是泪...
![08](https://github.com/AlvinMi/Linux-iMX6/blob/master/iMX6Q/03_Android/Picture/08.png)


PS： 附加！ 
如果遇到以下情况, 如图！ 那么就回到编译过程出错的时候, 增加 Swap 的大小。

![09](https://github.com/AlvinMi/Linux-iMX6/blob/master/iMX6Q/03_Android/Picture/09.png)
 



/etc/fstab 文件解释： http://ckc620.blog.51cto.com/631254/394238