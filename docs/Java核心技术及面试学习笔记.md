# 1、Java入门

1. 什么是对象？**EVERYTHING IS OBJECT（万物皆对象）**

2. 对于对象的要求： 高内聚、 低耦合， 这样容易拼装成为 个系统。

3. 实现高内聚就是要最大限度**降低提高复用性**（复用性好是因为高内聚）。 可复用性是OOP的基础

4. 比较面向过程的思想和面向对象的思想：

   ```java
   /*
   面向过程的思想： 由过程、 步骤、 函数组成， 以过程为核心；
   面向对象的思想： 以对象为中心， 先开发类， 得到对象， 通过对象之间相互通信实现功能。 
   面向过程先有算法， 后有数据结构
   面向对象是先有数据结构， 然后再有算法
   */
   ```

# 2、Java基本语法

## 1、基本数据类型跟封装类型

- 整型：`byte、int、short、long`
- 浮点型：`float、double`
- 字符型：`char`
- 布尔型：`boolean`

![image-20200410205715424](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200410205715424.png)

![image-20200410205741065](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200410205741065.png)

### 1、基本类型与封装类型数据之间的转换`valueOf()`

```java
public class StringtoNum { ／／主类名需要和文件名一致
 ／／这是个main 函数
public static void main(String[] args)
／／从数据库里取到的num 是String 类型
String num =”123”;
／／通过Integer 封装类进行数据转换
int intVal = Integer.valueOf(num);
／／通过Float 封装类进行数据转换
float fltVal = Float.valueOf(num);
／／通过Long 封装类进行数据转换
long longVal = Long.valueOf(num);
／／依次输出三个转换后的变量
System.out.println(intVal);
System.out.println(fltVal);
System.out.println(longVal);
```

### 2、`i++`与`++i`的使用建议

- `i++`是指表达式运算完后`i`再加1
- `++i`是指`i`加上1后再运算表达式的值

为了提升代码的可读性，建议**左加加和右加加操作不应（或尽量少）和他操作符混用，如果实在有必要，要分开写。**

```java
int i = 10;
int j = 10;
int k;
//k = i++ *3  不建议！！！++i也是一样
k = i * 3 ;
i ++;
```

### 3、三目运算代替if语句

表达式1 ？表达式2 ： 表达式3;

语法：判断表达式1的返回值，如果是true 则执行表达式2 ，否则执行表达式3 。

```java
int x = 1001;
String yhx = "nice";
if(x > 1000)
    yhx = "good";
else
    yhx = "nice";
//三目运算
x > 1000 ? yhx = "good": yhx = "nice";
```

### 4、==与`equals`的区别

- **对于基本类型，== 比较值是否相等；对于封装类型，== 比较在内存中存在的地址是否相等。**
- **对于封装类型，`equals` 比较它们的值是否相等；**

```java
public class BasicGrammer {
    public static void main(String[] args) {
        boolean flags = true;
        int i = 10;
        int j = 10;
        flags = (i == j) ? true : false;
        System.out.println(flags);  //true
        Integer x = new Integer("10");
        Integer y = new Integer("10");
        System.out.println(x.equals(y));  //true
        System.out.println(x == y);       //false
    }
}
```

### 5、常用面试题

