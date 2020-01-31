# Chapter4 Spring @AspectJ与声明式事务

---

### @AspectJ介绍

1. 增强类型

   @AspectJ采用了注解的编码规范，无须在applicationContext.xml文件中配置，仅填写注解属性即可。@AspectJ为各种类型的增强提供了不同的注解类，它们位于org.aspectj.lang.annotation包中。这些注解类拥有若干个成员，可以通过这些成员完成定义切点信息，绑定连接点参数等操作。以下为@AspectJ所提供的几个增强追截。

   ①@Before:前置增强

   ②@AfterReturning:后置增强

   ③@Around:环绕增强

   ④@AfterThrowing:抛出异常增强

   ⑤@After:Final增强，无论是抛出异常还是正常退出，该增强都会得到执行，该增强没有对应的增强接口。

   ⑥@DeclareParents:引介增强

2. 通配符

   切入点表达式通配符:

   @AspectJ支持以下3中通配符

   (1)”*“：匹配任意字符，但其仅能匹配上下文中的一个元素

   (2)"..":匹配任意字符，可以匹配上下文中的多个元素，但在表示类时，必须和 "\*"联合使用，而在表示入参时则单独使用。如"\*com.proc.bean.\*.\*(..)"表示com.proc.bean包下所有类的所有方法。

   (3)"+":表示按类型匹配指定类及其自雷，必须跟在类名之后。如`com.proc.UserService+`表示UserService类及其子类。

---

### 使用@After增强

使用@After实现强制后置增强功能:首先，创建目标对象；其次，创建AspctJ切面类，使用@After注解将普通方法定义为增强方法；然后，在applicationContext.xml文件中，使用aop:aspectj-autoproxy元素添加@AspectJ支持；最后，在测试类中，直接调用目标对象中的方法，查看控制台输出结果。以下:

1. 创建Web工程，添加Spring支持，选择Spring对AOP的支持。

2. 在Bean包中创建Employee对象，定义qqGame方法，lolGame以及work方法。

   ```
   package com.proc.bean;
   public class Employee{
       public void qqGame(){
           System.out.println("聊QQ")；
       }
        public void lolGame(){
           System.out.println("玩LOL")；
       }
        public void work(){
           System.out.println("在工作")；
       }
   }
   ```

3. 在Advice包中创建@AspectJ切面，使用@After注解拦截范围和连接点。

   ```
   package com.proc.advice;
   import org.aspectj.lang.annotation.*;
   @Aspect
   public class Camera{
       @After("execution(* com.proc.bean.Employee.*Game(..))")//拦截Employee类中以Game为结尾名的方法
       public void foundAfter(){
           System.out.pringln("摄像头发现员工在玩游戏!");
       }
   }
   ```
   <!--第一个*号后面是有空格的，代表的是返回值类型，可以写为string，int等-->

4. 在applicationContext.xml文件中使用aop:aspectj-autoproxy元素启动@AspectJ支持，然后依次注入目标对象，切面。

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <beans
   	xmlns="http://www.springframework.org/schema/beans"
   	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xmlns:aop="http://www.springframework.org/schema/aop"//添加AOP命名空间的代码，支持@AspectJ
   	xmlns:p="http://www.springframework.org/schema/p"
   	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"
   	http://www.springframework.org/schema/aop //添加地址
   	http://www.springframework.org/schema/aop/spring-aop-3.0.xsd//添加地址
   	>
   	//启动AspectJ支持
   	<aop:aspectJ-autoproxy></aop:aspectj-autoproxy>
   	//定义aspect
   	<bean id = "camera" class = "com.proc.advice.Camera"></bean>
   	//定义目标对象
   	<bean id = "target" class = "com.proc.bean.Employee"></bean>
   	</beans>
   ```

5. ```
   public class tests{
       public static void main(String[] args){
           ApplicationContext context = new 
           ClassPathXmlApplicationContext("applicationContext.xml");
           Employee emp = context.getBean("target",Employee.class);
           emp.work();
           emp.qqGame();
           emp.lolGame();
       }
   }
   ```

6. ```
   在工作
   聊QQ
   摄像头发现员工在玩游戏
   玩LOL
   摄像头发现员工在玩游戏
   ```

**注:如果有接口，那么测试类中还要以接口来实现类:**

```
EmployeeInterface emp = (EmployeeInterface)context.getBean("target");

emp.work();

emp.qqGame();

emp.lolGame();

```

---

### Spring声明式事务

​	Spring声明式事务使用AOP机制实现，只需要在配置文件中配置即可使用，而不需要修改Java代码，同时，也增强了代码的可维护性。

​	Spring对声明式事务配置只需要在applicationContext.xml文件中配置即可。同时，在编译器的“行号侧栏”中，能够查看是否配置正确。例如，AOP的事务增强会出现图标，被拦截的方法会出现图标。代码如下。

(1)定义事务管理器(声明式的事务)

```java
<bean id = "transactionManager" class = "org.springframework.orm.hibernate3.
HibernateTransactionManager">
<property name = "sessionFactory" ref = "sessionFactoryId"></property>
</bean>
```

(2)定义事务切面

```java
<tx:advice id = "txAdvice" transaction-manager = "transactionManager">
<tx:attributes>
	<tx:method name = "*" propagation = "REQIRED"/>
</tx:attributes>
</tx:advice>
```

(3)通过AOP配置提供事务增强，在pointcut参数内填写路径表达式

```java
<aop:config>
	<aop:advisor advice-ref="txAdvice" pointcut="execution路径表达式"/>
