

# Mybatis-note

## 1、简介

### 1.1、MyBatis 历史

- MyBatis是Apache 的一个开源项目iBatis, 2010 年6 月这个项目由Apache Software Foundation 迁移到了

  Google Code，随着开发团队转投Google Code 旗下， iBatis3.x正式更名为MyBatis ，代码于2013 年11 月

  迁移到Github

- iBatis 一词来源于“internet”和“abatis”的组合，是一个基于Java 的持久层框架。iBatis提供的持久层框架包括

  SQL Maps 和Data Access Objects（DAO）

### 1.2、简介

- MyBatis 是支持定制化SQL、存储过程以及高级映射的优秀的**持久层框架**
- MyBatis 避免了几乎所有的JDBC 代码和手动设置参数以及获取结果集
- MyBatis 可以使用简单的XML 或注解用于配置和原始映射，将接口和Java 的POJO（Plain Old Java Objects，普通的Java 对象）映射成数据库中的记录

```xml
Maven仓库
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.2</version>
</dependency>
```

### 1.3、为什么使用Mybatis---现有持久化技术的对比

#### 1.3.1、JDBC

- SQL 夹在Java 代码块里，耦合度高导致硬编码内伤
- 维护不易且实际开发需求中sql 是有变化，频繁修改的情况多见

#### 1.3.2、Hibernate 和JPA

- 长难复杂SQL，对于Hibernate 而言处理也不容易
- 内部自动生产的SQL，不容易做特殊优化
- 基于全映射的全自动框架，大量字段的POJO 进行部分映射时比较困难。导致数据
- 库性能下降

#### 1.3.3、MyBatis

- 对开发人员而言，核心sql 还是需要自己优化
- sql 和java 编码分开，功能边界清晰，一个专注业务、一个专注数据

## 2、第一个程序

### 2.1、搭建数据库

### 2.2、导入依赖

```xml
<!--导入依赖-->
<dependencies>
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.18</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.2</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/junit/junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### 2.3、编写实体类

```java
public class User {
    private int id;
    private String name;
    private String pwd;
    
    public User() {
    }

    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public int getId() {return id }

    public void setId(int id) this.id = id;}

   public String getName() {return name;}

    public void setName(String name) {this.name = name;}

    public String getPwd() {return pwd;}

    public void setPwd(String pwd) {this.pwd = pwd;}
    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}
```

### 2.4、创建核心配置文件mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 数据库连接环境的配置-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=GMT"/>
                <property name="username" value="root"/>
                <property name="password" value="100104"/>
            </dataSource>
        </environment>
    </environments>

<!-- 每一个mapper.xml都需要在Mybatis核心配置文件中-->
    <!-- 引入SQL映射文件,Mapper映射文件-->
    <mappers>
        <mapper resource="UserMapper.xml"/>
    </mappers>

```

### 2.5、创建Mybatis 的sql 映射文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.yan.dao.UserMapper">

    <select id="getUserList" resultType="com.yan.bear.User">
        select * from mybatis.user
    </select>
</mapper>

<!--
1 Mapper 接口与Mapper映射文件的绑定
在Mppper 映射文件中的<mapper>标签中的namespace 中必须指定Mapper 接口
的全类名
2 Mapper 映射文件中的增删改查标签的id 必须指定成Mapper 接口中的方法名.
测试中遇到的问题
-->
```

### 2.6、Mapper 接口

```java
public interface UserMapper {
    List<User> getUserList();
}
```

### 2.7、工具类

```java
public class MybatisUtils {

