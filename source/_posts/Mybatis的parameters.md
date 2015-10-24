title: Mybatis的parameters
date: 2015-10-21 21:24:47
tags: mybatis
---

## 传值：##

### 原生类型或简单数据类型（比如整型或字符串）###
<!-- more -->

	<select id="selectUsers" resultType="User">
	  select id, username, password
	  from users
	  where id = #{id}
	</select>
_**mybatis有内建的类型转换器**_

### 复杂对象（自定义User对象）###

	<insert id="insertUser" parameterType="User">
	  insert into users (id, username, password)
	  values (#{id}, #{username}, #{password})
	</insert>

_**会自动查找User对象的这几个属性，并进行匹配**_

_**在某些时候需要指定类型(比如某个值为空)**_

* 使用mybatis已有的类型转换器:

		1.#{property,javaType=int,jdbcType=NUMERIC}
		2.#{height,javaType=double,jdbcType=NUMERIC,numericScale=2}(小数类型保留位数)
		
* 使用自定义类型转换器：

		#{age,javaType=int,jdbcType=NUMERIC,typeHandler=MyTypeHandler}

_**如果某个传入的值可能为空，那么一定要指定jdbcType**_


---

## 字符串替换：##


默认情况下，使用#{}向预处理语句中（？）传值。

如果想要向sql语句中替换一个字符串，如表字段或表名，使用${}

_**为了防止sql注入，这个替换字符串不应该是被用户输入的字段，如果不可避免，则必须要校验**_



