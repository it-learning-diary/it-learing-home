&emsp;&emsp;光阴似箭，不知不觉春节将至，你准备好抢票了吗？每年的抢票大战都让人精神疲惫，手速不够只能求助黄牛。作为一名技术人员，我们也许能有更多、更好的方式去抢到票，今天博主就给大家安利一个Github上免费开源的抢票软件，助力大家春节归途！

&emsp;&emsp;废话不多说，先给大家看抢票结果(演示)：

![image-20211225201121653](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174247.png)


&emsp;&emsp;我们到Github上面输入关键词：12306，你会发现有许多抢票相关的免费开源软件，但是最著名的就是下面这两个。

![image-20211225200713761](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174255.png)




&emsp;&emsp;**有朋友会疑问，为什么不介绍第一个开源项目？它排名靠前不是更好？原因主要如下：**

&emsp;&emsp;1、第一个项目master分支最后一次提交的时间是今年的1月份，博主搭建后发现并不能正确运行起来，无法达到抢票的目的，虽然也尝试与该项目的开发者沟通(提了issue)，但是并未收到回复，因此只能暂时放弃。

&emsp;&emsp;2、第二个项目master分支最近一次代码提交是今年10月份，它是借鉴了第一个项目的一些思想，**但在此基础上提供了更丰富的功能如集群，多账户，多任务、图形化界面等，最重要的是，经过搭建运行，它是真实可以抢到票。**

![image-20211225202943327](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174304.png)




### 项目搭建

&emsp;&emsp;对项目进行一些简单的介绍后，下面正式进行项目的搭建(**博主尽可能将搭建过程细致，如有遇到新问题也可以私信博主帮忙定位**)。

&emsp;&emsp;**项目地址：** https://github.com/pjialin/py12306/

&emsp;&emsp;**部署要求：** 项目需要运行在python3.6以上版本

&emsp;&emsp;**部署环境和技术：** 京东云服务器、Python、Docker(可选)、Docker-Compose(可选)、Redis(可选)

&emsp;&emsp;**说明：**

&emsp;&emsp;1、按照博主教程，整个项目从搭建到运行大概需要半小时左右。

&emsp;&emsp;**2、为了避免有些朋友因为Github网络问题没办法直接拉取项目，博主将本次搭建项目的所需的文件都整理了一份，有需要的点击此处获取：** 项目资料。

&emsp;&emsp;3、建议抢票程序部署在自己的服务器上，不要在公司电脑或者公司服务器上运行该程序，因为该程序可能会被12306限制ip(一段时间内会自动恢复，不需要恐慌)，这样公司的网络可能一段时间内会无法访问到12306(一般是1个小时左右)，可能会导致其他人无法购票(不要做损害他人的事情)。

#### 1、安装python环境

&emsp;&emsp;一般情况下，服务器会默认带有python，但是版本都是比较低，要运行这个项目，需要安装高版本的python(3.6以上)，所以需要下检查服务器中已经存在的python版本，检查当前系统中的python版本命令：ll /usr/bin/python\*。如果已经有python3.6以上的版本，则无需重新安装，直接使用即可，如果没有，则可以使用下面的教程安装。

![image-20211225215632012](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174320.png)

&emsp;&emsp;python安装教程：https://www.cnblogs.com/simuhunluo/p/7704765.html

### 2、搭建流程



#### 步骤一：克隆项目到服务器



&emsp;&emsp;命令：git clone https://github.com/pjialin/py12306/

&emsp;&emsp;如果出现：git command not found异常，则先执行命令：yum install -y git，安装git组件。

![image-20211225220741245](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174328.png)

![image-20211225220911757](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174337.png)



#### 步骤二：安装项目所需依赖



&emsp;&emsp;先切换到项目目录下，再执行命令：pip install -r requirements.txt

![image-20211225221258640](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174344.png)



#### 步骤三：复制配置文件并修改



&emsp;&emsp;在项目的根目录下执行命令：cp env.py.example env.py。配置相应的信息如抢票人名字、账号密码，始发站等，

&emsp;&emsp;**良心推荐：** 因为需要配置的东西比较多，使用vi/vim命令配置可能不是很方便，**可以使用nodepad++软件连接到服务器，这样我们就可以直接在Window环境下编辑Linux系统的配置文件。**

