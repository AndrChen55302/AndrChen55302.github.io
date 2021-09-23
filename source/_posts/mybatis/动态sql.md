---
title: 动态sql
date: 2021-09-06
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632307909000.webp
tags: 动态sql
categories: mybatis
---
## 前言

作为一名j ava 程序员 数据库的重要性毋庸置疑，sql作为操作数据库的重要手段，

那么mybatis 中的动态sql自然也是很重要的

### 什么是动态sql

MyBatis 核心对sql语句进行灵活操作，通过表达式进行判断，对sql进行灵 活拼接、组装。 动态 SQL 元素和 JSTL 或基于类似 XML 的文本处理器相似。在 MyBatis 之 前的版本中，有很多元素需要花时间了解。MyBatis 3 大大精简了元素种类，现 在只需学习原来一半的元素便可。MyBatis 采用功能强大的基于 OGNL 的表达 式来淘汰其它大部分元素。

### MyBatis中用于实现动态SQL的元素主要有：

1. if ：利用if实现简单的条件选择 
2. choose（when，otherwise）：相当于java中的switch语句，通常与when和 otherwise搭配 
3. trim：可以灵活地去除多余的关键字 
4. set：解决动态更新语句 
5. foreach：迭代一个集合，通常用于in条件 
6. where：简化SQL语句中where的条件判断

### 使用if+where实现多条件查询

```mybatis
		<select id="getUserList" resultType="User">
			select * from user
			<where>
				<if test="username!=null and userName!=''">
					and userName like CONCAT('%',#{userName},'%')
				</if>
			</where>
		</select>
```

> ​	上述代码就是一个最简单的if+where的SQL映射语句，where元素标签会自动识别其标 签内是否有返回值，若有，就插入一个where关键字，此外，若该标签返回的内容是以 and或者or开头的，where元素会将其自动剔除，if元素标签里主要的属性就是test属 性，test后面跟的是一个表达式，返回true或者false，以此来进行判断。

### 使用if+trim实现对条件查询

```mybatis
		<select id="getUserList" resultType="User">
			select * from user
			<trim prefix="where" prefixOverride="and|or">
				<if test="username!=null and userName!=''">
					and userName like CONCAT('%',#{userName},'%')
				</if>
			</trim>
		</select>
```

> ​	从上述代码中可以看出trim和where元素标签的用法差不多，就trim标签中多了 几个元素，那多了啥元素呢？

- prefix：前缀，作用是在trim包含的内容上加上前缀。
- prefix：前缀，作用是在trim包含的内容上加上前缀。
- prefixOverride：对于trim包含内容的首部进行指定内容（如此处 的“and|or“）的忽略。
- suffixOverride：对于trim包含内容的尾部进行指定内容的忽略。

### 使用if+set实现更新操作

```mybatis	
		<update id="modify" parameterType="AppInfo">
			update app_info
			<set>
				<if test="logoPicPath != null">logoPicPath=#{logoPicPath},</if>
				<if test="logoLocPath != null">logoLocPath=#{logoLocPath},</if>
				<if test="modifyBy != null">modifyBy=#{modifyBy},</if>
				<if test="modifyDate != null">modifyDate=#{modifyDate},</if>
			</set>
			where id=#{id}
		</update>
```

> ​	上述代码就是一个最简单的if+set的动态SQL，从上面的代码中能看出其所做的更新操作是动态 的，意思就是说你这个值为不为空，不为空就给你更新，要是为空就不管它，emmmm，看样子 它的设计还是很人性化的。

### 使用if+trim实现更新操作

```mybatis
		<update id="modify" parameterType="AppInfo">
			update app_info
			<trim prefix="set" suffixOverride="," suffix="where id=#{id}">
				<if test="logoPicPath != null">logoPicPath=#{logoPicPath},</if>
				<if test="logoLocPath != null">logoLocPath=#{logoLocPath},</if>
				<if test="modifyBy != null">modifyBy=#{modifyBy},</if>
				<if test="modifyDate != null">modifyDate=#{modifyDate},</if>
			</trim>
		</update>
```

### choose, when, otherwise（类似于Switch）

> ​	choose, when, otherwise（类似于Switch）

```mybatis
		<select id="getProInfoList" resultType="Provider" parameterType="Provider">
			SELECT * FROM smbms_provider
			<where>
				<choose>
					<when test="proCode!=null">
						proCode LIKE concat('%',#{proCode},'%')
					</when>

					<when test="proName!=null">
						AND proName LIKE concat('%',#{proName},'%')
					</when>
					<otherwise>
						id=1;
					</otherwise>
				</choose>
			</where>
		</select>
```

测试代码

```test
		@Test
		void testgetProInfoList() {
			sqlSession=MybatisUtil.creatSqlSession();
			proList = sqlSession.getMapper(ProviderMapper.class).getProInfoList(null, null);
			for (Provider provider : proList) {
				logger.debug(provider.toString());;
			}	
		}
```

代码运行结果：

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1631025701000.png)

**foreach**

​	foreach主要用于在构建in条件中，它可以在SQL语句中迭代一个集合。 

**属性**：

​	item：表示集合中每一个元素进行迭代的别名(如此处的roleIds)。 

​	index：指定一个名称，用于表示在迭代过程中，每次迭代到的位置。 

​	open：表示该语句以什么开始（既然是in条件语句，所以必然是以 " ( " 开 始） 

