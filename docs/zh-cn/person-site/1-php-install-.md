### 一、PHP环境安装流程

&emsp;&emsp;说明：最近在搭建个人网站，环境有用到PHP，特此记录。本文安装案例是以PHP8.0.0版本为示例，**开始安装前建议先将第三步骤抛出异常的依赖安装，这样安装的时候就无需逐个解决问题。**

#### 1、下载PHP包(在线或者压缩包方式都可)

&emsp;&emsp;建议：提前创建好压缩包地址，方便后续对文件管理，下载地址：https://www.php.net/downloads.php

&emsp;&emsp;在线下载(如果网速很慢，则离线下载上传到服务器)：wget https://www.php.net/distributions/php-8.0.0.tar.gz

&emsp;&emsp;离线下载后可通过rz命令上传到服务器：rz -be 文件名

![image-20220206164701929](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220206164701929.png)

---

#### 2、解压压缩包

&emsp;&emsp;执行代码：tar -xvf  php-8.0.0.tar.gz

![image-20220206173311010](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220206173311010.png)

---

#### 3、指定配置信息

&emsp;&emsp;说明：在执行编译之前，需要配置此次编译和安装后的文件的存放位置以及安装的一些组件，**在安装前务必要清楚，如果不了解，建议直接使用本文的代码，防止出现异常问题。**

&emsp;&emsp;了解概念：

&emsp;&emsp;1、源码安装需要经历步骤：配置(configure)、编译(make)、安装( make install )。

&emsp;&emsp;2、Configure则是一个可执行文件，可以配置很多选项(可以理解为我们在Windows安装时的图像化界面的功能)，可以通过./configure --help来查看选项具体含义。

&emsp;&emsp;其中--prefix选项作用是配置源码安装的路径，--with-config-file-path选项作用是设置php配置文件(php.ini)的存放位置，一般来说安装的时候只需要指定这两个路径即可，其他的则是指定php支持哪一些组件了。

&emsp;&emsp;3、延伸说明下指定存放路径的好处，其实可以类比在Windows等图形化系统的安装软件步骤，安装时指定软件的位置，方便后续对软件进行统一管理(删除，查找等)。

&emsp;&emsp;4、注意：下面的代码必须处于同一行上，建议先复制到文本上查看是否在同一行，然后再复制到服务器中执行(否则会出现很多莫名其妙的问题，相信你也不想花上几天时间去处理)。

&emsp;&emsp;至于其他参数选项的含义，可以打官方或者使用./configure --help来查看选项具体含义，建议添加自己需要的组件即可，如果不清楚，进入到解压的文件后直接执行下面的代码。

```java
./configure --prefix=/usr/local/install/php8 --with-config-file-path=/usr/local/install/php8/etc --with-curl --with-freetype --enable-gd --with-jpeg  --with-gettext --with-kerberos --with-libdir=lib64 --with-libxml --with-mysqli --with-openssl --with-pdo-mysql  --with-pdo-sqlite --with-pear --enable-sockets --with-mhash --with-ldap-sasl --with-xsl --with-zlib --with-zip -with-bz2 --with-iconv  --enable-fpm --enable-pdo  --enable-bcmath  --enable-mbregex --enable-mbstring --enable-opcache --enable-pcntl  --enable-shmop --enable-soap --enable-sockets --enable-sysvsem --enable-xml --enable-sysvsem --enable-cli --enable-opcache --enable-intl --enable-calendar --enable-static --enable-mysqlnd
```

&emsp;&emsp;5、配置成功后会出现下面提示：

![image-20220208155945357](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220208155945357.png)

---

#### 4、编译中可能出现的异常和解决方案

&emsp;&emsp;说明，新服务器在执行configure脚本设置配置之前，需要存在的相关依赖脚本整合，下面会对每一个异常问题做具体说明：

