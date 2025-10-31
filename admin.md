# 包目录介绍：
😜😜😜😜😜😜😜😜😜😜
Common: 公共包      💩
  biz.user:biz业务  网关传过来的用户信息通过过滤器解析成用户上下文放在用户的请求里面，同时把UserInfo放到UserContext里面
  constant:系统常量类
  convention:规约
  database:数据库持久层  行数据的基础属性： 创建时间 修改时间 删除标识
  enums:系统枚举
  serialize:脱敏序列化类  json框架通过反序列化去做脱敏
  web:全局异常拦截器
Config:配置类
Controller：控制层  💩  MVC里面的控制器
dao:数据持久层    💩
  entity:数据库表实体
  mapper：持久层
dto:数据传输实体  💩
  req:数据请求实体
  resp:数据相应实体
remote:RPC远程调用  💩
Service:业务接口层  💩
  impl:业务实现层
toolkit:方法工具类  💩
ShortLinkAdminApplication:应用程序启动工具类  💩

Apifox：
  API文档   API调试  
  
# @RestController = @RequestMapping + @RequestBody 

# 注解：给代码贴上标签，使得编译器或者框架根据代码上的标签标识来执行代码    语言层面上：是一个元数据


# 访问主机+ 端口号 = 定位应用程序
通过URL：localhost  主机ip + 端口号 8080 ：  正在监听8080端口的应用程序，  /user/create  找到Spring MVC中映射到这个路径的方法 
localhost:8080//user/create

# Spring MVC里面的注解 @RequestMpaping  映射Web请求到控制器方法


# 用户名下功能
检查用户是否存在···注册用户···修改用户···根据用户名查询用户···用户登录···检查用户是否登录···用户退出登录···注销用户(软删除在数据库中的UserInfo)

# 数据库里面存的真实的用户信息： 真实姓名 手机号 邮箱 需要进行加密处理。国家要求

# 业务层使用接口层再加实现类的原因：
 1：面向接口编程
     接口：规范：定义做什么
     实现：具体怎么做
     通过接口调用对象，不依赖实现类      
 2.解耦层次
     Controller->Service->Mapper->DB
     Service屏蔽了具体的业务实现的逻辑
     Controller只关心实现什么功能，不关心如何实现
     这样做符合单一职责原则(SRP)和依赖倒置原则(DIP)
  UserService extends Iservice<UserDO> 是Mybatis-plus的设计思想,后者写好了了CRUD的接口，且是对UserDO进行操作，也就不必要在写一个CRUD，
  UserServiceImpl extends ServiceImpl<UserMapper,UserDO> 则是 后者实现了Iservice的CRUD的接口，且指定Service使用哪个Mapper操作
# Controler层里面注入Service 我们使用构造器方式注入
@RequiredArgsConstructor    lombok提供的注解，简化构造器注入，自动为类中的final字段生成一个带参构造器，final修饰一旦注入不能被修改，多线程下更安全，一眼看着就是必须的不可缺少。
@Data 注解自动生成 get set toString 以及 equals 和hashCode方法，如没有定义构造函数也会生成一个空参构造

@Service  
Spring提供的注解，标记 业务层 类，让Spring注册为Bean并管理，Spring容器启动时，会自动创建它的实例并注册为bean，这样就可以在其他地方通过依赖注入DI，使用它，例如Controller

baseMapper是父类ServiceImpl提供的Mapper代理对象，所以他本质是一个Mapper的实例，类型是UserMapper,本身继承了BaseMapper，本身没有特殊逻辑，只是动态代理Mapper接口，他也是ServiceImpl的成员变量。所以既可以调用BaseMapper提供的写好的CRUD方法也可以使用UserMapper自定义的方法。
