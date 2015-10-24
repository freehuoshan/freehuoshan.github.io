title: spring常用jar包依赖关系
date: 2015-10-19 22:57:27
tags: spring
---

## ** spring常用jar包功能简述: ** ##

1. spring.jar 是包含有完整发布模块的单个jar 包。这个在"3.03之后不再提供！想要该包的同学，把dist目录下的jar全部解压开，在打包成spring.jar即可。
<!-- more -->

2. org.springframework.aop 包含在应用中使用Spring的AOP特性时所需的类。


3. org.springframework.asm  Spring独立的asm程序, Spring2.5.6的时候需要asmJar 包,3.0开始提供他自己独立的asmJar。


4. org.springframework.aspects 提供对AspectJ的支持，以便可以方便的将面向方面的功能集成进IDE中，比如Eclipse AJDT。


5. org.springframework.beans所有应用都要用到的，它包含访问配置文件、创建和管理bean以及进行Inversion of Control / Dependency Injection（IoC/DI）操作相关的所有类。


6. org.springframework.context.support包含支持缓存Cache（ehcache）、JCA、JMX、邮件服务（Java Mail、COS Mail）、任务计划Scheduling（Timer、Quartz）方面的类。


7. org.springframework.context为Spring核心提供了大量扩展。可以找到使用Spring ApplicationContext特性时所需的全部类，JDNI所需的全部类，UI方面的用来与模板（Templating）引擎如 Velocity、FreeMarker、JasperReports集成的类，以及校验Validation方面的相关类。


8. org.springframework.core 包含Spring框架基本的核心工具类，Spring其它组件要都要使用到这个包里的类，是其它组件的基本核心。


9. org.springframework.expression  Spring表达式语言。


10. org.springframework.instrument.tomcat Spring3.0对Tomcat的连接池的集成。


11. org.springframework.instrument Spring3.0对服务器的代理接口。


12. org.springframework.jdbc 包含对Spring对JDBC数据访问进行封装的所有类。


13. org.springframework.jms 提供了对JMS 1.0.2/1.1的支持类。


14. org.springframework.orm 包含Spring对DAO特性集进行了扩展，使其支持 iBATIS、JDO、OJB、TopLink，因为Hibernate已经独立成包了，现在不包含在这个包里了。这个jar文件里大部分的类都要依赖spring-dao.jar里的类，用这个包时你需要同时包含spring-dao.jar包。


15. org.springframework.oxm  Spring 对Object/XMl的映射支持,可以让Java与XML之间来回切换。


16. org.springframework.test  对Junit等测试框架的简单封装。


17. org.springframework.transaction为JDBC、Hibernate、JDO、JPA等提供的一致的声明式和编程式事务管理。


18. org.springframework.web.portlet  SpringMVC的增强。


19. org.springframework.web.servlet  对J2EE6.0 的Servlet3.0的支持。


20. org.springframework.web.struts Struts框架支持，可以更方便更容易的集成Struts框架。


21. org.springframework.web 包含Web应用开发时，用到Spring框架时所需的核心类，包括自动载入WebApplicationContext特性的类、Struts与JSF集成类、文件上传的支持类、Filter类和大量工具辅助类。  
   
   


(1) spring-core.jar 

这个jar文件包含Spring框架基本的核心工具类，Spring其它组件要都要使用到这个包里的类，是其它组件的基本核心，当然你也可以在自己的应用系统中使用这些工具类。 


(2) spring-beans.jar 

这个jar文件是所有应用都要用到的，它包含访问配置文件、创建和管理bean以及进行Inversion of Control / Dependency Injection（IoC/DI）操作相关的所有类。如果应用只需基本的IoC/DI支持，引入spring-core.jar及spring- beans.jar文件就可以了。 


(3) spring-aop.jar 

这个jar文件包含在应用中使用Spring的AOP特性时所需的类。使用基于AOP的Spring特性，如声明型事务管理（Declarative Transaction Management），也要在应用里包含这个jar包。 



(4) spring-context.jar 

　　这个jar文件为Spring核心提供了大量扩展。可以找到使用Spring ApplicationContext特性时所需的全部类，JDNI所需的全部类，UI方面的用来与模板（Templating）引擎如 Velocity、FreeMarker、JasperReports集成的类，以及校验Validation方面的相关类。 



(5) spring-dao.jar 

　　这个jar文件包含Spring DAO、Spring Transaction进行数据访问的所有类。为了使用声明型事务支持，还需在自己的应用里包含spring-aop.jar。



(6) spring-hibernate.jar 

　　这个jar文件包含Spring对Hibernate 2及Hibernate 3进行封装的所有类。 