```
yum install dnf
dnf install libxml2-devel
dnf install sqlite-devel
dnf -y install bzip2-devel
dnf -y install libcurl-devel
dnf -y install libpng-devel
dnf -y install libjpeg-devel
dnf -y install freetype-devel
dnf -y install libicu-devel
yum install oniguruma-devel -y
dnf -y install libxslt-devel
dnf -y install libzip-devel
```

&emsp;&emsp;1、异常信息：

![image-20220206173718813](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220206173718813.png)

```
configure: error: Package requirements (libxml-2.0 >= 2.9.0) were not met:
No package 'libxml-2.0' found
```
&emsp;&emsp;解决方案：dnf install libxml2-devel

&emsp;&emsp;2、异常信息：

![image-20220206173919732](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220206173919732.png)

```
-bash: dnf: command not found
```
&emsp;&emsp;解决方案：yum install dnf

&emsp;&emsp;3、异常信息：

![image-20220206174403895](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220206174403895.png)

```
configure: error: Package requirements (sqlite3 > 3.7.4) were not met:
No package 'sqlite3' found
```

&emsp;&emsp;解决方案：dnf install sqlite-devel

&emsp;&emsp;4、异常信息：

![image-20220206175158134](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220206175158134.png)

```
checking for BZip2 in default path... not found
configure: error: Please reinstall the BZip2 distribution
```

&emsp;&emsp;解决方案：dnf -y install bzip2-devel

&emsp;&emsp;5、异常信息：

![image-20220206175450008](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220206175450008.png)

```
configure: error: Package requirements (libcurl >= 7.29.0) were not met:
No package 'libcurl' found
```

&emsp;&emsp;解决方案：dnf -y install libcurl-devel

&emsp;&emsp;6、异常信息：

![image-20220206175615010](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220206175615010.png)

```
configure: error: Package requirements (libpng) were not met:
No package 'libpng' found
```

&emsp;&emsp;解决方案：dnf -y install libpng-devel


&emsp;&emsp;7、异常信息：

```
configure: error: Package requirements (libjpeg) were not met:
Package 'libjpeg', required by 'virtual:world', not found
```

&emsp;&emsp;解决方案：dnf -y install libjpeg-devel

&emsp;&emsp;8、异常信息：

```
configure: error: Package requirements (freetype2) were not met:
Package 'freetype2', required by 'virtual:world', not found
```

&emsp;&emsp;解决方案：dnf -y install freetype-devel

&emsp;&emsp;9、异常信息：

![image-20220206180127070](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220206180127070.png)

```
configure: error: Package requirements (icu-uc >= 50.1 icu-io icu-i18n) were not met:
Package 'icu-uc', required by 'virtual:world', not found
Package 'icu-io', required by 'virtual:world', not found
Package 'icu-i18n', required by 'virtual:world', not found
```

&emsp;&emsp;解决方案：dnf -y install libicu-devel

&emsp;&emsp;10、异常信息：

```
configure: error: Package requirements (oniguruma) were not met:
No package 'oniguruma' found
```

&emsp;&emsp;解决方案：yum install oniguruma-devel -y

&emsp;&emsp;11、异常信息：

```
configure: error: Package requirements (libxslt >= 1.1.0) were not met:
No package 'libxslt' found
```

&emsp;&emsp;解决方案：dnf -y install libxslt-devel


&emsp;&emsp;12、异常信息：

```
configure: error: Package requirements (libzip >= 0.11 libzip != 1.3.1 libzip != 1.7.0) were not met:
Package 'libzip', required by 'virtual:world', not found
```

&emsp;&emsp;解决方案：dnf -y install libzip-devel

&emsp;&emsp;13、异常信息：

```
configure: error: Package requirements (libzip >= 0.11 libzip != 1.3.1 libzip != 1.7.0) were not met:
Requested 'libzip >= 0.11' but version of libzip is 0.10.1
```

&emsp;&emsp;解决方案：

&emsp;&emsp;1、先删除原来的安装：yum remove libzip-devel libzip

&emsp;&emsp;2、在线下载对应的版本：wget https://libzip.org/download/libzip-1.3.2.tar.gz --no-check-certificate

