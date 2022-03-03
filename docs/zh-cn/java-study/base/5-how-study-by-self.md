## 一:什么是对象

&emsp;&emsp;回答思路: 这个问题的主要考察的是你对面向对象语言的理解,回答时除了回答面向对象的定义外,更重要的是要学会举延伸说明和类相关的一些特性。

&emsp;&emsp;定义: 在系统中,对象是用于客观描述一个事物的一个实体,而类这是这类实体的抽象,它是构成系统的一个基本单位。一个对象由一组描述对象的属性和一组描述对象的动作组成。

&emsp;&emsp;类的实例化可以创建对象,每个对象都有它的生命周期,对象的生命周期可以简单的归为:生成、使用、销毁三个阶段。

&emsp;&emsp;在JAVA语言中,一个类如果不存在引用时,那它就是一个无用的对象,JAVA的垃圾回收器会自动扫描JVM虚拟机,对这些没有被引用的垃圾对象进行回收,开发者也可以显示调用System.gc()方法告知垃圾回收器进行回收垃圾对象,但是并不意味着在调用完这个方法后垃圾回收器就会垃圾去回收,具体的回收时间是由垃圾收回器自己确定,调用这个方法只是给垃圾回收器发送一个“信号”,告诉它现在内存不够或者可以去存在垃圾对象需要它回收。

### 延伸问题一:面向对象有什么特征

&emsp;&emsp;面向对象的三大特征:封装、继承、多态

&emsp;&emsp;封装: 隐藏对象的属性和实现细节、只提供访问的公共方法,实现的方式是通过访问修饰符来限定。

