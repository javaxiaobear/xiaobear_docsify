# [MyBatis-Plus](https://mybatis.plus/guide/)

> **为简化开发而生**

Mybatis简化JDBC操作

[MyBatis-Plus](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个 [MyBatis](http://www.mybatis.org/mybatis-3/) 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

## 1、特性

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

## 2、支持数据库

- mysql 、 mariadb 、 oracle 、 db2 、 h2 、 hsql 、 sqlite 、 postgresql 、 sqlserver
- 达梦数据库 、 虚谷数据库 、 人大金仓数据库

## 3、快速开始

### 1、创建数据库

```scheme
DROP TABLE IF EXISTS user;

CREATE TABLE user
(
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY (id)
);
INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```

### 2、新建Spring Boot项目

- 导入依赖

	```xml
	<dependency>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-devtools</artifactId>
	    <scope>runtime</scope>
	    <optional>true</optional>
	</dependency>
	<dependency>
	    <groupId>mysql</groupId>
	    <artifactId>mysql-connector-java</artifactId>
	</dependency>
	<dependency>
	    <groupId>org.projectlombok</groupId>
	    <artifactId>lombok</artifactId>
	    <optional>true</optional>
	</dependency>
	<!--mybatis-plus-->
	<dependency>
	    <groupId>com.baomidou</groupId>
	    <artifactId>mybatis-plus-boot-starter</artifactId>
	    <version>3.3.1.tmp</version>
	</dependency>
	```

- 连接数据库

	```yaml
	spring:
	  datasource:
	    url: jdbc:mysql://localhost:3306/mybatisplus?useSSL=true&useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT
	    #useSSL安全连接 useUnicode编码 characterEncoding编码格式 serverTimezone时区
	    username: root
	    password: 100104
	    driver-class-name: com.mysql.cj.jdbc.Driver
	```

- 创建实体类User

	```java
	@Data
	@AllArgsConstructor
	@NoArgsConstructor
	public class User {
	    private Long id;
	    private String name;
	    private Integer age;
	    private String email;
	}
	
	```

- 实现接口UserMapper

	```java
	//@Mapper
	@Repository  //代表持久层
	public interface UserMapper extends BaseMapper<User> {
	    //所有的CRUD已经编写完成
	}
	```

- 在 Spring Boot 启动类中添加 `@MapperScan` 注解，扫描 Mapper 文件夹

	```java
	@SpringBootApplication
	@MapperScan("com.xiaobear.mapper")  //扫描文件夹
	public class MybatisplusApplication {
	
	    public static void main(String[] args) {
	        SpringApplication.run(MybatisplusApplication.class, args);
	    }
	}
	```

- 测试

	```java
	@SpringBootTest
	class MybatisplusApplicationTests {
	
	    @Autowired
	    private UserMapper userMapper;
	    @Test
	    void contextLoads() {
	        //查询所有用户
	        List<User> list = userMapper.selectList(null);
	        list.forEach(System.out::println);
	    }
	}
	
	```


## 4、配置日志

使用默认控制台输出日志

```yaml
#配置日志
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

## 5、CRUD扩展

#### Insert

```java
//    测试插入
    @Test
    public void testInsert(){
        User user = new User();
        user.setName("小熊");
        user.setAge(18);
        user.setEmail("2861184805@qq.com");
        int insert = userMapper.insert(user); //自动生成id
        System.out.println(insert);
        System.out.println(user);
    }
}
```

结果

![image-20200414130350839](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200414130350839.png)

> **数据库插入的id默认值为：全局唯一的id**

#### [主键生成策略](https://blog.csdn.net/u013068377/article/details/78993475)

##### 雪花算法：Twitter 利用 zookeeper 实现了一个全局ID生成的服务 Snowflake：[github.com/twitter/sno…](https://github.com/twitter/snowflake)：

snowflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0

![img](https://user-gold-cdn.xitu.io/2018/7/2/1645b1a7a9beb2b6?imageslim)

> 主键自增

我们配置主键自增

- 实体类字段上加上@TableId(type = IdType.AUTO) 

- 数据库字段上一定要自增

![image-20200414132153485](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200414132153485.png)

- 再次测试即可

```java
public enum IdType {
    AUTO(0),  //数据库id自增
    NONE(1),  //未设置主键
    INPUT(2), //手动输入
    ASSIGN_ID(3),  //默认全局id
    ASSIGN_UUID(4), //全局唯一id
   
    @Deprecated
    ID_WORKER(3),

    @Deprecated
    ID_WORKER_STR(3), //ID_WORKER字符串表示法

    @Deprecated
    UUID(4);
    private final int key;
    private IdType(int key) {
        this.key = key;}
    public int getKey() {
        return this.key;}
}
```

#### update

```java
 @Test
    public void testupdate(){
        //通过条件自动拼接动态sql
        User user = new User();
        user.setId(6L);
        user.setName("你是最棒的");
        int i = userMapper.updateById(user);  //updateById参数是一个对象
        System.out.println(i);
    }
}
```

![image-20200414133719235](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200414133719235.png)

#### 自动填充

> 数据库级别（工作中不使用）

1、在表中字段增加create_time、update_time

![image-20200414134437994](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200414134437994.png)

2、通过测试插入方法

```java
private Date createTime;
private Date updateTime;
```

3、查看结果

![image-20200414134824719](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200414134824719.png)



> 代码级别

1、实体类属性上增加注解

```java
@TableField(fill = FieldFill.INSERT)
private Date createTime;
@TableField(fill = FieldFill.INSERT_UPDATE)
private Date updateTime;
```

2、实现元对象处理器接口：com.baomidou.mybatisplus.core.handlers.MetaObjectHandler

自定义实现类 MyMetaObjectHandler

```java
@Component  //加入到IOC容器里
@Slf4j
public class MyMetaObjectHandler implements MetaObjectHandler {
    @Override
    public void insertFill(MetaObject metaObject) {
        log.info("start insert fill ....");
        this.setFieldValByName("createTime",new Date(),metaObject);
        this.setFieldValByName("updateTime",new Date(),metaObject);
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        log.info("start update fill ....");
        this.setFieldValByName("updateTime",new Date(),metaObject);
    }
}