( 1 ）简述＆和＆＆，以及｜和｜｜的区别。

> 答：& 和 |是位运算符，不常用；&& 和||是逻辑运算符，通常在if、while、for中使用。

( 2 ）运行short s1 = 1, s1 = s1 + 1 ；会出现什么结果？运行short s1 = 1; s1 += 1 ；又会出现什么结果？

> 答：运行第一个会报错，因为1是`int`类型，而s是`short`类型，通过+运算后s1自动转换成`int`型。错误提示：Error:(21, 17) java: 不兼容的类型: 从`int`转换到`short`可能会有损失
> 	   运行第二个是正确的，s1=2，+1是`int`类型的操作，s1自动转换`int`类型 

( 3 ）用最高效率的方法算出2 乘以8 等于多少。

> 移位运算符：`int i = 2 << 3`; 

( 4 ) “ ＝＝ ”和equals 万法有什么区别

> ==在基本类型，比较值是否相等，在封装类型中比较内存中的地址
> equals在封装类型中比较值是否相等，

( 5 )` Integer` 与`int`的区别是什么。

> `Integer`是封装类型，包含`int`基本类型的基本操作，`int`是基本类型

> 12 , -12		

( 7 )` float` 型`float f = 3.4 `是否正确？

> No！！精度不正确，`float f = 3.4f`或者`float f = (float)3.4`



## 2、流程控制

### 1、if ...else

以闰年为例

```java
public class BasicGrammer {
    public static void main(String[] args) {
       int year = 2020;
       if((year % 4 ==0 && year % 100 != 0) || (year % 400 == 0)){
           System.out.println("yes");
       }else {
           System.out.println("no");
       }
    }
}
```

注：==**在表达式不要多次使用逻辑表达式&&和||，如果需求很复杂，应分解多个if...else语句**==

### 2、避免短路效应

这里主要讲&&跟||在if语句中使用！

- if（表达式1 && 表达式2）若表达式1为false，那么不管表达式2是true or false，都不会执行表达式2，if语句为false，就没有意义了

- if（表达式1 || 表达式2）若表达式1为true，那么不管表达式2是true or false，都不会执行表达式2，if语句为true

	```java
	public class BasicGrammer {
	    public static void main(String[] args) {
	    int a = 2 ;
	    int b = 3 ;
	    if(a > 0 || b-- > a ){
	        System.out.println(b);   //b = 3
	    }if (a < 0 && ++b > 0){
	        }else {
	            System.out.println(b);  // b = 3
	        }
	    }
	}
	```

### 3、`swtich`中的`default`和`break`

```java
public class BasicGrammer {
    public static void main(String[] args) {
        char grade = 'A';
        switch (grade){
            case 'A':{
                System.out.println("1000");
                break;
            }
            case 'B':{
                System.out.println("8000");
                break;
            }
            case 'C':{
                System.out.println("600");
                break;
            }
            case 'D':{
                System.out.println("400");
                break;
            }
            case 'E':{
                System.out.println("200");
                break;
            } default:{
                System.out.println("nice");
            }
        }
    }
}
```

**对于每个case语句，都要加上break，若没有break，则输出就变了，没加break的case都会输出！！**如果不加`break` ，即使执行完本部台case 分支后，还会执行后继的分支语句。

==不加default不会报错，但强烈加上！！！==



### 4、常用面试题

( 1 ) switch 语句能否作用在byte 、long 、String 上？

> - 可以用在`byte、int、short、char`以及它们的封装类上
> - 不能用在其他基本类型上`long、double、float、boolean`以及封装类
> - jdk1.7及以上，可以用以字符串
> - 可以用于枚举类型

( 2 ）在Java 中，如何跳出当前的多重嵌套循环。

> break or continue ; break跳出整个循环；continue跳出当前循环。

( 3 ) while 和do while 有什么区别？

> - while是先判断再执行；do...while是先执行再判断，同等条件下，后者多执行了一次。

( 4 ）你有没有用过关键字`goto` ？并简述你的看法。

> 不建议使用！会破坏程序的结构，可读性变差

## 3、String对象

>  String 对象虽然简单，但会召出“内存内容不可变”

### 1、String定义变量和常量

```java
String str = "abc";               //定义一个常量，存放在常量池中
String str ＝new String ("abc");  //定义了一个变量，存放在堆空间
```

- String 常量存放在常量油中， Java 虚拟机出于优化的考虑1 会让内容一致的对象共享内存块，但变量是放在堆空间中的， new意义的不同变量内存地址不会相同。
- String 常量连接常量，还是常量，依然用常量池管理，但**变量连接常量就是变量**了。

### 2、String 来了解“内存值不可变”

```java
    ......main方法省略
	String a =” 123456789 ”;
    System.out.println(a.substring(0,5)) ;//12345
    String b = "123456789 ”;             //假设1000 号内存放123456789
    b.substring(0,5);                   //开辟了2000 号内存，存放12345，但没返回，b值没变
    //b = b.substring(0,5);              // 开辟3000 号内存，存放12345
    System.out.println(b);               //1000 号内存依然是123456789
```

- **尽可能使用常量**，如`String a ＝"123 ”`，**避免使用变量**，如`String a = new String("123 ”）`。
- 尽量避免大规模地针对String 的（如连接字符串）操作，因为这样会频繁地主成内存碎片，导致内存性能问题。如果遇到这种业务需求，应改用后面提到的`StringBuilder`对象。
- 如果确实有需求，应采用`c = c.replace （'1','2'）`， 的写法，而不是直接写成`c .replace( '1','2')`。

### 3、String 和String Builder 的区别查看内存优化

> 频繁对字符串进行操作，应使用`StringBuilder`，它是可变的，不会像`String`产生内存碎片

```java
public class StringBuilderdemo {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();  //创建一个对象
        sb.append("yhx love ").append("lwh");    
        //sb = sb.append("yhx love ").append("lwh");是错误的 
        System.out.println(sb.substring(3,8));    //love
        //String val = sb.substring(3,8);而不是StringBuilder接收对象 
        System.out.println(sb);                  //yhx love lwh
        sb.replace(3,8," very like");             //替换
        System.out.println(sb);                    //yhx very like lwh
        sb.deleteCharAt(0);
        System.out.println(sb);                    //hx very like lwh
    }
}
```

**总结：**

> - `StringBuilder`线程不安全，`StringBuffer`线程安全
> - 为了保持线程安全的特性，`StringBuffer`性能略低于`StringBuilder`
> - 单线程情况下，使用`StringBuilder`

### 4、会被不知不觉调用的`toString`方法

```java
public class toString {
    public static void main(String[] args) {
        int val = 1001;
        System.out.println(val);
        float floatValue = 10;
        System.out.println(floatValue);
        char c = 'a';
        System.out.println(c);
        Integer integer = new Integer("1001");
        System.out.println(integer.toString());
        StringBuilder builder = new StringBuilder();
        builder.append("1001");
        System.out.println(builder);
        System.out.println(builder.toString());
    }
}

```

- `System.out.println() `方法能根据不同类型的输入参数（如`int`、`float` 、`lnteger`等）适当地完成输出

	动作，称为多态。

- 当自己定义类时，如果要让这个类输出合适值，需要在冥中自己定义`toStirng`方法。

### 5、String对象需注意

- `String a ＝” 123” `通过这种方式定义的是常量，` String a ＝new String("123");`，通过这种方式定义的是变量。
- **＝＝ 是比较地址值**是否一致，而**`equals` 是比较内容**是否一致。
- `String `的**常量（不仅是String，Integer等常量也是）是放在常量池中**的，**值相同**的常量是**共享同一块**内存的，通过＝＝比较它们的内存地址值是相同的。
- 通过`String `定义出来的值在内存中是不可变的，如果频繁地操作`String `，会产生内存碎片，不幸IJ于内存性能管理，遇到这种情况，建议使用`String Builder`和`String Buffer`,如果在单线程情况下，出于性能因素的考虑，建议使用`String Builder`。
- 在通过`System.out. println` 输出时，默认I也会调用`toString `方法，如果自己定义类，可以通过`toString`来定义类的输出结果。

### 6、常见面试题

( 1 ) `String`咱是最基本的数据类型吗？能不能被继承。

> 不能被继承，它只是一个对象

( 2 ) `String s ＝new String("xyz")；`，创建了几个String 对象？二者之间再什么区别。

> 2个对象，一个是内存中的xyz，一个是s，指向xyz

( 3)` String `和`String Buffer`的区别是什么？

> - String频繁操作字符串，会产生内存碎片，不可变
> - String Buffer不会产生内存碎片，可变

( 4) `String Buffer` 相`String Builder`的区别是什么？

> - `StringBuilder`线程不安全，`StringBuffer`线程安全
> - 为了保持线程安全的特性，`StringBuffer`性能略低于`StringBuilder`
> - 单线程情况下，使用`StringBuilder`

( 5) `String` 类是不可变类， 以`String `为例简述什么是不可变类。

> 

( 6) `String a = "12345";` `a.substring(0,2）`；此时a 的值是什么？

> a = "12345"，因为没有返回

( 7) `String a = "1 " ; String b = " 1 ”`，那么`a==b `的值是`true`还是`false `？并说明理由。

> true ，因为a，b都是常量



## 4、封装：类和方法

### 1、类和实例的区别

>  class关键字定义类，new 关键字创建一个对象（实例）

### 2、合理的访问控制符实现封装

- 如果没有特殊的需求， 把类内部的属性变量设置成私有的，通过公有的get 和set 万法来让外部使用。也有不少人为了省事，会设置成public ，如果积累多了会烦琐。
- 如果有特殊需求，把类的构造函数设置成公有的，否则在外部没法使用。
- 尽可能地在类、方法和属性变量前添加访问控制符。

| 访问控制符 | 同类   | 同包     | 子类     | 不同包   |
| ---------- | ------ | -------- | -------- | -------- |
| public     | 能访问 | 能访问   | 能访问   | 能访问   |
| protected  | 能访问 | 能访问   | 能访问   | 不能访问 |
| default    | 能访问 | 能访问   | 不能访问 | 不能访问 |
| private    | 能访问 | 不能访问 | 不能访问 | 不能访问 |

### 3、静态方法和静态变量

>  ==**静态类中只能使用静态方法和静态变量**==
>
> ```java
> public class Static {
>     private static int value = 1001;
>     static void setStatic(){
>         System.out.println(Math.abs(value));
>     }
>     public static void main(String[] args) {
>         Static.setStatic();
>     }
> }
> ```

- 由于可以不用new 就能使用万法， 一些程序员为了省事，会大量定义静态方法；这样会破坏类的封装性，而

	且会增加类之间的耦合度，因此只能在需要时定义静态类。

- 静态变量相当于全局变量，所以只把整个项目中都会用到的变量设置成静态的。

- 在尽可能小的范围中使用静态类和静态方法。

## 5、继承：类的继承和接口的实现

> **从语法角度来看，可以通过extends 来继承父类， 可以通过implements 来实现接口。**
> **从项目角度来看， 一般把通用的代码放入父类和接口中，这样可以避免大面积地重复代码。**

接口和抽象类有什么区别？

> 从设计层面来说，抽象是对类的抽象，是一种模板设计，接口是行为的抽象，是一种行为的规范。

### 1、子类覆盖父类的方法

```java
class person(){
    protected void speck(){}
}
class ChinesePerson extends person(){
     protected void speck(){}
}
```

( 1) 子类方法不能缩小父类方法的访问权限，如在上述第5 行覆盖父类的方法时，修饰符可以是protected ，也可以是更大的public ，但不能是缩小的private 。在父类中定义的private 方法，子类不能看到，所以也不存在缩小访问权限的问题，如果在父类中定义protected 、public 或默认的方法，总是希望子类能用到或重写，但如果缩小范围，如把第5 行的protected 缩小成private ，那么ChinsePerson 的子类就看不到爷爷类（也就是Person类）的speak 方法就会造成“父类方法失传”的问题。
( 2 ）子类方法不能抛出比父类方法更多的异常，关于原因等，我们在后面讲到异常处理时再作分析。

### 2、不能回避的final关键字

>  final 关键字相继承、方法的覆盖高直接的关系。它能作用到类、方法和属性上。

- final作用在类上，此类不能被继承

```java
final class Student{
    String id;
    public String get Id () {return id;}
    public void setid(String id) { this . id = id;}

 class subStudent extends Student {}／／这句话会出错
```

- final作用在方法上，此方法不能被重写(不能被子类覆盖)

```java
class Student{
final void print() {}
}
```

- final作用在属性上，此属性相当于常量，如赋予初始值，该值不能改变

```java
final Stirng a = "1";
String b = "1";
String c = "123";
String d = b + "23"; //此时d是一个变量，不等同于String d = "1" + "23";这个d是常量
System.out.println(c==d);  //false
String e = a + "23"; //e是一个常量，因为final修饰a，a是一个常量
System.out.println(c==e);  //true
//a = "4"; ✖
```

### 3、理解finalize 方法，但别重写

> Java 虚拟机提供了专门的垃坡回收机制，所高用不到的类都会由Java 虚拟机来回收？这个回收动作对程序员来说是透明的。
> 当虚拟机回收类时，会自动地调用该类的finalize 万法，如果类中没定义，会词用Object 类中的finalize ， 而Object 中的finalize 万法是空白的。

- 要了解finalize 方法有什么用，它什么时候会被调用；

- 大家在自定义的类中不要重写finalize 方法，如果写错，就会导致类无法被回收，从而导致

	内存使用量直线上升最后抛出内存溢出的错误。

## 6、多态：同一方法根据不同的输入有不同的作用

### 1、方法重载实现多态

```java
class demo{
    public void print();
    public void print(int row){
        System.out.println("with 1 int param");
    }
    public void print(int row,int column){
        System.out.println("with 2 int param");
    }
    public void print(int row,String type){
        System.out.println("with 1 int param,with 1 String param");
    }
    ...
}
```

**注：**

1. 判断重载时，Java编译器只看参数类型，不看参数名

	```java
	public void print(int row)
	public void print(int column)
	```

2. 修改方法的返回值实现重载

	```java
	public void print(int row,int column)
	public String print(int row,int column)
	```

### 2、方法的重载和覆盖

> 方法的重载（ Overload ）和覆盖（ Override ）是面试时常考的点

```java
class Base{
 public void print () { )
 //是重载，因为参数不同
 public void print （int row) {)
 //是重载，因为参数类型不同
 public void print(String type){}
 class Child extends Base{
 //是覆盖，覆盖了父类的同名无参方法
 public void print() {}
 //出锚， 因为父类已经有带int 参的同名方法
 public String print(int row){}
 //是覆盖， 覆盖了父类的带String 参的同名方法
 public void print(String type){}
     //是重载，因为参数个数不同
 public void print(int row, String type ) {}
 }

 publiC class DifferenceDemo {
 public static void main (String [] args) {
 }
```

- 如果方法名、参数和返回类型都一致，那么可以说是子类的方法覆盖了父类的方法。

- 如果方法同名、参数个数或类型不同，这时无须看返回类型，这种情况属于重载。

- 当把子类中的方法移动到父类后，两个同名方法参数完全一致，但返回类型不同，这种属于语法错误， Java 

	编译器会报“定义了重复方法”的锚误。

### 3、构造函数能重载但不能覆盖，兼说`this` 和`super`

> 构造函数是一种特殊的、不带返回值的方法，它在创建类（如new ）时被调用。

**错误示范：**

```java
class parent{
    private int val;
    public parent(int val){this.val = val}
}
class Subclass extends parent{
    public Subclass(int i){  //系统默认添加super()调用父类无参构造，而父类中没有无参构造
        //super(i);        法2:父类中加入无参构造
    }  
}
```

**经常用到的写法：**

```java
class Parent{
    private int value ;
    private String msg ;
    //在不带参的构造函数中通过this 调用带两个参的构造函数
    public Parent(){ this(O ,” call with No Param Function ”);}
    public Parent (int val )
    {this . value =val ;  //通过this 给本类的value 赋值
     System.out.println ( ” with one param ”);
    }
    public Parent (int val , String msg){
        System.out.println (”msg is :”+ msg) ;
        System.out.println (” with 2 param”);
    }
}
class Subclass extends Parent{
    public Subclass (int i) {//通过super ，调用父类的带一个参的构造函数
        super(i);
        System.out.println （” in subclass ”);
    }
}
public class ConstructDemo {
    public static void main(String[] args) {
        Parent pO =new Parent();    //msg is:call with No Param Function + with 2 param
        Parent pl ＝new SubClass(l); // with one param + in subclass
        Parent p2 ＝new Parent(l ，”new Parent”); //msg is new Parent + with 2 param
        //Subclass s =new Parent(l); //error
    }
}
```

**项目中在有继承的情况下重载构造函数的常规用法:**

- 在一个类中可以定义多个构造函数，在构造函数中，经常能看到`this. value = xx`的写法， 这是根据输入的参数给本对象的属性赋值， 也能经常看到`this(xx , " xxx ")`的做法，这是在一个构造函数中调用真他的问造函数。

- 在子类的构造函数体中，如果什么代码都不写， Java 编译器会默认加上`super() `,以调用父类中的不带参的构造 函数，如果父类中没有不带参的问造函数，会提示语法错误。要解决这个语法错误，可以在父类中定义一个带参的构造函数，或者加上`super`语句。

> this 指向所在方法的本类
>
> - 场景一：`this.value = xxx`，根据输入的参数给本对象复制
> - 场景二：`this(O ,xxx )`，调用本类的带有两个参数的构造方法
>
> super 是指向所在方法的父类，如这里是用super(xx)来调用父类的构造函数

### 4、通过多态减少代码修改成本

> 多态要解决的问题是：把抽象的业务逻辑和具体的实施细节分离，通俗地讲，多态可以分为“做什么”和“怎么做”。

多态有“**修改点隔富**”和“**无障碍扩展**”两大好处。

- “修改点隔离”是指当用户修改了业务方法的内部实现细节后，调用者可以在毫不知情的情况下继续使用，
- “无障碍扩展”是指如果遇到添加新功能的需求时，可以在不大量修改现有代码的前提下完成添加工作。

**继承+覆盖**

```java
abstract class Employee
{ protected abstract void talkBusiness() ;
 //Sales 继承Employee
 class Sales extends Employee {
     public void talkBusiness()
     {System.out.println(” Sales talk business. ”);}
 }
 //Manager 继承Employee
 class Manager extends Employee{
     public void talkBusiness ()
     {System.out.println (”Manager talk business.”);}
 }
 public class ExpandDemo {
     public static void main(String[) args){
         Employee sales ＝new Sales();
         sales.talkBusiness();  //Sales talk business
         Employee manager ＝new Manager();
         manager.talkBusiness(); //Manager talk business.
     }
}
```

- 如果要修改业务员和客户交流的业务代码，只需修改Sa l e 类的方法，无须更改其他的类和方法。
- 如果公司要增加新的业务，如要加上“总经理和客户交流”的业务动作，只需新增一个总经理类，让它继承Employee 类，然后在其中定义talkBusiness 方法即可。

### 5、面向对象思想的常用面试题

1、开放性问题，简述你对面向对象思想的了解？

> 要点1 ：先说基础概念，如面向对象思想包括封装、继承、多态，再说些语法，如可以通过Extends 继承类、通过Implement 来实现接口。
> 要点2 ：要结合具体实际简述在你做过的项目中1面向对象思想带来的具体好处，如结合一个具体的例子（如电信系统），介绍一下把方法都定义到父类中，然后通过继承子类来扩展，能改善代码结构，通过多~某减少代码修改后的维护量。注意不能只说理论，要结合你项目中的例子。
>
> 

2、接口和抽象类有什么区别？

> 从设计层面来说，抽象是对类的抽象，是一种模板设计，接口是行为的抽象，是一种行为的规范。

3、重载（ Overload ）和覆盖（ Override ）的区别是什么？

> | 区别     | 重载     | 重写（覆盖）                                   |
> | -------- | -------- | ---------------------------------------------- |
> | 参数列表 | 必须修改 | 一定不能修改                                   |
> | 返回类型 | 可以修改 | 一定不能修改                                   |
> | 异常     | 可以修改 | 可以减少或删除，一定不能抛出新的或者更广的异常 |
> | 访问     | 可以修改 | 一定不能做更严格的限制（可以降低限制）         |
>
> 方法的重写(Overriding)和重载(Overloading)是java多态性的不同表现，重写是父类与子类之间多态性的一种表现，重载可以理解成多态的具体表现形式。
>
> - (1)方法重载是一个类中定义了多个方法名相同,而他们的参数的数量不同或数量相同而类型和次序不同,则称为方法的重载(Overloading)。
> - (2)方法重写是在子类存在方法与父类的方法的名字相同,而且参数的个数与类型一样,返回值也一样的方法,就称为重写(Overriding)。
> - (3)方法重载是一个类的多态性表现,而方法重写是子类与父类的一种多态性表现。

4、this 和super的含义

> this 指向所在方法的本类
>
> - 场景一：`this.value = xxx`，根据输入的参数给本对象复制
> - 场景二：`this(O ,xxx )`，调用本类的带有两个参数的构造方法
>
> super 是指向所在方法的父类，如这里是用super(xx)来调用父类的构造函数

5、finalize方法有什么作用。

> - finalize 方法在类被回收时被调用； 
>
> - 一般在项目中不会重写这个万法，因为可能会引发内存无法回收的问题

6、final 关键的含义。

> - final作用在类上，此类不能被继承
> - 作用在方法上，不能被重写（不能被子类覆盖）
> - 作用在属性上，相当于常量，如赋予初始值，则该值不能被改变

7、构造函数能否被覆盖，能否被重载？

> 可以重载，不能覆盖

8、静态变量和实例变量的区别有哪些？

> - 静态变量存储在方法区,属于类所有.实例变量存储在堆当中,其引用存在当前线程栈.
> - 语法上：静态变量加static，实例变量不加
> - 静态变量不需要创建实例对象就可以直接使用类名进行引用；实例变量只有在创建了实例对象之后才会被分配空间，才可以使用
> - 静态变量属于类，该类不生产对象，通过类名就可以调用静态变量；实例变量属于该类的对象，必须产生该类对象，才能调用实例变量。

9、是否可以从一个static 方法内部发出对非static方法的调用。

> 不可以。static方法是静态方法，是属于类的方法，非static方法是属于对象的方法。因为非static方法是要与对象关联在一起的，必须在创建出一个对象后，才可以通过这个对象调用非static方法；而static方法可以直接通过类名来调用，不需要创建对象。也就是说，在一个static方法被调用时，还可能没有创建任何实例对象，此时如果从static内部发出对非static方法的调用，非static方法是无法关联到对象的。
>
> **static的意义在于方便在没有创建对象的情况下来进行调用（方法/变量）。**

10、简述作用域public 、private 、protected 及不写时的区别。

> | 访问控制符 | 同类   | 同包     | 子类     | 不同包   |
> | ---------- | ------ | -------- | -------- | -------- |
> | public     | 能访问 | 能访问   | 能访问   | 能访问   |
> | protected  | 能访问 | 能访问   | 能访问   | 不能访问 |
> | default    | 能访问 | 能访问   | 不能访问 | 不能访问 |
> | private    | 能访问 | 不能访问 | 不能访问 | 不能访问 |

# 3、集合类和常用的数据结构

## 1、常用的集合对象

> 集合类存放于Java.util包中，主要有3种：set(集）、list(列表包含Queue）和map(映射)。
>
> - Collection：Collection是集合List、Set、Queue的最基本的接口。
>
> - Iterator：迭代器，可以通过迭代器遍历集合中的数据 
>
> - Map：是映射表的基础接口

**集合分为两类：**

 **一类是以Collection为基类的线性表类，如数组和Arraylist 等**

**另一类是以Map 为基类的键值对类，如Hash Map 和Hash Table 。**

==集合是容器，冥中不仅可以存储String 、int 等数据类型，还可以存储自定义的Class 类型数据。==

### 1、通过数组来观察线性表类集合的常见用法

```java
class Person { int id; }
//这个是主类，在其中将定义针对的数组的操作
public class ArrayDemo{
    public static void main(String [] args){
        //定义了两个数组，但a 没有被初始化
        Person[] a =null;
        Person [] b = new Person [3];
        //通过for 循环，为数组赋值
        for(int i = 0; i < b.length;i++){
            //将Person 对象放入数组中
            b[i] ＝new Person();
        }
        //在Person 数组中存放Person 对象
        Person [] c = {new Person(),new Person(),new Person() };
        //3 会报错
        //!System.out.println(” a.length ＝ ” ＋ a.length);
        System.out.println (”b.length =”+ b.length);//输出是3
        int[] intArr = {1,2,3);
                        for(int i = 0; i < intArr.length; i++){
                            System.out.println(intArr[i]);
                        }
                       }
    }
```

**数组的价值在于“存储并管理”业务对象**

> ArrayList 在查找时速度快，LinkedList 在插入与删除时更具优势

### 2、以`HashMap `为代表，观察键值对类型的集合对象

```java
class Book{
    private String IBSN;
    private String name;
    private float price;
    public Book(String IBSN, String name, float price) {
        this.IBSN = IBSN;
        this.name = name;
        this.price = price;
    }
    public String getIBSN() {
        return IBSN;
    }
    public void setIBSN(String IBSN) {
        this.IBSN = IBSN;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public float getPrice() {
        return price;
    }
    public void setPrice(float price) {
        this.price = price;
    }
}
public class HashMapDemo {
    public static void main(String[] args) {
        HashMap map = new HashMap();
        Book java = new Book("123", "java", 123);
        Book book = new Book("456", "spring", 456);
        map.put(java.getName(),"javabook");
        map.put(book.getName(),"book");
        Book bk = null;
        if (map.containsKey("123")){
            bk = (Book) map.get("123");
            System.out.println(bk.getName()+","+bk.getPrice());
        }
    }
}
```

项目中对Map 类对象的常见用法，可以通过put 来存放对象，通过get 方法来获取对象，也可以通过`containsKey `方法来判断是否存在某个对象。

### 3、Set 类集合的使用场景

> 从存储数据的恪式上来看， Set 也属于线性表，但在真中不能存储重复的元素，而且，在一个Set 对象里最多只能存储一个null 元素。由于Set 南“自动去重”的特性，在项目中常用它来整理数据。

```java
public class SetDemo {
    public static void main(String[] args) {
        ArrayList list = new ArrayList();
        list.add("1");
        list.add("2");
        list.add("1");
        list.add(null);
        list.add(null);
        HashSet set = new HashSet();
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
            set.add(i);
        }
        System.out.println(set.size());
        for (Object o : set) {
            System.out.println(o);
        }
    }
}
```

### 4、学习线性表类集合，你必须掌握这些知识































































# JAVA多线程

```java
public class test {
    public static void main(String[] args) {
        Object o = new Object();
        System.out.println(ClassLayout.parseInstance(o).toPrintable());
        synchronized (o) {
            System.out.println(ClassLayout.parseInstance(o).toPrintable());
        }
    }
}
====================================run的结果=========================================

java.lang.Object object internals:
 OFFSET  SIZE   TYPE DESCRIPTION                               VALUE
      0     4        (object header)                           01 00 00 00 (00000001 00000000 00000000 00000000) (1)
      4     4        (object header)                           00 00 00 00 (00000000 00000000 00000000 00000000) (0)
      8     4        (object header)                           e5 01 00 20 (11100101 00000001 00000000 00100000) (536871397)
     12     4        (loss due to the next object alignment)
Instance size: 16 bytes
Space losses: 0 bytes internal + 4 bytes external = 4 bytes total
//上锁之后，由无锁变成了轻量级锁
java.lang.Object object internals:
 OFFSET  SIZE   TYPE DESCRIPTION                               VALUE
      0     4        (object header)                           58 f7 bb 02 (01011000 11110111 10111011 00000010) (45872984)
      4     4        (object header)                           00 00 00 00 (00000000 00000000 00000000 00000000) (0)
      8     4        (object header)                           e5 01 00 20 (11100101 00000001 00000000 00100000) (536871397)
     12     4        (loss due to the next object alignment)
Instance size: 16 bytes
Space losses: 0 bytes internal + 4 bytes external = 4 bytes total
    
    /*第一部分markword,用于存储对象自身的运行时数据，如哈希码（HashCode）、GC分代年龄、锁状态标志、线程持有的锁、偏向线程ID、偏向时间戳等，这部分数据的长度在32位和64位的虚拟机（未开启压缩指针）中分别为32bit和64bit，官方称它为“MarkWord”。

	对象头的另外一部分是klass,类型指针，即对象指向它的类元数据的指针，虚拟机通过这个指针来确定这个对象是哪个类的实例.
	
	内存空间必须被8整除，最后四个字节是后面补上去的
	*/
```



![image-20200318121626707](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200318121626707.png)

![image-20200323125814495](D:\360MoveData\Users\Administrator\Desktop\JAVA复习.assets\image-20200323125814495.png)

![image-20200318121649073](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200318121649073.png)

**偏向锁是否一定提高效率？why?**

不一定。在明确知道多个线程竞争的时候，系统会把资源大量消耗在撤销上。