&emsp;&emsp;Notepad++连接linux服务器教程：https://www.cnblogs.com/licm/p/12664731.html

![image-20211225222616456](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174352.png)



#### 步骤四：配置文件详解



&emsp;&emsp;**1、指定账号、密码以及登录方式**

&emsp;&emsp;登录方式默认使用扫码登录，直接使用密码登录的话程序会出现异常，这个应该是github登录接口有了变动，程序还没有同步更新。

![image-20211225223528804](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174401.png)



&emsp;&emsp;**2、打码平台配置**



![image-20211225224228780](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174408.png)



&emsp;&emsp;**3、接收通知配置**



&emsp;&emsp;py12306项目现在支持语音验证码、钉钉、Telegram、微信消息、Bark、以及邮箱等方式消息推送，**一般情况下，我们使用邮箱方式即可，其他的配置则保持默认。**

![image-20211225224706059](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174415.png)

&emsp;&emsp;**注意，如果要使用邮箱接收通知，需要登录到邮箱开启smtp协议，开启教程如下：**



&emsp;&emsp;**4、分布式集群配置**



![image-20211225230845731](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174423.png)




&emsp;&emsp;**5、web界面配置**

&emsp;&emsp;一般默认即可，运行程序后我们可以使用浏览器通过ip:8008访问到程序的界面，查看抢票情况(**注意需要在防火墙中放行8008端口**)

![image-20211225225218091](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174430.png)



&emsp;&emsp;**6、默认登录设置**

&emsp;&emsp;如果你不想使用第一步描述的每次都扫描登录，可以手动登录电脑端12306官网，然后看任意一个接口中的cookie值，将他们复制到此处并开启即可(**程序会根据这个值自动登录**)。

![image-20211225232339589](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174437.png)



&emsp;&emsp;获取RAIL_EXPIRATION和RAIL_DEVICEID两个字段对应的值，登录到网页版12306，使用F12打开控制台，然后在Network中查看请求任意12306接口携带的cookie值。

![image-20211225232914552](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174445.png)



&emsp;&emsp;**7、配置购票信息**

&emsp;&emsp;根据自己实际情况进行配置即可，每个属性都有相应的说明。

![image-20211225233002435](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174453.png)



#### 步骤五：启动前测试



&emsp;&emsp;目前程序提供了一些简单的测试，包括用户账号检测，乘客信息检测，车站检测等。

&emsp;&emsp;**开始测试：python main.py -t -n**

![image-20211225234322989](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174501.png)



#### 步骤六：启动程序



&emsp;&emsp;**方式1(python)：** 在py12306根目录下执行命令：python main.py

![image-20211225235153145](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174508.png)



&emsp;&emsp;**方式2(docker)：** 需要安装docker环境，然后执行下面的命令:

&emsp;&emsp;命令1、下载配置文件到本地：curl https://raw.githubusercontent.com/pjialin/py12306/master/env.docker.py.example -o env.py

&emsp;&emsp;命令2、使用docker运行程序：docker run --rm --name py12306 -p 8008:8008 -d -v $(pwd):/config -v py12306:/data pjialin/py12306



&emsp;&emsp;**方式3(docker-compose)：** docker-compose方式需要依赖docker，因此在启动前需要先启动docker服务(systemctl start docker)，然后执行下面的命令：

&emsp;&emsp;命令1、复制执行配置文件：cp docker-compose.yml.example docker-compose.yml

&emsp;&emsp;命令2、运行程序：docker-compose up -d



#### 步骤七：进入web页面，查看抢票情况



&emsp;&emsp;在浏览器输入：服务器ip:8008(需要在防火墙中开放8008端口),**抢票成功后会推送消息到你之前配置的邮箱或者其他配置好的通知方式中，大功告成！**

![image-20211225235247469](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174517.png)



![image-20211225201121653](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174524.png)

#### 搭建方式推荐



&emsp;&emsp;**看完教程，大家应该已经跃跃欲试了，在此处，博主建议将项目搭建在Linux环境下，理由如下：**

&emsp;&emsp;1、搭建在Linux中，可以一天24小时运行，无需担心网络或者电脑因为异常情况而导致抢票终止。

&emsp;&emsp;2、运行在Linux上，可以任意时刻、地点查看抢票情况，排查ip限制问题，简单方便。



&emsp;&emsp;**有些朋友可能会疑问，现在购买服务器的费用会不会很贵？叫黄牛不是更快？那下面我们来简单对比两者之间的差别。**

