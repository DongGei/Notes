# mybatis的逆向工程

本质是一个代码生成器

![image-20220502152137328](https://s2.loli.net/2022/05/02/vEDGc1C3IMFisS5.png)

### 一.使用方法过程

#### 1.导入依赖

```xml
<dependencies>
	<!-- MyBatis核心依赖包 -->
	<dependency>
		<groupId>org.mybatis</groupId>
		<artifactId>mybatis</artifactId>
		<version>3.5.9</version>
	</dependency>
	<!-- junit测试 -->
	<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.13.2</version>
		<scope>test</scope>
	</dependency>
	<!-- MySQL驱动 -->
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>8.0.27</version>
	</dependency>
	<!-- log4j日志 -->
	<dependency>
		<groupId>log4j</groupId>
		<artifactId>log4j</artifactId>
		<version>1.2.17</version>
	</dependency>
</dependencies>
<!-- 控制Maven在构建过程中相关配置 -->
<build>
	<!-- 构建过程中用到的插件 -->
	<plugins>
		<!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
		<plugin>
			<groupId>org.mybatis.generator</groupId>
			<artifactId>mybatis-generator-maven-plugin</artifactId>
			<version>1.3.0</version>
			<!-- 插件的依赖 -->
			<dependencies>
				<!-- 逆向工程的核心依赖 -->
				<dependency>
					<groupId>org.mybatis.generator</groupId>
					<artifactId>mybatis-generator-core</artifactId>
					<version>1.3.2</version>
				</dependency>
				<!-- 数据库连接池 -->
				<dependency>
					<groupId>com.mchange</groupId>
					<artifactId>c3p0</artifactId>
					<version>0.9.2</version>
				</dependency>
				<!-- MySQL驱动 -->
				<dependency>
					<groupId>mysql</groupId>
					<artifactId>mysql-connector-java</artifactId>
					<version>8.0.27</version>
				</dependency>
			</dependencies>
		</plugin>
	</plugins>
</build>
```

#### 2.创建MyBatis的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="jdbc.properties"/>
    <typeAliases>
        <package name=""/>
    </typeAliases>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <package name=""/>
    </mappers>
</configuration>
```
#### 3.创建逆向工程的配置文件

- 文件名必须是：`generatorConfig.xml`

**如果发现生成的代码有误 在这个配置数据库后加**```&amp;nullCatalogMeansCurrent=true"```

亲测有效

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
  <!--
    targetRuntime: 执行生成的逆向工程的版本
    MyBatis3Simple: 生成基本的CRUD（清新简洁版）
    MyBatis3: 生成带条件的CRUD（奢华尊享版）
    -->
<generatorConfiguration>
    <context id="DB2Tables" targetRuntime="MyBatis3">
        <!-- 数据库的连接信息 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf8&amp;nullCatalogMeansCurrent=true"
                        userId="root"
                        password="123456">
        </jdbcConnection>
        <!-- javaBean的生成策略-->
        <javaModelGenerator targetPackage="com.dong.mybatis.pojo" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" /> <!--使用子包-->
            <property name="trimStrings" value="true" /> <!--字段名前后去空格-->
        </javaModelGenerator>
        <!-- SQL映射文件的生成策略 -->
        <sqlMapGenerator targetPackage="com.dong.mybatis.mapper"
                         targetProject=".\src\main\resources">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>
        <!-- Mapper接口的生成策略 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.dong.mybatis.mapper" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>
        <!-- 逆向分析的表 -->
        <!-- tableName设置为*号，可以对应所有表，此时不写domainObjectName -->
        <!-- domainObjectName属性指定生成出来的实体类的类名 -->
        <table tableName="t_emp" domainObjectName="Emp"/>
        <table tableName="t_dept" domainObjectName="Dept"/>
    </context>
</generatorConfiguration>
```

#### 4.执行MBG插件的generate目标

![image-20220504001200130](https://s2.loli.net/2022/05/04/qbJQC1eI9ZAy8Y6.png)

### 二.自动生成接口方法说明：

------

1.根据条件查询

XXXExample  根据条件  任意字段的任意需求
QBC风格Query By Criteria

- `selectByExample`：按条件查询，需要传入一个example对象或者null；如果传入一个null，则表示没有条件，也就是查询所有数据
- `example.createCriteria().xxx`：创建条件对象，通过andXXX方法为SQL添加查询添加，每个条件之间是and关系
- `example.or().xxx`：将之前添加的条件通过or拼接其他条件

------

2. 选择性修改 

int insert(Dept record);如果属性值是null就插入null
int insertSelective(Dept record); 选择性添加,只插入不是null的属性
对于添加好像有点鸡肋 两个还是一样 但是对于修改就有区别了
updateByExampleSelective 选择性修改
updateByExample  普通修改

> Example 理解成根据模板干嘛干嘛

> Selective 理解成选择性干嘛干嘛

> Example Selective根据模板选择性干嘛干嘛

### 三.测试代码

```java
/**
 * @className: MBGTest
 * @description: TODO 类描述
 * @author: Dong
 * @date: 2022/5/2
 **/
public class MBGTest {
    @Test
    public void testMBG() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
        SqlSession sqlSession = new SqlSessionFactoryBuilder().build(resourceAsStream).openSession(true);
        EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
/*
        //查询所有
        List<Emp> emps = mapper.selectByExample(null);//根据条件查询，不传条件就是查所有
        emps.forEach(emp-> System.out.println(emp));
*/
/*
      //根据条件查询
        EmpExample empExample = new EmpExample();
        //链式添加条件
        empExample.createCriteria().andEmpNameEqualTo("张三").andAgeGreaterThanOrEqualTo(20);
        empExample.or().andDidIsNotNull();
        //Preparing: select eid, emp_name, age, sex, email, did
        // from t_emp WHERE ( emp_name = ? and age >= ? ) or( did is not null )
        List<Emp> emps = mapper.selectByExample(empExample);
        emps.forEach(emp-> System.out.println(emp));

 */
        //有无Selective的区别
        //属性是null 也会修改成null
        //添加 insert 如果有默认值也会改成null
        mapper.updateByPrimaryKey(new Emp(1,"admin",22,null,"626@qq.com",3));
        //null的属性 不会修改 选择性修改
        //添加也是 如果有默认值就是默认值
        mapper.updateByPrimaryKeySelective(new Emp(1,"admin",22,null,"626@qq.com",3));



    }
}
```

