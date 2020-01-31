# Chapter2 Spring面向切面编程

---

### AOP基本概念

1. **AOP**是**Aspect Oriented Programming** 的缩写，意思是面向切面编程。AOP是为了解决程序中的共性问题，通过预编译方式和运行期动态代理的方式，在不修改源代码的情况下，为对象添加新的方法和属性。

2. AOP关键术语:

   ①切面（Aspect）:是公有功能的实现，如日志切面，权限切面和事务切面等。在实际的应用中，通常是一个存放公共功能实现的普通Java类，其之所以能被AOP容器识别成切面，是在配置中指定的。

   ②通知（Advice）:即增强，是切面的具体实现。以目标方法为参照点，根据放置的地方不同，可分为前置增强，后置增强，异常增强，环绕增强和引介增强5中，在实际应用中，其通常是切面类中的一个方法，至于具体属于哪类通知，同样是在配置中指定的。

   ③连接点（Joinpoint）:程序在运行过程中能够插入切面的地点。例如:方法调用，异常抛出或字段修改等，但Spring仅支持方法级的连接点。

   ④切入点（Pointcut）:用于定义通知应该切入到那些连接点上。不同的通知通常需要切入到不同的连接点上，这种精准的匹配是由切入点的正则表达式来定义的。

   ⑤目标对象（Target）:即将切入切面的对象，即那些被通知的对象。这些对象中已经只剩下纯粹的核心业务逻辑代码，所有的共有功能代码均等待AOP容器的切入。

   ⑥代理对象（Proxy）:将通知应用到目标对象之后被动态创建的对象。

   ⑦织入（Weaving）:将切面应用的目标对象从而创建一个新的代理对象的过程。这个过程可以发生在编译期，类装载期及运行期。

3. 增强类型介绍

   (1)前置增强:org.springframework.aop.BeforeAdvice代表前置增强，表示在目标方法执行前实施增强。

   (2)后置增强:org.springframework.aop.AfterReturningAdvice代表后置增强，表示在目标方法后执行。

   (3)环绕增强:org.springframework.intercept.MethodInterceptor代表环绕增强，表示在目标方法执行前后执行增强。

   (4)异常增强:org.springframework.aop.ThrowsAdvice代表抛出异常增强，表示在目标方法抛出异常后实施增强。

   (5)引介增强:org.springframework.aop.IntroductionInterceptor代表引介增强，表示在目标类中添加一些新的属性和方法。

4. 例:使用AOP完成后置增强

   (1)创建Web工程，添加Spring支持，选择Spring对AOP的支持。

   (2)在Bean包中创建客户类接口，即目标对象接口。定义buyIpadMini方法。

   ```java
   package com.proc.bean;
   public interface ICustomer{
       public void buyIpadMini();
   }
   ```

   (3)在Bean包中创建客户类，事项Icustomer接口和接口中定义的方法:

   ```java
   package com.proc.bean;
   public class Customer implements ICustomer{
       public void buyIpadMini(){
           System.out.pringln("购买了一台iPad Mini");
       }
   }
   ```

   (4)在Advice包中创建后置增强，实现AfterReturningAdvice接口和接口中定义的方法。

   ```java
   package com.proc.advice;
   import java.lang.reflect.Method
   import org.springframework.aop.AfterRerurningAdvice;
   public class SurveyAfterAdvice implements AfterReturningAdvice{
       public void afterReturning(Object returnObj,Method method,Object[] args,
       Object obj)throws Throwable{
           System.out.pringln("帮助Apple公司完成问卷，送给用户一个贴膜。");
       }
   }
   ```

   (5)在applicationContext.xml文件中分别注入后置增强，目标对象和代理对象

   ```java
   //注入后置增强
   <bean id = "surveyAfterAfvice" class = "com.proc.advice.SurveyAfterAdvice"></bean>
   //注入目标对象
   <bean id = "target" class = "com.proc.bean.Customer"></bean>
   //定义代理对象:proxyInterfaces:织入目标对象接口；target-ref：织入目标引用;interceptorNames:
   //织入后置增强
   <bean id = "customerbean" class = "org.springframework.aop.framework.ProxyFactoryBean"
   p:proxyInterfaces = "com.proc.bean.ICustomer" p:target-ref = "target" p:interceptorNames
   ="surveyAfterAfvice"></bean>
   ```

   (6)测试类

   ```java
   public class TestMain{
       public static void main(String[] args){
           ApplicationContext context = new 
           ClassPathXmlApplicationContext("applicationContext.xml");
           ICustomer cus = (ICustomer)context.getBean("customerBean");
           cus.buyIpadMini();
       }
   }
   ```

   <!--因为要实现增强用的并不是Customer对象，而是Spring存的Proxy（代理）对象,所以在测试中用接口声明是多态的一种方式，即cus用接口声明，但实现的是代理对象。-->

   (7)输出结果:

   ```
   购买了一台IPad mini
   帮助Apple公司完成问卷，送给客户一张贴膜。
   ```


### 正则表达式方式匹配切面

有时，仅通过AOP简单切面是远远不够的，这种描述方式无法面对较为复杂的业务。假设目标对象中有多个方法，在执行中，我们要让某几个方法在执行时才获得增强，就可以使用正则表达式进行匹配描述来完成。

1. 修改ICustomer接口，定义一个buyIphone方法，并在Customer类中实现该方法

   ```java
   pacakge com.proc.bean;
   public class Customer implements ICustomer{
       //增加一个购买iPhone的方法
       public void buyIphone(){
           System.out.println("购买了一台iPhoneX！");
       }
       public void buyIpadMini(){
           System.out.println("购买了一台iPad mini");
       }
   }
   ```

2. 在applicationContext.xml文件中注入正则表达式方式匹配切面，并通过advice-ref属性引入已经实现的后置增强

   ```java
   //正则表达式方式匹配切面的切面实现类
   <bean id = "surveyRegexAdvice" class = "org.springframework.aop.support.RegexpMethodPointcutAdvisor" p:advice-ref = "surveyAfterAdvice">
   <property name = "pattern" value = ".*IpadMini.*"></property>
   </bean>
   ```

3. 在applicationContext.xml文件中修改CustomerBean代理对象，织入正则表达式匹配切面

   ```
   <bean ..... p:interceptorNames = "surveyRegexpAdvice"></bean>
   ```

   