&emsp;&emsp;4、进入解压后的文件夹，执行配置脚本：./confugure

&emsp;&emsp;5、编译并安装：make & make install

&emsp;&emsp;6、安装完成后，查询/usr/local/lib目录下是否有pkgconfig目录，有的话执行命令export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig/"指定PKG_CONFIG_PATH，然后重新执行切换php解压之后的文件夹，执行./configure脚本即可

---

#### 5、编译和安装

&emsp;&emsp;配置完成后，执行make指令对源代码进行编译，执行make install指令对源代码进行安装，也可以同时执行这两个指令，具体如下(注意：编译和安装都是在解压后的php文件夹下执行-即存在configure脚本的目录):

​		1、编译：make

![image-20220208170518841](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220208170518841.png)

​		2、安装：make install

![image-20220208170619708](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220208170619708.png)

&emsp;&emsp;3、出现异常：cc: internal compiler error: Killed (program cc1)

&emsp;&emsp;原因：大概率是因为内存不够使用，可以先使用交换分区来解决，编译安装后再删除掉即可。

&emsp;&emsp;解决：

```
sudo dd if=/dev/zero of=/swapfile bs=64M count=16
sudo mkswap /swapfile
sudo swapon /swapfile
```

&emsp;&emsp;取消交换分区：

```
sudo swapoff /swapfile
sudo rm /swapfile
```

---

#### 6、查看php安装情况

&emsp;&emsp;切换到configure指定的安装路径的bin目录，执行版本查看：

```
/usr/local/install/php8/bin/php --version
```

&emsp;&emsp;执行结果：

![image-20220208170643929](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220208170643929.png)

---

### 二、管理各种配置文件

1、生成php.ini配置文件(在php源码压缩包目录下)：

```
cp php.ini-production /usr/local/install/php8/etc/php.ini
```

2、生成www配置文件：

```
cd /usr/local/install/php8/etc/php-fpm.d/
cp www.conf.default www.conf
```

3、生成php-fpm配置文件：

```
cd /usr/local/install/php8/etc/
cp php-fpm.conf.default php-fpm.conf
```

4、生成php-fpm可执行文件：

```
-- 创建存放配置文件的目录
mkdir /usr/local/install/php8/fpm

-- 从源码中复制一份fpm可执行脚本
cp /usr/local/php/php-8.0.0/sapi/fpm/init.d.php-fpm /usr/local/install/php8/fpm/php-fpm

-- 将php-fpm修改为可执行文件
chmod 740 /usr/local/install/php8/fpm/php-fpm
```

---

### 三、管理php

说明：本文推荐使用php-fpm进行管理php程序，php-fpm(FastCGI Process Manager：FastCGI进程管理器)是一个PHPFastCGI管理器，旨在将FastCGI进程管理整合进PHP包中(来源：百度百科)。

优点：相对Spawn-FCGI，php-fpm在CPU和内存方面的控制都更胜一筹，而且前者很容易崩溃，必须用crontab进行监控，而php-fpm则没有这种烦恼。

1、启动php-fpm:

```
/usr/local/install/php8/fpm/php-fpm start
```

![image-20220208180228571](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220208180228571.png)

2、查看php-fpm进程是否启动成功

```
ps aux | grep php | grep -v grep
```

![image-20220208180409541](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220208180409541.png)

3、查看pfp-fpm占用的端口

```
ss -lntp | grep php
```

![image-20220208180643917](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220208180643917.png)

4、执行php --version出现php command not found异常，解决步骤

```java
// 1、修改配置文件
vim /etc/profile
 
//2、添加php环境变量(即configure脚本时指定的安装路径下的bin目录)
PATH=$PATH:/usr/local/install/php8/bin
export PATH
    
// 刷新配置文件
source /etc/profile
    
```

5、大功告成

![image-20220208181510517](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220208181510517.png)

---

### 写在最后

&emsp;&emsp;**如果觉的文章有帮助，请给博主点赞、收藏、关注。** 后续博主会带来更多优质、有质量的文章。