​	separator：表示在每次进行迭代之间以什么符号作为分隔符（既然是in条 件语句，所以必然以 " , " 作为分隔符）。 

​	close：表示该语句以什么结束（既然是in条件语句，所以必然是以 " ) " 结 束）。 

​	collection：最关键并最容易出错的属性，需格外注意，该属性必须指定， 不同情况下，该属性的值是不一样的。主要有三种情况 

- 若入参为单参数且参数类型是一个list的时候，collection属性值 为list。 
- 若入参为单参数且参数类型是一个数组的时候，collection属性值 为array（此处传入参数Integer[] roleIds为数组类型，故此处 collection属性值设为“array”）。 
- 若传入参数为多参数，就需要把它们封装为一个Map进行处理。

```mybatis
 //根据角色编号，查询所有的用户‐数组方式
 public List<User> getUserInfoByRole(Integer[] roleIds);
 //根据角色编号，查询所有的用户‐list方式
 public List<User> getUserInfoByRole2(List<Integer> roleIds);

 <!‐‐ 根据角色编号，查询所有的用户信息 ‐‐>
 <select id="getUserInfoByRole" resultType="user">
 SELECT * FROM smbms_user WHERE userRole IN
 <!‐‐ 数组方式 ‐‐>
 <foreach collection="array" item="roleId" open="("
	eparator="," close=")">
	 #{roleId}
 </foreach>
 <!‐‐ list方式 ‐‐>
 <foreach collection="list" item="roleId" open="("
parator="," close=")">
 #{roleId}
 </foreach>
 </select>

 @Test
 void testgetUserInfoByRole() {
 String resource="mybatis‐config.xml";
 SqlSession sqlSession = null;
 List<User> userList=null;
 Integer [] roleIds= {2,3};
 try {
 sqlSession=MybatisUtil.creatSqlSession();
 userList = sqlSession.getMapper(UserMapper.class).getUserInfoB
yRole(roleIds);
 } catch (Exception e) {
 e.printStackTrace();
 } finally {
 MybatisUtil.closeSqlSession(sqlSession);
 logger.debug("userList的行数：" + userList.size());
 for (User user : userList) {
 logger.debug(user.toString());
 }
 }
 }
 @Test
 void testgetUserInfoByRole2() {
 String resource="mybatis‐config.xml";
 SqlSession sqlSession = null;
 List<User> userList=null;
 List<Integer> roleIds=new ArrayList<Integer>();
 try {
 sqlSession=MybatisUtil.creatSqlSession()
 @Test
 void testgetUserInfoByRole2() {
 String resource="mybatis‐config.xml";
 SqlSession sqlSession = null;
 List<User> userList=null;
 List<Integer> roleIds=new ArrayList<Integer>();
 try {
 sqlSession=MybatisUtil.creatSqlSession() 
```

代码运行效果：

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1631026105000.png)

map方式

```mybatis
 //根据角色编号或姓名，查询用户信息
 public List<User> getUserInfoByRole3(Map<String, Object>
params);

 <select id="getUserInfoByRole3" resultType="user">
 SELECT * FROM smbms_user WHERE userName like concat('%',#
{name},'%') and userRole IN
 <!‐‐ map ‐‐>
 <foreach collection="rolIdsKey" item="roleId" open="(" separato
r="," close=")">
 #{roleId}
 </foreach>
 </select>
 @Test
 void testgetUserInfoByRole3() {
 String resource="mybatis‐config.xml";
 SqlSession sqlSession = null;
 List<User> userList=null;
 List<Integer> roleIds=new ArrayList<Integer>();
 roleIds.add(2);
 roleIds.add(3);
 Map<String, Object> params = new HashMap<String, Object>();
 params.put("rolIdsKey", roleIds);
 params.put("name", "张");

 try {
 sqlSession=MybatisUtil.creatSqlSession();
 userList = sqlSession.getMapper(UserMapper.class).getUserInfoB
yRole3(params);
 } catch (Exception e) {
 e.printStackTrace();
 } finally {
 MybatisUtil.closeSqlSession(sqlSession);
 logger.debug("userList的行数：" + userList.size());
 for (User user : userList) {
 logger.debug(user.toString());
 }
 }
 }
```

代码运行效果：

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1631026290000.png)

### SQL片段

​	**作用**：将实现的动态sql判断代码块抽取出来，组成一个sql片段。其它的 statement（声明）中就可以引用sql片段。方便程序员进行开发 

​	先在mapper.xml中定义一个sql片段：

```mybatis
 <!‐‐ 根据供应商编码或供应商名称查询供应商信息 if+where ‐‐>
 <!‐‐ 定义sql片段
 id：sql片段的唯 一标识
 经验：是基于单表来定义sql片段，这样话这个sql片段可重用性才高
 在sql片段中不要包括 where
 ‐‐>
 <sql id="query_getProInfoWhere">
 <if test="proCode!=null">
 proCode LIKE concat('%',#{proCode},'%')
 </if>
 <if test="proName!=null">
 AND proName LIKE concat('%',#{proName},'%')
 </if>
 </sql>

 <select id="getProInfoList" resultType="Provider"
parameterType="Provider">
 SELECT * FROM smbms_provider
 <where>
 <!‐‐ 引用sql片段 的id，如果refid指定的id不在本mapper文件中，需要前
加namespace ‐‐>
 <include refid="query_getProInfoWhere"></include>
 </where>
 </select>
```

​	代码运行结果：

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1631026299000.png)



