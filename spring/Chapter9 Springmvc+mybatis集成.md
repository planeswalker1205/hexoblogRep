# Chapter9 SpringMVC与MyBatis框架集成

### MyBatis Generator自动工具

使用SpringMVC和MyBatis框架整合是常用的一种项目架构，本章将其简称为SMM框架集合。

##### Mybatis Generator简介

Mybatis Generator是MyBatis官方提供的工具包，类似与之前的Hibernate，通过数据库的表生成映射文件，包括Entity，DAO类和MapperXML配置文件。我们只需要配置好XML文件，然后通过Java命令执行即可生成。Generator配置文件结构:

```
<generatorConfiguration>
	<classPathEntry location=""/>
	<context id="" targetRuntime="">
	<commentGenerator>
	<property name="suppressAllComments" value="true"/>
	</commentGenerator>
	<jdbcConnection driverClass="" connectionURL="" userId="" password="">
	</jdbcConnection>
	<javaTypeResolver>
	<property name="forceBigDecimals" value="false"/>
	</javaTypeResolver>
	<javaModelGenerator targetPackage="" targetProject="">
	<property name="enableSubpackages" value="true"/>
	<property name="trimStrings" value="true"/>
	</javaModelGenerator>
	<sqlMapGenerator targetPackage="" targetProject="">
	<property name="enableSubpackages" value="true"/>
	</sqlMapGenerator>
	<javaClientGenerator type="XMLMAPPER" targetPackage="" targetProject="">
	<property name="enableSuboackages" value="true"/>
	</javaClientGenerator>
	<table tableName="" domainObjectName=""></table>
	</context>
</generatorConfiguration>
```

(1)classPathEntity:连接数据库的驱动包，驱动包通常与配置文件位于同一路径。

(2)commentGenerator:指定生成注解的模板，suppressAllComments=true表示去掉自动生成的代码注释。

(3)jdbcConnection:指定数据库的连接信息

(4)javaTypeResolver:Java类型转换器，默认false，表示将JDBC DECIMAL和NUMERIC类型解析为Integer；当其为true时，则表示将JDBC DECIMAL和NUMERIC类型解析为java.math.BigDecimal

(5)javaModelGenerator:指定数据模型(实体类)的生成信息。

​      ①targetPackage属性:指定实体类生成的包。如com.proc.smm.entity。

​      ②targetProject属性:指定实体类实际生成的物理位置，可以配置相对或绝对路径。

(6)sqlMapGenerator:指定sqlMapper XML文件的生成路径。

(7)javaClientGenerator:指定DAO类生成的路径。

(8)table:配置反向生成的表信息。

​     ①tablename属性:匹配数据库中的表名

​     ②domainObjectName属性:自动生成的文件头名称，通常该配置的首字母大写。

要得到Ebtity，DAO类和Mapper XML配置文件，还需要使用Java命令执行生成。

语法:

java -jar mybatis-generator-core.jar   -configfile Generator 配置文件 -overwrite

####使用MtBatis Generator工具自动生成源代码

首先，导入所需要的Jar包文件；然后，编写Generator配置文件属性；最后使用Java命令执行生成。

1. 创建Web工程，命名为“ssmProject”

2. 在src目录下创建名为"Auto"的包，导入所需要的Jar包文件

   1)mybatis-3.1.1.jar:MyBatis所需要的驱动

   2)mybatis-generator-core-1.3.2.jar:Generator工具包的驱动

   3)mysql-connector-java-5.1-bin.jar:MySql数据库连接驱动。

   为了降低程序的复杂性，将生成时所需的文件同一存放于该包内。

3. 在Auto包中，创建config.xml文件。编写Generator配置文件属性

