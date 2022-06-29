# 在SpringSecurity中使用数据库代理原本的内存查询

## 1.实现原理

​	SpringSecurity的原理其实就是一个过滤器链，内部包含了提供各种功能的过滤器。这里是入门案例中的过滤器。

![image-20220515224217283](https://s2.loli.net/2022/05/15/usvwUtMSZfHhOkE.png)

**UsernamePasswordAuthenticationFilter**:负责处理我们在登陆页面填写了用户名密码后的登陆请求。入门案例的认证工作主要有它负责。

![image-20220515224323120](https://s2.loli.net/2022/05/15/uxXj1ZClWrGTbzn.png)

然后这个过滤器会调用这些方法 这些方法默认写好了  我们要做的就是自己写一个UserDetailsService的实现类 来代替它默认的
InMemeryUserDetilsManager。因为我们想到去数据库中查找数据。

> 我们怎么样可以覆盖这个类呢？因为当前是springboot项目 我猜想autoConfig类应该是写了 如果我们自定义了UserDetailsService的实现类，springboot 就不会容器注入默认的InMemeryUserDetilsManager

接下来我们查看springbooot源码查看（扩展）

1. 因为它sping自己家的东西 我们在这里查看源码![image-20220515225355854](https://s2.loli.net/2022/05/15/NUD35w7tO9JLbSV.png)

![image-20220515225411849](https://s2.loli.net/2022/05/15/9L1TYWosjflwJPF.png)

2.

判断spring容器中不存在AuthenticationManager、AuthenticationProvider、UserDetailsService这三个类的实例，默认我们没有自定义这三个类，**满足条件**综上，满足所有条件，将UserDetailsServiceAutoConfiguration实例注册到spring容器中最终将InMemoryUserDetailsManager注册到spring容器中

我们要自己写UserDetailsService类的实例  所以这个自动配置实效了（符合我的猜想）

![image-20220515230251037](https://s2.loli.net/2022/05/15/mOHhWuC5EDvPoiz.png)

## 2.怎么使用

demo结构 (许多东西在这个当前demo都没用到)

![image-20220515224921893](https://s2.loli.net/2022/05/15/8dVGDX4ZavltIi2.png)

### （1）环境

springboot mybatisPuls

```xml
<properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
</properties>
<parent>
    <artifactId>spring-boot-starter-parent</artifactId>
    <groupId>org.springframework.boot</groupId>
    <version>2.6.7</version>
</parent>
<dependencies>
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.5.1</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>2.6.7</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```

### （2）测试代码

写一个UserDetailsServiceImpl代替原本的UserDetailsService

```java
@Service
public class UserDetailsServiceImpl implements UserDetailsService {

    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // 查询用户信息
        LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(User::getUserName,username);
        User user = userMapper.selectOne(queryWrapper);
        //没有查询到用户
        if(Objects.isNull(user)){
            throw new RuntimeException("用户名或者密码错误");
            //这里的  “用户名或者密码错误” 会错误后回显在默认页面 搭建完可以自己试一下
             // 对这个异常的处理 security有默认的过滤器处理 这个以后再说 
        }

        // TODO 查询对应权限信息

        //查到了 返回
        return new LoginUser(user);
    }
}
```

里面用到的mapper 以及实体类（许多属性没用到）

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@TableName(value = "sys_user")
public class User implements Serializable {
    private static final long serialVersionUID = -40356785423868312L;
    
    /**
    * 主键
    */
    @TableId
    private Long id;
    /**
    * 用户名
    */
    private String userName;
    /**
    * 昵称
    */
    private String nickName;
    /**
    * 密码
    */
    private String password;
    /**
    * 账号状态（0正常 1停用）
    */
    private String status;
    /**
    * 邮箱
    */
    private String email;
    /**
    * 手机号
    */
    private String phonenumber;
    /**
    * 用户性别（0男，1女，2未知）
    */
    private String sex;
    /**
    * 头像
    */
    private String avatar;
    /**
    * 用户类型（0管理员，1普通用户）
    */
    private String userType;
    /**
    * 创建人的用户id
    */
    private Long createBy;
    /**
    * 创建时间
    */
    private Date createTime;
    /**
    * 更新人
    */
    private Long updateBy;
    /**
    * 更新时间
    */
    private Date updateTime;
    /**
    * 删除标志（0代表未删除，1代表已删除）
    */
    private Integer delFlag;
}
```

> 记得加注解扫描
>
> ```
> @SpringBootApplication
> @MapperScan("com.dong.jwt.mapper")
> ```

```java
public interface UserMapper extends BaseMapper<User> {
}
```

因为UserDetailsServiceImpl 的接口UserDetailsService中的方法让我们返回一个UserDetails

然后UserDetails 是一个接口 所以我们就自己写一个类去实现一下它

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class LoginUser implements UserDetails {

    private User user;
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return null;
    }

    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getUserName();
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
```

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/dzzs_t?characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
```

```sql
CREATE TABLE `sys_user` (
  `id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT '主键',
  `user_name` VARCHAR(64) NOT NULL DEFAULT 'NULL' COMMENT '用户名',
  `nick_name` VARCHAR(64) NOT NULL DEFAULT 'NULL' COMMENT '昵称',
  `password` VARCHAR(64) NOT NULL DEFAULT 'NULL' COMMENT '密码',
  `status` CHAR(1) DEFAULT '0' COMMENT '账号状态（0正常 1停用）',
  `email` VARCHAR(64) DEFAULT NULL COMMENT '邮箱',
  `phonenumber` VARCHAR(32) DEFAULT NULL COMMENT '手机号',
  `sex` CHAR(1) DEFAULT NULL COMMENT '用户性别（0男，1女，2未知）',
  `avatar` VARCHAR(128) DEFAULT NULL COMMENT '头像',
  `user_type` CHAR(1) NOT NULL DEFAULT '1' COMMENT '用户类型（0管理员，1普通用户）',
  `create_by` BIGINT(20) DEFAULT NULL COMMENT '创建人的用户id',
  `create_time` DATETIME DEFAULT NULL COMMENT '创建时间',
  `update_by` BIGINT(20) DEFAULT NULL COMMENT '更新人',
  `update_time` DATETIME DEFAULT NULL COMMENT '更新时间',
  `del_flag` INT(11) DEFAULT '0' COMMENT '删除标志（0代表未删除，1代表已删除）',
  PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COMMENT='用户表'
```

![image-20220515223859074](https://s2.loli.net/2022/05/15/oywWhzsCDduT62E.png)

密码这个在目前测试时 记得在前面加一个{noop}  表示明文密码 因为默认情况是有加密的  这个以后再更新



测试的话 自己写一个hello的controller去访问即可 会先让你登录

# Spring Security

## 入门案例

### 测试环境：

### maven工程

```xml
<properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
</properties>
<parent>
    <artifactId>spring-boot-starter-parent</artifactId>
    <groupId>org.springframework.boot</groupId>
    <version>2.6.7</version>
</parent>
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>2.6.7</version>
    </dependency>
    <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```

![image-20220514220923794](https://s2.loli.net/2022/05/14/OikLCmWjsQqvV7J.png)

导入依赖 编写/h请求

发现：访问http://localhost:8080/h 会自动跳转到 http://localhost:8080/login 

默认用户名user 密码在控制台 输入后成功访问/h

## 一.认证

### 前后分离端项目中流程

非基于session认证

![image-20220514222146165](https://s2.loli.net/2022/05/14/CbKJit38zM52dPq.png)

> Json web token (JWT)

SpringSecurity的原理其实就是一个过滤器链，内部包含了提供各种功能的过滤器。

这里我们可以看看入门案例中的过滤器。

![image-20220514223451142](https://s2.loli.net/2022/05/14/zfF6SiPbErThLIn.png)

认证-->异常处理-->授权

