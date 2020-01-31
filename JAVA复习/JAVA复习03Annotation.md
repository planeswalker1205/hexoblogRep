# JAVA复习02 IO

### 一、Annotation

1.如果在定义Annotation类型时将@Retention设置为RetentionPolicy.RUNTIME，那么在运行程序时通过反射就可以获取到相关的Annotation信息，如获取构造方法，字段，和方法的Annotation信息。

2.类Constructor、Field和Method均继承了AccessibleObject类，在AccessibleObject中定义了3个关于Annotation的方法，其中方法isAnnotationPresent(Class<?extends Annotation>annotationClass)用来查看是否添加了指定类型的Annotation，如果是则返回TRue，否则返回false；方法getAnnotation(Class\<T>annotationClass)用来获得指定类型的Annotation，如果存在则返回相应的对象，否则返回null；方法getAnnotations()用来获得所有的Annotation，该方法将返回一个Annotation数组。在类Constructor和Method中还定义了方法getParameterAnnotations(),用来获得为所有参数他添加的Annotation,将以Annotation类型的二维数组返回，在数组中的顺序与声明的顺序相同，如果没有参数则返回一个长度为0的额数组，如果存在未添加Annotation的参数，将用一个长度为0的嵌套数组占位。

### 二、多线程

#####在Java中主要通过两种方式实现线程，分别为继承Java.lang.Thread类与实现java.lang.Runnable接口。

1.继承Thread类

Thread类是Java.lang包中的一个类，从这个类中实例化的对象代表一个线程，程序员启动一个新线程需要建立Thread实例，

```
public Thread();//创建一个新的线程对象
public Thread(String threadName);//创建一个名称为threadName的线程对象。
```

### .

完成线程真正的代码放在类的run()方法中。 当一个类继承Thread类时，就可以在该类中覆盖run方法。Thread对象需要一个任务执行。任务是指线程在启动时执行的工作。该工作的功能代码被写在run方法中。run方法格式如下:

```
public void run(){
    
}
```

当执行一个线程程序时，就自动产生一个线程，主方法正是在这个线程中运行的，当不在启动其他线程时，该线程就为单线程程序。实例如下:

```
public class coursetest extends Thread{
    public void run(){
        int i = 10;
        while(true){
            System.out.println(i);
            i--;
            if(i==0){
                return;
            }
        }
        public static void main(String[] args){
            new Thread().start();
        }
    }
}
```

在main方法中，如果需要使线程执行则需要调用start()方法。

2.如果想要实现多线程，还可以继承Runnable接口