4. 使用Java命令执行配置脚本。打开MS-DOS窗口，将当前路径指向到auto目录下，执行Java命令

   “java -jar mybatis-generator-core.jar  -configfile config.xml -overwrite”执行生成，当最后控制台输出

   "MyBatis Generator finished successfully."时，表示正确生成配置。

   ---

   ### SpringMVC，MyBatis框架整合 

   ##### Spring 3.0在各层中使用的注解

   推荐在Spring 3.0 中使用全注解的方式开发。@Repository，@Service和@Controller分别在DAO，Service和Controller层中使用。使用了注解后，我们就不需要使用\<bean\>进行注入，如此减少了配置环节。提高了开发效率。除此之外，还能使用@Autowired自动装载对象，步骤如下:

   1. 在Spring配置文件中使用\<context:component-scan\>自动扫描注解，如@Repository和@Autowired等。
   2. 依次在各层使用@Repository，@Service和@Controller注解
   3. 在依赖类中使用@Autowired对接口声明。

   ##### SMM架构整合搭建和使用

   1. 在Web工程中，添加Spring支持，选择Core Libraries，AOP Libraries，Persistence Core和Web Libraries支持，其中Persistence Core代表对持久层框架的支持，并非特指Hibernate框架，所以需要添加Persistence Core。

   2. 在lib文件夹下，导入所需要的Jar包文件，其中，mybatis-spring-1.1.1.jar用于整合所需要的关键包。

      commons-dbcp.jar:Apach提供的DBCP数据源

      commons-pool-1.5.4.jar:DBCP数据源中连接池的实现

      mybatis-3.1.1.jar:MyBatis 3.1驱动

      mybatis-spring-1.1.1.jar:由MyBatis提供的Spring的依赖支持包

      jstl.jar:JSTL支持

      standard.jar:JSTL支持

   3. 在src目录中，创建MyBatis配置文件，因为架构整合的原因，只需要配置Mappers映射即可

      ```
      <configuration>
      <mappers>
      	<mapper resource="com/proc/smm/dao/CustomerMapper.xml"/>
      </mappers>
      </configuration>
      ```

   4. 在viewSpace-servlet.xml文件中，添加Spring的配置信息。

      ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:p="http://www.springframework.org/schema/p"
      xmlns:mvc="http://www.springframework.org/schema/mvc"
      xmlns:context="http://www.springframework.org/schema/context"
      xmlns:tx="http://www.springframework.org/schema/tx"
      xmlns:aop="http://www.springframework.org/schema/aop"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
      	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
      	http://www.springframework.org/schema/context
      	http://www.springframework.org/schema/context/spring-context-3.0.xsd
      	http://www.springframework.org/schema/mvc
      	http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd
      	http://www.springframwork.org/schema/tx
      	http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
      	http://www.springframework.org/schema/aop
      	http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">
      	<!-- 自动注册SpringMVC所需的所有驱动 -->
      	<mvc:annotation-driven/>
      	<context:component-scan base-package="com.proc.mvc.action,com.proc.Service"/>
      	<!-- 配置dbcp数据源，通过数据源管理数据库连接 -->
      	<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
      		<property name="driverClassName" value="com.mysql.jdbc.Driver"/>
      		<property name="url" value="jdbc:mysql://localhost:3306/mstf?useUnicode=
      		true&amp;characterEncoding=utf-8"></property>
      		<property name="username" value="root"></property>
      		<property name="password" value=""></property>
      	</bean>
      	<!-- 管理MyBatis Session工厂 -->
      	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
      		<!-- 加载MyBatis配置文件 -->
      		<property name="configLocation" value="classpath:sqlMapConfig.xml"/>
      		<!-- 获取数据源 -->
      		<property name="dataSource" ref="dataSource"></property>
      	</bean>
      </beans>
      ```

   5. 完成Spring对SpringMVC和MyBatis框架全部完成整合。

   ---

   ### MapperDAO

   使用Generator自动生成工具时，系统产生了一个Mapper类，它封装了DML的常用方法，每个Mapper类都对应一个MapperXML文件。其中，每个方法都与XML文件中的配置ID对应。