&emsp;&emsp;1、现在市场上让黄牛帮抢票，一张票价钱大概100-150之间，有些甚至还根据始发站不同价格上涨，如果还需要帮家人购买的话，来回一趟至少要500~600左右，**况且黄牛也不能够保证百分百抢到票，它们的抢票机制本质和我们自己搭建的这个程序一样，只不过内部可能有多套系统。**

&emsp;&emsp;2、再来看看2022年后面的假期，除了春节还有清明节、劳动节、端午节、中秋节、国庆节等，如果我们搭建了自己的一套系统，那么后续只需要修改一下抢票时间和始发站，程序就可以自动我们抢票，无需再借助他人。

&emsp;&emsp;3、其实购买一个服务器并不像想象的那么贵，只需要一瓶水的价格就够了，**所以相比之下，自己搭建一个程序抢票比叫黄牛抢票的花费要低得多。**

#### 服务器推荐

&emsp;&emsp;有朋友会疑问，服务器真的这么便宜了？没错，大家正好赶上好时机了，因为之前购买的服务器准备过期了，所以博主最近一直在各大云厂商来回穿梭，寻找“薅羊毛”的机会，**皇天不负有心人，终于被博主发现了一个大力搞活动的云厂商-京东云，服务器2核4G只需1块钱，所以赶紧和小伙伴们分享。**

![image-20211226122243533](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174532.png)



&emsp;&emsp;当然，在购买服务器的时候除了价格外，还需要考虑到我们的实际的运用场景，**我们现在是用来搭建抢票服务，所以性能、服务、安全方面就要求比较高，下面就是博主对比后发现京东云服务器的一些特点。**

&emsp;&emsp;**1、活动服务器是100%cpu独享**：和其他厂商的一些虚拟主机中共享cpu不一样，所以它处理任务的速度会更快，**我们抢到票的机率也会更大。**

&emsp;&emsp;**2、提供星盾-体验版**：优惠低至1元/1年，支持一键接入，全站安全，攻击防御、证书免费，安全性有保障。

&emsp;&emsp;**3、提供非常优质有保障的售后服务**：7×24小时售后支持、售前1v1服务、免费备案服务。这一点也很重要，一旦服务器出现一些解决不了的问题，可以随时请求售后接入，博主之前就遇到过一次ssh无论如何都连接不到服务器，后来请求售后帮助，当时已经是晚上12点了，售后很快定位出是因为升级了openssl导致的，帮博主修复了问题，效率和态度都非常不错。

&emsp;&emsp;**4、支持1元秒杀、1折续费，提交使用体验评价还可以享受低至0.8折扣的续费优惠。** 如果抢票完成后想要继续使用服务器，可以低价续费，性价比非常高。同时云主机及相关计算资源均支持包年包月或按配置计费，按需购买，随时调整。

&emsp;&emsp;**5、对个人和企业支持力度非常大，品质值得信赖**，引入京东云案例遍布各大行业如政府、金融、零售、交通物流等等，**如果有小伙伴公司准备上云，京东云是一个非常不错的选择。**

![image-20211226124737401](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174543.png)



![image-20211226125125727](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326174551.png)

#### 博主支持

&emsp;&emsp;**1、搭建技术支持：** 博主知道，可能有些小伙伴就算完整跟着教程搭建中间也会遇到一些奇奇怪怪的问题，有些网上并不是很好找到答案，所以博主提供了友情帮助，**如果小伙伴在搭建中出现了不可解决的问题，可以私信博主协助解决。**

&emsp;&emsp;**2、赠送服务器：** 这一年从几十粉到2w+粉，博主收获了很多东西，都不开小伙伴们的支持，所以，年底了博主也给粉丝反馈一波福利，**只要是通过下面链接购买秒杀服务器(个人版首购：2核4G云主机)，前50名小伙伴免单，可以通过购买截图找博主返现。**

&emsp;&emsp;**购买链接：** https://www.jdcloud.com/cn/activity/20211111?utm_source=DMT_CSDN13&utm_medium=banner&utm_campaign=20211212&utm_term=NA

&emsp;&emsp;最后，祝福每一位在外的游子都能够买到回家的车票，和家人团聚！**如果文章有帮助，可以给作者Star，让博主有动力创作更加优质的文章。**

