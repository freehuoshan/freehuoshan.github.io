title: Mybatis自动生成主键
date: 2015-10-21 21:24:29
tags: mybatis
---


## 数据库支持自动生成主键 ##
<!-- more -->

	<insert id="insertAuthor" useGeneratedKeys="true"
	    keyProperty="id">
	  insert into Author (username,password,email,bio)
	  values (#{username},#{password},#{email},#{bio})
	</insert>
	
## 对于不支持自动生成主键的数据库也可以这样 ##

	<insert id="insertAuthor">
	  <selectKey keyProperty="id" resultType="int" order="BEFORE">
	    select CAST(RANDOM()*1000000 as INTEGER) a from SYSIBM.SYSDUMMY1
	  </selectKey>
	  insert into Author
	    (id, username, password, email,bio, favourite_section)
	  values
	    (#{id}, #{username}, #{password}, #{email}, #{bio}, #{favouriteSection,jdbcType=VARCHAR})
	</insert>
	
**但不建议这样干**