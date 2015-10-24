title: Mybatis的使用
date: 2015-10-21 21:06:20
tags: mybatis
---

mybatis文档:[http://mybatis.github.io/mybatis-3/zh/index.html](http://mybatis.github.io/mybatis-3/zh/index.html)

## Maven依赖: ##

	<dependency>
	  <groupId>org.mybatis</groupId>
	  <artifactId>mybatis</artifactId>
	  <version>x.x.x</version>
	</dependency>
<!-- more -->
## Mybatis配置文件（从xml构建SessionFactory）##

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE configuration
	  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
	  "http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>
	  <environments default="development">
	    <environment id="development">
	      <transactionManager type="JDBC"/>
	      <dataSource type="POOLED">
	        <property name="driver" value="${driver}"/>
	        <property name="url" value="${url}"/>
	        <property name="username" value="${username}"/>
	        <property name="password" value="${password}"/>
	      </dataSource>
	    </environment>
	  </environments>
	  <mappers>
	    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
	  </mappers>
	</configuration>

## Mybatis映射文件（采用xml方式）##

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper
	  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<mapper namespace="org.mybatis.example.BlogMapper">
	  <select id="selectBlog" resultType="Blog">
	    select * from Blog where id = #{id}
	  </select>
	</mapper>

## Mybatis映射器类(采用xml文件配置) ##

	package org.mybatis.example;
	public interface BlogMapper {
	  @Select("SELECT * FROM blog WHERE id = #{id}")
	  Blog selectBlog(int id);
	}

## Mybatis的使用##


	String resource = "org/mybatis/example/mybatis-config.xml";
	InputStream inputStream = Resources.getResourceAsStream(resource);
	SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
	SqlSession session = sqlSessionFactory.openSession();
	try {
	//这种方式可以不使用映射器类
	 // Blog blog = (Blog) session.selectOne("org.mybatis.example.BlogMapper.selectBlog", 101);
	//这种方式必须使用映射器类，相当于抽象出一层接口
		BlogMapper mapper = session.getMapper(BlogMapper.class);
	  	Blog blog = mapper.selectBlog(101);
	} finally {
	  session.close();
	}


