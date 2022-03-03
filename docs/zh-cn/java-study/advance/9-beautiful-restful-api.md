
  &emsp;&emsp;<font>温馨提示:  本文总共3200字,阅读完大概需要2-3分钟,希望您能耐心看完,倘若你对该知识点已经比较熟悉,你可以直接通过目录跳转到你感兴趣的地方,希望阅读本文能够对您有所帮助,如果阅读过程中有什么好的建议、看法,欢迎在文章下方留言或者私信我,您的意见对我非常宝贵,再次感谢你阅读本文。



### 一: Restful API展示

&emsp;&emsp;废话不多说、先展示Restful 风格的API

```java
1、// 新增一篇文章
@RequestMapping(value = "/articles",method = RequestMethod.POST)

2、// 删除一篇文章
@RequestMapping(value = "/articles",method = RequestMethod.DELETE)

3、// 删除某一类文章下的一篇文章
@RequestMapping(value = "/types/{id}/articles",method = RequestMethod.DELETE)

4、 // 查询一篇文章
@RequestMapping(value = "/articles/{id}",method = RequestMethod.GET)

5、// 查询某一类文章中的所有文章
@RequestMapping(value = "/types/{id}/articles",method = RequestMethod.GET)

6、// 修改一篇文章(全部属性)
@RequestMapping(value = "/articles/{id}",method = RequestMethod.PUT)

7、// 修改一篇文章(某些属性)
@RequestMapping(value = "/articles/{id}",method = RequestMethod.PATCH)
```


### 二: Restful API风格的由来

&emsp;&emsp;Rest(Representational State Transfer)全称是表述性状态转移,它是由[Roy Thomas Fielding](https://www.ics.uci.edu/~fielding/)博士在2000年提出的,它表示的是一种新的架构风格，一种轻量级,跨平台,跨语言的架构设计。<br>

&emsp;&emsp;API(Application Programming Interface): 既我们熟知的接口,是一组编程接口规范、客户端与服务端通过请求响应进行数据通信。<br>

&emsp;&emsp;RestfulAPI: 它不是一种新的技术,而是基于Rest架构思想的API设计风格。

### 三: Restful API风格的优点

(一) 优点: 

 1. 它是面向资源的(名词)

 2. 通过URL就知道需要什么资源

 4. 通过Http Method(get/post...)就知道针对资源干什么

 5. 通过Http Status Code就知道结果如何

(二) 优点解释:

&emsp;&emsp;(1)通过URL就知道需要什么资源：表示Restful风格的API可以直接通过URL就可以看到需要操作的是什么资源,有语义化。<br>

&emsp;&emsp;(2)Restful风格的API是面向资源(名称)的,既URL中不会带相应的动词,针对资源的操作是通过Http Method(既:post-增、delete-删、put-改(一般是提供实体的全部信息)、patch-改(修改实体的某些属性)、get-查)来实现的。<br>

&emsp;&emsp;(3)通过Http Status Code就知道结果如何: 如常见的200(成功)、400(错误的请求参数)、500(服务器错误)等。

### 四: Restful API风格的注意事项

 1. 请求资源应该使用复数而不是单数,因为Restful API风格是是面向资源的(名词)

 2. 强制性添加API版本声明，不要发布无版本的API,如: api.v1/blogs(开闭原则)，对拓展开发、对修改关闭,如果后面需要添加新的功能，可以直接新开接口加上版本号就可。


### 五: 总结
&emsp;&emsp; 无论是面试或者工作中,总会听到别人问到关于Restful风格API的问题,其实,它并不是我们想象中的那么高深莫测,它只是一种设置API架构风格,而不是一种新的技术,遵循这种风格设计的API就被称为Restful API。<br>

&emsp;&emsp;相信，看完这篇文章，你已经对Restful API有了一个新的认识,如果还有什么问题需要反馈的,可以在下方留言或者私信我,我看到会第一时间回复。<br>
