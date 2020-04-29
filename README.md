# bx-cloud

## 一、背景

前后端分离已经成为互联网项目开发标准，它会为以后的大型分布式架构打下基础。[SpringBoot](https://projects.spring.io/spring-boot/)使编码配置部署都变得简单，越来越多的互联网公司已经选择SpringBoot作为微服务的入门级微框架。

[Mybatis-Plus](https://github.com/baomidou/mybatis-plus)是一个 [Mybatis](http://www.mybatis.org/mybatis-3/) 的增强工具，有代码生成器，并且提供了类似hibernate的单表CRUD操作，又保留了mybatis的特性支持定制化 SQL。

由于升级到了SpringCloud，认证和授权改用SpringSecurity Oauth2.0。如果需要看原来shiro单项目版本，请选择springboot分支。

现在API越来越流行，如何安全保护这些API？ [JSON Web Tokens](https://jwt.io/)(JWT)能提供基于JSON格式的安全认证。JWT可以跨不同语言，自带身份信息，并且非常容易传递。项目基于灵活配置方式选择是uuid的令牌还是JWT令牌

## 二、项目特性

1.自定义@Log注解自动记录日志到数据库。

2.自定义@Pass注解接口不用进行认证身份。

3.使用JSONObject统一获取body请求参数，减少实体类的数量。完成自定义@ValidationParam注解验证请求参数是否为空。

      ![](https://oscimg.oschina.net/oscnet/f3baf3e96123d41a8fff8bf2ac62684b9bb.jpg)

4.使用bcrypt算法加密密码，著名代码托管网站Github和美国军方防火墙同样采用此算法，靠bcrypt算法会成功保住密码强度不算很高的大部分账户。

5.搭配Shiro注解配置权限，高度灵活，提供按钮级别的权限控制，后端接口只验证权限，不看角色。用自定义@CurrentUser注解获取当前登录用户，Controlle层统一异常处理：

     ![](https://static.oschina.net/uploads/space/2018/0512/234950_u2kv_3577599.png)

6.用SpringAOP切面编程进行声明式事务(service层增删改方法命名规范会自动加上事务)，过滤请求参数，防止XSS攻击。

7.使用POST请求登录返回token和权限信息，保证请求无状态，返回实体如果属性为空不显示。

    ![](https://oscimg.oschina.net/oscnet/a12b5783657b7fd29f413c4a231bb6d6a7e.jpg)

8.完成微信/微博/QQ第三方登录功能，完成用户名电话邮箱三种方式登录,WebSocket实时消息推送,短信登录注册等功能.

9.完成方法限流注解,重要防刷方法被访问距离下一次时间可调节

10.自己实现轻量级工作流,用状态机完成

11.整合快捷操作excel组件,加快开发速度

## 三、程序逻辑

1.填写用户名密码用POST请求访问/login接口，返回token令牌等信息，失败则直接返回身份错误信息。

2.在之后需要验证身份的请求的Headers中添加Authorization和登录时返回的token令牌。

3.服务端进行token认证，失败身份错误信息。


## 四、运行项目


-   通过git下载源码，本项目基于JDK1.8

-   采用Maven项目管理，模块化，导入IDE时直接选定liugh-parent的pom导入

-   升级的SpringCloud版，基本环境只需要启动MySQL，Redis和bx-core-server里的两个服务。

-   每个项目中的sql文件对应一个数据库先创建好。

-   启动顺序Redis-->MySQL-->register-config-center-->user-center

-   其他服务根据需要启动

-   修改bootstrap.properties，更新MySQL，Redis账号和密码。

-   Eclipse、IDEA运行每个项目的XXApplication.java启动类。或在liugh-parent目录下运行命令mvn clean package，然后在每个项目/target目录下运行java -jar xx.jar命令

-   访问登录接口：localhost:8000/oauth/user/token

    ![](https://oscimg.oschina.net/oscnet/up-55bdfe18f6a908ad3ed3ab9f6a750b65f21.png)

-   Content-Type用application/json方式，账号密码：
{
	"username":"admin",
	"password":"admin",
	"clientId":"system",
	"clientSecret":"system",
	"scope":"app",
	"grantType":"password"
}

-   获取token访问其他接口

-   注意!!!!!编译器请安装lombok插件,不然会报红


运行截图：

![](https://oscimg.oschina.net/oscnet/up-bc8acff8b14d093d3bf0ae0ea08df8576fe.png)


第一次做自己的项目，经验不足，如果大家有什么好的意见或批评，请务必issue一下。

加博主进交流群，一起优化进步（或wx搜索：53182347）：


![](https://oscimg.oschina.net/oscnet/up-82beddfedc2723fc46f04606ac698792659.png)











