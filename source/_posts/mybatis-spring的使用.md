title: mybatis-spring的使用
date: 2015-10-21 21:24:59
tags: [mybatis, spring]
---


**使用mybatis-spring：**[http://mybatis.github.io/spring/zh/](http://mybatis.github.io/spring/zh/)

## maven中添加依赖: ##
<!-- more -->

	<dependency>
	  <groupId>org.mybatis</groupId>
	  <artifactId>mybatis-spring</artifactId>
	  <version>x.x.x</version>
	</dependency>
		
_**mybatis-spring 依赖mybatis**_

----
## spring中配置 ##

**配置数据库常量文件位置:**

        <!-- 数据库常量配置 -->
        	<bean id="preServiceDaoPropertyConfig"
        		class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        		<property name="order" value="1" />
        		<property name="ignoreUnresolvablePlaceholders" value="true" />
        		<property name="locations">
        			<list>
        				<value>classpath:config/sbird-preservice-dao-config.properties</value>
        			</list>
        		</property>
        	</bean>
        	
**配置数据源:**

        <!-- DataSource定义。 -->
        	<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" 
        		destroy-method="close"> 
        	     <property name="url" value="${sbird.mysql.url}"/>
        	     <property name="username"><value>${sbird.mysql.user}</value></property>
        	     <property name="password" value="${sbird.mysql.passwd}" />
        		 <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        	     <property name="filters"><value>${sbird.mysql.filters}</value></property>
        	     <property name="maxActive"><value>${sbird.mysql.maxActive}</value></property>
        	     <property name="initialSize"><value>${sbird.mysql.initialSize}</value></property>
        	     <property name="maxWait"><value>${sbird.mysql.maxWait}</value></property>
        	     <property name="minIdle"><value>${sbird.mysql.maxIdle}</value></property>
        	     <property name="timeBetweenEvictionRunsMillis"><value>${sbird.mysql.timeBetweenEvictionRunsMillis}</value></property>
        	     <property name="minEvictableIdleTimeMillis"><value>${sbird.mysql.minEvictableIdleTimeMillis}</value></property>
        	     <property name="validationQuery"><value>${sbird.mysql.validationQuery}</value></property>
        	     <property name="testWhileIdle"><value>${sbird.mysql.testWhileIdle}</value></property>
        	     <property name="testOnBorrow"><value>${sbird.mysql.testOnBorrow}</value></property>
        	     <property name="testOnReturn"><value>${sbird.mysql.testOnReturn}</value></property>
        	     <property name="poolPreparedStatements"><value>${sbird.mysql.poolPreparedStatements}</value></property>
        	     <property name="maxOpenPreparedStatements"><value>${sbird.mysql.maxOpenPreparedStatements}</value></property>
         	</bean>

**配置sqlSessionFactory通过sqlSessionfactoryBean,注入数据源、mybatis xml文件位置:**

        <!-- 配置spring整合mybatis框架的一个会话工厂bean -->
        	<bean id="sessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        		<property name="dataSource" ref="dataSource"/>
        		<property name="configLocation" value="classpath:sqlMapConfig.xml"/>
        	</bean>
        	
**配置事物:**

        <!-- 事务管理 -->
        	<bean id="transactionManager"
        		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        		<property name="dataSource" ref="dataSource" />
        	</bean>
        	<!-- 注解事务 -->
	       <tx:annotation-driven transaction-manager="transactionManager" />
        	

**sqlSession:**

  * sqlSessionTemplate

          <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
             <constructor-arg index="0" ref="sqlSessionFactory" />
          </bean>
          <bean id="userDao" class="org.mybatis.spring.sample.dao.UserDaoImpl">
             <property name="sqlSession" ref="sqlSession" />
          </bean>
          
          
          public class UserDaoImpl implements UserDao {

              private SqlSession sqlSession;
            
              public void setSqlSession(SqlSession sqlSession) {
                this.sqlSession = sqlSession;
              }
            
              public User getUser(String userId) {
                return (User) sqlSession.selectOne("org.mybatis.spring.sample.mapper.UserMapper.getUser", userId);
              }
            }
            
  * sqlSessionDaoSupport
    
          <bean id="userMapper" class="org.mybatis.spring.sample.mapper.UserDaoImpl">
             <property name="sqlSessionFactory" ref="sqlSessionFactory" />
          </bean>
          
          
          public class UserDaoImpl extends SqlSessionDaoSupport implements UserDao {
              public User getUser(String userId) {
                return (User) getSqlSession().selectOne("org.mybatis.spring.sample.mapper.UserMapper.getUser",userId);
              }
            }
        
