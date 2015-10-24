title: maven的聚合于继承
date: 2015-10-18 09:12:35
tags: maven
---

### 聚合


        <modules>
        	<module>preservice-dao</module>
        	<module>preservice-core</module>
        	<module>preservice-impl</module>
        	<module>preservice-share</module>
        	<module>preservice-war</module>
        </modules>
        
<!-- more-->
        
### 继承


        <dependencies>
              <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
        </dependency>
            
        <dependency>
            	<groupId>${group.id}</groupId>
            	<artifactId>preservice-dao</artifactId>
            	<version>${project.version}</version>
        </dependency>
            
        <dependency>
            	<groupId>${group.id}</groupId>
            	<artifactId>preservice-core</artifactId>
            	<version>${project.version}</version>
        </dependency>
            
        <dependency>
            	<groupId>com.huoshan.preservice</groupId>
            	<artifactId>preservice-share</artifactId>
            	<version>0.0.1-SNAPSHORT</version>
            </dependency>
        </dependencies>
        
         
  一般在父模块中这样:
  
        
        <dependencyManagement>
          	<dependencies>
        	    <dependency>
        	      <groupId>junit</groupId>
        	      <artifactId>junit</artifactId>
        	      <version>${junit.version}</version>
        	      <scope>test</scope>
        	    </dependency>
          	</dependencies>
         </dependencyManagement>
         
         
这样如果子模块想要使用某个模块只需:



        <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
        </dependency>
            

这样声明即可,如果不声明,子模块不会继承父模块 <dependencyManagement>中的依赖
        