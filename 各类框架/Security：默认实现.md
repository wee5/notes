# Security：默认实现



采用security的默认实现，用户信息是直接创建的，并未使用redis或mybatis存储

使用了模板引擎thymeleaf，不过在默认实现中没有使用到，可以不添加依赖

默认实现并不适用于真实项目，仅用来展示security部分原理



## 依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-test</artifactId>
    <scope>test</scope>
</dependency>
```



## 配置文件

```yml
spring:
  thymeleaf:
    mode: HTML5
    encoding: utf-8
    cache: false
    servlet:
      content-type: text/html
  mvc:
    view:
      prefix: /
      suffix: .html
  resources:
    static-locations: classpath:/templates/
```



## 配置类

```java
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private UserDetailsService userDetailsServiceImpl;

    //自定义认证实现类
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //指定用户认证时，默认从哪里获取认证用户信息
        auth.userDetailsService(userDetailsServiceImpl);
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .formLogin()
                    /*.loginPage("/login.html")
                    .loginProcessingUrl("/login")*/
                    .defaultSuccessUrl("/home")
                    .permitAll()
                    .and()
                //controller和这里的权限设置会取最小权限
                .authorizeRequests()
                    .antMatchers("/home","/logout")
                    .authenticated()
                    .anyRequest()
                    .permitAll();
    }

    /* 自定义的密码加密方式 */
    @Bean
    public PasswordEncoder passwordEncoder() {
        /**
         * BCryptPasswordEncoder：相同的密码明文每次生成的密文都不同，安全性更高
         */
        return new BCryptPasswordEncoder();
    }

}
```



## 控制器

```java
@Controller
public class UserController {

    @RequestMapping("/home")
    @PreAuthorize("hasRole('role1')")//使用security默认的登录页面，不需要在权限前加上"ROLE_"
    public String method1(){
        System.out.println("home控制器");
        return "home";
    }
}
```



## 获取用户信息的实现类

```java
import org.springframework.security.core.userdetails.User;

@Component("userDetailsServiceImpl")
public class UserDetailsServiceImpl implements UserDetailsService {

    @Autowired
    private PasswordEncoder passwordEncoder;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        UserVo userVo = new UserVo();
        if(username.equals("weezy")){
            List<GrantedAuthority> roles = new ArrayList<>();
            roles.add(new SimpleGrantedAuthority("ROLE_role1"));
            roles.add(new SimpleGrantedAuthority("ROLE_role2"));
            return new User("weezy",passwordEncoder.encode("lilwayne"),roles);
        }else if(username.equals("fit")){
            List<GrantedAuthority> roles = new ArrayList<>();
            roles.add(new SimpleGrantedAuthority("ROLE_role1"));
            return new User("fit",passwordEncoder.encode("cent"),roles);
        }
        System.out.println("错误");
        return null;
    }
}
```



## 实体类（需要继承UserDetails）

```java
public class UserVo implements UserDetails {
    private String username;
    private String password;
    private List<String> roles;

    public UserVo(String username, String password, List<String> roles) {
        this.username = username;
        this.password = password;
        this.roles = roles;
    }

    public UserVo() {
    }

    public String getUsername() {
        return username;
    }

    @Override
    public boolean isAccountNonExpired() {
        return false;
    }

    @Override
    public boolean isAccountNonLocked() {
        return false;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return false;
    }

    @Override
    public boolean isEnabled() {
        return false;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {

        return null;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public List<String> getRoles() {
        return roles;
    }

    public void setRoles(List<String> roles) {
        this.roles = roles;
    }
}
```



## 需要注意

默认实现时，控制器处设置需要的权限时，不需要写“ROLE_"

配置加密器后，用户密码需要通过加密器转换