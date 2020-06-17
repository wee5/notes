# Security：用户验证1：简单实现



基于角色，方法级别的权限控制

仅实现用户认证的相关配置；没有使用token（后续添加），无mvc

## 依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-test</artifactId>
    <scope>test</scope>
</dependency>
```



## 配置类

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private UserDetailsService userDetailsServiceImpl;

    //用户认证配置
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //指定用户认证时，默认从哪里获取认证用户信息
        auth.userDetailsService(userDetailsServiceImpl);
    }

    //Http安全配置
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        /* 使用默认登录页面，"/home"请求需要认证，其它请求允许通过 */
        http.formLogin().and().authorizeRequests().antMatchers("/home").authenticated().anyRequest().permitAll();
    }

    /**
     * 密码加密器
     */
    @Bean
    public PasswordEncoder passwordEncoder() {
        /**
         * BCryptPasswordEncoder：相同的密码明文每次生成的密文都不同，安全性更高
         */
        return new BCryptPasswordEncoder();
    }

}
```



## 其它类

```java
@Component
public class UserDetailsServiceImpl implements UserDetailsService {
    @Autowired
    private UserVoService userServiceImpl;
    @Autowired
    private PasswordEncoder passwordEncoder;
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        if(StringUtils.isEmpty(username)) {
            throw new UsernameNotFoundException("UserDetailsService没有接收到用户账号");
        } else {
            /**
             * 根据用户名查找用户信息
             */
            UserVo userVo = userServiceImpl.getUserByUsername(username);
            if(userVo == null) {
                throw new UsernameNotFoundException(String.format("用户'%s'不存在", username));
            }
            List<GrantedAuthority> grantedAuthorities = new ArrayList<>();
            for (String role : userVo.getRoles()) {
                //封装用户信息和角色信息到SecurityContextHolder全局缓存中
                grantedAuthorities.add(new SimpleGrantedAuthority(role));
            }
            /**
             * 创建一个用于认证的用户对象并返回，包括：用户名，密码，角色
             */
            return new User(userVo.getUsername(), passwordEncoder.encode(userVo.getPassword()), grantedAuthorities);
        }
    }
}
```