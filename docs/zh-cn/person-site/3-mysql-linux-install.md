<br>

---

> - **如果文章对你有帮助**、**<font color=1E88E5>欢迎关注、点赞、收藏(一键三连)</font>**、**有任何问题欢迎私信,看到会及时回复!**

---


# 前言

- 大家好,我是小诚,**<font color=red>这段时间在网上进行了一些面试,发现无论什么公司,数据库的面试题都是不可避免的**,甚至一些前端工程师面试的时候都避免不了被询问到和数据库有关的一些问题。


- 通过面试,也发现了一些现象,网上的很多数据库教程都是讲得比较浅或者只讲解了片面,比较好的教程要么找不到要么就是收费昂贵,为了方便自己复习以及帮助到一些想从全面了解数据库的小伙伴,**<font color=red>这段时间在不断恶补数据库的知识,打算出一个关于《从0到1-全面深刻理解MySQL》的教程,教程是以小白视角出发,从最简单的安装数据库到深入理解数据库如何执行SQL语句到数据库如何实现数据存储和查询的全方位讲解**。


- 争取让所有对MySQL库感兴趣的小伙伴都能够从中学习到一些知识,无论是用于面试或者拓展自己的知识广度方面起到一些帮助,**<font color=red>当我们对一个知识从"知其然"到"知其所以然"时,涨薪和升职自然也随之而来。**


- 《从0到1-全面深刻理解MySQL系列》第二篇就从最基本的安装MySQL-Linux环境开始,**<font color=red>感兴趣的小伙伴可以关注我,系列文章会持续更新,一起加油,一起进步!**

<br>


