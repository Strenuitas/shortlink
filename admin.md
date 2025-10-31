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

# mybatis框架

