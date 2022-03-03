



# 🏘️ 一、前言
<br>

- 本篇文章重点介绍：**Window环境搭建FTP服务器，JAVA程序实现FTP服务器文件上传、下载功能。**

<br>



# 🏚️ 二、完成效果
<br>
&emsp;&emsp;**1、上传文件到FTP服务器：**
<br>

![上传文件到FTP服务器](https://img-blog.csdnimg.cn/2be9ed4b5b02420b8ae23a36804cc052.gif#pic_center)
<br>

&emsp;&emsp;**2、从FTP服务器下载文件到本地：**
<br>

![从FTP服务器下载文件到本地](https://img-blog.csdnimg.cn/dd3ab4162c38433c86bcc5ad96151e23.gif#pic_center)


<br>


# 🏠 三、Window系统FTP服务器搭建
<br>

&emsp;&emsp;在搭建FTP服务器之前，先跟大家介绍下什么是FTP服务器，**不然怕有些小伙伴可能只是有个模糊的概念，学习要尽量知其然，知其所以然，不要一知半解，否则自己用着也不踏实。**
<br>

## ♈ 3.1、FTP服务器怎么玩
<br>

&emsp;&emsp;**在介绍前，我们先来看看搭建完FTP服务器后能怎么玩**，不然总是有些小伙伴觉的文章太长看到一半就跑路了，错过就没有了！<br>

&emsp;&emsp;1、上传、下载小视频(共享文件)，自定义权限控制，控制使用用户。<br>

&emsp;&emsp;2、实现某些业务场景下文件存储和文件下载(即文件服务器操作)。<br>

&emsp;&emsp;3、更多玩法等你开发，尽情发挥你的脑洞...


<br>

## ♉ 3.2、FTP(File Transfer Protocol，文件传输协议)
<br>

&emsp;&emsp;先简单认识下FTP协议，FTP即文件传输协议的简称，它是TCP/IP协议簇中的一员，也是Internet上最早使用的协议之一，**通过它可以实现电脑与电脑间对文件的各种操作（如文件的增、删、改、查、传送等），FTP的目标是提高文件的共享性，提供非直接使用远程计算机，实现计算机文件的相互操作，使存储介质对用户透明和可靠高效地传送数据。**<br>

&emsp;&emsp;它是基于C/S(客户端/服务端)模型设计，工作在网络体系结构中的应用层，使用TCP进行传输，保证客户与服务器之间的连接是可靠的。<br>

&emsp;&emsp;**支持的连接方式：** <br>

&emsp;&emsp;FTP支持Standard （PORT方式，主动方式），Passive （PASV，被动方式）两种连接模式，连接的流程大致如下：<br>

&emsp;&emsp;1、FTP客户端发起FTP会话，与FTP服务器建立相应的连接，在会话期间，FTP会建立控制信息进程与数据进程两个连接。<br>

&emsp;&emsp;**2、控制进程连接的用途：** 用于传输FTP内部命令以及命令的响应等控制信息，无法进行数据传输。<br>

&emsp;&emsp;**3、数据进程连接的用途：** 用于客户端与服务端之间数据的传输，它是全双工的，可以支持双向数据传输，当数据传输完成后，它就会撤销然后回到FTP会话状态，直到控制连接进程也取消，退出整个FTP会话。<br>

&emsp;&emsp;**PORT模式：** <br>

&emsp;&emsp;FTP客户端会与服务端的TCP 21端口创建连接(**控制连接**)，用于发送命令，当客户端需要接收数据时，会通过这个连接向服务端发送PORT命名，PORT命令中包含了会使用什么端口来接收服务端传输的数据，此时，服务端会通过TCP 20端口跟FTP客户端创建连接(**数据连接**)完成数据传输。<br>

&emsp;&emsp;**Passive模式：** <br>

&emsp;&emsp;FTP客户端会与服务端的TCP 21端口创建连接(**控制连接**)，用于发送命令，当客户端需要接收数据时，会通过这个连接向服务端发送Pasv命名，**服务器收到Pasv命令后，打开一个临时端口（端口号大于1023小于65535）并且通知客户端在这个端口上传送数据的请求**，客户端连接FTP服务器此端口，然后FTP服务器将通过这个端口传送数据。<br>

&emsp;&emsp;说明：上面FTP协议知识介绍参考百度百科：[FTP协议](https://baike.baidu.com/item/FTP/13839?fr=aladdin)<br>
<br>

## ♊ 3.3、FTP服务器
<br>

&emsp;&emsp;了解了FTP协议，那FTP服务器就很容易理解了。FTP服务器就是支持FTP协议的服务器，我们平常可以在电脑上安装一个FTP工具就可实现与FTP服务器进行文件传输，FTP服务器常见分为：Windows FTP服务器和Linux  FTP服务器。<br>

&emsp;&emsp;**我们自己的电脑也可以当做一个FTP服务器，如Windows系统就可以通过自带的ISS管理器来搭建一个FTP服务器(本文案例就是使用这个)，Linux系统最常用的借助vsftp软件做FTP服务器搭建。**<br>

&emsp;&emsp;**常见的例子：** 在学校里上电脑课或者电脑考试时，老师会将上课题目或者考试题目放在某个文件夹中，让学生访问某个地址如：ftp://ip地址，通过这个地址每位同学看到老师共享的文件，下载的对应的试题完成考试。<br>

&emsp;&emsp;上面例子上过电脑课的同学应该都经历过(多么美好的学生时代)，学生们访问到的其实就是老师搭建好的FTP服务器，老师提前将共享的文件上传到FTP服务器，学生们可以进行下载等操作。<br>

&emsp;&emsp;啰啰嗦嗦了一大堆，下面开始进行FTP服务器搭建和上传下载功能开发吧！

<br>

## ♋ 3.4、FTP服务器搭建
<br>

&emsp;&emsp;**安装环境：** Win10<br/>

&emsp;&emsp;**步骤一：** 安装FTP服务器支持和IIS管理平台。<br/>

&emsp;&emsp;**操作步骤：** 电脑 => 控制面板 => 程序和功能 => 启用和关闭Windows功能 => Internet Infomation Services => 勾选【FTP服务器】和Web管理工具的【IIS管理控制台】=> 点击确定等待安装完成
<br/>

<center><img src="https://img-blog.csdnimg.cn/c290eb6c5d0a4a109968e333fa90fd8d.png" /></center>

<br>

&emsp;&emsp;**步骤二：** 打开IIS管理器<br/>

&emsp;&emsp;**操作步骤：** 电脑 => 控制面板 => 管理工具 => Internet Infomation Services(IIS)管理器
<br/>

<center><img src="https://img-blog.csdnimg.cn/330d711d3982481fb807ef176bc51cb7.png" /></center>

<br/>

&emsp;&emsp;**步骤三：** 创建FTP服务器<br/>

&emsp;&emsp;**操作1：** 在某个盘符如D盘，创建一个FTP共享文件夹，用于FTP共享文件存放地址<br/>

&emsp;&emsp;**步骤2：** 右键IIS管理器左边导航栏 => 添加FTP站点
<br/>

<center><img src="https://img-blog.csdnimg.cn/cfc4ee0ce9804c3d9b808723868c6c12.png" /></center>

<br/>

&emsp;&emsp;**步骤3：** 指定【站点名称】和【FTP共享的文件夹路径】
<br/>

<center><img src="https://img-blog.csdnimg.cn/a9bbc40230294da29daaa6a27f092c93.png" /></center>


<br/>

&emsp;&emsp;**步骤4：** 配置FTP服务器相关信息
<br/>

<center><img src="https://img-blog.csdnimg.cn/bdb2eb463f054d9eb9b746557cefb341.png" /></center>


<br/>

&emsp;&emsp;**步骤5：** 配置FTP服务器验证和权限信息【**注意：如果想通过程序实现上传、下载功能，身份验证中的基本选项需要勾选上，后面程序需要通过这个方式使用账号和密码登录到FTP服务器**】
<br/>

<center><img src="https://img-blog.csdnimg.cn/fbac9cee894b441090e93669b09d93f4.png" /></center>


<br/>

&emsp;&emsp;**步骤5：** 到这一步，一个FTP服务器就已经搭建完成了，在IIS管理器还可以对搭建好的FTP服务器进行配置管理。
<br/>

<center><img src="https://img-blog.csdnimg.cn/5b7a2219c074408f926ff5e18ec1c4ec.png" /></center>


<br/>

&emsp;&emsp;**步骤6：** 在同一网段的小伙伴可以通过：**ftp://ftp配置的ip地址** 格式访问到FTP服务器。
<br/>

<center><img src="https://img-blog.csdnimg.cn/8a43acede186470dbd3010a6193e260c.png" /></center>


<br>

## ♌ 3.5、FTP服务器搭建出现的问题
<br>

&emsp;&emsp;**问题一：** FTP按照流程搭建完成后，在同一网段的小伙伴却无法访问！<br>

&emsp;&emsp;**原因：** 可能是开启了防火墙拦截，需要在防火墙放行FTP服务器。<br>

&emsp;&emsp;**解决：** 电脑 => 控制面板 => Windows Defender 防火墙 => 允许应用通过Windows Defender 防火墙进行通信 => 勾选【FTP服务器】
<br>

<center><img src="https://img-blog.csdnimg.cn/7d0542d5039e4315a7a7a83d47d298bf.png" /></center>


<br>



# 🏡 四、通过程序实现FTP文件的上传和下载
<br>


&emsp;&emsp;通过上面的步骤，我们完成了FTP服务器的搭建，可以手动将文件上传到服务器，让在同一网段的小伙伴自由从上面下载，**但是，在实际业务开发中，我们需要的是通过程序实现上传和下载，而不是通过人为手动的方式，下面，就来看看如何实现吧。**<br>

&emsp;&emsp;**说明：** 文章只贴出部分代码，全部案例代码已经上传到Gitee，需要者可直接访问下载(**有帮助记得给个star呀**)：【[实战-FTP服务器搭建，实现上传、下载](https://gitee.com/it-learning-diary/ftp-server-manager)】

<br>

## 💗 4.1、项目结构
<br>

<center><img src="https://img-blog.csdnimg.cn/a073a80560ac45d0aa43d08f77e1d3bd.png" /></center>


<br>

## 💙 4.2、实现技术
<br>

&emsp;&emsp;**1、Apache下的commons-net依赖包：** 它包含了一组网络实用工具和协议实现，支持的协议包括:FTP、NNTP、NTP、POP3(S)、SMTP(S)、Telnet、Whois等等，**可以用它来实现文件的上传和下载功能。**<br>

&emsp;&emsp;**2、spring-boot-starter-web：** web相关支持<br>

&emsp;&emsp;**3、SpringBoot依赖：** 快速构建JAVA项目
<br>

```java
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        

        <dependency>
            <groupId>commons-net</groupId>
            <artifactId>commons-net</artifactId>
            <version>3.6</version>
        </dependency>
        
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
```

<br>

## 💚 4.2、相关配置
<br>

```java
ftp:
  client:
    # ftp客户端文件使用的字符集
    charset: GBK
  server:
    # ftp服务器绑定ip或者域名
    hostname: 127.0.0.1
    # 端口
    port: 21
    # 连接ftp服务器的用户名
    username: user
    # 密码
    password: 123456
    # ftp的共享文件路径
    workingPath: D:/share/FTPServer
    # ftp服务器文件使用的字符集(用于上传包含中文名的文件和下载包含中文名的文件 - 很重要)
    charset: ISO-8859-1
```


<br>

## 💛 4.3、核心代码
<br>

&emsp;&emsp;因为FTP服务器的上传、下载都是很通用的功能，**所以博主封装成了一个工具类，有需要的小伙伴可以引入依赖和相关配置后，直接就可以使用该工具类。**
<br>

&emsp;&emsp;**1、上传核心代码：**
<br>

```java
/**
     * 上传
     *
     * @return
     */
    public boolean upload(FtpUploadParam param) {
        boolean flag = false;
        FTPClient ftpClient = new FTPClient();
        //1 测试连接
        if (connect(ftpClient, param.getHostname(), param.getPort(), param.getUsername(), param.getPassword())) {
            try {
                //2 检查工作目录是否存在，不存在则创建
                if (!ftpClient.changeWorkingDirectory(param.getWorkingPath())) {
                    ftpClient.makeDirectory(param.getWorkingPath());
                }
                // 将文件编码成Ftp服务器支持的编码类型（FTP协议里面，规定文件名编码为iso-8859-1，所以目录名或文件名需要转码。）
                String fileName = new String(param.getSaveName().getBytes(ftpClientCharset), ftpServerCharset);
                // 3 上传文件
                if (ftpClient.storeFile(fileName, param.getInputStream())) {
                    flag = true;
                } else {
                    log.warn("FtpUtils uploadFile unsuccessfully!!");
                }
            } catch (IOException e) {
                log.error("FtpUtils upload in error:{}", e);
            } finally {
                disconnect(ftpClient);
            }
        }
        return flag;
    }

```

<br>

&emsp;&emsp;**2、下载核心代码：**
<br>

```java
public boolean download(FtpDownloadParam param, String downloadFileName) {
        FTPClient ftpClient = new FTPClient();
        FileOutputStream out = null;
        //1 测试连接
        if (connect(ftpClient, param.getHostname(), param.getPort(), param.getUsername(), param.getPassword())) {
            try {
                File file;
                String localPath = param.getDownloadPath() + param.getFileName();
                out = new FileOutputStream(new File(localPath));
                //2 检查工作目录是否存在，不存在返回
//                if (!ftpClient.changeWorkingDirectory(param.getWorkingPath())) {
//                    return false;
//                }
                /*
                 * 打开FTP服务器的PASS模式(不记得FTP协议支持的模式请翻到文章第一阶段)
                 * 这个方法的意思就是每次数据连接之前，ftp client告诉ftp server开通一个端口来传输数据. 因为ftp
                 * server可能每次开启不同的端口来传输数据，但是在linux上，由于安全限制，可能某些端口没有开启，可能出现出现阻塞
                 */
                ftpClient.enterLocalPassiveMode();
                // 设置文件的传输方式
                ftpClient.setFileType(FTP.BINARY_FILE_TYPE);
                // 将文件编码成Ftp服务器支持的编码类型（FTP协议里面，规定文件名编码为iso-8859-1，所以目录名或文件名需要转码。）
                // 缺少编码转换会导致：从FTP服务器下载下来的文件是破损的，无法被打开
                boolean b = ftpClient.retrieveFile(new String(downloadFileName
                        .getBytes(clientCharset), serverCharset), out);
                out.flush();
            } catch (IOException e) {
                log.error("FtpUtils upload in error:{}", e);
                return false;
            } finally {
                try{
                    if(Objects.nonNull(out)){
                        out.close();
                    }
                }catch (Exception e){
                    log.error("FtpUtils upload in error:{}", e);
                }
                disconnect(ftpClient);
            }
        }
        return true;
    }

```
<br>

## 💜 4.4、执行结果
<br>

&emsp;&emsp;**1、演示代码：**
<br>

<center><img src="https://img-blog.csdnimg.cn/b8ebb79ea17d488cad35e51cbf445928.png" /></center>


<br>

&emsp;&emsp;**2、上传文件到FTP服务器：**
<br>

![上传文件到FTP服务器](https://img-blog.csdnimg.cn/2be9ed4b5b02420b8ae23a36804cc052.gif#pic_center)
<br>

&emsp;&emsp;**3、从FTP服务器下载文件到本地：**
<br>

![从FTP服务器下载文件到本地](https://img-blog.csdnimg.cn/dd3ab4162c38433c86bcc5ad96151e23.gif#pic_center)

<br>

## 💝 4.5、开发过程中遇到的坑
<br>

&emsp;&emsp;**问题1、连接FTP服务器失败：** <br>

&emsp;&emsp;**问题描述：** 在配置文件中指定了账号和密码，但是却连接失败。<br>

&emsp;&emsp;**解决方案：** 经过排查，发现是在搭建FTP服务器的时候只开启了匿名验证，没有开启基本验证(账号和密码登录的方式)，只需要到IIS管理器中开启【基本验证】即可。<br>

&emsp;&emsp;**解决步骤：** 电脑 => 控制面板 => 管理工具 => IIS管理器 => 搭建好的FTP服务器 => FTP身份验证 => 开启基本身份验证模式。
<br>

<center><img src="https://img-blog.csdnimg.cn/de8bf32be1704d3f9b11d8b0b5ba85a1.png" /></center>


<br>

&emsp;&emsp;**问题2、FTP上传中文文件失败：** <br>

&emsp;&emsp;**问题描述：** 选择文件名为英文的文件上传正常，但是选择中文的文件名上传却失败，错误信息：550-The filename, directory name, or volume label syntax is incorrect. 。<br>

&emsp;&emsp;**解决方案：** 经过排查，发现 **FTP协议里面，规定文件名编码为iso-8859-1(注意：这个现在是在Windows搭建的FTP服务器出现的情况，如果是Linux环境的话，还需要查看linux默认的支持编码而定，但是需要将上传的文件名编码这个步骤是确定的)，所以目录名或文件名需要转码。** 所以在上传文件代码处你会看到下面的对文件解码再编码的代码：
<br>

<center><img src="https://img-blog.csdnimg.cn/b290cf1d6f0042e19f20d355d146ed56.png" /></center>


<br>

&emsp;&emsp;**问题3、调用FTPClient的切换目录方法changeWorkingDirectory总是失败** <br>

&emsp;&emsp;**原因和解决：** FTP服务器搭建的时候需要我们制定共享的一个文件路径，当我们和FTP服务器建立连接后，默认就在这个目录下了，如果想切换到该目录下的子目录，不需要写全路径。 <br>

&emsp;&emsp;**示例：** 如果FTP服务器共享的文件夹路径为：D:/ftpserver，此时我们需要切换到ftpserver文件夹下的子文件demo中，**正确的写法：fTPClient.changeWorkingDirectory("demo") ** 而不是 fTPClient.changeWorkingDirectory("D:/ftpserver/demo")
<br>

<center><img src="https://img-blog.csdnimg.cn/f623d48a88c84042aa11754c6bea096b.png" /></center>

<br>

&emsp;&emsp;**问题4、从FTP服务器下载的文件破损，无法打开** <br>

&emsp;&emsp;**问题描述：** 尝试从FTP服务器下载有中文字符文件名的文件，成功下载到本地后却无法正常打开，提示已经破损。<br>

&emsp;&emsp;**问题原因：** 原因其实和第二个问题一样，是因为包含中文字符的文件名下载时需要进行编码转换，否则下载后无法被打开。
<br>

<center><img src="https://img-blog.csdnimg.cn/86188876e41a44ca81d4054c1ec95e62.png" /></center>


<br>


# 🚀 六、写在最后
<br>

&emsp;&emsp;**FTP服务器实战项目所有代码都已上传到Gitee，有需要可以自取(后面会传到CSDN免费下载)**，**如果有帮助不要忘了star哦**，Gitee项目直通车如下：【[实战-FTP服务器搭建，实现上传、下载](https://gitee.com/it-learning-diary/ftp-server-manager)】<br>
