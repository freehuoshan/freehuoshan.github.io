title: Eclipse中maven第三方jar包源码关联
date: 2015-10-28 21:14:04
tags: [eclipse, maven]
---


> 在pom文件中增加插件

<!-- more -->

	<plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-eclipse-plugin</artifactId>
        <version>2.9</version>
        <configuration>
            <additionalProjectnatures>
                <projectnature>org.eclipse.jdt.core.javanature</projectnature>
                <projectnature>org.eclipse.m2e.core.maven2Nature</projectnature>
                <projectnature>org.springframework.ide.eclipse.core.springnature</projectnature>
            </additionalProjectnatures>
            <additionalBuildcommands>
                <buildcommand>org.eclipse.jdt.core.javabuilder</buildcommand>
                <buildcommand>org.eclipse.m2e.core.maven2Builder</buildcommand>
                <buildcommand>org.springframework.ide.eclipse.core.springbuilder</buildcommand>
            </additionalBuildcommands>
        </configuration>
    </plugin>


> 然后分别执行

![http://7xire1.com1.z0.glb.clouddn.com/QQ截图20151028131605.png](http://7xire1.com1.z0.glb.clouddn.com/QQ截图20151028131605.png)

![http://7xire1.com1.z0.glb.clouddn.com/QQ截图20151028131651.png](http://7xire1.com1.z0.glb.clouddn.com/QQ截图20151028131651.png)

_**项目中的第三方jar包源码已经关联**_


