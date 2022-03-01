&emsp;&emsp;Nginx作为一款著名的Http服务器、反向代理服务器，在日常工作和学习中，我们都避免不了与它进行接触，本文通过亲自实践的方式，完整详细地讲解了nginx的安装流程，希望能够帮助到有需要的同学。

#### 1、下载nginx压缩包

&emsp;&emsp;地址：https://nginx.org/en/download.html

![image-20220210220131517](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220210220131517.png)	

---

#### 2、上传到服务器

&emsp;&emsp;说明：使用rz命令实现文件上传到服务器

![image-20220210220800499](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220210220800499.png)

---

#### 3、安装nginx所需相关依赖

&emsp;&emsp;检查linux服务器中是否存在: gcc-c++、 pcre-devel、 zlib-devel、openssl-devel

&emsp;&emsp;**一、检查方式如下：**

&emsp;&emsp;1、如果是使用rpm安装，则使用rpm qa来检查，例如：rpm qa | grep gcc-c++(软件名称)

&emsp;&emsp;2、如果是使用yum安装，则使用yum list installed来检查，例如：yum list installed | grep pcre-devel(软件名称)

&emsp;&emsp;3、如果是使用deb安装，则使用dbkg -l来检查，例如：dbkg -l | grep pcre-devel(软件名称)

&emsp;&emsp;**二、如果都没有安装，则可以直接使用下面一个语句将所有依赖都安装：**

> yum -y install gcc pcre-devel zlib-devel openssl openssl-devel

&emsp;&emsp;**三、如果有部分依赖已经安装，则可以通过下面的语句安装需要的依赖：**

> 1、yum install gcc-c++
>
> 2、yum install pcre-devel
>
> 3、 yum install zlib-devel
>
> 4、yum install openssl openssl-devel

---

### 4、解压压缩包

&emsp;&emsp;命令：tat -xvf  压缩包名称

![image-20220210222556198](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220210222556198.png)

&emsp;&emsp;**配置安装地址，执行编译、安装操作**

&emsp;&emsp;1、配置安装地址：./configure --prefix=/usr/local/blog/nginxconf

![image-20220210222915921](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220210222915921.png)

&emsp;&emsp;2、编译：make

![image-20220210223021388](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220210223021388.png)

&emsp;&emsp;3、安装：make install

![image-20220210223249247](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220210223249247.png)

---

#### 5、检查并启动Nginx

&emsp;&emsp;1、测试nginx是否安装成功：./nginx -t

![image-20220210223457210](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220210223457210.png)

&emsp;&emsp;2、启动nginx：./nginx

![image-20220210223725317](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220210223725317.png)

&emsp;&emsp;3、在浏览器访问nginx(记得要在防火墙中放行80端口)：

![image-20220210223922115](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220210223922115.png)

&emsp;&emsp;4、设置开启自启动（可选）：vim /etc/rc.d/rc.local，然后将nginx的sbin目录添加到该文件中

![image-20220210224421091](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220210224421091.png)

---

#### 写在最后

&emsp;&emsp;**如果觉得文章有帮助，请给博主点赞、收藏、关注。** 后续博主会带来更多优质、有质量的文章。

&emsp;&emsp;想要学习更多知识，了解更多开源项目，请点击下面添加博主，进入技术圈子(**圈子里所有资源全免费，但要求加入的小伙伴要有长久兴趣，如果只是一时冲动就不推荐加入，毕竟名额有限**)。

&emsp;&emsp;加入技术圈子，除了遇到搭建问题免费指导，还能第一时间收到行业最新咨询和认识各专业大佬！
