


# 💘 一、前言

- 大家好,我是小诚,又到了愉快的学习时间，上一周因为小伙伴投稿，所以写了：《[什么是面向接口编程](https://blog.csdn.net/qq_40891009/article/details/119581757)》，文章颇受大家欢迎，于是又有小伙伴建议介绍关于：面向切面编程的知识点，于是就有了本篇文章。


- 如面试中遇到一些奇怪或者比较新颖的题目，欢迎私信投稿，感谢阅读(私信我即可投稿)!

![在这里插入图片描述](https://img-blog.csdnimg.cn/f2e843e48ddc4f36bc13e4e1bad2046c.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODkxMDA5,size_16,color_FFFFFF,t_70#pic_center)
<br>



# 💔 二、专栏推荐
<br>

&emsp;&emsp;JAVA和MySQL技术专栏还在免费分享哦，大家可以帮忙点点订阅哦！<br>

&emsp;&emsp;[《JAVA知识大全》](https://blog.csdn.net/qq_40891009/category_9412713.html)<br>

&emsp;&emsp;[《从0到1-全面深刻理解MySQL系列》](https://blog.csdn.net/qq_40891009/category_11189442.html)

<br>



# 💕 三、初次见面-面向切面编程
<br>

&emsp;&emsp;前一篇文章我们介绍了[什么是面向接口编程](https://blog.csdn.net/qq_40891009/article/details/119581757)，现在又来一个面向切面编程，两者到底存在什么联系呢? 下面慢慢道来！<br>

&emsp;&emsp;AOP(Aspect-Oriented Programming的简称)，也就是面向切面编程的意思，**它是一种编程思想**，在Spring的官方文档中描述: **面向切面编程(AOP)提供了另一种思考程序结构的方式来对面向对象编程(OOP)的进行补充和完善，面向对象编程(OOP)中关键的是对象，而面向切面编程(AOP)中关键的是切面。**<br>

&emsp;&emsp;AOP中切面可以实现关注点的模块化(即统一抽取，提高复用)，例如跨越多种类型和对象的事务管理，这种关注点在 AOP 文献中通常被称为横切关注点。<br>

![请添加图片描述](https://img-blog.csdnimg.cn/aac07be603ea48dab55a7d0111e2c0d5.png)
<br>

&emsp;&emsp;单单看文字描述可能比较抽象，下面我们通过具体例子结合图片来形象化这些概念。<br>

&emsp;&emsp;**例子： 洗澡(声明，以下步骤是个人构想，不代表大家，如果觉的我的设想不够丰富的，欢迎评论留言，送你上热搜**)<br>

&emsp;&emsp;**步骤(男)：** 脱衣服、唱歌、洗脸、洗头、洗身体、擦干身体、穿衣服<br>

&emsp;&emsp;**步骤(女)：** 脱衣服、洗脸、洗头、护发、洗身体、擦干身体、护肤、穿衣服<br>

&emsp;&emsp;**发现问题：** 通过上面的例子，我们会发现无论男女，**脱衣服、穿衣服是洗澡不可缺少的步骤，而且这两个步骤在“洗澡”这个业务不是核心，它只是一个关注点**，因为脱衣服和穿衣服的场景并不只是在洗澡中存在(还有什么场景自己联想)，就比如: 天气热我们需要脱衣服，天气冷我们需要穿衣服，**所以，将衣服的管理定义成一个模块，然后在需要的地方调用才是是一个更加合理的设计，具体如下图：**<br>

![请添加图片描述](https://img-blog.csdnimg.cn/e64ae5d6d4614e84a537c5fe2920a5d6.png)
<br>

&emsp;&emsp;**小结**<br>

&emsp;&emsp;通过上一篇文章我们能够知道，**面向对象编程(OOP)的出现让开发者能够实现纵向的业务逻辑处理，但面向对象编程(OOP)并不适合用于定义横向业务逻辑的关系，这样的设计会导致系统出现大量重复代码，复用性极差**，如最常用的日志以及事务功能，它们都可能是横向的分布在不通的业务层级(对象层级)中，但是又和具体的核心业务无直接关系，诸如这样类型的代码，在程序中被称作横切(cross cutting)，我们应该考虑将这一类代码进行统一管理，提高复用性。<br>

&emsp;&emsp;**面向切面编程(AOP)就是将这类与核心业务无关的，但又影响着多个类的公共行为抽取、封装到一个可重用模块，从而实现代码复用和模块解耦的目的，这种开发思想则被称为面向切面编程。**

<br>


# 💖 四、面向切面编程的作用
<br>

&emsp;&emsp;通过上面例子和图形，大家心里多少对面向切面编程有了初步的了解，那下面就来看看面向切面编程能够给我们什么好处。<br>

## 💐 4.1、降低模块间的耦合度
<br>

&emsp;&emsp;系统实现“高内聚、低耦合”一直是我们程序开发者的追求。**AOP技术将软件系统划分成了核心关注点和横切关注点两部分，业务的核心功能则为核心关注点，与业务无关或者关系不大的则为横切关注点。**<br>

&emsp;&emsp;**横切关注点总是作用于核心关注点周围，且对应的业务含义类似**，系统开发中常见的如：权限认证、事务管理、日志记录(如所有请求接口的入参都需要记录到日志中)等都属于横切关注点。<br>

&emsp;&emsp;AOP技术的出现，将系统的核心关注点和横切关注点分离，**避免了非核心业务耦合在核心业务中，降低了模块间的耦合度，提高了系统的可读性、可操作性和可维护性。**
<br>

## 🌸 4.2、代码复用
<br>

&emsp;&emsp;**AOP技术将与核心业务无关或者关系不大却为不同业务模块公用或者需要的逻辑抽取成新的模块，在需要的地方再引入，大大减少的系统的重复代码。**(在这里有个问题需要大家思考: 要实现代码的复用，直接通过继承或者抽取公共类不就行了，为什么还要引入AOP这种技术?)


<br>



# 💗 五、面向切面编程的实现分类
<br>

&emsp;&emsp;**面向切面编程(AOP)实现的效果就是在不修改源代码的情况下，给系统中的某些组件添加某些与核心业务无关的通用逻辑**，AOP框架本质就是对目标对象源代码修改，生成具有新功能的代理对象，**根据对目标对象源代码的修改时机，可以将AOP框架划分为以下两大类：**<br>

## 🌹 5.1、静态AOP实现
<br>

&emsp;&emsp;**采用了这种方案的AOP框架会在编译阶段对程序的需要增强的代码进行修改，从而生成静态的AOP代理类**，即这样实际上编译出来的类的class文件实际上已经是增强后的代理类，因为编译期的特殊性，所以要实现静态AOP需要用特殊的编译器，常见的静态AOP框架如：AspectJ
<br>

## 🥀 5.2、动态AOP实现
<br>

&emsp;&emsp;**采用这种方案的AOP框架是在运行时才会对目标对象生成代理对象**(如在内存: 通过JDK的动态代理技术或者CGlib动态字节码技术生成AOP代理对象)，这种方案常见的框架：Spring AOP(就是本文的主角)
<br>

## 🌺 5.3、不同AOP框架之间的特点比较
<br>

![请添加图片描述](https://img-blog.csdnimg.cn/62e4e0f8813b4f78aa31244f4131a39a.png)


<br>

# 💙 六、面向切面编程的术语

<br>

&emsp;&emsp;通过上文，我们已经对面向切面编程有了大概的印象，下面我们就开始真正了解关于面向切面编程的相关知识点。<br>

&emsp;&emsp;**俗话说得好: “见人说人话，见鬼说鬼话”，想要理解面向切面编程的精髓，那就要先读懂面向切面编程中的术语**，连术语都不懂是什么含义，谈何认识、使用。下面先通过一个图片直观的认识AOP中常用术语之间的一个联系。
<br>

![请添加图片描述](https://img-blog.csdnimg.cn/4d9485de06ad4a8cb4c78a72d64bdb19.png)
<br>

&emsp;&emsp;看完上图大家可能一头雾水，没关系，下面来具体介绍AOP相关的术语，然后再重复看上面的联系图，相信你会有更深刻的理解。
<br>

## 🌷 6.1、相关概念
<br>

&emsp;&emsp;**1、增强**<br>

&emsp;&emsp;**可以理解为通过一些操作让类可以完成原来做不到的事情**，如: 在上面的洗澡的例子中，洗澡方法中不包含"脱衣服"和“穿衣服”的功能，但是通过AOP可以让"洗澡方法"具有这两个功能，那我们就可以说“洗澡方法”被增强了(再简单点你就看看美国队长，打了个药能锤出地球....)。<br>

&emsp;&emsp;**常见的增强方法：** 继承、装饰者模式、动态代理(留个伏笔，你猜文章有使用到哪个方式实现增强呢?)
<br>

&emsp;&emsp;**2、目标对象：**<br>

&emsp;&emsp;即我们需要增强的类生成的对象，如上面例子中包含“洗澡”方法的类对应的对象。
<br>

&emsp;&emsp;**3、代理对象：**<br>

&emsp;&emsp;通过AOP框架，对目标对象增强生成的新对象，它可以拥有目标对象没有的行为和属性。

<br>

## 🌱 6.2、通知(Advice)
<br>

&emsp;&emsp;**切面的具体功能和使用场景，它定义了切面的具体功能是什么以及何时被使用。** 在上面洗澡的例子中，切面的"具体功能"则为:"脱衣服和穿衣服"，"何时被使用"则为: "在洗澡前要脱衣服"，"在洗澡后要穿衣服"。

<br>

## 🌿 6.3、通知类型
<br>

&emsp;&emsp;在Spring中，根据使用时机的不同，将通知划分为了5种类型，下面来一个个认识下吧！<br>

- **前置通知(Befor)：** 在目标方法被调用之前调用的通知功能。如上文例子中在调用“洗澡方法”前先执行“脱衣服”通知。


- **后置通知(After)：** 在目标方法完成后调用的通知功能，即使在执行目标方法出现异常，也照常执行。如上面的例子中在洗澡时喷洒炸了，但是照样在洗完澡后继续完成“穿衣服”的通知功能。


- **返回通知(After-returning)：** 在目标方法成功执行之后才调用通知功能。如上面的例子，在洗澡过程中厕所爆炸了，直接送去医院了，“穿衣服”的通知功能就不会被调用(味道太大....)


- **异常通知(After-throwing)：** 在目标方法抛出异常后执行通知功能。如上面例子，在洗澡中厕所又炸了，但是我提前准备了厕所炸后应该怎么办的方案，一旦出现了这个异常，就会去调用这个通知功能,总不能光溜溜的出去吧。


- **环绕通知(Around)：** 通知包围了目标方法，在调用目标方法前后可以执行自定义的通知功能。如上面例子，在洗澡前脱衣服，在洗澡后穿衣服的两个功能可以直接通过环绕通知完成。

<br>

## ☘️ 6.4、连接点(Join point)
<br>

&emsp;&emsp;**它表示在业务逻辑执行过程中能够插入切面通知的一个点。在Spring中，这个点可以是调用方法时、调用方法后、抛出异常时。** 切面代码可以利用这些点插入到应用的正常流程之中，并添加新的行为。<br>

&emsp;&emsp;Spring AOP 是基于动态代理的，所以它只支持方法的连接点，但像其他的AOP框架如AspectJ在修改属性时或者构造器执行时都可以织入通知，达到更细颗粒度的控制拦截。

<br>

## 🍀 6.5、切点(Pointcut)
<br>

&emsp;&emsp;**通俗理解: 通知作用于哪些连接点,这个点可以称为切点，通过上面通知介绍可知，“通知”定义了切面是"什么"和"何时使用"作用，切点则定义了"切面"在"何处"使用。** 通过配置切点，可以将通知织入到一个或者多个连接点中。如上文的例子中，洗澡方法的执行前后就可以理解为切点。<br>

&emsp;&emsp;**在实际应用中，是通过指定类名称、方法名称、正则表达式进行来匹配指定切点**，甚至还存在一些AOP框架允许动创建动态的切点，根据运行时的决策(如：方法的参数值)来确定是否需要使用通知增强(这些是拓展的一些知识点，本文暂不涉及)。
<br>

## 🌿 6.6、切面(Aspect)
<br>

&emsp;&emsp;**切面即切点和通知的集合，它定义了它是什么，在何时、何处完成这个功能。** 如上文的例子中，洗澡方法的执行前后就可以理解为切点，"脱衣服"和"穿衣服"就可以理解为通知。

<br>

## ☘️ 6.7、引入(Introduction)
<br>

&emsp;&emsp;**允许向类中添加新的属性和方法,实际上就是增强类(让类拥有原来没有的功能或者属性)。** 具体可以理解成将切面应用到目标类上，从而实现不修改类的代码，而让它具有新的功能和属性。


<br>

## 🍀 6.8、织入(Weaving)
<br>

&emsp;&emsp;**织入是把切面应用到目标对象并创建新的代理对象的过程，这是一个动态的过程，** 切面在指定的切点被织入到目标对象中，实际上在目标对象的生命周期中，存在以下的多个点可以进行织入：<br>

&emsp;&emsp;**1、编译期：** 切面在目标类编译时被织入。这种方式需要特殊的编译器。AspectJ 的织入编译器就是以这种方式织入切面的。<br>

&emsp;&emsp;**2、类加载期：** 切面在目标类加载到 JVM 时被织入。这种方式需要特殊的类加载器（ClassLoader），它可以在目标类被引入应用之前增强该目标类的字节码。AspectJ 5 的加载时织入（load-time weaving，LTW）就支持以这种方式织入切面。<br>

&emsp;&emsp;**3、运行期：** 切面在应用运行的某个时刻被织入。一般情况下，在织入切面时，AOP 容器会为目标对象动态地创建一个代理对象。Spring AOP 就是以这种方式织入切面的。

<br>

# 💚 七、AOP、Spirng AOP和代理之间的关系
<br>

&emsp;&emsp;前面介绍了比较多理论知识，大家可能会混淆一些概念，下面来具体介绍这些概念的一个联系，帮助大家更好的例子这些概念。
<br>

## 🍁 7.1、代理
<br>

&emsp;&emsp;**可以理解成委托别人帮忙处理事情，生活中最常见的例子就是二手东。** 真正房东一般会给二手东工资然后将房租的出租等事情委托给二手东帮忙处理，在这个例子中二手东就是房东的代理，负责帮房东跑腿。

<br>

## 🍂 7.2、动态代理
<br>

&emsp;&emsp;**在程序运行期,动态创建目标对象的代理对象,并对目标对象中的方法进行功能性增强的一种技术**，根据实现方式可以划分为：JDK动态代理和CGlib动态代理。

<br>

## 🍃 7.3、Spring AOP
<br>

&emsp;&emsp;通过上文介绍我们知道**AOP是一个编程的思想，并不是一个具体的技术或者框架，这种思想延伸出来的框架叫做AOP框架，Spring AOP就是其中的一种，Spring AOP是构建在动态代理之上的。**<br>
&emsp;&emsp;它包含了JDK动态代理和CGlib动态代理，当一个类如果实现了接口，那么Spring AOP会使用JDK动态代理给它生成代理对象，反之则使用CGlib代理技术生成代理对象。


<br>

## 🚉 7.4、Spring支持的AOP类型
<br>

- 基于代理的经典 Spring AOP


- **纯 POJO 切面(使用xml结合pojo实现Spring AOP，重点介绍)**


- **@AspectJ 注解驱动的切面(基于注解实现Spring AOP，重点介绍)**


- 注入式 AspectJ 切面（适用于 Spring 各版本）。

&emsp;&emsp;基于代理的经典 Spring AOP是通过ProxyFactory Bean实现，在引入了了简单的声明式 AOP 和基于注解的 AOP 之后，看起来会显得比较笨重和复杂，所以本文不介绍，第四种方案使用需要学习一些额外的知识，本文也暂不介绍。


<br>

## 🚊 7.5、Spring AOP框架的一些特点
<br>

&emsp;&emsp;**1、Spring AOP框架和AspectJ框架的对比**<br>

&emsp;&emsp;**Spring AOP的通知是由Java编写的，这样使用起来就可以跟普通的JAVA功能一样快速入手，并且提供了XML和注解两种方式，可以供开发者有更多的选择。** 而AspectJ则与之相反，它最初是以JAVA语言拓展的，虽然支持了更强大的功能和更细颗粒度的控制，但是使用的话需要学习新的工具和语法。
<br>

&emsp;&emsp;**2、Spring AOP如何实现增强目标对象**<br>

&emsp;&emsp;**实际上在代理对象中包含了切面和目标对象，并且可以进行拦截通知方法的调用，当调用者进行调用时，先执行切面逻辑，然后再将调用转发给目标对象的bean，从而实现增强效果**，因为在运行时才创建代理对象，所以Spring AOP不像AspectJ一样需要特殊的编译器来实现切面织入，如下图所示：<br>

![请添加图片描述](https://img-blog.csdnimg.cn/57ab05933b8b49a7975d345bddb51e79.png)
<br>

&emsp;&emsp;**3、Spring AOP只支持方法级别的连接点**<br>

&emsp;&emsp;并非所有的AOP框架都是相同的，**不同的框架在连接点模型上就可能存在强弱之分。如AspectJ除了支持方法切点外，还提供了字段和构造器的切点，因为Spring AOP是基于动态代理的，所以只支持方法级别的连接点**，但是方法拦截已经可以满足绝大多数的需求，如果是需要方法拦截之前的连接点拦截，则需要使用其他的AOP框架如AspectJ来实现。


<br>


# 💜 八、Spring AOP支持的切点指示器
<br>

&emsp;&emsp;注: 虽然本文介绍的是Spring AOP，但实际上Spring 和 AspectJ 项目之间有大量的协作，而且 Spring 对 AOP 的支持在很多方面借鉴了 AspectJ 项目，比如切点指示器。<br>

&emsp;&emsp;在上文中有提到，切面就是通知和切点结合，切点定义了应该在哪里应用通知，**而在Spring AOP中，切点是通过切点指示器来实现具体匹配到哪些连接点的，下面就来认识下Spring AOP支持的切点指示器吧！**<br>

![在这里插入图片描述](https://img-blog.csdnimg.cn/bb64adac78974f1e847f5c5b31c1ad8d.png)
<br>

&emsp;&emsp;**小结：上面虽然列出了一堆指示器，但是只有execution 指示器是实际执行匹配的，其他的指示器都只是用来限制匹配的，** 就像男厕所只能进入，女厕所只能女生进入，人妖只能站外边了！<br>

&emsp;&emsp;因为切点指示器的类别比较多，下面只举例一些常使用到的指示器，**关于每个指示器的运用，下面会专门开一篇文章进行讲解，大家记得点下订阅专栏哦!**<br>

&emsp;&emsp;**温馨提示：** 上面的指示器并不需要大家死记硬背，在使用到的时候如果忘记直接查询一下就可以，**我们学习AOP更重要的是理解这种编程思想和运用，并不是去死记硬背这里面的一些关键词，这样不仅浪费时间而且效率也低。**


<br>

## 🚋 8.1、Spring AOP 切点表达式举例剖析
<br>

&emsp;&emsp;**写在切点指示器中用于匹配连接点的表达式叫做切点表达式，有了切点表达式，我们就可以根据自己的需要去匹配需要增强的方法。** 为了能够让大家更清晰的理解关于切点表达式，还是上面洗澡的例子，我们将它抽象成下面的代码，不同使用者去使实现它：
<br>

```java
package wash;
public interface Wash{
    void takeAWash();
}
```

&emsp;&emsp;**假设我们要实现只要调用"洗澡"这方法，就要触发"脱衣服，穿衣服"的行为，那切点表达式定义就如下:**
<br>

![请添加图片描述](https://img-blog.csdnimg.cn/12607c963afc402bb71f83d93c22121d.png#pic_center)
<br>

&emsp;&emsp;由此，我们可以总结出切点表达式的一个格式语法如下:<br>

&emsp;&emsp;**格式：execution( [修饰符] 返回值类型 包名.类名.方法名 (参数) )，其中*表示任意类型，方法参数中 . . 表示任意的参数。**
<br>

## 🚌 8.2、Spring AOP 常用指示器解析
<br>

&emsp;&emsp;**常用注解**<br>

&emsp;&emsp;1、@Pointcut：封装相同的切点表达式，提供统一的引用<br>

![请添加图片描述](https://img-blog.csdnimg.cn/78149ddd73604349bdc7f3ba8976bb89.png)
<br>

&emsp;&emsp;2、@within：用于匹配所以持有指定注解类型内的方法；而within是用于匹配指定类型内的方法执行；
<br>

![在这里插入图片描述](https://img-blog.csdnimg.cn/853cb9e0df2d43b8ac67b5f3a18407bb.png)
<br>

&emsp;&emsp;**切点通配符**
<br>

- *通配符 : 表示任意数量的字符，如:
<br>

```java
// 匹配com.demo.IPerson类中任意返回值类型和任意参数的takeAWash方法
execution(* com.demo.IPerson.takeAWash(..))
```
<br>

- ..通配符：表示匹配系统中任意包和任意方法中的任意参数，如：
<br>

```java
// 匹配任意名称，任意返回值，任意参数的方法
execution(* *(..))

// 匹配com.demo包及其子包下的所有类的所有方法
within(com.demo.*)
```
<br>

- +通配符：表示给定类的任意子类
<br>

```java
// 匹配Demo接口下的任意子类
within(com.elvis.springaopinaction.Demo+)
```
<br>



# 💝 九、Spring AOP中的一些问题
<br>

## 🚖 9.1、AOP和OOP有什么关联
<br>

&emsp;&emsp;OOP(面向对象编程)侧重的是在业务处理中对问题实体的属性和行为的抽象封装，从而达到更清晰高效的逻辑单元划分，提高系统的可理解性、易维护性。<br>

&emsp;&emsp;AOP(面向切面编程)侧重的是将系统的核心关注点和横切关注点分离，核心关注点关注核心业务处理，横切关注点关注与核心业务无关却被核心业务引用的公共行为，将它们单独抽取成新的模块，和核心业务分离，降低模块之间的耦合度，提高代码的复用性，可以说它是面向对象编程(OOP)的进行补充和完善。
<br>

## 🚐 9.2、后置通知(After)和返回通知(After-returning)有什么区别
<br>

&emsp;&emsp;后置通知(After)：表示在目标方法执行完成后执行的通知功能，即使在执行目标方法出现异常，也照常执行这个逻辑。<br>

&emsp;&emsp;返回通知(After-returning)表示目标方法正常执行完成后置的通知功能，如果目标方法执行过程中出现异常，那么这个通知的逻辑就不执行。
<br>

![请添加图片描述](https://img-blog.csdnimg.cn/36089dda99614baf8350bb9c9e1a800a.png)
<br>

![请添加图片描述](https://img-blog.csdnimg.cn/3f7222ad336a419295099eb749d8001a.png)
<br>

## 🚑 9.3、五种通知的一个执行顺序是怎样
<br>

&emsp;&emsp;**执行业务中不包含异常时的执行顺序:**<br>

&emsp;&emsp;环绕通知前部分(Around-Before) =》 前置通知(Before) =》业务逻辑 =》返回通知(AfterReturning) =》后置通知(After) =》环绕通知后(Around-After)<br>

&emsp;&emsp;**执行业务中包含异常时的执行顺序：**<br>

&emsp;&emsp;环绕通知前部分(Around-Before) =》 前置通知(Before) =》异常业务 =》异常通知(AfterThrowing) =》后置通知(After) =》环绕通知后(Around-After) =》环绕通知中捕获的异常输出
<br>

![请添加图片描述](https://img-blog.csdnimg.cn/b0ea80c75d0c4f86a0d061d01e6e5e0e.png)
<br>

## 🚒 9.4、既然可以通过继承或者装饰者模式增强，为什么要引入AOP
<br>

&emsp;&emsp;这个问题上面就有留下一个悬念，让大家思考，不知道你有没有猜对呢？<br>

&emsp;&emsp;**在JAVA中，要是实现对对象增强，最常用的三种方法就是：继承、装饰者模式、动态代理。** 下面我们通过具体的例子来理解三种方式的区别。<br>

&emsp;&emsp;以喝奶茶举例，我们可以往奶茶中添加珍珠，椰果、红豆等配料，从而得到我们喜欢的口味，这些配料改变了原味奶茶的味道，我们可以说奶茶被“增强”了，使用JAVA代码表示如下：<br>

&emsp;&emsp;**1、继承方式增强**<br>

```java
/**
 * 奶茶基础类
 */
public class MilkTea {
    public void makeMilkTea(){
        System.out.print("原味奶茶");
    }
}
/**
 * 加珍珠奶茶
 */

class PearlMilkTea extends MilkTea{

    @Override
    public void makeMilkTea(){
        super.makeMilkTea();
        System.out.println("  + 加珍珠");
    }

}
/**
 * 加椰奶奶茶
 */

class CreamMilkTea extends MilkTea{

    @Override
    public void makeMilkTea(){
        super.makeMilkTea();
        System.out.println("  + 加椰奶");
    }

}

```
&emsp;&emsp;如果此时又有一个人来说要喝珍珠椰奶奶茶，我们又需要创建一个PearlCreamMilkTea子类(代码如下)，**如此类推，每一种排列组合我们就需要通过一个新的类去维护，这样非常容易出现类爆炸的情况**，那以维护，此时，我们可以通过装饰者模式来解决这个问题：<br>

```java
// 珍珠椰奶奶茶
class CreamMilkTea extends PearlMilkTea {

    @Override
    public void makeMilkTea(){
        super.makeMilkTea();
        System.out.println("  + 加椰奶");
    }

}
```
<br>

&emsp;&emsp;**2、装饰者方式增强**
<br>

```java
/**
 * 抽象组件:定义一个抽象接口，来规范准备附加功能的类
 */
public interface IMikeTea {
    void makeMilkTea();
}

/**
 * 具体组件：将要被附加功能的类，实现抽象构件角色接口
 */
public class MikeTeaImpl implements IMikeTea{
    @Override
    public void makeMilkTea() {
        System.out.print("原味奶茶");
    }
}

/**
 * 抽象装饰者：持有对具体构件角色的引用并定义与抽象构件角色一致的接口
 */
public abstract class DecoratorMikeTea implements IMikeTea{

    private IMikeTea mikeTea;

    public DecoratorMikeTea(IMikeTea mikeTea) {
        this.mikeTea = mikeTea;
    }


    public void setIMikeTea(IMikeTea mikeTea){
        this.mikeTea = mikeTea;
    }

    @Override
    public void makeMilkTea(){
        mikeTea.makeMilkTea();
    }

}

/**
 * 具体装饰：实现抽象装饰者角色，负责对具体构件添加额外功能。
 */
public class ConcreteCreamMilkTea extends DecoratorMikeTea{

    public ConcreteCreamMilkTea(MikeTeaImpl mikeTea) {
        super(mikeTea);
    }

    @Override
    public void makeMilkTea(){
        super.makeMilkTea();
        System.out.print("  + 加椰奶");
    }
}
...
```
&emsp;&emsp;测试代码如下：<br>

```java
/**
 * 测试装饰者模式
 */
public class DecorarorMilkTeaTest {
    public static void main(String[] args) {
        MikeTeaImpl man = new MikeTeaImpl();
        ConcreteCreamMilkTea md1 = new ConcreteCreamMilkTea(man);
        ConcretePearlMilkTea md2 = new ConcretePearlMilkTea(man);

        // 珍珠奶茶
        md1.makeMilkTea();
        System.out.println();
        // 椰奶奶茶
        md2.makeMilkTea();
        System.out.println();
        // 珍珠椰奶奶茶
        md1.setIMikeTea(md2);
        md1.makeMilkTea();
    }
}
```
<br>

&emsp;&emsp;对比上面继承的方式，**我们会发现，即使我们新增一种排列组合，不需要通过新的子类来维护对应的代码，通过装饰着模式可以动态给对象添加新的职责**(注：上面案例只放部分代码，全部代码已经上传到[《Spring-in-action》](https://gitee.com/it-learning-diary/spring-aop-in-action?_from=gitee_search))。<br>

&emsp;&emsp;**3、继承、装饰者模式、AOP(动态代理)小结**<br>

&emsp;&emsp;**(1)、继承**<br>
&emsp;&emsp;面向对象编程中三大特性之一，它允许我们子类继承父类特有的属性或行为，从而提高代码的重用性。<br>

&emsp;&emsp;**优点:** 可以进行功能增强和代码重用<br>

&emsp;&emsp;**缺点：** JAVA是单继承，继承提高了类之间的耦合性，这样容易导致一个脆弱的对象体系，如果场景变复杂，延伸出来的子类会爆炸，难以进行维护。<br>

&emsp;&emsp;**(2)、装饰者模式(Decorator Pattern)**<br>

&emsp;&emsp;它可以动态地拓展一个对象的功能，比继承更具有弹性，本质是一种组合的思想。继承是在编译时就已经决定了子类的一个行为，如果是使用组合的方式来拓展对象的功能，可以在运行时动态地对对象拓展。<br>

&emsp;&emsp;**优点：** 符合开闭原则，比继承具有更多灵活性，可以进行设计不同的装饰类和对它们进行排列组合，从而得到满足不同场景的组合。<br>

&emsp;&emsp;**缺点：** 相比继承的使用，会显得更加复杂，开发难度更大。<br>

&emsp;&emsp;**(3)、AOP(动态代理)**<br>

&emsp;&emsp;将非核心业务与核心业务分离解耦，在需要的地方动态织入，大大提高了代码的复用性和可拓展性。<br>

&emsp;&emsp;**(4)、继承、装饰者模式、AOP之间的区别**<br>

-  使用继承方式增强时：被增强的对象时固定的，被增强的内容也是固定的

- 使用装饰者模式增强时：被增强的对象时可以切换的，被增强的内容时固定的

- 使用面向切面编程时：它的增强对象和增强内容都是可以切换的，比装饰者更加灵活，这也是为什么有了继承和装饰者模式我们还引入AOP模式的原因。

<br>



# 💞 十、Spring AOP实战
<br>

&emsp;&emsp;千呼万唤始出来，前面介绍了这么多关于Spring AOP的相关知识点和概念，下面就通过注解和XML两种方式实现对Spring AOP的应用吧！**因为涉及到的演示代码比较多，所以例子中只会放出部分代码和截图，全部的代码会传到gitee上，大家想了解的可以通过[《Spring-in-action》](https://gitee.com/it-learning-diary/spring-aop-in-action?_from=gitee_search)直通车到达！**
<br>

## 🔴 10.1、使用注解方式实现Spring AOP的使用
<br>

&emsp;&emsp;**1、添加依赖**
<br>

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
    <version>2.3.7.RELEASE</version>
</dependency>
```
<br>

&emsp;&emsp;**2、抽象“洗澡行为”，并通过具体实现类实现**
<br>

![请添加图片描述](https://img-blog.csdnimg.cn/34a5750949a94ad4830a3a83a92506f5.png)
<br>

&emsp;&emsp;**3、添加切面**
<br>

![请添加图片描述](https://img-blog.csdnimg.cn/c0087adba8ea4574ab2b50e71e7033e9.png)

![请添加图片描述](https://img-blog.csdnimg.cn/cf4f8d4b9c60447898acb85b15f624ef.png)


<br>

&emsp;&emsp;**4、执行测试**
<br>

![请添加图片描述](https://img-blog.csdnimg.cn/b32f3be5029a4926a3aecd9216929c50.png)

<br>

## 🟠 10.2、使用xml方式实现Spring AOP的使用
<br>

&emsp;&emsp;**1、同上，引入相应依赖(注:如果运行时提示找不到对应的类则添加下面的代码)**
<br>

![请添加图片描述](https://img-blog.csdnimg.cn/e57e3299c0bf4852b8e0c735119b7b5a.png)
<br>

&emsp;&emsp;**2、添加通知和相应测试实体**
<br>

![请添加图片描述](https://img-blog.csdnimg.cn/f4508eb09ada40068aaac02440d0dd5a.png)

![请添加图片描述](https://img-blog.csdnimg.cn/8911b0e74d4040cd81e3dddc8a6c092f.png)
<br>

&emsp;&emsp;**3、添加xml配置文件**
<br>

![请添加图片描述](https://img-blog.csdnimg.cn/9b14af76b6bf4e0a8cc469679c3cf70e.png)
<br>

&emsp;&emsp;**4、测试执行结果**
<br>

![请添加图片描述](https://img-blog.csdnimg.cn/68e4cb4a2ff54ebfbb7f58a6560ca48d.png)
<br>

&emsp;&emsp;**小结**<br>

&emsp;&emsp;通过上面的例子发现，起始理解了AOP的一个编程思想后，真正的运用对应的AOP框架起始还是比较简单的。上面的例子只是简单的运用了前置通知和后置通知，更多的通知运用已经传递到gitee上，欢迎查看[《Spring-in-action》](https://gitee.com/it-learning-diary/spring-aop-in-action?_from=gitee_search)，不要忘记一个订阅噢。



<br>


# 💟 十一、写在最后
<br>


&emsp;&emsp;"纸上得来终觉浅，绝知此事要躬行"，**因为AOP设计的专业术语还是比较多的，要想真正的掌握单单看完文章还是不行的，一定要亲自实践，大家赶紧行动起来吧，如在实践中遇到问题，可以私信我！**<br>

&emsp;&emsp;因为文章篇幅限制，面向切面编程涉及到的知识太广，本文也只是从基础方面对aop进行了一个剖析，还有许多细节没有进行解读，**但是任何知识都是万变不离其宗，只要将知识的"根"抓住，对应这个知识的其他拓展就可以很好的去理解。**<br>

&emsp;&emsp;看完文章后，**如果大家对Spring AOP甚至Spring想要更深的了解，推荐大家去阅读《Spring实战》这一本书，个人阅读了一部分，感觉还是十分受用的，如果想要电子版的，可以私信我。**<br>

&emsp;&emsp;**第二波福利来了，为了方便管理博客，个人最近开通了CSDN的年VIP，VIP有一些免费下载特权，如果有粉丝有紧急下载需求，可以私信我！如果文章有帮助，不要忘记一键三连和订阅专栏哦，ღ( ´･ᴗ･` )比心！**<br>

&emsp;&emsp;注：文章所有案例的代码已经上传到[《Spring-in-action》](https://gitee.com/it-learning-diary/spring-aop-in-action?_from=gitee_search)
<br>



# 🏳️‍🌈 十二、参考资料
<br>

&emsp;&emsp;[Spring官方文档](https://docs.spring.io/spring-framework/docs/5.0.6.RELEASE/spring-framework-reference/core.html#aop)<br>

&emsp;&emsp;Spring实战-第四版<br>

&emsp;&emsp;[Spring 中面向切面编程](https://www.cnblogs.com/joy99/p/10941543.html)

