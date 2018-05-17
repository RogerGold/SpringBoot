# Spring boot 配置mybatis

1. 添加依赖pom.xml

      <!-- 设置mybatis -->
          <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.1.1</version>
          </dependency>
          <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>6.0.4</version>
          </dependency>

2. 创建接口Mapper（不是类）并在resource目录下创建对应的Mapper.xml文件, 注意方法名称要和Mapper.xml文件中的id一致，这样会自动对应上。


    public interface StudentMapper {

        List<Student> getAll();

        Student getByName(String name);

        void insert(Student user);

        void update(Student user);

        void deleteByID(int id);

    }
   
Mapper.xml文件
   
     <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
    <!--你接口的包名是com.abc.dao,接口名是NameMapper.java，那么你的mapper.xml的namespace应该是com.abc.dao.NameMapper-->
    <mapper namespace="com.example.demo.dao.StudentMapper" >

        <!--resultMap对应的是表与实体类的映射  - type 数据库表对应的实体类，别名或完整类名都可以-->
        <resultMap id="BaseResultMap" type="com.example.demo.entity.Student" >
            <!-- 结果集的主键 -->
            <id column="id" property="id" jdbcType="INTEGER" />
            <!-- 普通的列  -column 是数据库中字段， property是实体类中字段-->
            <result column="name" property="name" jdbcType="VARCHAR" />
            <result column="age" property="age" jdbcType="INTEGER" />
        </resultMap>

        <sql id="Base_Column_List" >
            id, name, age
        </sql>
       <!--注意id和Mapper中的方法名一致-->
        <select id="getAll" resultMap="BaseResultMap"  >
            SELECT
            <include refid="Base_Column_List" />
            FROM student
        </select>
        <!--parameterType(输入类型)、resultType(输出类型)-->
        <select id="getByName" parameterType="java.lang.String" resultMap="BaseResultMap" >
            SELECT
            <include refid="Base_Column_List" />
            FROM student
            WHERE name = #{name}
        </select>

        <insert id="insert" parameterType="com.example.demo.entity.Student" >
           INSERT INTO
              student
              (name,age)
            VALUES
              (#{name}, #{age})
        </insert>

        <update id="update" parameterType="com.example.demo.entity.Student" >
            UPDATE
            student
            SET
            <if test="name != null">name = #{name},</if>
            age = #{age}
            WHERE
            id = #{id}
        </update>
        <delete id="deleteByID" parameterType="java.lang.Long" >
           DELETE FROM
               student
           WHERE
               id =#{id}
        </delete>
    </mapper>
    
3. 在application.properties添加数据库和mybatis配置

    spring.datasource.driverClassName = com.mysql.jdbc.Driver
    spring.datasource.url = jdbc:mysql://localhost:3306/students_db?useUnicode=true&characterEncoding=utf-8&useSSL=true
    spring.datasource.username = root
    spring.datasource.password = root

    #mybatis.typeAliasesPackage：为实体对象所在的包，跟数据库表一一对应
    #mybatis.mapperLocations：mapper文件的位置
    mybatis.typeAliasesPackage=com.example.demo.entity
    mybatis.mapperLocations=classpath:mybatis/mapper/*Mapper.xml

4. 配置启动类，添加MapperScan注解

  @SpringBootApplication
  @MapperScan("com.example.demo.dao") // mybatis扫描路径，针对的是接口Mapper类
  public class SpringBootDemoApplication {

      public static void main(String[] args) {
          SpringApplication.run(SpringBootDemoApplication.class, args);
      }
  }