![](https://gitee.com/whose-white-moon/blog-image/raw/master/20220303082529.png)



&emsp;&emsp;继承: 继承表示的是一个类拥有另一个类的相关信息,通过关键字extends实现,被继承的类叫父类(基类、超类),得到继承信息的类也叫子类或派生类,JAVA中类只能单继承,但是可以实现多个接口。

​	![image-20210607200843724](https://gitee.com/whose-white-moon/blog-image/raw/master/20220303082537.png)



&emsp;&emsp;多态: 同一个行为可以有不同的表现形式的能力。具体来说就是一个类型可以有多种表现的形式,如:动物可以是狗、也可以是猫,具体如图所示:

![image-20210607215654546](https://gitee.com/whose-white-moon/blog-image/raw/master/20220303082544.png)


### 延伸问题二:多态有什么优点

&emsp;&emsp;对类型解耦,可以使用父类或者接口接收子类对象
&emsp;&emsp;可替换性,如实例一个猫对象,可以用动物接收:Animal cat = new Cat()
&emsp;&emsp;可拓展性,多态是对象的多种表现形式的体现,很易于拓展,如动物除了猫狗外，还可以是鸡鸭鱼等
&emsp;&emsp;更灵活,可以随意拓展新的表现形式而不影响其他的形式

### 延伸问题三:多态存在的必要条件

&emsp;&emsp;继承父类或者实现接口
&emsp;&emsp;重写
&emsp;&emsp;使用父类/接口接收子类对象

![image-20210607222417728](https://gitee.com/whose-white-moon/blog-image/raw/master/20220303082548.png)

``

```
class Animal2 {
    void draw() {}
}
 
class Dog extends Animal2 {

    @Override
    void draw() {
        System.out.println("汪汪");
    }
}
 
class Cat2 extends Animal2 {
    @Override
    void draw() {
        System.out.println("喵喵");
    }
}
 
class Fish extends Animal2 {
    @Override
    void draw() {
        System.out.println("泡泡");
    }
```

### 延伸问题四:JAVA作为面向对象,它有什么特点或者好处

 &emsp;&emsp;1、易于理解,有更好的可读性

 &emsp;&emsp;2、平台无关性,一次编译,处处运行

 &emsp;&emsp;3、提供了许多类库,方便开发者的工作,减少开发时间

 &emsp;&emsp;4、提供了对web的支持

 &emsp;&emsp;5、具有较好的安全性和健壮性(如垃圾回收)

 &emsp;&emsp;6、去除了C++中难以理解,易于混淆的特性


## 二:多态的实现方式

&emsp;&emsp;1、继承父类重写父类方法,关键字extends

```java
/**
 * 图形
 */
class Shape {
    void draw() {}
}
/**
 * 圆
 */
class Circle extends Shape {
    @Override
    void draw() {
        System.out.println("Circle.draw()");
    }
}
/**
 * 正方形
 */
class Square extends Shape {
    @Override
    void draw() {
        System.out.println("Square.draw()");
    }
}
/**
 * 三角形
 */
class Triangle extends Shape {
    @Override
    void draw() {
        System.out.println("Triangle.draw()");
    }
}
```

&emsp;&emsp;2、实现接口,重写接口方法,关键字implements

​		``

```
/**
 * 图形
 */
interface Shape {
     void draw();
}
/**
 * 圆
 */
class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Circle.draw()");
    }
}
/**
 * 正方形
 */
class Square implements Shape {
    @Override
    public void draw() {
        System.out.println("Square.draw()");
    }
}
/**
 * 三角形
 */
class Triangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Triangle.draw()");
    }
}
```

&emsp;&emsp;3、同一个类中方法重载

​	``

```
class Animal3{
    /*
        重载实现多态
     */
    
    public void call(){
        System.out.println("无参数的叫声");
    }
    
    public void call(String mode){
        System.out.println("带参数的叫声");
    }
}
```



### 延伸问题一: 虚拟机是如何实现多态的

&emsp;&emsp;通过动态绑定技术(dynamic binding),执行期间判断所引用对象的实际对象类型,根据实际类型调用对应的方法。


## 三:重载（Overload）和重写（Override）的区别。重载能够根据返回值的类型区分？

&emsp;&emsp;方法的重载和重写都是实现多态的方式，区别在于前者实现的是编译时的多态性，而后者实现的是运行时的多态性,具体差别如下:

&emsp;&emsp;重载发生在一个类中，同名的方法如果有不同的参数列表（参数类型不同、参数个数不同或者二者都不同）则视为重载；

&emsp;&emsp;重写发生在子类与父类或者子类和接口之间,要求如下:
&emsp;&emsp;（1）重写方法不能缩小访问权限；

&emsp;&emsp;（2）参数列表必须与被重写方法相同（包括显示形式）；

&emsp;&emsp;（3）返回类型必须与被重写方法的相同或是其子类；

&emsp;&emsp;（4）重写方法不能抛出新的异常，或者超过了父类范围的异常，但是可以抛出更少、更有限的异常，或者不抛出异常。


&emsp;&emsp;不能,构造器不能被继承，因此不能被重写，但可以被重载。重载对返回类型没有特殊的要求,所以无法通过返回值类型来区分重载。

&emsp;&emsp;父类的静态方法不能被子类重写。重写只适用于实例方法，不能用于静态方法,当父类和子类有相同名称的静态方法时,如果使用父类接收子类对象,则调用静态方法时,会直接使用父类的方法而隐藏掉子类的静态方法。

![image-20210607225844097](C:\Users\helloworld\AppData\Roaming\Typora\typora-user-images\image-20210607225844097.png)

### 延伸问题一:构造器（constructor）是否可被重写（override）？

&emsp;&emsp;不能,因为构造器不能被继承，因此不能被重写，但可以被重载。


## 四:抽象类和接口有什么区别

&emsp;&emsp;1、抽象类和接口都不能够实例化，但可以定义抽象类和接口类型的引用(既使用抽象类或者接口接收实际的类型创建出来的对象,如Animal a =new Dog()，Animal可以是接口或者抽象类)

&emsp;&emsp;2、一个类如果继承了某个抽象类或者实现了某个接口都需要对其中的抽象方法全部进行实现，否则该类仍然需要被声明为抽象类。

&emsp;&emsp;3、接口比抽象类更加抽象，因为抽象类中可以定义构造器，可以有抽象方法和具体方法，而接口中不能定义构造器而且其中的方法全部都是抽象方法。

&emsp;&emsp;3、抽象类中的成员可以是private、默认、protected、public的，而接口中的成员全都是public的。

&emsp;&emsp;4、抽象类中可以定义成员变量，而接口中只能定义常量。

&emsp;&emsp;5、有抽象方法的类必须被声明为抽象类，而抽象类不一定需要有抽象方法


### 4.1:访问修饰符public,private,protected,以及不写（默认）时的区别

![image-20210607230714096](C:\Users\helloworld\AppData\Roaming\Typora\typora-user-images\image-20210607230714096.png)

## 五:String是最基本的数据类型吗？

&emsp;&emsp;不是。Java中的基本数据类型只有8个：byte、short、int、long、float、double、char、boolean；除了基本类型（primitive type）和枚举类型（enumeration type），剩下的都是引用类型（reference type）。

### 5.1:JAVA中基本数据类型和包装类型的对应关系和占用字节

| 数据类型 | byte         | char      | short          | int            | long           | float                | double                | boolean                      |
| -------- | ------------ | --------- | -------------- | -------------- | -------------- | -------------------- | --------------------- | ---------------------------- |
| 取值范围 | -2^7 ~ 2^7-1 | 0~ 2^16-1 | -2^15 ~ 2^15-1 | -2^31 ~ 2^31-1 | -2^63 ~ 2^63-1 | -3.403E38 ~ 3.403E38 | -1.798E308 ~ -4.9E324 | false或true                  |
| 字节数   | 1            | 2         | 2              | 4              | 8              | 4                    | 8                     | 占用的字节数跟虚拟机类型相关 |
|          |              |           |                |                |                |                      |                       |                              |



### 5.2:JAVA为什么要引入8中基本数据类型

&emsp;&emsp;因为编码中使用到基本数据类型的场景非常多,为了减少使用时需要创建的的步骤,提升编码的编程的方便索引引入了基本数据类型。

&emsp;&emsp;同时,但是为了能够将这些基本数据类型当成对象操作，Java为每一个基本数据类型都引入了对应的包装类型（wrapper class）,如:int的包装类是Integer，且从Java 5开始引入了自动装箱/拆箱机制，使得二者可以相互转换。
