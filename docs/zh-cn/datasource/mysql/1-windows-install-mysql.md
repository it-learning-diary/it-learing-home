
# 前言

<br>

- 大家好,我是小诚,**这段时间在网上进行了一些面试,发现无论什么公司,数据库的面试题都是不可避免的**,甚至一些前端工程师面试的时候都避免不了被询问到和数据库有关的一些问题。


- 通过面试,也发现了一些现象,网上的很多数据库教程都是讲得比较浅或者只讲解了片面,比较好的教程要么找不到要么就是收费昂贵,为了方便自己复习以及帮助到一些想从全面了解数据库的小伙伴,**这段时间在不断恶补数据库的知识,打算出一个关于《从0到1-全面深刻理解MySQL》的教程,教程是以小白视角出发,从最简单的安装数据库到深入理解数据库如何执行SQL语句到数据库如何实现数据存储和查询的全方位讲解**。


- 争取让所有对MySQL库感兴趣的小伙伴都能够从中学习到一些知识,无论是用于面试或者拓展自己的知识广度方面起到一些帮助,**当我们对一个知识从"知其然"到"知其所以然"时,涨薪和升职自然也随之而来。**


- 《从0到1-全面深刻理解MySQL系列》第一篇就从最基本的安装MySQL环境开始,**感兴趣的小伙伴可以关注我,系列文章会持续更新,一起加油,一起进步!**



<br>

# 下载前需要了解的一些概念
<br>

&emsp;&emsp;在进入到官网下载的时候,我们会发现官网上提供了很多类型的版本,它们到底是什么意思,哪个才是我们需要的呢?下面就就来简单介绍下常见的,如下:
<br>

&emsp;&emsp;**1、MySQL Enterprise Edition:** Mysql企业版本,包含了最新的特性和管理工具,以及可以提供技术支持(**但是是要收费**)。
<br>

&emsp;&emsp;**2、MySQL Cluster CGE:** 一个用于高吞吐量快速、稳定的访问数据的开源事务数据库,它包含了MySQL Cluster、MySQL Enterprise Edition、MySQL Cluster Manager的功能。
<br>

&emsp;&emsp;**3、MySQL Community (GPL)：** 遵循GPL开源协议的MySQL版本,平常我们使用的大多数遵循这个协议下的社区版(它是免费的)
<br>

&emsp;&emsp;**4、MySQL Installer**: 是一个安装管理程序,因为MySQL家族包括了许多产品,所以提供了一个统一管理下载的工具。
<br>

&emsp;&emsp;**5、MySQL Community Server**: MySQL Community (GPL)下的开源社区版本,是使用的数据库开源版本(**免费的,盘它**)。
<br>

&emsp;&emsp;**6、Mysql Workbench**: 类似navicat是个图形界面UI工具，可以实现远程Mysql数据库访问(一开始不建议直接使用图形化管理工具,建议先通过命令行了解,这样能够更快的认识Mysql,**高手都是用命令行的**(PS: 如有需要图形化工具的可以私我))。
<br>

<center><img src="https://img-blog.csdnimg.cn/2021070721570625.png" /></center>


<br>

# 选择自己需要的版本
<br>

