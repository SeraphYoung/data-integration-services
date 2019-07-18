

dataintegrationservices 模块中子模块构造如下：
 1. gatherer 模块负责数据的收集
 2. operationscenter 模块负责数据的处理（调用common-plugins模块中的功能）
 3. server 用来服务启动（使用了spring-eureka）
    3.1 eureka 是一个类似与管理各种服务的框架（这个项目中存在两个核心项目 gather 和 operationscenter). 这里的eureka主要负责 gather 和 operationscenter 两个服务的维护. 所以项目的运行应该是要先开启eureka服务，再开启 gatherer 和 operationscenter.
    3.2 由于每个服务开启之后，都需要向 eureka 注册， eureka 本身也不例外。 因此 server 有两个eureka, 都需要开启，完成互相注册（如果只开启一个则会存在自己向自己注册的循环情况）. 如果只想开启一个eureka， 那么需要在配置文件（例如只开启eurekaone, 配置文件路径为：eurekaone/src/main/resources/application.properties）修改：
      eureka.client.fetch-registry=false
      eureka.client.register-with-eureka=false
     3.3 如果eureka 启动失败，可能是eureka 的配置文件logback.xml （第9行） 
          <property name="log_home" value="/home/gavin/workspace/info" /> 
          value 值 修改 为你在本地已经有的文件夹路径
 4. gatcommon 和 gatcontroller 的结构框架很接近，举例gatherer而言， gatcommon存放了基本类型，gatcontroller存放了url控制信息，gatcore 有properties 控制文件（如果报错提示找不到文件， 基本上都在 logback.xml 中修改文件路径）， gatentity 是一些实体类， gatrepository 放置依赖,数据库结构等. gatservice 处理用户业务逻辑。 gatweb 是服务启动入口。
 
 
 
 common-plugins 模块中存在有众多数据库操作
 其中： 1. mysql, redis, mongodb 三个子模块只是单纯的三大数据库的增删改查
       2. elasticsearch 和 solr 是两种检索引擎， 用来对数据库的数据进行检索操作。 部署和代码都是通用的，参考：              http://i.zhcy.tk/blog/elasticsearchyu-solr/
       3. thirdparty-interface 调用了一堆第三方接口（支付宝、腾讯什么的）。应该是为了方便诸如：微信登录、支付宝支付等功能
       4. klutz 中包含了标准的mvc架构，猜测应该数据服务启动类
       5. common 中还是包含了基本的自定义数据类

