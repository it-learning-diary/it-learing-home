标题：30分钟搭建属于自己酷酷的个人网站！

&emsp;&emsp;拥有自己的特色的个人博客网站真是一件很酷的事情！**不仅在朋友面前可以装杯，还可以随时随地在上面记录自己的学习经验，不用担心丢失，在面试的时候说不定也是一个加分项哦！** 先来看看下面这几款是不是您喜欢的类型呢? 

---

&emsp;&emsp;**款式一：与你相遇的夏天**

![image-20220213161220115](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220213161220115.png)

![image-20220213161256693](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220213161256693.png)

---

&emsp;&emsp;**款式二：卡哇伊**

![image-20220213164302329](https://gitee.com/whose-white-moon/blog-image/raw/master/20220213164302.png)

![image-20220213164341330](https://gitee.com/whose-white-moon/blog-image/raw/master/20220213164341.png)

---

&emsp;&emsp;**款式三：妹妹说紫色很有韵味**

![image-20220213161612272](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220213161612272.png)

---

&emsp;&emsp;**款式四：二次元**

![image-20220213162117741](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220213162117741.png)

---

&emsp;&emsp;博主个人主页：http://42.194.186.190/

&emsp;&emsp;看完后有没有想搭建自己个人博客的冲动呢？**我们的主题可不止上面举例的几款而已哦，还有更多更优美、更精致的主题还没有完全列举出来，而且，我们还可以在这些主题上根据自己的需要DIY(自定义)，灵活度很高!** 

&emsp;&emsp;**只需要一台服务器 + 30分钟即可拥有属于自己个人的网站，你还在犹豫什么？** 

- 服务器很贵？非也，白菜价！详细情况请看文章末尾介绍！

- 搭建要花费很长时间？非也，搭建过程中所遇到的坑，我都给你总结出来了，解决不了免费提供帮助，你还有什么可以顾虑？继续往下看吧！

---

### 一、认识Wordpress

&emsp;&emsp;在开始进行部署WordPress之前，我们先来简单认识下什么是WordPress，以及为什么选择它作为个人博客的搭建(学习或者使用一个知识，尽量要知其然，知其所以然)，如果您已经了解过WordPress或者对这些知识不感兴趣，可以直接跳到后面的安装流程。

#### 1、WordPress是什么?

&emsp;&emsp;WordPress诞生于2003年，是一款基于PHP和MySQL的【免费开源】内容管理系统(即CMS)。它是目前为止全球使用最广泛的CMS，2019年据有关网站统计，它在使用CMS构建的所有网站中，预估占有60%的市场份额。

#### 2、为什么选择它作为个人博客系统的搭建

&emsp;&emsp;其实WordPress在设计之初，目的就是用于博客系统，随着时间的发展，各种功能逐渐完善，目前在它的社区生态中，至少已经拥有数千款插件、小工具和主题，相比于市场上的其他博客系统，它算是非常完善的一款系统。

&emsp;&emsp;与此同时，WordPress使用简单，对操作者的技术门槛相对较低，还是免费开源的的，它遵循开源协议共用许可证(GPLv2或更高版本)授权，对于个人博客的需求，无论是功能复杂的工具，还是大气美观的样式，它都能够通过社区中的工具和主题给予您支持，因此，个人博客希望使用WordPress搭建再合适不过。

---

### 二、安装WordPress

&emsp;&emsp;对WordPress进行简单介绍后，我们开始正式开始使用WordPress来搭建我们的个人网站。因为WordPress是基于PHP和MySQL的，所以在搭建它之前，我们需要先将PHP、MySQL的环境搭建好，同时官方推荐使用Nginx或者Apache作为运行WordPress最佳性能、功能的服务器，所以我们也需要搭建一个Nginx的环境。

#### 1、安装注意事项

&emsp;&emsp;(1)、php环境安装：wordpress要求7.4以上，建议使用8.0

&emsp;&emsp;(2)、mysql环境安装：wordpress要求5.6之后，推荐8.0

&emsp;&emsp;(3)、nginx环境安装：没有具体要求，使用较新稳定版即可

&emsp;&emsp;(4)、wordpress安装，推荐使用最新版本

#### 2、安装教程

&emsp;&emsp;1、PHP8.0安装教程

&emsp;&emsp;2、MySQL8安装教程

&emsp;&emsp;3、Nginx安装教程

#### 1、下载压缩包并上传到服务器

&emsp;&emsp;下载地址：https://cn.wordpress.org/download/

![image-20220212100923777](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212100923777.png)

&emsp;&emsp;上传到服务器并解压：tar -xf 压缩包名称

![image-20220212101022511](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212101022511.png)

---

#### 2、修改wordpress中数据库和秘钥配置

&emsp;&emsp;(1)、复制配置文件：cp wp-config-sample.php wp-config.php 


![image-20220212104308020](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212104308020.png)

&emsp;&emsp;(2)、修改配置文件数据库信息

&emsp;&emsp;注意：mysql8安装完后会生成一个默认密码，首次连接时需要输入，查询默认密码的方式：cat /var/log/mysqld.log | grep local

![image-20220212105250242](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212105250242.png)

&emsp;&emsp;(3)、修改配置文件秘钥信息

&emsp;&emsp;设置这些秘钥的目的是为了让你的wordpress网站更加安全，所以这一步建议要修改。

![image-20220212105757386](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212105757386.png)


![image-20220212105515475](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212105515475.png)

---

#### 3、配置Nginx信息

&emsp;&emsp;(1)、修改nginx文件，指定根目录和代理php请求

```
# 修改nginx配置文件
vi nginx.conf

# 指定nginx根目录
root /usr/local/blog/wordpress/wordpress;
index index.php;

# 代理php请求
location ~ \.php$ {
      fastcgi_pass 127.0.0.1:9000;
      fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
      include fastcgi_params;
      add_header Access-Control-Allow-Methods *;
      add_header Access-Control-Allow-Oriain '*';
}

```

![image-20220212102721169](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212102721169.png)

&emsp;&emsp;(2)、保存修改并退出：Esc键 + 冒号 + wq(保存并退出vi的修改模式)

![image-20220212103241515](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212103241515.png)



---

#### 4、在浏览器进行安装wordpress

&emsp;&emsp;注意事项：

&emsp;&emsp;1、如果出现404，那可能是修改了nginx配置没有重新加载，解决：进入到nginx的sbin目录，执行: ./nginx -s reload。

&emsp;&emsp;2、mysql中内置的root用户默认的访问权限是localhost即本机，如果需要远程连接，则需要连接mysql数据，进入mysql库修改用户root的host字段，命令为：UPDATE mysql.user SET host='%' WHERE user = 'root';

&emsp;&emsp;3、如果访问出现“连接数据库报错”，可能是因为没有修改数据库默认密码，连接数据库，然后切换到mysql库，执行：ALTER USER 'root@%' IDENTIFIED WITH caching_sha2_password BY '密码'; 然后将设置的新密码更新到wp-config.php的文件中，重新访问即可。

&emsp;&emsp;(1)、访问下面地址进入安装引导：http://ip地址或者域名/wp-admin/index.php

![image-20220212114917490](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212114917490.png)

&emsp;&emsp;(2)、配置完成，开始登录到后台

![image-20220212115135361](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212115135361.png)

&emsp;&emsp;(3)、选择符合自己的语言环境

![image-20220212115254836](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212115254836.png)

&emsp;&emsp;(4)、进入到后台管理界面

![image-20220212115929656](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212115929656.png)


&emsp;&emsp;(5)、管理个人博客地址

![image-20220212121200194](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212121200194.png)

&emsp;&emsp;(6)、访问个人博客主页

&emsp;&emsp;注意：因为默认的是http://ip地址(默认是80)端口，但此时80端口已经被nginx代理，所以会进入到Nginx的默认页面，想要进入wordpress默认的主页，则需要在nginx配置文件中添加：**rewrite /wp-admin$ $scheme://$host$uri/ permanent;**  然后进入sbin目录重新加载nginx配置文件(./nginx -s reload)即可。

![image-20220212133304061](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212133304061.png)

![image-20220212133723194](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20220212133723194.png)

---

### 四、服务器的获取

&emsp;&emsp;看完了整个搭建的过程，是否感觉搭建过程其实也不复杂？现在就差一台服务器，就可以正式开始了，刚好最近云厂商在搞活动，现在的价格相比平常就是白菜价。

&emsp;&emsp;购买链接：https://curl.qcloud.com/L1epMj7b

![image-20220213171502922](https://gitee.com/whose-white-moon/blog-image/raw/master/20220213171503.png)

&emsp;&emsp;现在入手还可以免费领取优惠卷，领取链接：https://curl.qcloud.com/YIRVbiCZ

![image-20220213172055255](https://gitee.com/whose-white-moon/blog-image/raw/master/20220213172055.png)

---

### 五、写在最后

&emsp;&emsp;跟着上面的步骤，通过IP地址可以访问到第一篇文章，证明你的博客网站已经初步搭建完成。如果想要让自己的博客变得高大上、变得炫酷，那还需要选择符合自己的主题和工具。**下一篇，博主会搭建网站常见的问题汇总，让你更好地使用WordPress搭建出个人特色的网站。**

&emsp;&emsp;**如果觉得文章有帮助，请给博主点赞、收藏、关注。** 后续博主会带来更多优质、有质量的文章。

&emsp;&emsp;想要学习更多知识，了解更多开源项目，请点击下面添加博主，进入技术圈子(**圈子所有资源全免费，但要求加入的小伙伴要有长久兴趣，如果只是一时冲动就不推荐加入，毕竟名额有限**)

&emsp;&emsp;加入技术圈子，除了遇到搭建问题免费指导，还能第一时间收到行业最新咨询和认识各专业大佬！

&emsp;&emsp;博主个人技术博客主页：http://42.194.186.190/

