# Mybatis：*mapper.xml

| parameterType     |                    | resultType        |              |
| ----------------- | ------------------ | ----------------- | ------------ |
| String Integer .. | 单个参数           | String Integer .. | 单行单个字段 |
| 略                | 多个参数（未封装） | entity            | 单行多个字段 |
| Map               | 多个参数（封装）   | List Map Array    | 多行多个字段 |



**动态sql标签**

```xml
<if>		sql语句拼接，依次判选（语意处理）
		select * from user where 1=1	<if test="userName!=null">and userName=#{userName}</if>

<when>		像java中的switch语句，多中选一（语意处理）
		<choose>
			<when test="____">	*******	</when>
			<when test="____">	******	</when>
			<otherwise>	********	<otherwise>
		<choose>

<trim>		对后整段sql语句做处理。（结构处理）
		prefix,suffix分别是整个<trim>标签包含的sql语句的前后缀，suffixOverride为去除多余的后缀
		<trim  prefix="("  suffix=")"  suffixOverride=","  prefixOverride="">
			<if  test="____">*******</if>
			<if  test="____">*******</if>
			<if  test="____">*******</if>
		</trim>

<foreach>		（结构处理）将集合中的元素写成（*，*，*，*）的形式，符合sql语句id in (1,3,4,7)形式
		select * from user where id in <foreach collection="list" item="li" open="("  separator=","  close=")"></foreach>

<sql>		封装sql语句，同时可包含标签	<sql  id="A"><if  test="id!=null">id=#{id}</sql>
<include>		调用<sql>标签封装的sql语句		<include   refid="A"  />
```



**标签映射和驼峰规则**

* 应用场景：数据库字段名和实体类属性名不同，查询结果无法包装成对象
* 标签映射和驼峰规则不兼容：标签映射一个表配置一次，驼峰规则为全局设置；使用标签映射意味着，要配置所有需要配置的标签映射；因为驼峰规则已不能再开启

**驼峰规则**

* 开启：

  ```xml
  Mybatis配置文件中添加：
  <!-- 是否开启自动驼峰命名规则（camel case）映射， -->
  <setting name="mapUnderscoreToCamelCase" value="true"/>
  
  在SqlSessionFactory Bean下添加：
  <!-- 开启驼峰式命名规则的映射 -->
  <property name="configLocation" value="classpath:spring-mybatis-config.xml"/>
  ```

* 作用：去下划线，大小写转化

  * Parent_id（数据库）——>parentid（实体类）

**标签映射**

* 应用场景：数据库字段命和实体类属性名不仅是下划线的区别；多表关联查询

* 开启：

  ```xml
  <!-- type指向你的javabean类，id可以自定义 -->
  <resultMap type="com.Category" id="category">
      <!-- property对应实体类的属性名称，column为数据库结果集的列的名称 -->
     <id property="id" column="id" />
     <result property="parentId" column="parent_id" jdbcType="INTEGER"/>
     <result property="name" column="name" jdbcType="VARCHAR"/>
     <result property="enName" column="en_name" jdbcType="VARCHAR"/>
  </resultMap>
  <select id="selectList"  resultMap="testResultMap">
          select * from test2
  </select>
  ```