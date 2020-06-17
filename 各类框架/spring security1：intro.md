# spring security1：intro



spring security是基于spring aop和servlet过滤器的安全框架，它提供全面的安全性解决方案，同时在web请求级和方法调用级处理身份确认和授权

原理技术：filter，servlet，spring di，spring aop

## 核心功能

认证（你是谁，用户/设备/系统）

验证（你能干什么，权限控制/授权，允许执行的操作）

攻击防护（反之伪造身份）





## 基于内存的认证信息和角色授权

### 依赖

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### 配置文件

```yml
security:
	basic:
		enabled: false #关闭验证功能，5.x版本后提示过时；推荐使用启动类注解，效果相同
spring:
	security:
		user:
			name: admin #不配置的话，默认随机生成用户名和密码，密码输出在控制台
			password: 123456 #自定义用户名和密码
```

### 配置类

```java
@Configuration
@EnableWebSecurity//启用spring security
@EnableGlobalMethodSecurity(prePostEnabled = true)//会拦截注解了@PreAuthrize的配置
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //配置从内存中进行加载认证信息，这里配置两个用户user和admin
        auth.inMemoryAuthentication().withUser("user").password(passwordEncoder().encode("123456")).roles();
        auth.inMemoryAuthentication().withUser("admin").password(passwordEncoder().encode("123456")).roles();
    }

    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }
}
```

### 启动类

```java
@SpringBootApplication(exclude = SecurityAutoConfiguration.class)
public class AnimalAidSystemApplication {
```

### 使用

```java
//拦截该方法，指定角色的用户可以进入该方法
@PreAuthorize("hasAnyRole('normal','admin')")
@PostMapping("/")
public @ResponseBody AjaxResponse updateMedicines(@RequestBody MedicinesVo medicinesVo){
```





## 基于内存数据库的身份认证和角色授权

## 依赖

```xml
<!-- spring data jpa用于持久化操作 -->
<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<!-- 内存数据库 -->
<dependency>
	<groupId>org.hsqldb</groupId>
    <artifactId>hsqldb</artifactId>
    <scope>runtime</scope>
</dependency>
```

