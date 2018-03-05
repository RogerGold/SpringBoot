# 连接M有SQL数据库

## MySQL安装教程

[下载地址](https://dev.mysql.com/downloads/mysql/)
[参考地址](https://www.cnblogs.com/chengxs/p/5986095.html)

## SpringBoot连接MySQL

### pom.xml添加依赖:

    <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
    </dependency>
    
### resources/application.properties文件添加数据库配置

    # 数据库基本配置
    spring.datasource.url=jdbc:mysql://localhost:3306/world
    spring.datasource.username=root
    spring.datasource.password=root
    # 显示后台处理的SQL语句
    spring.jpa.show-sql=true
    
[参考地址](http://blog.csdn.net/jinbaosite/article/details/77587600)

## NoSuchMethodError: javax.persistence.Table.indexes()[Ljavax/persistence/Index
在依赖的Lib里把JEE6删除。
依赖包里有两个方法的参数虽然都是table，但是类型是不同的。一个是JPA api中的Table类，另一个是hibernate的Table类。

[参考地址](https://my.oschina.net/JasonZhang/blog/539095)

##  Unknown column in field list问题 

默认Spring使用org.springframework.boot.orm.jpa.SpringNamingStrategy生成表名和列名，SpringNamingStrategy是org.hibernate.cfg.ImprovedNamingStrategy的继承，ImprovedNamingStrategy 会将驼峰格式转换成下划线格式。
这就会导致 “Unknown column in field list”这种错误。

解决这种问题有两种办法：

- 将列名全部改为小写 @column(name="testname")
- 修改命名策略：spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl

[参考地址1](https://github.com/spring-projects/spring-boot/issues/2129)

[参考地址2](https://stackoverflow.com/questions/25283198/spring-boot-jpa-column-name-annotation-ignored)
