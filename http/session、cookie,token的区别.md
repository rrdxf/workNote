### session、cookie,token的区别

#### session（会话）

##### 介绍

当用户打开某个web应用时，便与web服务器产生一次session。服务器会使用session把用户的信息临时保存在服务器上，用户离开网站后，session会销毁。这种信息存储方式相对cookie来说更安全。

​	用户首次与web服务器建立连接的时候，服务器会给用户分发一个sessionid作为标识。sessionid包含在http头中交给web服务器，这样web服务器就能区分当前请求页面是哪一个客户端。这个sessionid就是保存在客户端的，属于客户端session。

​	客户端session默认以cookie的形式来存储，所以当用户禁用了cookie的话，服务器就得不到sessionid。这时我们可以用url的方式在存储客户端session。也就是将sessionid直接写入url中（不常用）

##### 产生：

sessionid是一个会话的key，浏览器第一次访问会在服务器端产生session，有一个sessionid对应。tomcat生成的sessionid叫做jsessionid

session在访问tomcat服务器HttpServletRequest的getSession(true) 。tomcat的ManagerBase类提供创建sessionid的访问：随机数+时间+jvmid。客户端只保存sessionid到cookie中，而不会保存session，session销毁只能通过invalidate或超时，关掉浏览器并不会关闭session。 

##### 使用：

**getSession()/getSession(true)、getSession(false)的区别**

getSession()/getSession(true)：当session存在时返回该session，否则新建一个session并返回该对象

getSession(false)：当session存在时返回该session，否则不会新建session，返回null

**缺陷：**如果服务器做了负载均衡，那么下一个操作请求到了另一台服务器的时候session会丢失

##### 解决

**session共享问题**

当下的互联网网站为了提高网站安全性和并发量，服务端的部署的服务器的数量往往是大于或等于两台，多台服务器对外提供的服务是等价的，但是不同的服务器上面肯定会有不同的web容器，由上面的讲述我们知道session的实现机制都是web容器里内部机制，这就导致一个web容器里所生成的session的id值是不同的，因此当一个请求到了A服务器，浏览器得到响应后，客户端存下的是A服务器上所生成的session的id，当在另一个请求分发到了B服务器，B服务器上的web容器是不能识别这个session的id值，更不会有这个sessionID所对应记录下来的信息，这个时候就需要两个不同web容器之间进行session的同步。

一般大型互联公司的网站都是有一个个独立的频道所组成的，例如我们常用的百度，会有百度搜索，百度音乐，百度百科等等，我相信他们不会把这些不同频道都给一个开发团队完成，应该每个频道都是一个独立开发团队，因为每个频道的应用的都是独立的web应用，那么就存在一个跨站点的session同步的问题，跨站点的登录可以使用单点登录的（SSO）的解决方案，但是不管什么解决方案，跨站点的session共享任然是逃避不了的问题。

#### cookie