</aop:config>
```

**使用Spring实现事务管理**：

1. 使用SQLServer创建表结构，ccb_account(银行账户)和account_log(账户日志表)表。

2. 搭建SH框架，并反向生成表对象，获得实体类和DAO。

3. 在services包中创建CcbAccountService类，实现转账方法，模拟"转账日志表的备注字段长度超出"的故障发生。

   ```java 
   public class CcbAccountService{
       private AccountLogDAO accountLogDAO;
       private CcbAccountDAO ccbAccountDAO;
       public String testTransfer(Integer outId,Integer inId,Float money){
           //1.查询两个账户
           CcbAccount outAccount = ccbAccountDAO.findById(outId);
           CcbAccount inAccount = ccbAccountDAO.findById(inId);
           //2.判断账户余额
           if(outAccount.getAccAmount()<money){
               return "账户余额不足!";
           }
           //3.转账
           outAccount.setAccAmount(outAccount.getAccAmount()-money);
           ccbAccountDAO.merge(outAccount);
           inAccount.setAccAmount(inAccount.getAccAmount()+money);
           ccbAccountDAO.merge(inAccount);
           //添加转账日志
           accountLogDAO.save(new AccountLog(outAccount,"0","错误模拟:转账日志表的备注字段长度超出！"));
           return "转账成功!";
       }
       //省略DAO方法的get/set方法
       ...
   }
   ```

   <!--上述代码中，testTranfer方法有三个参数，outId :代表转账人账户ID；inId:代表收款然账户ID;money:代表转账金额-->

4. 在applicationContext.xml文件中注入CcbAccountService对象.

   ```java
   <bean id = "CcbAccountService" class = "com.proc.service.CcbAccountService">
   	<property name = "ccbAccountDAO" ref = "CcbAccountDAO">
   	</property>
   	<property name = "accountLogDAO" ref = "AccountLogDAO">
   	</property>
   </bean>
   ```

5. 在applicationContext.xml文件中修改beans元素的命名空间，加入对tx和aop命名空间的支持

   ```java
   <beans
   ...
   ...
   xmlns:tx="http://www.springframework.org/schema/tx"
   xmlns:aop="http://www.springframework.org/schema/aop"
   xsi:schemaLocation="http://www.springframework.org/schema/beans
   ...
   http://www.springframework.org/schema/tx
   http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
   http://www.springframework.org/schema/aop
   http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">
   ```
   <!--注 ：如果 http://www.springframework.org/schema/aop 和 http://www.springframework.org/schema/aop/spring-aop-3.0.xsd 引入顺序相反，那么会出错。（错误信息:White spaces are required between publicId and systemId）-->

6. 在applicationContext.xml文件中添加Spring声明式事务配置:

   ```java
   //定义事务管理器
   <bean id = "transactionManager"
   class = "org.springframework.orm.hibernate3.HibernateTransactionManager">
   	<property name = "sessionFactory" ref="sfid">
   </bean>
   //定义事务切面
   <tx:advice id = "txAdvice" transaction-manager="transactionManager">
   <tx:attribute>
   	<tx:method name = "*" propagation="REQUIRED"/>
   </tx:attribute>
   </tx:advice>
   //通过AOP配置提供事务增强，使service包下的所有类，方法命名开头为test的方法拥有事务
   <aop:config>
   	<aop:advisor advice-ref="txAdvice" pointcut="execution
   	(* com.proc.service.*.test*(..))"
   </aop:config>
   ```

7. 可在行号侧栏中查看是否配置正确，aop的事务增强会出现图标

8. 在行号侧栏中被拦截的方法会出现图标

9. 在Test包中，创建TestMain测试类，调用testTransfer方法

   ```java
   public class TestMain{
       public static void main(String[] args){
           ApplicationContext context = new 
           ClassPathXmlApplicationContext("applicationContext.xml");
           CcbAccountService service = (CcbAccountService)context.getBean("CcbAccountService");
           System.out.println(service.testTranfer(1,2,100f));
       }
   }
   ```

10. 执行测试，由于插入字段超出，控制台会打印错误信息。此时，事务会发生回滚，转账业务撤销执行。

11. 查看SQLServer客户端，数据库不会执行任何更改和操作。

12. 如果转账执行成功，控制台会打印所执行的sql语句，数据库会更新。

问题:

1. **如果把一整个类中的所有方法都执行为事务，那么在bean注入相关联类的时候由于set方法被写成事务而导致application.xml文件中的对象注入失败**

2. **关于java.lang.NoClassDefFoundError: org/objectweb/asm/Type**

    

   **调试SPRING MVC（或者整合SSH）的时候遇到了org/objectweb/asm/Type**

   **解决方法1：**

   **原因是Spring中的cglib-nodep-2.x.x.jar与Hibernate中的cglib-2.2.jar相冲突! 两种框架整合时Spring中的cglib-nodep-2.x.x.jar是必须的,取消Hibernate中的cglib-2.2.jar即可**

   **解决方法2:**

   **在Hibernate 3.2.6.中的 cglib 是 cglib-2.1.3.,jar 使用 cglib-2.2.jar 则出现以上问题。将cglib-2.2.jar换成cglib-2.1.3.jar**

   **解决方法3（我使用这个成功了）:**

   **如果以上方法不行，则删除cglib-2.2,cglib-2.1.3,cglib-nodep-2.x.x.jar,直接导入ams-all-4.0.jar包。**

   (个人是配置事务管理器后注入bean失败)使用第1种方法解决