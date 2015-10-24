title: maven问题
date: 2015-10-18 23:34:32
tags: maven
---

-  ### **使用私服时,所使用仓库组添加错** ###
<!-- more -->
![](http://7xire1.com1.z0.glb.clouddn.com/2015-10-18%2022%3A56%3A17屏幕截图.png)


左边为添加到仓库租的仓库,右边为可使用仓库.添加错无法使用的.


-  ### **使用新版本maven,在eclipse中运行命令汇报异常:-Dmaven.multiModuleProjectDirectory system propery is not set. Check $M2_HOME environment variable and mvn script match.** ###

解决办法:配置M2_HOME环境变量,然后在Window->Preference->Java->Installed JREs->Edit
在Default VM arguments中设置
-Dmaven.multiModuleProjectDirectory=$M2_HOME


![](http://7xire1.com1.z0.glb.clouddn.com/2015-10-18%2023%3A41%3A45屏幕截图.png)




