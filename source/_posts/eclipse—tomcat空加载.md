title: eclipse—tomcat空加载
date: 2015-11-19 20:57:31
tags: eclipse
---

 经常有svn下来的代码，导入eclipse，部署到tomcat空加载，原因在于代码中.setting与....那三个配置文件中的项目
<!-- more -->
名信息与现在真实项目名不同。以及代码下的webRoot或webContent与.setting。。。。那三个文件中的不同。

解决办法：

** 在eclipse中新建一个想要命名的空项目，在新建时，设置到webcontent还是使用webRoot，然后将该项目.setting与.....那三个项目相关文件覆盖掉原来的。**
