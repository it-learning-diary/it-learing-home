
  &emsp;&emsp;<font>温馨提示:  本文总共1200字,阅读完大概需要1-3分钟,希望您能耐心看完,倘若你对该知识点已经比较熟悉,你可以直接通过目录跳转到你感兴趣的地方,希望阅读本文能够对您有所帮助。

### 一: 定义

  &emsp;&emsp;**1、@RequestParam注解作用:**

  &emsp;&emsp;获取URL中携带的请求参数的值既URL中“?”后携带的参数,传递参数的格式是:key=value

 &emsp;&emsp;如: https://localhost/requestParam/test?key1=value1&key2=value2...

 &emsp;&emsp;**2、@PathVariable注解作用:**

&emsp;&emsp;用于获取URL中路径的参数值,参数名由RequestMapping注解请求路径时指定,常用于restful风格的api中,传递参数格式:直接在url后添加需要传递的值即可

&emsp;&emsp;如: https://localhost/pathVariable/test/value1/value2...

### 二: 语法


&emsp;&emsp;1、 RequestParam使用案例: @RequestParam(value = "param",required = false,defaultValue = "test")String param

&emsp;&emsp;2、参数解析:

&emsp;&emsp;**value/name:**  URL中需要获取的参数名称

&emsp;&emsp;**required:** true/false，为true时,url中必须携带这个参数(否则会出现: Required String parameter XXX is not present"),为false时,可以选填这个参数。

&emsp;&emsp;**defaultValue：**默认值,如果这个url没有携带这个参数时,默认设置的值。



&emsp;&emsp;**3、 PathVariable使用案例:**

&emsp;&emsp;@RequestMapping("/pathVariable/test/{param}")

&emsp;&emsp;@PathVariable(value = "param",required = false)String param

&emsp;&emsp;**4、参数解析：**

&emsp;&emsp;5、name/value：RequestMapping注解中url路径绑定参数的名称,如/pathVariable/test/{param},则name的值就为param

&emsp;&emsp;6、required: 为true时,这个参数必选填写,默认是true,为false时:参数可选是否填写




### 三: 项目结构

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729211233573.png)

### 四: 测试代码


```java
/**
 * @author 
 * @version V1.0
 * @Description: 注解测试
 * @date 2020-7-29
 */
@Controller
public class AnnotationController {

    /**
     * RequestParam: 用于获取URL中“?”后携带的参数的值,如: http://localhost:8080/requestParam/test?param=xxx中param参数的值
     * 相关属性:
     *    1、name/value：url中指定参数的名称
     *    2、required: 为true时,这个参数必选填写,默认是true,为false时:参数可选是否填写
     *    3、defaultValue：参数不填写时的默认值
     **/
    @RequestMapping("/requestParam/test")
    @ResponseBody
    public String requestParamTest(@RequestParam(value = "param",required = true)String param){
        return "接受到的参数:" + param;
    }


    /**
     * RequestParam: 用于获取URL中路径的参数值,参数名由RequestMapping注解请求路径时指定,常用于restful风格的api中
     * 如: http://localhost:8080/pathVariable/test/123 中123的值
     * 相关属性:
     *    1、name/value：RequestMapping注解中url路径绑定参数的名称,如/pathVariable/test/{param},则name的值就为param
     *    2、required: 为true时,这个参数必选填写,默认是true,为false时:参数可选是否填写
     **/
    @RequestMapping("/pathVariable/test/{param}")
    @ResponseBody
    public String pathVariableTest(@PathVariable(value = "param",required = false)String param){
        return "pathVariable接受到的参数:" + param;
    }

}

```


### 五: 测试结果

#### (一) @RequestParam注解测试结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729211448902.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729211456630.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729211511676.png)



#### (二) @PathVariable注解测试结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729211611714.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729211617422.png)