```

3、测试插入，观察时间

#### 乐观锁

> 十分乐观，它总是认为不会出现问题，无论干什么都不上锁，如果出现了问题，再次更新值测试！

乐观锁实现方式：

- 取出记录时，获取当前version
- 更新时，带上这个version
- 执行更新时， set version = newVersion where version = oldVersion
- 如果version不对，就更新失败

1、给数据库新增字段verison，默认值为1

2、实体类新增字段

```java
@Version //乐观锁注解
private Integer version;
```

3、注册插件

```java
@Configuration
@MapperScan("com.xiaobear.mapper")
public class MybatisPlusConfig {
    //注册乐观锁插件
    @Bean
    public OptimisticLockerInterceptor optimisticLockerInterceptor() {
        return new OptimisticLockerInterceptor();
    }
}
```

4、测试

```java
@Test
public void testOptimisticLocker(){
    //查询用户信息
    User user = userMapper.selectById(1L);
    //修改信息
    user.setName("xiaobear");
    user.setAge(2);
    //执行
    int update = userMapper.updateById(user);
    System.out.println(update);

}
//测试失败
@Test
public void testOptimisticLocker2(){
    User user = userMapper.selectById(1L);
    user.setName("xiaobear");
    user.setAge(2);
    User user2 = userMapper.selectById(1L);
    user2.setName("xiaobear111");
    user2.setAge(10);
    //模拟另一个线程进行插队操作
    int update = userMapper.updateById(user);
    int update2 = userMapper.updateById(user2);
    System.out.println(update);
    System.out.println(update2);
}
```

#### 悲观锁

> 十分悲观，它总是认为会出现问题，无论干什么都会上锁，再去操作！

#### 查询操作

```java
 //单个查询
    @Test
    public void testSelect(){
        User user = userMapper.selectById(2L);
        System.out.println(user);

    }

    //批量查询
    @Test
    public void testSelectByBatchId(){
        List<User> users = userMapper.selectBatchIds(Arrays.asList(1, 2, 3));
        users.forEach(System.out::println);
    }

    //条件查询map
    @Test
    public void testSelectByBatchIds(){
        HashMap<String, Object> map = new HashMap<>();
        map.put("name","xiaobear");
        List<User> users = userMapper.selectByMap(map);
        users.forEach(System.out::println);

    }
```

#### 分页查询

> 分页插件

```java
  //分页插件
    @Bean
    public PaginationInterceptor paginationInterceptor() {
          return new PaginationInterceptor();
       /* PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
        // 设置请求的页面大于最大页后操作， true调回到首页，false 继续请求  默认false
        // paginationInterceptor.setOverflow(false);
        // 设置最大单页限制数量，默认 500 条，-1 不受限制
        // paginationInterceptor.setLimit(500);
        // 开启 count 的 join 优化,只针对部分 left join
        paginationInterceptor.setCountSqlParser(new JsqlParserCountOptimize(true));
        return paginationInterceptor;*/
    }
```

测试

```java
//测试分页插件
@Test
public void testPage(){
    //参数一：当前页 参数二：页面大小
    Page<User> page = new Page<>(1,5);
    userMapper.selectPage(page,null);
    page.getRecords().forEach(System.out::println);
    System.out.println(page.getTotal());
}
```

#### 删除操作

```java
//测试删除
@Test
public void deleteById(){
    int i = userMapper.deleteById(1L);
    System.out.println(i);
}
//测试id批量删除
@Test
public void deleteById2(){
    userMapper.deleteBatchIds(Arrays.asList(1L,2L,3L));
}

//通过map删除
@Test
public void testDeleteMap(){
    HashMap<String, Object> map = new HashMap<>();
    map.put("name","小熊");
    userMapper.deleteByMap(map);
}
```

#### 逻辑删除

> 物理删除：从数据库中直接删除
>
> 逻辑删除：在数据库没有移除，而是通过一个变量来让他失效！防止数据丢失，类似回收站

数据库添加字段，实体类上加上字段

```java
@TableLogic
private Integer deleted;
```

测试删除、查询

## 6、性能分析插件

> 性能分析拦截器，用于输出每条 SQL 语句及其执行时间

  ```java
