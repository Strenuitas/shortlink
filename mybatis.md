mybatis是一个半持久层框架，通过xml配置文件或者注解来描述sql语句，简化了java程序与数据库之间的交互，赋予我们完全控制sql语句的权利，方便我们实现比简单的增删改查更为复杂的操作数据库的能力，  
1.sql映射：允许将sql语句与java方法进行映射，可以将sql语句从数据库里得到的结果映封装为一个java的对象，可以写原生的sql语句。  
ORM框架提供方法与sql语句的映射，例如User user = userMapper.selectById(1);  
mybatis是一个半自动化的持久层框架  
而Hibernate则是一个ORM的全自动化的框架，我们只需要给出要具体的实现的操作，sql语句则不用自己写，框架能帮我们完全实现，适合表简单的结构。  



## @MapperScan()
使得Spring在扫描后面的路径将对应的接口，并将他们注册为容器中的bean   


## Bean  
Spring注册到容器里的bean是类而不是对象  

@Mppaer   Mybatis里面的注解，本质是告诉Mybatis和Spring，这个接口是一个Mpper，需要被Spring扫描并生成代理对象（实现类），以便进行sql操作。是一个数据库操作接口。  