&emsp;&emsp;到官网下载自己需要的版本([https://www.mysql.com/](https://www.mysql.com/))或者直接到云盘下载(私信我,这种方式更快),下载步骤如下:
<br>

&emsp;&emsp;**1、进入MySQL官网 =》选择“DOWNLOADS”选项 =》 点击MySQL Community (GPL) Downloads »**
<br>

<center><img src="https://img-blog.csdnimg.cn/20210707220141867.png" /></center>


<br>

&emsp;&emsp;**2、根据自己的需要选择“MySQL Community Server(不带图形化界面-推荐)”或者MySQL Workbench(带图形化界面的)**
<br>

<center><img src="https://img-blog.csdnimg.cn/2021070722050186.png" /></center>

<br>

&emsp;&emsp;**3、下载安装包(32位电脑下载32位的安装包,64位电脑下载64位的安装包)**

<br>

<center><img src="https://img-blog.csdnimg.cn/20210707220711970.png" /></center>


<center><img src="https://img-blog.csdnimg.cn/20210707220757674.png" /></center>

<br>

&emsp;&emsp;**4、解压下载好的安装包**

<br>

<center><img src="https://img-blog.csdnimg.cn/20210707220855463.png" /></center>


<br>

&emsp;&emsp;**5、配置环境变量**

<br>

&emsp;&emsp; **环境变量:**  **指的是当你在命令行属于任意一个值时,win系统会去环境变量池中匹配,如果有匹配到可执行的路径,则直接去对应的路径下进行执行**。
<br>

&emsp;&emsp;如你输入mysql,然后你在环境变量中配置了你安装的MySQL路径为:D:\\mysql8.x\bin,则系统会匹配成D:\\mysql8.x\bin\mysql，如果这个目录下有这个可执行文件,则运行,具体步骤如下:。

<br>

<center><img src="https://img-blog.csdnimg.cn/20210707221053115.png" /></center>


<center><img src="https://img-blog.csdnimg.cn/20210707221139130.png" /></center>


<center><img src="https://img-blog.csdnimg.cn/2021070722120779.png" /></center>


<center><img src="https://img-blog.csdnimg.cn/20210707221235118.png" /></center>


<br>

#  连接MySQL服务


<br>

&emsp;&emsp; **1、启动MySQL服务器,并进行登录**
<br>

<center><img src="https://img-blog.csdnimg.cn/20210707221648426.png" /></center>


<br>

&emsp;&emsp; **2、如果发现MySQL服务无法启动,则进行下面的配置即可**
<br>

<center><img src="https://img-blog.csdnimg.cn/2021070722183157.png" /></center>

<br>

&emsp;&emsp; **(1)、在mysql压缩的路径中添加以下my.ini文件,内容如下(把其中的两处工作路径改为自己的按照路径即可):**
<br>

```java
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=D:\Mylargeprogram\Mysql\mysql-8.0.12-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:\Mylargeprogram\Mysql\mysql-8.0.12-winx64\data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```
<br>

&emsp;&emsp; **(2)、执行执行 mysqld --initialize-insecure 指令进行配置，安装路径会默认生成一个data文件夹，如下:**
<br>

<center><img src="https://img-blog.csdnimg.cn/20210707222025848.png" /></center>


<br>

&emsp;&emsp; **3、输入mysqld --install将mysql注册到Window服务中，如果提示已经存在则跳过**
<br>


&emsp;&emsp; **4、启动mysql服务:net start mysql**
<br>

&emsp;&emsp; **5、连接Mysql服务: mysql -u root -p 回车(默认密码为空,输入密码时直接回车即可,为了安全性,记得修改密码哦</font>),到此Mysql安装完成,可以随便操作了！**

<br>

<center><img src="https://img-blog.csdnimg.cn/20210707222252255.png" /></center>


&emsp;&emsp; **6、修改密码,分为MySQL5.x版本和8.x版本,步骤如下:**
<br>

&emsp;&emsp;(1)、选中mysql数据库: use mysql
<br>

&emsp;&emsp;(2)、修改root用户的密码

```java
// 5.x版本的修改
UPDATE USER SET PASSWORD=PASSWORD('你的密码') WHERE USER='root';

// 8.x版本的修改
// 格式: alter 表名 用户名@user表中用户名对应的Host字段值 IDENTIFIED WITH 指定使用哪种加密技术 BY ‘修改后的密码’
ALTER USER root@localhost IDENTIFIED WITH caching_sha2_password BY '123456';
```

<br>

# 小结
<br>

&emsp;&emsp; 不积跬步，无以至千里；不积小流，无以成江海。今天播种努力的种子,总会有一天发芽!
<br>

 &emsp;&emsp;  **欢迎大家关注,如果觉得文章对你有帮助,不要忘记一键三连哦,你的支持是我创作更加优质文章的动力,有任何问题可以私信我,看到会及时给你答复！**。