/**
     * SQL执行效率插件
     */
    @Bean
    @Profile({"dev","test"})// 设置 dev test 环境开启
    public PerformanceInterceptor performanceInterceptor() {
        return new PerformanceInterceptor();
    }
  ```

## 7、条件构造器`wrapper`

```java
    @Autowired
    private UserMapper userMapper;
    @Test
    void contextLoads() {
        QueryWrapper<User> wrapper = new QueryWrapper<> ();
        wrapper
                .isNotNull("name")
                .isNotNull("email")
                .ge("age",3);
        userMapper.selectList(wrapper).forEach(System.out::println);
    }
    @Test
    void test(){
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.eq("name","Tom"); //查询名字
        User user = userMapper.selectOne(wrapper); //查询一个数据
        System.out.println(user);
    }
    @Test
    void test2(){
        //查询年龄10-20
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.between("age",10,20);
        Integer count = userMapper.selectCount(wrapper);
        System.out.println(count);
    }
    @Test
    void test3(){
        //模糊查询
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper
                .notLike("name","e")
                .likeRight("email","t");
        List<Map<String, Object>> maps = userMapper.selectMaps(wrapper);
        maps.forEach(System.out::println);
    }

    @Test
    void test4(){
        //
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.inSql("id","select id form user where id<3");
        List<Object> objects = userMapper.selectObjs(wrapper);
        objects.forEach(System.out::println);
    }
```

## 8、代码生成器

```java
public class XiaoBearCode {

    public static void main(String[] args) {
        AutoGenerator mpg = new AutoGenerator();
        // 全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        gc.setOutputDir(projectPath + "/src/main/java");
        gc.setAuthor("xiaobear");
        gc.setOpen(false);
        // gc.setSwagger2(true); 实体属性 Swagger2 注解
        mpg.setGlobalConfig(gc);

        // 数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/mybatisplus?useSSL=true&useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT");
        // dsc.setSchemaName("public");
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("100104");
        mpg.setDataSource(dsc);

        // 包配置
        PackageConfig pc = new PackageConfig();
        pc.setModuleName("blog");
        pc.setParent("com.xiaobear");
        pc.setEntity("pojo");
        pc.setMapper("mapper");
        pc.setService("com/xiaobear/service");
        pc.setServiceImpl("com.xiaobear/Service/Impl");
        pc.setController("com/xiaobear/controller");
        mpg.setPackageInfo(pc);

        // 策略配置
        StrategyConfig strategy = new StrategyConfig();
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        strategy.setEntityLombokModel(true);  //自动lombok
        strategy.setRestControllerStyle(true);
        // 公共父类
        strategy.setSuperControllerClass("你自己的父类控制器,没有就不用设置!");
        // 写于父类中的公共字段
        strategy.setSuperEntityColumns("id");
        strategy.setInclude("user");//映射的表
        strategy.setLogicDeleteFieldName("deleted");  //逻辑删除字段
        //自动填充策略
        TableFill gmt_create = new TableFill("gmt_create", FieldFill.INSERT);
        TableFill gmt_modified = new TableFill("gmt_strate", FieldFill.INSERT);
        ArrayList<TableFill> list = new ArrayList<>();
        list.add(gmt_create);
        list.add(gmt_modified);
        strategy.setTableFillList(list);
        //乐观锁
        strategy.setVersionFieldName("version");
        strategy.setRestControllerStyle(true);  //驼峰
        strategy.setControllerMappingHyphenStyle(true);  //localhost:8080/hello_id_1
        strategy.setTablePrefix(pc.getModuleName() + "_");
        mpg.setStrategy(strategy);
        mpg.setTemplateEngine(new FreemarkerTemplateEngine());

        mpg.execute(); //执行
    }

}
```

错误：

```java
19:39:38.943 [main] DEBUG com.baomidou.mybatisplus.generator.AutoGenerator - ==========================准备生成文件...==========================
Exception in thread "main" java.lang.NoClassDefFoundError: freemarker/template/Configuration
	at com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine.init(FreemarkerTemplateEngine.java:41)
	at com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine.init(FreemarkerTemplateEngine.java:34)
	at com.baomidou.mybatisplus.generator.AutoGenerator.execute(AutoGenerator.java:103)
	at com.xiaobear.XiaoBearCode.main(XiaoBearCode.java:66)
Caused by: java.lang.ClassNotFoundException: freemarker.template.Configuration
	at java.net.URLClassLoader.findClass(URLClassLoader.java:382)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:418)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:355)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:351)
	... 4 more
```

解决：

> 导入模板引擎依赖
>
> ```xml
> <dependency>
>     <groupId>org.freemarker</groupId>
>     <artifactId>freemarker</artifactId>
>     <version>2.3.30</version>
> </dependency>
> ```