(7) spring-jdbc.jar 

　　这个jar文件包含对Spring对JDBC数据访问进行封装的所有类。 



(8) spring-orm.jar 

　　这个jar文件包含Spring对DAO特性集进行了扩展，使其支持 iBATIS、JDO、OJB、TopLink，因为Hibernate已经独立成包了，现在不包含在这个包里了。这个jar文件里大部分的类都要依赖 spring-dao.jar里的类，用这个包时你需要同时包含spring-dao.jar包。 



(9) spring-remoting.jar 

　　这个jar文件包含支持EJB、JMS、远程调用Remoting（RMI、Hessian、Burlap、Http Invoker、JAX-RPC）方面的类。 



(10) spring-support.jar 

　　这个jar文件包含支持缓存Cache（ehcache）、JCA、JMX、邮件服务（Java Mail、COS Mail）、任务计划Scheduling（Timer、Quartz）方面的类。 



(11) spring-web.jar 

　　这个jar文件包含Web应用开发时，用到Spring框架时所需的核心类，包括自动载入WebApplicationContext特性的类、Struts与JSF集成类、文件上传的支持类、Filter类和大量工具辅助类。 



(12) spring-webmvc.jar 

　　这个jar文件包含Spring MVC框架相关的所有类。包含国际化、标签、Theme、视图展现的FreeMarker、JasperReports、Tiles、Velocity、 XSLT相关类。当然，如果你的应用使用了独立的MVC框架，则无需这个JAR文件里的任何类。 



(13) spring-mock.jar 

　　这个jar文件包含Spring一整套mock类来辅助应用的测试。Spring测试套件使用了其中大量mock类，这样测试就更加简单。模拟HttpServletRequest和HttpServletResponse类在Web应用单元测试是很方便的。 

---

## ** Spring常用jar包依赖说明: ** ##


1) spring-core.jar需commons-collections.jar，spring-core.jar是以下其它各个的基本。 

2) spring-beans.jar需spring-core.jar，cglib-nodep-2.1_3.jar 

3) spring-aop.jar需spring-core.jar，spring-beans.jar，cglib-nodep-2.1_3.jar，aopalliance.jar 

4) spring-context.jar需spring-core.jar，spring-beans.jar，spring-aop.jar，commons-collections.jar，aopalliance.jar 

5) spring-dao.jar需spring-core.jar，spring-beans.jar，spring-aop.jar，spring-context.jar 

6) spring-jdbc.jar需spring-core.jar，spring-beans.jar，spring-dao.jar 

7) spring-web.jar需spring-core.jar，spring-beans.jar，spring-context.jar 

8) spring-webmvc.jar需spring-core.jar/spring-beans.jar/spring-context.jar/spring-web.jar 

9) spring -hibernate.jar需spring-core.jar，spring-beans.jar，spring-aop.jar，spring- dao.jar，spring-jdbc.jar，spring-orm.jar，spring-web.jar，spring-webmvc.jar 

10) spring-orm.jar需spring-core.jar，spring-beans.jar，spring-aop.jar，spring- dao.jar，spring-jdbc.jar，spring-web.jar，spring-webmvc.jar 

11) spring -remoting.jar需spring-core.jar，spring-beans.jar，spring-aop.jar，spring- dao.jar，spring-context.jar，spring-web.jar，spring-webmvc.jar 

12) spring-support.jar需spring-core.jar，spring-beans.jar，spring-aop.jar，spring-dao.jar，spring-context.jar，spring-jdbc.jar 

13) spring-mock.jar需spring-core.jar，spring-beans.jar，spring-dao.jar，spring-context.jar，spring-jdbc.jar  

>  以上内容引自: [http://blog.csdn.net/shenzhen_zsw/article/details/11399373](http://blog.csdn.net/shenzhen_zsw/article/details/11399373)


---

** 由于依赖性,所以一般maven项目中只需要简短依赖： **

        <!-- spring依赖 -->
        	
        	<dependency>
        	  <groupId>org.springframework</groupId>
        	  <artifactId>spring-tx</artifactId>
        	  <version>4.2.1.RELEASE</version>
        	</dependency>
        	
        	<dependency>
        	  <groupId>org.springframework</groupId>
        	  <artifactId>spring-context-support</artifactId>
        	  <version>4.2.1.RELEASE</version>
        	</dependency>
        	
        	<dependency>
        	  <groupId>org.springframework</groupId>
        	  <artifactId>spring-aspects</artifactId>
        	  <version>4.2.1.RELEASE</version>
        	</dependency>
        	
        	
        	





