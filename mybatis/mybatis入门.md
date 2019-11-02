## mybatis入门

***

### 简介

什么是mybatis?

MyBatis 是一款优秀的持久层框架，它支持定制化 SQL、存储过程以及高级映射。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以使用简单的 XML 或注解来配置和映射原生类型、接口和 Java 的 POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

### 核心sqlSessionFactory

mybatis核心是以**sqlSessionFactory**实例，该实例可以通过SqlSessionFactoryBuilder来获得，SqlSessionFactoryBuilder是通过xml文件来获取。

* 通过Mybatis带的Resource工具类构建sqlSessionFactory

  ```java
  String resource = "org/mybatis/example/mybatis-config.xml";
  InputStream inputStream = Resources.getResourceAsStream(resource);
  SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
  ```

* xml文件核心配置，以下包括DataSource和TransactionManager（事务管理器）

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
    <environments default="development">
      <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
          <property name="driver" value="${driver}"/>
          <property name="url" value="${url}"/>
          <property name="username" value="${username}"/>
          <property name="password" value="${password}"/>
        </dataSource>
      </environment>
    </environments>
    <mappers>
      <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>
  </configuration>
  ```

  当然也可以通过非xml方式配置mybatis，就是用一些mybatis提供的类来进行java代码的配置，由于很多复杂如嵌套联合映射等都还要用xml配置，尤其是现在很多场景用springboot的applicalion.yml配置，这里对Java代码配置就不过多赘述。

### 从 SqlSessionFactory 中获取 SqlSession

有了SqlSessionFactory ，就要获取sqlsession最终查出我们想要查的数据。先是通过sqlsessionFactory获得到sqlSession，然后通过sqlSession获得相应的mapper,相应的mapper可以查的相应的数据，以后还会慢慢展开这点。下面看一下这一过程代码：

```java
try (SqlSession session = sqlSessionFactory.openSession()) {
  BlogMapper mapper = session.getMapper(BlogMapper.class);
  Blog blog = mapper.selectBlog(101);
}
```

以上就是mybatis的简单入门，以后还会在此基础上对sql执行过程以及mybatis的特性进行展开。

