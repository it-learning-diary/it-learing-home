&emsp;&emsp;眼见为实，前面的文章洋洋洒洒几千字介绍了JAVA的历史和学习方法，但是都只是在字面上，我们也没有看到一个真正的JAVA程序运行需要经过哪些步骤，流程是否复杂?

&emsp;&emsp;本篇文章则通过Hello World程序带大家真正走进JAVA的世界，看看如何完成它吧。

&emsp;&emsp;工欲善其事必先利其器，既然JAVA要喊出了“Write Once，Run Anywhere”的口号，那肯定有自己独特的一套工具，让JAVA代码一次编译、处处运行，这套工具也叫JAVA运行环境或JAVA程序平台，到目前为止，这个平台主要分为JAVA SE、JAVA EE、JAVA ME三个版本。

![image-20211206230951548](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211206230951548.png)


### JAVA三大平台介绍

#### 1、JAVA SE

&emsp;&emsp;全称为JAVA Standard Edition(也曾简称为J2SE)JAVA标准版或JAVA标准平台，是JAVA技术的核心和基础，同时也是JAVA ME和JAVA EE的基础。它提供了标准的JAVA开发工具包(JDK)，通过它能够实现桌面应用程序、低端服务器和JAVA Applet程序等功能的开发，目前该平台官方已经更新到JDK17版本。

![image-20211206231723268](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211206231723268.png)


#### 2、JAVA EE

&emsp;&emsp;全称为Java  Enterprise Edition(也曾称为J2EE)JAVA企业版或JAVA 企业平台，通过它能够构建企业级服务应用。实际上，JAVA EE包含了JAVA SE，并在这个基础上添加了许多功能强大的类库，用于支持企业级别的业务开发如目录管理、消息管理等，目前该平台官网最新版本为JAVA EE8。

![image-20211206232237525](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211206232237525.png)


#### 3、JAVA ME

&emsp;&emsp;全称JAVA Micro Edition(也曾称J2ME)JAVA微型版或JAVA小型平台，与JAVA EE主要构建企业级应用相反，JAVA ME是一种很小的JAVA运行环境，它主要是应用在嵌入式的产品中，如移动电话，掌上电脑、电视机机顶盒等，使得编译好的JAVA程序能够在上面执行，目前该平台官网最新版本为JAVA ME8。

![image-20211206234702456](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211206234702456.png)


### 选择哪个平台学习

&emsp;&emsp;**三大平台各有特点，但是共同点都是平台内部包含了JAVA 虚拟机**，编译好的程序在平台执行的流程大致如下：虚拟机将编译好的字节码文件加载到内存，然后采用解释执行的方式执行字节码(所谓解释执行即：平台根据响应系统将字节码解释一条，执行一条，就跟初学英语时一样，老师每讲一句英语都会给你解释这句话的含义)。

&emsp;&emsp;既然三大平台处理的方式都是大同小异，那初学者该选择哪个平台学习更好呢？**根据官方书籍和无数实践证明，初学者最好先学习JAVA SE，选用它提供的软件开发工具包-JDK，它是学习和掌握JAVA知识的最佳平台，也是学习JAVA EE和JAVA ME的基础。**

![image-20211207000058379](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211207000058379.png)


### 搭建JAVA SE平台

&emsp;&emsp;**1、到官网下载JAVA SE平台**

&emsp;&emsp;说明：从搭建开始提到的JDK指的是OpenJDK而不是Oracle JDK，至于为什么不用Oracle JDK主要原因是因为Oracle(甲骨文)公司在19年宣布后续停止免费对JAVA SE8版本进行更新，商用的话需要进行购买，而Open JDK作为Oracle JDK的免费开源版本，更适合个人开发者使用(至于Oracle JDK和Open JDK之间的事，要扯起来能说半天，有时间再单独开一篇文章谈谈)

&emsp;&emsp;**下载地址：**http://jdk.java.net/

&emsp;&emsp;注：本文演示使用JDK11版本(推荐大家学习时尽量使用JDK8或者JDK11版本，因为这两个版本是长期维护的版本，更加稳定，但因为OpenJDK下载中windows环境下只有32位的，所以演示就使用JDK11版本)

![image-20211208223233475](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211208223233475.png)

![image-20211208223338828](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211208223338828.png)


&emsp;&emsp;**2、将下载好的压缩包解压，可以查看到下面的目录(注意：OpenJDK压缩包是绿色软件，既不需要安装即可直接使用)**

![image-20211208225136354](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211208225136354.png)


&emsp;&emsp;**3、配置环境变量：** 虽然解压后我们能够到指定的文件夹去执行对应的java.exe文件，但是，如果是想在电脑的任意一个目录下都可以调用刚刚解压的java jdk，那就需要将jdk的执行路径配置到系统的环境变量中。

&emsp;&emsp;配置环境变量可以通俗理解为身份证上的家庭住址，只要身份证上标记了这个地址，以后别人想要找你的时候，都可以直接通过这个地址去到你家找你。**程序配置了系统环境变量，以后在执行关于jdk的相关命令时，会现根据配置的地址去找到对应的执行程序，然后执行对应的命令，这样就不会出现"既不是内部或外部命令，也不是可执行程序.."的错误。**

&emsp;&emsp;步骤一：此电脑 = > 属性 = 》高级系统设置

![image-20211208230701791](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211208230701791.png)

&emsp;&emsp;步骤二： 环境变量 

![image-20211208231118507](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211208231118507.png)

&emsp;&emsp;步骤三：新增JAVA_HOME环境变量,值指向刚刚压缩好的openjdk目录

![image-20211208232527798](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211208232527798.png)

&emsp;&emsp;步骤四：在PATH变量中引入JAVA_HOME变量,并指向bin目录，格式：%变量名%\bin

![image-20211208233005762](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211208233005762.png)


&emsp;&emsp;步骤五：打开命令控制台(快捷键: Win键+R，输入cmd回车)，输入:java -version查看安装好的JAVA版本

![image-20211209074857251](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211209074857251.png)

&emsp;&emsp;**看到此处，先要恭喜你终于完成了JAVA SE平台的搭建，从现在开始，我们可以真正进入到JAVA程序的开发了，准备好大展身手了吗? 让我们先来完成第一个程序“Hello World”的搭建吧！**


### 第一个程序的开发

&emsp;&emsp;1、创建一个名为HelloWorld.txt的文本文件，并添加入以下代码：

![image-20211209074348857](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211209074348857.png)

&emsp;&emsp;2、将文本HelloWorld.txt后缀名修改成java，然后在地址栏输入cmd回车进入命令控制台

![image-20211209074529679](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211209074529679.png)


&emsp;&emsp;3、编译、并执行JAVA程序，大功告成

![image-20211209080044854](https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211209080044854.png)


### 小结

&emsp;&emsp;本文主要介绍了关于JAVA的三大平台、如何搭建JAVA SE平台以及搭建第一个JAVA程序，初学者会感觉步骤稍微麻烦，但是这个搭建是永久性的，搭建完之后我们只需要关注JAVA程序编写而无需再关注环境的搭建，所以，之前的步骤都是值得的。

&emsp;&emsp;本文的HelloWorld程序案例的运行后大家会发现存在两个比较大的问题。第一个是虽然程序运行了，但是还是没有了解这个程序是如何被运行的流程？第二个是使用记事本编写程序比较麻烦，有没有更好的方式？

&emsp;&emsp;答案都在下篇文章，敬请期待！如果文章有帮助，请给作者Star。