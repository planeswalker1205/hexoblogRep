# JAVA复习

1.Java的版本

​	JavaSE：Java SE是Java的标准版，主要用于桌面应用程序的开发，同时也是Java的基础，它包含Java语言基础，JDBC（Java数据库连接）操作，I/O（输入/输出），网络通信，多线程等技术

​	JavaEE：Java EE是Java的企业版，主要用于开发企业级分布式的网络程序，如电子商务网站和ERP（企业资源规划）系统，其核心为EJB（企业Java组件模型）

​	JavaME：Java ME主要应用于嵌入式系统开发，如掌上电脑，手机登移动通信电子设备，现在大部分手机厂商生产的手机都支持Java技术

2.Java语言的特性

​	2.1.面向对象

​	2.2.分布性

​	2.3.可移植性

​	2.4.解释型

​	2.5.安全性

​	2.6.健壮性

​	2.7.多线程

​	2.8.高性能

​	2.9.动态

3.逻辑运算符“&&”和“&”的区别

​	逻辑运算符“&&”与“&”都表示“逻辑与”，那么它们之间的区别：当两个表达式都为true时，“逻辑与”的结果才会是true。使用逻辑运算符运算符“&”会判断两个表达式；而逻辑运算符“&&”则是针对boolean类型的类进行判断。当第一个表达式为false时则不去判断第二个表达式，直接输出结果从而节省计算机判断的次数。通常将这种在逻辑表达式中从左端的表达式可推断出整个表达式的值称为“短路”，而那些始终执行逻辑运算符两边的表达式称为“非短路”。故“&&”属于“短路”运算符，而“&”则属于“非短路运算符”

4.移位操作

<<:左移； >>:右移； >>>:无符号右移

如：

```
int x = 2;
int y = x<<3;
```

此时y的值就相当于2*2的三次方，如果是右移的话就相当于2/2的n次方

5.对象类型的转换

(1)向上转型

创建Quadrangle（四边形）类并定义draw方法，参数是Quadrangle类型的a

```
class Quadrangle{
    public static void draw(Quarangle a){
        
    }
}
```

创建Parallelogram（平行四边形）类,继承Quadrangle.在主方法中调用draw方法，参数是Parallelogram类型

```
class Parallelogram extends Quadrangle{
    public static void main(String[] args){
        Parallelogram p = new Parallelogram();
        draw(p);
    }
}
```

像上述代码，把子类对象赋值给父类类型的变量，这种技术称为向上转型。相当于`Quadrangle q = new Parallelogram();`

(2)向下转型

如果将父类的对象直接赋予子类，会发生编译器错误，因为父类对象不一定是子类的实例，例如，一个四边形不一定就是一个平行四边形，所以这时需要告诉编译器这个四边形就是平行四边形，。将父类对象强制转换成某个子类对象，这种方式成为显示类型转换。

```
Quadrangle q = new Quadrangle();
Parallelogram p = (Parallelogram)q;
```

当程序在执行向下转型时操作时，如果父类对象不是子类对象的实例，就会发生ClassCastException异常，所以在执行向下转型之前需要养成一个良好的习惯，那就是判断是否一个类实现了某个接口。可使用instanceof操作符。

<!--向下转型需要考虑安全性，如果父类引用的对象是父类本身，那么在向下转型的过程中是不安全的，编译不会出错，但是运行时会出现java.lang.ClassCastException错误。它可以使用instanceof来避免出错此类错误即能否向下转型，只有先经过向上转型的对象才能继续向下转型。 -->

6.内部类

1）成员内部类

```
public class outterclass{
	innerclass in = new innerclass();
    class innerclass{
    	private int a = 10;
        public void innermethod(){
            System.out.println("内部类方法");
            int y = a;
        }
        public innerclass newinnerclass(){
            return new innerclass();
        }
        public static void main(String[] args){
            outterclass out = new outterclass();
            innerclass inner = out.new innerclass();
            //上一行等同于:innerclass inner = out.in;
            innerclass inner = out.newinnerclass();
        }
    }
}
```

内部类对象的实例化操作必须在外部类或外部类的**非静态**方法中实现。内部类可以访问它的外部类成员，但内部类成员只有在内部类的范围之内是可知的，不能被外部类使用。

**由于是要在主方法中实例化一个内部类对象。所以必须要在new操作符之前提供一个外部类的引用。**

注：内部类对象会依赖于外部类对象。除非已经存在一个外部类对象，否则类中不会存在内部类对象。

如果将一个权限修饰符为private的内部类向上转型其为父类对象，或者直接向上转型为一个接口，在程序中就可以完全隐藏内部类的具体实现过程。可以在外部提供一个接口，在接口中声明一个方法。如果在实现该接口的内部类中实现该接口的方法，就可以定义多个内部类以不同的方式实现接口中的同一个方法，而在一般的类中，是不能多次实现接口中的同一个方法。（这种技巧经常被应用在Swing编程中，可以在一个类中做出多个不同的响应事件。

<!--当外部类的成员变量和内部类的成员变量名称相同时，可以使用this关键字进行区分。比如：外部类outclass和内部类innerclass中都有相同的变量名称x，那么在内部类中使用内部类的x成员变量时就可以使用this.x，当要使用外部的成员变量x时，就可以使用outclass.this.x调用-->

(2)局部内部类

内部类不仅可以在类中定义，也可以在类的局部位置定义。如在类的方法或者任意的作用域中均可以定义内部类。

```java
interface outinterface{
    
}
class Outclass3{
    public outinterface doit(final String x){
        class innerclass3 implements outinterface{
            innerclass3(String s){
                s=x;
                System.out.println(s);
            }
            return new innerclass3("doit");
        }
    }
}
```

(3)匿名内部类

```java
public Outinterface tests(){//Outinterface 是一个接口
        return new Outinterface(){
           public void remark(){
               System.out.println("一个匿名内部类");
           }
        };
    }
```

实质上这种匿名内部类的作用就是创建一个实现于Outinterface接口的匿名类对象。匿名内部类结束后，需要使用分号作为标识，这个分号并不是代表定义内部类结束的标识，而是代表创建Outinterface引用表达式的标识。比如Integer i = new Integer();此处分号就是这个意思

注：new的是谁那么继承的就是哪个类，所以在匿名类定义非继承类的方法也不能在外面使用。

注:匿名内部类如果用在方法的参数里面的话，那么它不是用在定义方法的时候，而是应该用在实现（调用）方法的时候。

(4)静态内部类

在内部类前添加修饰符static，这个内部类就变成静态内部类了。一个静态内部类中可以声明static成员，但是在非静态内部类中不可以声明静态成员，静态内部类有一个最大的特点，就是不可以使用外部类的非静态成员。

静态内部类具有以下两个特点：

```
1.如果创建静态内部类的对象，不需要其外部类的对象。
2.不能从静态内部类的对象中访问非静态外部类的对象。
```

(5)内部类的继承

```
public class Outputinnerclass extends classA.classB{
    public Outputinnerclass(classA a){
        a.super();
    }
}

public class classA{
    class classB{
        
    }
}
```

上述为Outputinnerclass继承类classA中的内部类classB。

在某个类继承内部类时，必须硬性给予这个类一个带参数的构造方法。并且该构造方法的参数为需要继承内部类的外部类的引用，同时在构造方法体中使用a.super()语句，这样才为继承提供了必要的对象引用。