&emsp;&emsp;Windows安装教程：[《从0到1-全面深刻理解MySQL系列》- 最详细的MySQL安装流程(Window环境)](https://blog.csdn.net/qq_40891009/article/details/118557709?spm=1001.2014.3001.5501)


<br>

# 1、删除旧版本
<br>

&emsp;&emsp;**查看服务器是否有自带的MySQL,如果有可以直接使用,如果自带的版本比较低,可以删除然后安装自己想要的版本**(在安装新版本MySQL之前，需要卸载服务器自带的MySQL包和MySQL数据库分支mariadb的包)
<br>

&emsp;&emsp;1、rpm -qa|grep mysql -- 查询服务器是否有mysql,如有,则执行下面的语句进行删除
<br>

&emsp;&emsp;2、rpm -qa |grep mariadb -- 查询服务器是否有mariadb,有则执行第三步进行删除
<br>

&emsp;&emsp;3、rpm -e  --nodeps 要删除的文件名(**nodeps表示强制删除**)
<br>

<center><img src="https://img-blog.csdnimg.cn/20210708214110184.png" /></center>

<br>


# 2、查看服务器内核类型,下载合适的版本并上传到服务器
<br>


## 2.1、使用cat /proc/version查看系统的内核类型
<br>

<center><img src="https://img-blog.csdnimg.cn/20210708214248326.png" /></center>

<br>

## 2.2、到官网下载合适的类型
<br>

<center><img src="https://img-blog.csdnimg.cn/20210708214537123.png" /></center>

<br>

<center><img src="https://img-blog.csdnimg.cn/20210708214600693.png" /></center>

<br>

<center><img src="https://img-blog.csdnimg.cn/2021070821475196.png" /></center>

<br>

## 2.3、通过rz命令或者xftp工具上传到服务器
<br>

&emsp;&emsp;**小贴士1:** 如果使用rz命令时提示找不到命令,直接执行: yum -y install lrzsz 则可以在线下载。<br>

&emsp;&emsp;**命令:** rz 或者rz -be <br>

&emsp;&emsp;**格式:** rz -be 选择需要上传的文件<br>

&emsp;&emsp;批量或者单个上传文件，通过ZMODEM协议,除此之外,还可以通过ftp或者sftp进行上传<br>

&emsp;&emsp;**小贴士2:** 如果觉得通过rz命令上传时间比较久,**可以下载一个xftp工具,通过这个工具上传效率更高**(此篇就不展开将这个工具,如有需要,大家可以在下方留言,后续会展开一片文章具体介绍,)
<br>

<center><img src="https://img-blog.csdnimg.cn/20210708215148596.png" /></center>

<br>

# 3、解压并逐步安装对应的组件
<br>

## 3.1、解压命令: tar -xvf 需要解压的文件名 -C 需要加压到的路径(-C和后面的参数可以省略)

<br>

<center><img src="https://img-blog.csdnimg.cn/20210708215419184.png" /></center>


<br>

## 3.2、安装组件命令: rpm -ivh 需要安装的组件名

<br>

&emsp;&emsp;**按照下面的命令顺序执行,文件名修改成你压缩后的文件名称即可**
<br>

```java
// mysql-community-common
1、rpm -ivh mysql-community-common-8.0.16-2.el7.x86_64.rpm

// mysql-community-libs
2、rpm -ivh mysql-community-libs-8.0.16-2.el7.x86_64.rpm --force --nodeps

// mysql-community-libs-compat
3、rpm -ivh mysql-community-libs-compat-8.0.16-2.el7.x86_64.rpm

// mysql-community-client
4、rpm -ivh mysql-community-client-8.0.16-2.el7.x86_64.rpm --force --nodeps

// mysql-community-server
5、rpm -ivh mysql-community-server-8.0.16-2.el7.x86_64.rpm --force --nodeps

// 如果还是启动不了，则直接将解压出来的除了test外的包都安装一遍
   
// 查看已安装的组件
6、rpm -qa | grep mysql

```

<br>

<center><img src="https://img-blog.csdnimg.cn/20210708215535694.png" /></center>

<br>

## 3.3、启动MySQL服务器,如果报错，则执行第4步
<br>

&emsp;&emsp;**启动命令: systemctl start mysql** 

​		如果使用这个命令启动报错：Failed to start mysqld.service: Unit not found

​		则使用：start mysqld start


<br>

## 3.4、如启动报如下的错,则进行响应的步骤操作修复

<br>

&emsp;&emsp;**报错信息:** Job for mysqld.service failed because the control process exited with error code. See "systemctl status mysqld.service" and "journalctl -xe" for details.<br>

&emsp;&emsp;**根据报错信息执行**: systemctl status mysqld.service" 或者 "journalctl -xe"命令查看报错详情,发现报错信息中存在:  **<font  color=red>Data Dictionary upgrade from MySQL 5.7 in progress**。<br>

&emsp;&emsp;说明是因为新版本和之前服务器自带的版本对应的包存在冲突,删除对应的冲突目录即可,执行: **<font  color=red>rm -rf /var/lib/mysql/*(执行删除命令的时候要看清楚哦)**
<br>

<center><img src="https://img-blog.csdnimg.cn/20210708220307981.png" /></center>


<br>

## 3.5、修复完成后再执行:systemctl start mysql启动MySQL服务

<br>

<br>

# 4、连接MySQL服务并修改密码
<br>

&emsp;&emsp;第一次成功启动MySQL会被设置默认一个密码,通过以下命令查看并进行登录。
  <br>

&emsp;&emsp;**1、查看第一次启动的临时密码**：grep password /var/log/mysqld.log
  <br>

&emsp;&emsp;**2、连接到服务器**: mysql -u root -p 回车,然后输出密码
  <br>

&emsp;&emsp;**3、第一次连接会强制你必须修改连接密码**，可以使用以下的语句进行修改密码:
  <br>

&emsp;&emsp;ALTER USER root@localhost IDENTIFIED WITH caching_sha2_password BY '123456';**<font color=red>(MySQL8.x适合使用这个语句)**
 <br>

&emsp;&emsp;UPDATE USER SET PASSWORD=PASSWORD('你的密码') WHERE USER='root';**<font color=red>(MySQL5.x版本的修改)**
<br>

<center><img src="https://img-blog.csdnimg.cn/20210708220841225.png" /></center>

<br>

# 5、小结
<br>

&emsp;&emsp; 不积跬步，无以至千里;不积小流，无以成江海。今天播种努力的种子,总会有一天发芽!
<br>

 &emsp;&emsp;  **<font color=red>欢迎大家关注,如果觉得文章对你有帮助,不要忘记一键三连哦,你的支持是我创作更加优质文章的动力,有任何问题可以私信我,看到会及时给你答复！**。
<br>