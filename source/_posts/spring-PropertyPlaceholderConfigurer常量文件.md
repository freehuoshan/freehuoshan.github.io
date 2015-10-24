title: spring_PropertyPlaceholderConfigurer常量文件
date: 2015-10-21 21:08:01
tags: spring
---
## 单个文件: ##
<!-- more -->

	<bean id="propertyConfigurerForAnalysis" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
	    <property name="location">
	        <value>classpath:/spring/include/dbQuery.properties</value>
	    </property>
	</bean>
## 多个文件：##

	<bean id="propertyConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
	    <property name="locations">
	       <list>
	          <value>classpath:/spring/include/jdbc-parms.properties</value>
	          <value>classpath:/spring/include/base-config.properties</value>
	        </list>
	    </property>
	</bean>
## 整合多工程下的多个分散的Properties 文件: ##

	<bean id="propertyConfigurerForProject2" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
	    <property name="order" value="2" />
	    <property name="ignoreUnresolvablePlaceholders" value="true" />
	    <property name="locations">
	      <list>
	        <value>classpath:/spring/include/jdbc-parms.properties</value>
	        <value>classpath:/spring/include/base-config.properties</value>
	      </list>
	    </property>
	</bean>