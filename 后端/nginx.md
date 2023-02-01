## nginx

### 1、nginx基本概念

#### 1）nginx是什么

nignx是一个高性能的http反向代理服务器，特点是内存少，并发能力强，事实上nginx的并发能力确实在同类的网页服务器表现好。nginx转性能优化开发，性能是最重要的考量能承受50000个并发连接

#### 2）反向代理

正向代理![1646211101353](C:\Users\xf\Desktop\学习笔记\后端\assets\1646211101353.png)

反向代理![1646211247123](C:\Users\xf\Desktop\学习笔记\后端\assets\1646211247123.png)

#### 3）负载均衡

客户端发送很多请求到服务端

#### 4）动静分离

为了加快网站的速度，可以把动态资源和静态资源分开处理![1646211729344](C:\Users\xf\Desktop\学习笔记\后端\assets\1646211729344.png)

### 2、nginx安装、常用命令和配置文件

#### 1）在linux系统中安装nginx

![1646213331085](C:\Users\xf\Desktop\学习笔记\后端\assets\1646213331085.png)

#### 2）nginx常用命令

1、使用nginx操作命令的条件：必须进入nginx的目录：/usr/local/nginx/sbin

2、查看nginx的版本号

./nginx -v

3、启动nginx

./nginx

4、关闭nginx

./nginx -s stop

5、重新加载nginx

./nignx -s reload

#### 3）nginx配置文件

位置：/usr/local/nginx/conf/nginx.conf

**组成**：

第一部分：全局块

​	从配置文件开始到events块之间的内容，主要会设置一些影响nginx服务器整体运行的配置指令，主要包括配置运行Nginx服务器的用户组、允许生成的worker process数，进程pid存放路径、入职存放路径和类型已经配置文件的引入等

![1646227260636](C:\Users\xf\Desktop\学习笔记\后端\assets\1646227260636.png)

第二：events块

events块设计的指令主要影响Nginx服务器与用户的网络连接

比如worker_connections 1024;//支持最大连接数

第三：http块	

Nginx服务i去配置中最频繁的部分

http块也可以包括http全局快，server块

### 3、配置反向代理

![1646229321339](C:\Users\xf\Desktop\学习笔记\后端\assets\1646229321339.png)

1、实现效果
1）、打开浏览器、在浏览器输入地址www.123.com,跳转tomcat主页面
2、准备工作
1）、在linux装tomcat，默认端口8080

2)、访问过程

3）、浏览器输入域名之后：会到本地的host（c://windows/system32/drivers/etc/hosts）文件中查找，如果没有的话，去网络上的dns域名解析器上寻找



![1646230246619](C:\Users\xf\Desktop\学习笔记\后端\assets\1646230246619.png)

### 4、配置负载均衡

负载均衡nginx.conf文件中的配置

![1646273212515](C:\Users\xf\Desktop\学习笔记\后端\assets\1646273212515.png)

1、实现效果浏览器输入地址，负载均衡的到8080和8081上面

##### nginx负载均衡策略

###### 轮询策略

每个请求按时间顺序琢一分配到不同的后端服务器，如果后端服务器dow，可一自动删除。

###### weight权重策略

![1646273587376](C:\Users\xf\Desktop\学习笔记\后端\assets\1646273587376.png)

###### iphash的方式

每个请求按访问ip的hash结果分配，这样每个方可固定访问一个后端服务器，可以解决session的问题。

###### fair

按照后端服务器的相应时间分配

### 5、配置动静分离

通过location指定不同的后缀名实现不同的请求赚翻。同个expires参数设置，可以使浏览器缓存国企时间，减少与服务器之前的请求和流量。具体expires定义：是给一个资源设定一个国企时间，也就是无需区服务端验证，直接通过浏览器自身确认是否过期

### 6、nginx高可用

keepaliced 配置虚拟ip