    public static SqlSessionFactory sqlSessionFactory;
    static {
        try {
            //获取sqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}
```

### 2.8、测试类

```java
public class UserDaoTest {
    @Test
    public void Test(){
            SqlSession sqlSession = (SqlSession) MybatisUtils.getSqlSession();
            try{
                //方式一：Mapper接口:获取Mapper接口的代理实现类对象
                UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
                List<User> userList = userMapper.getUserList();
                //方式二
/*
        List<User> userList = sqlSession.selectList("com.yan.dao.UserMapper.getUserList");
*/
                for (User user : userList) {
                    System.out.println(user);
                }
            } finally {
                sqlSession.close();
            }
    }
}
```

![image-20200218222424596](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200218222424596.png)

### 2.9、遇到的问题

```xml
<!--资源过滤问题--> 
<build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>

异常错误：java.sql.SQLException: The server time zone value '?й???????' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.

显示新版本的数据库连接程序需要指定UTC时区，改正方法将配置文件中的“url”后面加上指定的时区，将其值改
 <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=GMT"/>
```

## 3、CURD

### 3.1、namespace

**namespace 中必须指定Mapper 接口的全类名**

### 3.2、select

#### 3.2.1、编写接口

```java
//根据id查询
    User getUserById(int id);
```

#### 3.2.2、编写对应的mapper中的sql语句（mapper映射文件）

```xml
<select id="getUserById" parameterType="int" resultType="com.yan.bear.User">
        select * from mybatis.user where id = #{id}
    </select>
<!--
id:namespace中的方法名
parameterType:参数类型
resultType:sql语句执行的返回值
-->
```

#### 3.2.3、测试

```java
 @Test
    public void getUserId(){
        SqlSession sqlSession =  MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        User userById = mapper.getUserById(1);
        System.out.println(userById);
        sqlSession.close();
    } 
```

### 3.3、insert、delete、update

1. 编写接口

   ```java
   //inset一个用户
   int addUser(User user);
   //修改用户
   int updateUser(User user);
   //删除用户
   int deleteUser(int id);
   ```

2. 编写对应的mapper中的sql语句（mapper映射文件）

   ```xml
   <insert id="addUser" parameterType="com.yan.bear.User">
       insert into mybatis.user (id, name, pwd) value (#{id},#{name},#{pwd})
   </insert>
   
   <update id="updateUser" parameterType="com.yan.bear.User">
       update mybatis.user set name = #{name},pwd = #{pwd}  where id = #{id};
   </update>
   
   <delete id="deleteUser" parameterType="com.yan.bear.User">
       delete from mybatis.user where id = #{id}
   </delete>
   ```

3. 测试

   ```java
   //增删改需要提交事务
   @Test
   public void addUser(){
       SqlSession sqlSession =  MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       int res = mapper.addUser(new User(4, "ly", "1111111"));
       if (res > 0){
           System.out.println("插入成功！");
       }
       sqlSession.commit();  //提交事务
       sqlSession.close();
   }
   
   @Test
   public void updateUser() {
       SqlSession sqlSession =  MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       int res = mapper.updateUser(new User(4,"hehhe","123123"));
       if (res > 0){
           System.out.println("修改成功！");
       }
       sqlSession.commit();  //提交事务
       sqlSession.close();
   }
   
   @Test
   public void deleteUser(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       int deleteUser = mapper.deleteUser(4);
       if (deleteUser > 0){
           System.out.println("删除成功！");
       }
       sqlSession.commit();//提交事务
       sqlSession.close();
   }
   ```

### 3.4、万能Map

如果实体类或者数据库中的表、字段或者参数很多，应当考虑**Map**

```java
int addUser2(Map<String,Object> map);
```

```xml
<!--对象的属性，可以直接取出来
传递map的key
-->
<insert id="addUser" parameterType="map">
    insert into mybatis.user (id, name, pwd) value (#{userid},#{username},#{password})
</insert>
```

```java
@Test
public void addUser2(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    Map<String, Object> map = new HashMap<String, Object>();
    map.put("userid",5);
    map.put("username","hh");
    map.put("password","123456");
    mapper.addUser2(map);
    sqlSession.close();
}
```

### 3.5、模糊查询

在Java执行代码中使用通配符% %

```java
List<User> userList = mapper.getUserLike("%李%");
```

在sqlp拼接中使用通配符

```xml
<select id="getUserLike" resultType="com.yan.bear.User">
    select * from mybatis.user where name like "%"#{value}"%"
</select>
```



```java
 //模糊查询
    List<User> getUserLike(String name);
```

```xml
<select id="getUserLike" resultType="com.yan.bear.User">
    select * from mybatis.user where name like ""#{value}
</select>
```

```java
@Test
public void getUserLike(){
    SqlSession sqlSession =(SqlSession) MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    List<User> userList = mapper.getUserLike("%李%");
    for (User user : userList) {
        System.out.println(user);
    }
    sqlSession.close();
}
```

## 4、全局配置文件

### 4.1、properties 属性

编写一个配置文件db.properties

```properties
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT
username=root
password=100104
```

```xml
<configuration>
    <properties resource="db.properties"/>
    <!--
properties: 引入外部的属性文件
resource: 从类路径下引入属性文件
url: 引入网络路径或者是磁盘路径下的属性文件
-->
```

读取配置优先级：外部配置db.properties > 内部配置.xml

### 4.2、setting 设置

![image-20200219210835860](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200219210835860.png)

![image-20200219210923843](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200219210923843.png)

```xml
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <setting name="defaultStatementTimeout" value="25"/>
  <setting name="defaultFetchSize" value="100"/>
  <setting name="safeRowBoundsEnabled" value="false"/>
  <setting name="mapUnderscoreToCamelCase" value="false"/>
  <setting name="localCacheScope" value="SESSION"/>
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```

### 4.3、typeAliases 别名处理

类型别名是为 **Java 类型设置一个短的名字**。 它只和 XML 配置有关，存在的意义仅在于用来**减少类完全限定名的冗余**

```xml
<!--实体类别名-->
<typeAliases>
    <typeAlias type="com.yan.pojo.User" alias="user"/>
</typeAliases>
```

可以指定一个包，默认就是这个类的类名，首字母小写

```xml
<typeAliases>
    <package name="com.yan.pojo"/>
</typeAliases>
-----------------------------------------------------------------------------------------
<select id="getUserList" resultType="user">
    select * from mybatis.user
</select>
```

实体类较少的时候，用第一种；实体类十分多，用第二种；第一种可以DIY别名1，第二种不行

### 4.4、environments 环境配置

1. MyBatis 可以配置多种环境，比如开发、测试和生产环境需要有不同的配置
2. environment-指定具体环境     id：指定当前环境的唯一标识

```xml
<!-- 数据库连接环境的配置-->
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

    <environment id="test">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <property name="driver" value="${driver}"/>
            <property name="url" value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>
        </dataSource>
    </environment>

</environments>
```

**transactionManager**

type： JDBC | MANAGED | 自定义

**JDBC（默认）**：使用了JDBC 的提交和回滚设置，依赖于从数据源得到的连接来管理事务

范围。JdbcTransactionFactory

**MANAGED**：不提交或回滚一个连接、让容器来管理事务的整个生命周期（比如JEE应用服务器的上下文）。

ManagedTransactionFactory

自定义：实现TransactionFactory 接口，type=全类名/别名

**dataSource**

type： UNPOOLED | POOLED | JNDI | 自定义

**UNPOOLED**：不使用连接池， UnpooledDataSourceFactory

**POOLED（默认）**：使用连接池， PooledDataSourceFactory

**JNDI**： 在EJB 或应用服务器这类容器中查找指定的数据源

自定义：实现DataSourceFactory 接口，定义数据源的获取方式。

### 4.5、mapper映射器

**resource** : 引入类路径下的文件

**url** : 引入网络路径或者是磁盘路径下的文件

**class** : 引入Mapper 接口.

1. 方法一：

   ```xml
   <mappers>
       <!--   <mapper resource="UserMapper.xml"/>-->
       <mapper class="com.yan.dao.UserMapper"/>
       <package name="com.yan.dao"/>
   </mappers>
   ```

2. 使用批量注册，这种方式要求SQL 映射文件名必须和接口名相同并且在同一目录下

   ```xml
   <mappers>
       <!--   <mapper resource="UserMapper.xml"/>-->
       <package name="com.yan.dao"/>
   </mappers>
   ```



## 5、解决属性名跟字段名不一致的问题

### 5.1、resultMap 自定义映射

- ResultMap 的设计思想是，对于简单的语句根本不需要配置显式的结果映射，而对于复杂一点的语句只需要

  描述它们的关系就行了。

1) 、自定义resultMap，实现高级结果集映射

2) 、id ：用于完成主键值的映射

3) 、result ：用于完成普通列的映射

4) 、association ：一个复杂的类型关联;许多结果将包成这种类型

5) 、collection ： 复杂类型的集

![image-20200220162318480](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200220162318480.png)

```xml
<resultMap id="UserMap" type="User">
    <result property="id" column="id"/>
    <result property="name" column="name"/>
    <result property="password" column="pwd"/>
</resultMap>

<select id="getUserById" parameterType="int" resultMap="UserMap">
    select * from mybatis.user where id = #{id}
</select>
```



## 6、日志

Mybatis 的内置日志工厂提供日志功能，内置日志工厂将日志交给以下其中一种工具作代理：

- SLF4J

- Apache Commons Logging

- Log4j 2

- Log4j

- JDK logging

  ```xml
  <!-- 标准的日志工厂实现-->
  <!--<settings>
          <setting name="logImpl" value="STDOUT_LOGGING"/>
      </settings>-->
  ```

### 6.1、log4j

```xml
<dependencies>
    <!-- https://mvnrepository.com/artifact/log4j/log4j -->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
</dependencies>
```

![image-20200220171923866](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200220171923866.png)

```java
public class UserMapperTest {

    static  Logger log = Logger.getLogger(String.valueOf(UserMapperTest.class));

    @Test
    public void Test() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        try {
            //方式一：Mapper接口:获取Mapper接口的代理实现类对象
            UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
            User user =  userMapper.getUserById(1);
            /*方式二
        List<User> userList = sqlSession.selectList("com.yan.dao.UserMapper.getUserList");
*/
            System.out.println(user);
        } finally {
            sqlSession.close();
        }
    }

    @Test
    public void testLog4j(){
        log.info("info:进入了log4j方法");
        log.fine("fine:进入了log4j方法");
    }
```



## 7、分页

### 7.1、limit分页

**sql语句分页**

1. 接口

   ```java
   //分页
   List<User> getUserByLimit(Map<String, Object> map);
   ```

2. mapper映射文件

   ```xml
   <!--    //分页-->
   <select id="getUserByLimit" parameterType="map" resultMap="UserMap">
       select * from mybatis.user limit #{startIndex},#{pageSize}
   </select>
   ```

3. 测试

   ```java
   //分页
   @Test
   public void getUserByLimit(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       HashMap<String, Object> map = new HashMap<String, Object>();
       map.put("startIndex",0);
       map.put("pageSize",2);
       List<User> userList = mapper.getUserByLimit(map);
       for (User user : userList) {
           System.out.println(user);
       }
       sqlSession.close();
   }
   ```

### 7.2、RowBounds分页

```java
//分页2
List<User> getUserByRowBounds();
```

```xml
<!--分页2-->
<select id="getUserByRowBounds" resultMap="UserMap">
    select * from mybatis.user
</select>
```

```java
//分页2
@Test
public void getUserByRowBounds(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    RowBounds rowBounds = new RowBounds(1, 2);
    List<User> userList = sqlSession.selectList("com.yan.dao.UserMapper.getUserByRowBounds",null,rowBounds);

    for (User user : userList) {
        System.out.println(user);
    }
    sqlSession.close();
}
```

### 7.3、分页插件-PageHelper

官方文档：https://github.com/pagehelper/Mybatis-PageHelper/blob/master/README_zh.md







### 7.4、使用注解开发

1. 注解在接口上实现

   ```java
    @Select("select * from mybatis.user")
       List<User> getUsers();
   ```

2. 在核心配置文件中绑定接口

   ```xml
   <!--    绑定接口-->
   <mappers>
       <mapper class="yan.dao.UserMapper"/>
   </mappers>
   ```

3. 测试

```java
@Test
public void test(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    List<User> users = mapper.getUsers();
    for (User user : users) {
        System.out.println(user);
    }
    sqlSession.close();
}
```

本质：反射机制实现

底层：动态代理

#### **CURD**

1. 开启自动提交事务

   ```java
   public static SqlSession getSqlSession(){
       //自动提交事务
       return sqlSessionFactory.openSession(true);
   }
   ```

2. 注解在接口上

```java
@Select("select * from mybatis.user where id = #{id}")
User getUserById(@Param("id") int id);

@Insert("insert into user(id,name,pwd) value (#{id},#{name},#{password})")
int addUser(User user);

@Update("update user set name=#{name},pwd=#{password} where id = #{id}")
int updateUser(User user);

@Delete("delete from mybatis.user where id = #{id}")
int deleteUser(@Param("id") int id);
```

## 8、分步查询

### 8.1、association 关联 【多对一】

```mysql
create table student
(
    id   int          not null
        primary key,
    name varchar(255) null,
    tid  int          not null,
    constraint student_ibfk_1
        foreign key (tid) references teacher (id)
);
create index tid
    on student (tid);

create table teacher
(
    id   int          not null comment 'tid'
        primary key,
    name varchar(255) null
);
```

```java
List<Student> getStudent();

List<Student> getStudent2();
```

```xml
<!--==================方式一=================-->
<select id="getStudent" resultMap="TeacherStudent">
    select * from mybatis.student
</select>
<resultMap id="TeacherStudent" type="Student">
    <result property="id" column="id"/>
    <result property="name" column="name"/>

    <association property="teacher" column="tid"
                 javaType="com.yan.pojo.Teacher" select="getTeacher"/>
</resultMap>
<select id="getTeacher" resultType="Teacher">
    select * from mybatis.teacher where id = #{id}
</select>
<!--====================方式二==================-->
<resultMap id="TeacherStudent2" type="com.yan.pojo.Student">
    <result property="id" column="sid"/>
    <result property="name" column="sname"/>
    <association property="teacher" javaType="Teacher">
        <result property="name" column="tname"/>
    </association>
</resultMap>
<select id="getStudent2" resultMap="TeacherStudent2">
    select s.id as sid, s.name as sname,t.name as tname
    from mybatis.student s, mybatis.teacher t
    where s.tid = t.id
</select>
```

association 分步查询使用延迟加载

 在分步查询的基础上，可以使用延迟加载来提升查询的效率，只需要在**全局的Settings** 中进行如下的配置:

```xml
<!-- 开启延迟加载-->
<setting name="lazyLoadingEnabled" value="true"/>
<!-- 设置加载的数据是按需还是全部-->
<setting name="aggressiveLazyLoading" value="false"/>
```

### 8.2、collection 集合 【一对多】

```java
@Data
public class Teacher {
    private int id;
    private String name;

    private List<Student> students;
}
```

```java
Teacher getTeacher(@Param("tid") int id);
```

1. 按照结果嵌套处理

   ```XML
   <resultMap id="TeacherStudent" type="com.yan.pojo.Teacher">
       <result property="id" column="tid"/>
       <result property="name" column="tname"/>
       <collection property="students" ofType="Student">
           <!--property: 关联的属性名  ofType: 集合中元素的类型-->
           <result property="id" column="sid"/>
           <result property="name" column="sname"/>
           <result property="tid" column="tid"/>
       </collection>
   </resultMap>
   <select id="getTeacher" resultMap="TeacherStudent">
       select s.id sid, s.name sname, t.name tname, t.id tid
       from mybatis.student s,mybatis.teacher t
       where s.tid = t.id and t.id = #{tid}
   </select>
   ```

2. 按照查询嵌套处理

```xml
<resultMap id="TeacherStudent2" type="com.yan.pojo.Teacher">
    <collection property="students" ofType="com.yan.pojo.Student"
                select="getStudentByTeacherId" column="id">
    </collection>
</resultMap>

<select id="getStudentByTeacherId" resultType="Student">
    select * from mybatis.student where tid = #{tid}
</select>

<select id="getTeacher2" resultMap="TeacherStudent2">
    select * from mybatis.teacher where id = #{tid}
</select>
```

**扩展: association 或collection 的fetchType 属性**

1. 在<association> 和<collection>标签中都可以设置fetchType，指定本次查询是否要使用延迟加载。默

   **认为fetchType=”lazy”** ,如果本次的查询**不想使用延迟加载**，则可设置为**fetchType=”eager”.**

2. fetchType 可以灵活的设置查询是否需要使用延迟加载，而不需要因为某个查询不想使用延迟加载将全局的延

   迟加载设置关闭.

   

## 9、动态SQL

动态SQL 是MyBatis 强大特性之一。极大的简化我们==**拼装SQL 的操作**==

 动态SQL 元素和使用JSTL 或其他类似基于XML 的文本处理器相似

- **if**
- **choose (when, otherwise)**
- **trim (where, set)**
- **foreach**

### 9.1、if  where

1. **if** 用于完成简单的判断.
2. **Where** 用于解决SQL 语句中where 关键字以及条件中第一个and 或者or 的问题

```xml
<select id="queryBlogIF" resultType="com.yan.pojo.Blog">
    select * from mybatis.blog where 1=1
    <if test="title != null">
        and title = #{title}
    </if>
    <if test="author != null">
        and author = #{author}
    </if>
</select>
```

### 9.2、choose (when, otherwise)

choose 主要是用于**分支判断**，类似于**java 中的switch case**,只会满足所有分支中的一个

```xml
<select id="queryBlogIF2" resultType="com.yan.pojo.Blog">
    select * from mybatis.blog
    <where>
        <choose>
            <when test="titie != null">
                title = #{title}
            </when>
            <when test="author != null">
               AND author = #{author}
            </when>
            <otherwise>views = #{views}</otherwise>
        </choose>
    </where>
</select>
```

### 9.3、trim (where, set)

**Trim** 可以在条件判断完的SQL 语句前后**添加**或者**去掉**指定的字符

- prefix: 添加前缀
- prefixOverrides: 去掉前缀
- suffix: 添加后缀
- suffixOverrides: 去掉后缀

```xml

```

**set** 主要是用于解决修改操作中SQL 语句中可能**多出逗号**的问题

```xml
<update id="queryBlogIF3" parameterType="map">
    update mybatis.blog
    <set>
        <if test="title != null">
            and title = #{title},
        </if>
        <if test="author != null">
            and author = #{author},
        </if>
    </set>
    where id = #{id}
</update>
```

### 9.4、foreach

**主要用户循环迭代**

- collection: 要迭代的集合
- item: 当前从集合中迭代出的元素
- open: 开始字符
- close:结束字符
- separator: 元素与元素之间的分隔符
- index:   迭代的是**List 集合**: index 表示的**当前元素的下标**    迭代的**Map 集合**: index 表示的**当前元素的key**

```xml
<!-- select * from mybatis.blog where 1=1 and(id=1 or id=2 or id=3)-->
<select id="queryBlogForeach" resultType="com.yan.pojo.Blog">
    <include refid="selectsql"></include>
    <where>
        <foreach collection="ids" item="id" open="and (" close=")" separator="or">
            id = #{id}
        </foreach>
    </where>
</select>
```

### 9.5、sql

sql 标签是用于**抽取可重用的sql 片段，将相同的，使用频繁的SQL 片段抽取出来**，单独定义，方便多次引用.

**抽取sql**:

```xml
<sql id="selectsql">
    select * from mybatis.blog 
</sql>    
```

**引用sql:**

```xml
 <include refid="selectsql"></include>
```

**注意：**

- [x] 最好基于单表定义sql片段
- [x] 不要存在where标签



## 10、缓存机制

- MyBatis 包含一个非常强大的查询缓存特性,它可以非常方便地配置和定制。缓存可以极大的提升查询效率

- MyBatis 系统中默认定义了两级缓存：一级缓存   二级缓存

- 默认情况下，只有**一级缓存（SqlSession 级别的缓存，也称为本地缓存）开启**。

- **二级缓存**需要手动开启和配置，他是**基于namespace 级别的缓存**。

- 为了提高扩展性。MyBatis 定义了缓存接口Cache。我们可以通过实现Cache 接口来自定义二级缓存

### 10.1、一级缓存

**一级缓存失效的几种情况**

- 不同的SqlSession 对应不同的一级缓存
- 同一个SqlSession 但是查询条件不同
- 同一个SqlSession 两次查询期间执行了任何一次增删改操作
- 同一个SqlSession 两次查询期间手动清空了缓存

### 10.2、二级缓存

- MyBatis 提供二级缓存的接口以及实现，缓存实现要求POJO 实现Serializable 接口

- 二级缓存==**在SqlSession 关闭或提交之后才会生效**==

- 二级缓存使用的步骤:

  1、全局配置文件中开启二级缓存

  ```xml
  <setting name="cacheEnabled" value="true"/>
  ```

  2、需要使用二级缓存的映射文件处使用cache 配置缓存

  ```xml
  <cache />
  ```

  3、注意：POJO 需要实现Serializable 接口

**二级缓存相关的属性**

-  eviction=“FIFO”：缓存回收策略：


​		LRU – 最近最少使用的：移除最长时间不被使用的对象。

​		FIFO – 先进先出：按对象进入缓存的顺序来移除它们。

​		SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。

​		WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。==**默认的是LRU。**==

- flushInterval：刷新间隔，单位毫秒默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句时刷新


- size：引用数目，正整数  代表缓存最多可以存储多少个对象，太大容易导致内存溢出

- readOnly：只读，true/false

  - true：只读缓存；会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了很重要

    的性能优势。

  - false：读写缓存；会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是false。

```xml
<!--在当前mapper使用二级缓存-->
    <cache
            eviction="FIFO"
            flushInterval="60000"
            size="512"
            readOnly="true"/>
```

### 10.3、缓存的相关属性设置

1. 全局setting 的cacheEnable：配置二级缓存的开关，一级缓存一直是打开的。

2. select 标签的useCache 属性：配置这个select 是否使用二级缓存。一级缓存一直是使用的

3. sql 标签的flushCache 属性：增删改默认flushCache=true。**sql 执行以后，会同时清空一级和二级缓存**。查

   询默认flushCache=false。

4. **sqlSession.clearCache()：只是用来清除一级缓存。**

### 10.4、自定义缓存--EhCache

**EhCache** 是一个纯Java 的进程内缓存框架，具有快速、精干等特点，是Hibernate 中默认的CacheProvider

1. 导入依赖

   ```xml
   <!-- https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-ehcache -->
   <dependency>
       <groupId>org.mybatis.caches</groupId>
       <artifactId>mybatis-ehcache</artifactId>
       <version>1.2.0</version>
   </dependency>
   ```

2. 编写ehcache.xml 配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd">
       <!-- 磁盘保存路径-->
       <diskStore path="D:\360MoveData\Users\Administrator\Desktop\ehcache" />
       <defaultCache
                     maxElementsInMemory="1000"
                     maxElementsOnDisk="10000000"
                     eternal="false"
                     overflowToDisk="true"
                     timeToIdleSeconds="120"
                     timeToLiveSeconds="120"
                     diskExpiryThreadIntervalSeconds="120"
                     memoryStoreEvictionPolicy="LRU">
       </defaultCache>
   </ehcache>
   <!--
           属性说明：
            diskStore：指定数据在磁盘中的存储位置。
            defaultCache：当借助CacheManager.add("demoCache")创建Cache时，EhCache
           便会采用<defalutCache/>指定的的管理策略
           以下属性是必须的：
            maxElementsInMemory - 在内存中缓存的element的最大数目
            maxElementsOnDisk - 在磁盘上缓存的element的最大数目，若是0表示无穷大
            eternal - 设定缓存的elements是否永远不过期。如果为true，则缓存的数据始
           终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断
            overflowToDisk - 设定当内存缓存溢出的时候是否将过期的element缓存到磁
           盘上
           以下属性是可选的：
            timeToIdleSeconds - 当缓存在EhCache中的数据前后两次访问的时间超过
           timeToIdleSeconds的属性取值时，这些数据便会删除，默认值是0,也就是可闲置
           时间无穷大
            timeToLiveSeconds - 缓存element的有效生命期，默认是0.,也就是element存活
           时间无穷大
           diskSpoolBufferSizeMB 这个参数设置DiskStore(磁盘缓存)的缓存区大小.默认
           是30MB.每个Cache都应该有自己的一个缓冲区.
            diskPersistent - 在VM重启的时候是否启用磁盘保存EhCache中的数据，默认是
           false。
            diskExpiryThreadIntervalSeconds - 磁盘缓存的清理线程运行间隔，默认是120
           秒。每个120s，相应的线程会进行一次EhCache中数据的清理工作
            memoryStoreEvictionPolicy - 当内存缓存达到最大，有新的element加入的时
           候， 移除缓存中element的策略。默认是LRU（最近最少使用），可选的有LFU
           （最不常使用）和FIFO（先进先出）
           -->
   ```

3. 配置cache 标签

```xml
  <cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```

