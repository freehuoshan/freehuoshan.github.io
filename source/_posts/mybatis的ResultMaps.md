title: mybatis的ResultMaps
date: 2015-10-21 22:23:04
tags: mybatis
---

>  **（官网文档:[http://mybatis.github.io/mybatis-3/zh/sqlmap-xml.html#Result_Maps](http://mybatis.github.io/mybatis-3/zh/sqlmap-xml.html#Result_Maps))**

<!-- more -->

## 简单结果映射

* 列名与属性名匹配（会自动匹配,忽略大小写）官网文档[http://mybatis.github.io/mybatis-3/zh/sqlmap-xml.html#Auto-mapping](http://mybatis.github.io/mybatis-3/zh/sqlmap-xml.html#Auto-mapping)

        <select id="selectUsers" resultType="com.someapp.model.User">
          select id, username, hashedPassword
          from some_table
          where id = #{id}
        </select>
        
* 列名与属性名不匹配（手动匹配）

         <resultMap id="userResultMap" type="User">
          <id property="id" column="user_id" />
          <result property="username" column="user_name"/>
          <result property="password" column="hashed_password"/>
        </resultMap>
       
        
 或
        
        
        <select id="selectUsers" resultType="User">
          select
            user_id             as "id",
            user_name           as "userName",
            hashed_password     as "hashedPassword"
          from some_table
          where id = #{id}
        </select>
        
---
        
## 复杂结果映射 ##

官方例子:一个博客对应一个作者，一个博客对应多篇文章

* 含有复合类型(自定义类型)

        <resultMap id="blogResult" type="Blog">
          <id property="id" column="blog_id" />
          <result property="title" column="blog_title"/>
          <!-- 对应一个作者 ,使用javaType指定类型-->
          <association property="author" javaType="Author">
            <id property="id" column="author_id"/>
            <result property="username" column="author_username"/>
            <result property="password" column="author_password"/>
            <result property="email" column="author_email"/>
            <result property="bio" column="author_bio"/>
          </association>
        </resultMap>
        
        
  可以复用
        
        
        
        <resultMap id="blogResult" type="Blog">
          <id property="id" column="blog_id" />
          <result property="title" column="blog_title"/>
          <!-- 需要使用resultMap指定结果映射 -->
          <association property="author" column="blog_author_id" javaType="Author" resultMap="authorResult"/>
          
                          
                          
        </resultMap>
        
        <resultMap id="authorResult" type="Author">
          <id property="id" column="author_id"/>
          <result property="username" column="author_username"/>
          <result property="password" column="author_password"/>
          <result property="email" column="author_email"/>
          <result property="bio" column="author_bio"/>
        </resultMap>
        
* 含有集合(collection)

        <resultMap id="blogResult" type="Blog">
          <id property="id" column="blog_id" />
          <result property="title" column="blog_title"/>
          <!-- 对应多篇文章，使用ofType指定类型 -->
          <collection property="posts" ofType="Post">
            <id property="id" column="post_id"/>
            <result property="subject" column="post_subject"/>
            <result property="body" column="post_body"/>
          </collection>
        </resultMap>
        
        
 可以复用
        
        
        <resultMap id="blogResult" type="Blog">
          <id property="id" column="blog_id" />
          <result property="title" column="blog_title"/>
          <!-- 需要使用resultMap指定结果映射 -->
          <collection property="posts" ofType="Post" resultMap="blogPostResult" columnPrefix="post_"/>
        </resultMap>
        
        <resultMap id="blogPostResult" type="Post">
          <id property="id" column="id"/>
          <result property="subject" column="subject"/>
          <result property="body" column="body"/>
        </resultMap>
        

        
        
        





