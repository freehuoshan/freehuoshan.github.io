title: nexus私服使用
date: 2015-10-18 12:03:20
tags: maven
---

### 下载

在[http://www.sonatype.org/](http://www.sonatype.org/)

<!-- more -->

![](http://7xire1.com1.z0.glb.clouddn.com/2015-10-18%2012%3A25%3A24屏幕截图.png)

选择download第一项下载.tar.gz或.zip压缩包

---

### 启动

在本地解压到nexus,进入到nexus/nexus-2.11.4-01/bin/运行

``` bash
        huoshan-ThinkPad-Edge-E431# ./nexus start
        ****************************************
        WARNING - NOT RECOMMENDED TO RUN AS ROOT
        ****************************************
        If you insist running as root, then set the environment variable RUN_AS_USER=root before
        running this script.
        huoshan-ThinkPad-Edge-E431# ls
```

没有正常启动,然后编辑nexus,设置RUAN_AS_ROOT=root或其他linux用户再次启动正常.正常启动在8081端口,修改端口可在nexus/nexus-2.11.4-01/conf/nexus.properties文件中修改端口.

访问:登录,admin默认密码admin123

![](http://7xire1.com1.z0.glb.clouddn.com/2015-10-18%2013%3A35%3A03屏幕截图.png)

![](http://7xire1.com1.z0.glb.clouddn.com/2015-10-18%2013%3A37%3A12屏幕截图.png)

---

### 仓库
central代理仓库:代理maven中央仓库是代理仓库,策略为release,会下载中央仓库release版本

release宿主仓库:部署组织内部发布的release版本

snapshort宿主仓库:部署组织内部发布的snapshort版本

apacheSnapshort代理仓库:代理maven中央仓库snapshort版本

codehaussnapshort代理仓库:代理codehaus仓库snapshort版本

type:group(仓库组),proxy(代理远程仓库),host(组织内部发布)


----

### 搜索
为nexus设置搜索:点击central,在下方configration中配置download remoteindex为true.下载远程索引,点击admanistartion/Scheduled Tasks可看到下载状态,下载完成即可搜索.

---
### 使用私服

- pom配置


        <repositories>
          	<repository>
        	  	<id>nexus</id>
        	  	<name>Nexus</name>
        	  	<url>http://localhost:8081/nexus/content/groups/public/</url>
        	  	<releases><enabled>true</enabled></releases>
        	  	<snapshots><enabled>true</enabled></snapshots>
          	</repository>
        </repositories>
        <pluginRepositories>
          	<pluginRepository>
          		<id>nexus</id>
        	  	<name>Nexus</name>
        	  	<url>http://localhost:8081/nexus/content/groups/public/</url>
        	  	<releases><enabled>true</enabled></releases>
        	  	<snapshots><enabled>true</enabled></snapshots>
          	</pluginRepository>
        </pluginRepositories>
        
- settings配置

        <profiles>
          	<profile>
          		<repositories>
        		  	<repository>
        			  	<id>nexus</id>
        			  	<name>Nexus</name>
        			  	<url>http://localhost:8081/nexus/content/groups/public/</url>
        			  	<releases><enabled>true</enabled></releases>
        			  	<snapshots><enabled>true</enabled></snapshots>
        		  	</repository>
        		  </repositories>
        		  <pluginRepositories>
        		  	<pluginRepository>
        		  		<id>nexus</id>
        			  	<name>Nexus</name>
        			  	<url>http://localhost:8081/nexus/content/groups/public/</url>
        			  	<releases><enabled>true</enabled></releases>
        			  	<snapshots><enabled>true</enabled></snapshots>
        		  	</pluginRepository>
        		  </pluginRepositories>
          	</profile>
          </profiles>
          <activeProfiles>
          	<activeProfile>nexus</activeProfile>
          </activeProfiles>
          
 - 镜像
 
>    _**配置所有下载请求转到私服镜像**_


        <mirrors>
          	<mirror>
          		<id>nexus</id>
          		<mirrorOf>*</mirrorOf>
          		<url>http://localhost:8081/nexus/content/groups/public/</url>
          	</mirror>
          </mirrors>
          <profiles>
           	<profile>
           		<id>nexus</id>
           		<repositories>
         		  	<repository>
         			  	<id>central</id>
         			  	<url>http://central</url>
         			  	<releases><enabled>true</enabled></releases>
         			  	<snapshots><enabled>true</enabled></snapshots>
         		  	</repository>
         		  </repositories>
         		  <pluginRepositories>
         		  	<pluginRepository>
         		  		<id>central</id>
         			  	<url>http://central</url>
         			  	<releases><enabled>true</enabled></releases>
         			  	<snapshots><enabled>true</enabled></snapshots>
         		  	</pluginRepository>
         		  </pluginRepositories>
           	</profile>
           </profiles>
           <activeProfiles>
           	<activeProfile>nexus</activeProfile>
           </activeProfiles>


        







