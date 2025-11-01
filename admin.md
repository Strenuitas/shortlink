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


# 全局统一返回实体 Result和Results  后面 implements Serializable  标记这个类的对象可以被序列化和反序列化 序列化 对象-二进制字节流存入外部系统(redis)  反序列化反之
class Result{
  private static final long serialVersionUID = 5679018624309023727L;    维护一个版本号，提示如果微服务下另一个存储了相同类的服务(应用程序)要从Redis或外部系统中取数据，要进行反序列化的时候，如果 有这个版本号，写死的就会兼容，即如果有小的改动 比如加一个字段，照样能反序列化成功。
}
# 这里还有一个Results类，设计的目的是为了使全局返回实体Result使用set链式创建对象，从而能够创建带不同参数的Result返回给用户。

# Controller  如果是请求的URL上面加上要传的参数   响应的方法的参数 前要加注解 @PathVariable  且在方法上面的GetMapping("url/{username}")加上占位符 ；      如果不想更改url 只是传递参数，改为@RequestParam   就可以在请求附带的参数下面的Query后面设置 username = wangba了

# 全局统一异常码设计    
# 全局异常设计    只有服务端和远程调用的时候的异常我们才需要报警，需要后端人员尽快处理，如果是客户端异常则不需要，那是因为客户的问题，并不是我们的应用的问题  所以将 客户端异常，服务端异常，远程调用异常抽象出来一个抽象异常接口
# 全局异常拦截器  @RestControllerAdvice    GlobalExceptionHandler      有异常 先是 提示对应的异常，然后到Results.failure里面去构造返回的实体带上异常信息

##  脱敏处理 普通想法即在请求对应方法上面加一层注解然后经过AOP扫描到这个注解，然后经过反射递归的一层层直到去获取到RespDTO，获取到返回实体的某个加了一层注解(标记是比如手机号类型)的字段，然后对这个字段进行脱敏展示，这样太麻烦
    不如直接使用Jackson默认序列化的方式，就是我们方法返回的是对象，到前端却是Jason字符串，是因为SpringBoot在我们的web请求，默认的会把返回的对象进行序列化的方式，把它序列化成字符串
    即创建一个字段的序列化类继承JSonSerializer<String>，进行对应的脱敏，可以利用hutool工具包的DesensitizedUtil.mobilePhone(phone)对手机号进行脱敏，然后在对应的返回对象即RespDTO上相应的字段上加上注解 @JsonSerialize(using = PhoneDesensitizationSerializer.class)，即可让JackSon进行序列化的时候，能对这个
    字段进行泛解析，去实现我们的自定义的一个序列化。


# Wrappers是Mybatis-plus提供的查询条件封装器，用于构造查询条件，先使用Wrappers.lambdaQuery().eq(UserDO::username,username)后面是请求传入的，前者则是UserDO的username，构建一个查询条件queryWrapper，查询UserDO中字段username=传入的username值的记录，然后再使用
  baseMapper,这个也是Mybatis-plus提供的基础Mapper接口，用与给出基础的CRUD的操作，baseMapper.selectOne(queryWrapper)执行查询.等于说前者写好sql语句，后者则是带着sql语句去数据库里面去查

# 枚举类型 是写好的枚举类的对象，只不过写法例如     USER_NULL("B000200", "用户记录不存在"),
  
  
