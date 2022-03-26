&emsp;&emsp;上一篇文章我们完成了基础的WordPress环境搭建，但对于要搭建一个有特色的网站还远不够，因为WordPress自身以及依赖的环境的一些存在一些默认的设置，**为了更好的运用WordPress，我们需要将这些问题都解决掉，下面就来总结一下使用WordPress后台遇到的一系列问题。**

---

### 一、WordPress默认只能上传小于2M的文件

&emsp;&emsp;产生原因：确实这个并非WordPress软件做的限制，实际上是PHP默认配置文件中限制了大小，因此我们需要修改PHP中限制最大的上传大小。

![image-20220213003637193](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326130841.png)

&emsp;&emsp;解决方案：

&emsp;&emsp;1、找到php配置文件php.ini，命令如下：php -i | grep 'php.ini'

![image-20220213004811174](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326130850.png)

&emsp;&emsp;2、修改里面限制的最大大小参数：upload_max_filesize和post_max_size，php给这些参数设置默认值的目的主要是为了防止程序上传太大的文件，占用太多的资源，从而导致网站响应缓慢，下面看看这些具体参数的含义：

> - upload_max_filesize: 最大上传尺寸
> - post_max_size: POST 请求最大尺寸
> - memory_limit: PHP 进程可以使用的内存限制
> - max_execution_time：PHP 程序的最大执行时间
> - max_input_time：最大输入时间

![image-20220213005626672](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326130900.png)

![image-20220213010809512](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326130910.png)

&emsp;&emsp;3、使用Pho-fpm重启PHP服务(不知道如何重启的，请参考PHP环境安装教程篇)，命令：./php-fpm restart

![image-20220213010336479](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326130919.png)

&emsp;&emsp;4、重新进入博客系统后台，再刷新看是否生效

![image-20220213010941766](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326130928.png)



---

### 二、依旧无法上传大于2M的图片或者媒体文件

&emsp;&emsp;问题描述：从服务器收到预料之外的响应。此文件可能已被成功上传或者图像后期处理失败。可能服务器忙或没有足够的资源。

![image-20220213013701614](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326130937.png)

&emsp;&emsp;产生原因：既然PHP限制我们已经修改，那还可能是什么原因呢？此时我们考虑到，我们的所有请求都是经过nginx然后代理进来的，所以失败的原因是Nginx也有限制(默认情况下最大只能上传1M)，需要修改nginx.conf重新设置大小，命令：client_max_body_size 128m;

&emsp;&emsp;解决方案：

&emsp;&emsp;1、找到nginx配置文件

![image-20220213012532288](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326130947.png)

&emsp;&emsp;2、添加client_max_body_size属性

![image-20220213012754394](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326130957.png)

&emsp;&emsp;3、进入sbin目录，重新加载nginx配置，命令：./nginx -s reload

![image-20220213012901715](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131009.png)

&emsp;&emsp;4、进入博客后台，尝试重新上传

![image-20220213013224775](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131020.png)

---

### 三、上传文件失败，提示无法创建目录xxx

&emsp;&emsp;问题描述：无法创建目录 wp-content/uploads/xxx。它的父目录是否可以被服务器写入？

&emsp;&emsp;原因：wp-content目录没有写入权限

&emsp;&emsp;解决方案：

&emsp;&emsp;1、找到wordpress目录下的wp-content目录，使用chmod给给文件夹赋予写的权限

![image-20220213014018951](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131033.png)

&emsp;&emsp;2、回到博客后台，重新上传文件成功

![image-20220213014132235](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131043.png)

---

### 四、修改WordPress地址和站点地址后，系统访问404

&emsp;&emsp;问题描述：默认情况下博客后台地址和博客前台地址是一样的，许多小伙伴为了区别后台系统地址(即WordPress地址)和博客访问地址(即站点地址)，在初始化后都会修改它们，但是已修改后发现无法所有页面都出现了404。

![image-20220213084922209](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131053.png)

![image-20220213092533246](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131105.png)

&emsp;&emsp;解决方案：

&emsp;&emsp;1、找到nginx配置文件，在server{}中添加以下代码：

```
try_files $uri $uri/ /index.php?$args;
rewrite /wp-admin$ $scheme://$host$uri/ permanent;
```

![image-20220213092452051](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131115.png)

&emsp;&emsp;2、重新加载nginx配置文件，重试访问异常解决

![image-20220213085800690](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131125.png)

---

### 五、修改固定链接规则(伪静态)后，博客文章404

&emsp;&emsp;问题描述：修改WordPress默认的Url结构有利于我们提高文章链接的美感、可用性以及前向兼容性，便于提升网站SEO，但是修改这个结构后发现所有的文章都出现了404。

&emsp;&emsp;相关概念介绍：**伪静态是相对真实静态来讲的，通常为了增强搜索引擎的友好面，都将文章内容生成静态页面**

![image-20220213090233867](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131137.png)


&emsp;&emsp;解决方案：设置nginx的伪静态规则

&emsp;&emsp;1、找到nginx配置文件，在server{}中添加以下代码：try_files $uri $uri/ /index.php?$args;

![image-20220213085337830](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131147.png)

&emsp;&emsp;2、重新加载nginx配置文件，重试访问异常解决

![image-20220213085800690](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131159.png)

---

### 六、安装主题时出现请输入FTP账号和密码

&emsp;&emsp;问题描述：在线安装WordPress主题时，出现请输入FTP账号密码或者无法创建目录问题

![image-20220213093046590](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131225.png)

&emsp;&emsp;产生原因：wordpress文件夹对应的访问权限不够

&emsp;&emsp;解决方案：

&emsp;&emsp;1、将wordpress文件夹的访问权限修改为可读可写可执行，命令：chmod -R  777 /wordpress

![image-20220213100329137](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131253.png)

&emsp;&emsp;2、在wp-config.php下添加代码：define('FS_METHOD','direct');

![image-20220213100536924](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131304.png)

&emsp;&emsp;3、回到博客后台，重新安装成功

![image-20220213100347113](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326131317.png)

### 抱歉，出于安全的考虑，不支持此文件类型

解决：https://www.lmdouble.com/0001262167.html

### 无法将上传的文件移动至wp-content/uploads/

修改所属用户和所属用户组：sudo chown root:root template -R

解决：将提示错误的文件夹权限赋予可执行即可：chmod 755 文件夹位置

---

### 写在最后

&emsp;&emsp;使用WordPress搭建个人网站所常遇到的问题基本都在上面汇总出现了，解决完这些问题后，我们就可以正式开始个人特色博客的搭建了。**下一篇文章将主要讲述如何使用主题和工具，让自己的网站更加炫酷，里面的主题都是博主历经几天挑选出来的，肯定比你去网上一个个找效率要高。**

&emsp;&emsp;**如果觉得文章有帮助，可以给博主Star。** 后续博主会带来更多优质、有质量的文章。

&emsp;&emsp;想要学习更多知识，了解更多开源项目，可以添加博主，

