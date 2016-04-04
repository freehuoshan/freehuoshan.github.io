title: CAS单点登录证书配置
date: 2016-03-31 21:21:58
tags: CAS
---

前段时间公司需要做单点登录，现在记录一下过程，好记性不如烂笔头。有时候想写这些也没什么用，一个好的程序员不应该死板的记这些。看来我只能属于不怎样的行列了。

## 配置证书

首先使用cas单点登录需要使用https协议，所以需要配置证书。我这里只有cas服务器使用了https协议，单点登录客户端（应用）都使用http协议，所以：只需要

1. 使用cas服务器域名生成证书，配置cas服务器。
2. 使用刚才生成的证书导出客户端证书，然后导入到客户端jre证书库中（/jre/lib/security/cacerts）

使用java自带keytool生成证书：

### 生成服务器端证书

    keytool -genkey -alias  casserver -keyalg RSA -keystore /opt/keystore/caskey
    
  casserver是该证书在证书库中的别名，在证书库中别名是唯一的
    
  ![](http://7xire1.com1.z0.glb.clouddn.com/springsecurity-cas-zhengshu1.png)
  
  在这里，您的名字与姓氏，要输入该应用的域名，在这里就是cas服务器的域名
  
### 生成客户端证书

使用刚才生成的服务器端证书导出客户端证书

        keytool -export -file /opt/keystore/caskey.crt -alias casserver -keystore /opt/keystore/caskey 

![http://7xire1.com1.z0.glb.clouddn.com/springsecurity-cas-zhengshu2.png](http://7xire1.com1.z0.glb.clouddn.com/springsecurity-cas-zhengshu2.png)

### 配置

OK,现在证书都已经生成了，接下来就是配置了

#### 服务器端配置

如果使用Tomcat：

        <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"  
               maxThreads="150" scheme="https" secure="true"  
               clientAuth="false" sslProtocol="TLS"   
               keystoreFile="/opt/keystore/caskey" 
               keystorePass="123456"/>
               

就这样简单，服务器端证书就配置完成了

如果要是使用到apache或者nginx,那也就不用在tomcat上配置了，生成apatche或nginx对应证书在Apatche或nginx服务器上配置就OK了

#### 客户端配置

因为我这里客户端使用的是http协议，所以不需要配置什么东西，只需要把生成的客户端证书导入到jre证书库中就可以了。当然如果客户端也使用的https协议，那么就像cas服务器配置就是了

导入证书库证书:

    keytool -import -keystore /opt/jdk8/jre/lib/security/cacerts -file /opt/keystore/caskey.crt -alias   caskey
    
![http://7xire1.com1.z0.glb.clouddn.com/springsecurity-cas-zhengshu3.png](http://7xire1.com1.z0.glb.clouddn.com/springsecurity-cas-zhengshu3.png)

在导入时要求输入证书库密码：jre证书库默认密码为changeit。如果实在忘记密码，而证书库中又没有重要证书，可以直接把cacerts文件删掉。重新生成

到此证书配置完成

        






  
  