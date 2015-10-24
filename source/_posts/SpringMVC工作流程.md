title: SpringMVC工作流程
date: 2015-10-22 21:05:21
tags: SpringMVC
---

> springMVC工作流程图:

![http://7xire1.com1.z0.glb.clouddn.com/springmvc.png](http://7xire1.com1.z0.glb.clouddn.com/springmvc.png)

<!-- more -->

> 配置DispatcherServlet（这是SpringMVC的入口）

        <servlet>  
            <servlet-name>hello</servlet-name>  
            <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
            <load-on-startup>1</load-on-startup>  
        </servlet>  
        <servlet-mapping>  
            <servlet-name>hello</servlet-name>  
            <url-pattern>/</url-pattern>  
        </servlet-mapping>  
        
_**配置所有的请求都交由SpringleMVC框架处理**_

> 配置视图模板的解析

        <!-- ViewResolver -->  
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">  
            <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>  
            <property name="prefix" value="/WEB-INF/jsp/"/>  
            <property name="suffix" value=".jsp"/>  
        </bean>  