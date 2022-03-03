

# 前言

- 大家好,我是小诚,之前说到,最近在准备《从0到1-全面深刻理解MySQL系列》文章,前两天已经将Window和Linux环境的安装流程出了具体的教程,但是最近收到一些小伙伴的反馈,说忘记了MySQL的登录密码导致无法连接数据库,**考虑再三,既然决定写从0到1的数据库教学文章,就要将各种情况都考虑周全,所以本次准备出一片关于忘记MySQL登录密码时如何处理的教程。**<br>


- 最近时常会收到一些小伙伴反馈的问题,为了方便交流,所以创建了**交流群,感兴趣的可查看主页进入,欢迎大家一起交流,吹水,摸鱼,进步。**
<br>


- **注: 该文章适对Windows和Linux系统都适用哦**



<br>


# 1、以跳过权限表的方式启动MySQL服务,进行密码修改

<br>

&emsp;&emsp;**(1)、如果之前有启动过MySQL服务,则通过:net stop mysql命令(Windows系统)或者systemctl stop mysql命令(Linux环境)先停止服务。**
<br>

<center><img src="https://img-blog.csdnimg.cn/20210709212004748.png" /></center>


<br>

&emsp;&emsp;**(2)、根据不同的版本适用以下的语句跳过权限表启动MySQL服务(要以管理员的身份打开命名控制台哦)。**
<br>


```csharp
// --skip-grant-tables 的意思是启动MySQL服务的时候跳过权限表认证

mysql8之前使用: mysqld --skip-grant-tables 回车。

mysql8使用: mysqld --console --skip-grant-tables --shared-memory 
```
<br>

<center><img src="https://img-blog.csdnimg.cn/20210709213125401.png" /></center>


<br>

&emsp;&emsp;**(3)、当前窗口不关闭,重新使用管理员身份打开一个控制台,使用: <font color=red>mysql -u root -p命令连接到服务器</font>,此时不用输入密码,直接回传即可,然后使用: use mysql命令切换到mysql数据库**
<br>

<center><img src="https://img-blog.csdnimg.cn/20210709215918185.png" /></center>


<br>

&emsp;&emsp;**(4)、执行修改用户密码操作, 注意,此处不同的MySQL版本有不同的SQL。**
<br>

&emsp;&emsp;1、mysql 5.7之后的密码字段改成了authentication_string，如果是5.7之前的,则修改为password,具体执行的SQL区分如下:
<br>

```csharp
// 5.7之前的版本的修改密码方式
UPDATE USER SET PASSWORD=PASSWORD('你的密码') WHERE USER='root';

// 5.7之后版本的修改密码方式
// 格式: alter 表名 用户名@user表中用户名对应的Host字段值 IDENTIFIED WITH 指定使用哪种加密技术 BY ‘修改后的密码’
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY '密码';

// 修改完密码后需要执行下面的语句刷新权限
flush privileges;

// 然后重新关闭另外的窗口

```
<br>

&emsp;&emsp;2、MySQL88之前用户的加密方式是使用mysql_native_password的方式,在数据库中看到的密码是明文不安全,**所以在MySQL8的时候将密码的加密方式修改为:caching_sha2_password，在数据库查看只能看到密文(这也是很多人版本是8.0使用了update语句修改成功后却登录不进去的原因** ,mysql8及之后登录的方式应该使用下面的语句)

<br>

<center><img src="https://img-blog.csdnimg.cn/20210709220704530.png" /></center>

<br>

<center><img src="https://img-blog.csdnimg.cn/2021070922092477.png" /></center>
<br>

&emsp;&emsp;**(5)、关闭另外打开的窗口,然后重新启动MySQL服务,正常使用修改后的密码连接即可。**

# 2、小结

<br>

&emsp;&emsp; 不积跬步，无以至千里;不积小流，无以成江海。今天播种努力的种子,总会有一天发芽!
<br>
