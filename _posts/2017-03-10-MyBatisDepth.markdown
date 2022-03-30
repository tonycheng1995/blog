---
layout: post
title: "MyBatis深入"
date:   2017-03-10 21:23:58 +0800
toc: true
category: MyBatis
tags: MyBatis
excerpt: 看了“MyBatis初识”之后我们对MyBatis有了初步的认识，接下来让我们深入学习MyBatis的使用。
---

属性|描述
:--|:--
id|命名空间中唯一的标识符，可以被用来引用这条语句
parameterType|传入语句的参数类的完全限定名或别名
statementType|可选值:STATEMENT、PREPARED或CALLABLE
useGeneratedKeys|是否使用生成的键
resultType|返回类型的完全限定名或别名
resultMap|命名引用外部的resultMap
flushCache|默认值为true,缓存清空
useCache|默认值为true，导致本条语句结果被缓存


## 增加(Create)
#### 配置map文件
{% highlight xml %}
<insert id="inserUser" parameterType="User" statementType="PREPARED"
keyProperty="id" useGeneratedKeys="true">
	insert into User (userName,password) values (#{userName},#{password})
</insert>
{% endhighlight xml %}

#### 测试类
{% highlight java %}
User u=new User();
u.setUserName("lisi");
u.setPassword("123");
session.insert("inserUser", u);
session.commit();
{% endhighlight java %}

## 更新(Update)
#### 配置map文件
{% highlight xml %}
<update id="updateUser" parameterType="User">
	UPDATE User SET
	userName=#{userName},password=#{password}
	WHERE id=#{id}
</update>
{% endhighlight xml %}
#### 测试类
{% highlight java %}
User u=new User();
u.setUserName("wangwu");
u.setPassword("54321");
u.setId(2);
session.update("updateUser",u);
session.commit();
{% endhighlight java %}

## 使用注解进行删除(Delete)
#### 建立一个接口类InterfaceUser.java
{% highlight java %}
package cheng.book.map;

import org.apache.ibatis.annotations.Delete;

public interface InterfaceUser {

	@Delete("delete from User where id=#{id}")
	public void deleteUser(Integer id);
}
{% endhighlight java %}

#### 在基本配置文件MyBatisConfig.xml中配置
{% highlight xml %}
<mappers>
	<mapper resource="cheng/book/map/User.xml"/>
	<mapper class="cheng.book.map.InterfaceUser"/>
</mappers>
{% endhighlight xml %}

#### 测试类
{% highlight java %}
InterfaceUser iu=session.getMapper(InterfaceUser.class);
iu.deleteUser(1);
session.commit();
{% endhighlight java %}

## parameterType封装：HashMap
#### 配置map文件
{% highlight xml %}
<select id="loginSelect" resultType="User" parameterType="hashmap">
	select * from User where userName=#{userName} and password=#{password}
</select>
{% endhighlight xml %}
#### 测试类
{% highlight java %}
HashMap<String, String> hm=new HashMap<>();
hm.put("userName", "zhangsan");
hm.put("password", "12345");
User u=session.selectOne("loginSelect",hm);
if(u!=null){
System.out.println("登录成功！");
}
{% endhighlight java %}
## parameterType封装：对象
#### 配置map文件
{% highlight xml %}
<select id="login2" resultType="User" parameterType="User">
	select * from User where userName=#{userName} and password=#{password}
</select>
{% endhighlight xml %}
#### 测试类
{% highlight java %}
User u=new User();
u.setUserName("zhangsan");
u.setPassword("12345");
User user=session.selectOne("login2",u);
if(user!=null){
	System.out.println("登录成功！");
}
{% endhighlight java %}

## 返回多行记录时MyBatis自动封装成List
#### map配置文件
{% highlight xml %}
<select id="selectUserList" resultType="User">
		select * from User
</select>
{% endhighlight xml %}
#### 测试类
{% highlight java %}
List<User> list=session.selectList("selectUserList");
for(User u:list){
	System.out.println(u.getUserName());
}    
{% endhighlight java %}

## resultMap解决复杂查询时的映射问题(如一个表含有另一个表的ID索引)
#### 配置map文件
{% highlight xml %}
<resultMap type="User" id="U">
	<id property="id" column="id"/>
	<result property="userName" column="userName"/>
	<result property="password" column="password"/>
</resultMap>
<select id="selectUsers" resultMap="U">
	select id,userName,password from User
</select>
{% endhighlight xml %}
#### 测试类
{% highlight java %}
List<User> list=session.selectList("selectUsers");
for(User u:list){
	System.out.println(u.getUserName());
}
{% endhighlight java %}

## 事务处理
#### 两种控制方式：1.JDBC、2.MANAGED
#### 在基本配置文件中配置事务处理
{% highlight xml %}
<!-- 配置事务处理 -->
<transactionManager type="JDBC">
{% endhighlight xml %}
#### MyBatis JDBC事务管理：1.添加作者前要添加用户、2.自动id返回
#### 1.新建作者表
#### 2.新建作者类Author.java
{% highlight java %}
package cheng.book.pojo;

public class Author {

	private Integer id;
	private User user;
	private String realName;
	private String IDCard;
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public User getUser() {
		return user;
	}
	public void setUser(User user) {
		this.user = user;
	}
	public String getRealName() {
		return realName;
	}
	public void setRealName(String realName) {
		this.realName = realName;
	}
	public String getIDCard() {
		return IDCard;
	}
	public void setIDCard(String iDCard) {
		IDCard = iDCard;
	}

}
{% endhighlight java %}

#### 3.配置事务处理
#### 4.新建author.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" ?>
<mapper namespace="/">

	<insert id="inserAuthor" parameterType="Author" statementType="PREPARED"
	keyProperty="id" useGeneratedKeys="true">
		insert into Author (userID,realName,IDCard) values (#{User.id},#{realName},#{IDCard})
	</insert>

</mapper>
{% endhighlight xml %}
#### 5.测试类
{% highlight java %}
try {
	User u=new User();
	u.setUserName("zhangsan");
	u.setPassword("1234");
	session.insert("inserUser",u);
	System.out.println("id"+u.getId());

	Author a=new Author();
	a.setUser(u);
	a.setRealName("libai");
	session.insert("inserAuthor",a);
	session.commit();
} catch (Exception e) {
	e.printStackTrace();
	session.rollback(); //事务回滚
}finally {
	session.close();
}
{% endhighlight java %}

## 联合查询 resultMap

属性|描述
:--|:--
id|一个ID结果；标记结果为ID可以帮助提高整体效能
result|注入到字段或JavaBean属性的普通结果
association|复杂类型的关联
collection|复杂类型的集合
constructor|类在实例化时，用来注入结果到构造方法
discriminator|使用结果值来决定使用哪个结果映射

#### 查询所有作者的登录名
#### 配置author.xml
{% highlight xml %}
<resultMap type="Author" id="AuthorMap">
	<id property="id" column="author.id"/>
	<result property="realName" column="realName"/>
	<result property="IDCard" column="IDCard"/>
	<!-- 联合查询 -->
	<association property="user" column="userID" javaType="User">
	<id property="id" column="user.id"/>
	<result property="userName" column="userName"/>
	<result property="password" column="password"/>
	</association>
</resultMap>
<select id="selectAuthorJoin" resultMap="AuthorMap">
	select * from author inner join user on user.id=author.userID
</select>
{% endhighlight xml %}

#### 测试类
{% highlight java %}
List<Author> author=session.selectList("selectAuthorJoin");
for(Author a:author){
	System.out.println(a.getUser().getUserName()+a.getRealName());
}
{% endhighlight java %}
## 构造查询
#### 配置map文件Author.xml
{% highlight xml %}
<!-- 构造函数 -->
<resultMap type="Author" id="AuthorMapByCon">
	<id property="id" column="author.id"/>
	<result property="realName" column="realName"/>
	<result property="IDCard" column="IDCard"/>
	<association property="user" column="userID" javaType="User">
	<constructor>
		<arg column="userName" javaType="String"/>
		<arg column="password" javaType="String"/>
	</constructor>
	</association>
</resultMap>
<select id="selectAuthorJoin2" resultMap="AuthorMapByCon">
	select * from author inner join user on user.id=author.userID
</select>
{% endhighlight xml %}
#### 添加构造函数User.java
{% highlight java %}
package cheng.book.pojo;

public class User {

	private int id;
	private String userName;
	private String password;

	public User() {
		super();
	}
	public User(String userName, String password) {
		super();
		this.userName = userName;
		this.password = password;
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getUserName() {
		return userName;
	}
	public void setUserName(String userName) {
		this.userName = userName;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
}
{% endhighlight java %}
#### 测试类
{% highlight java %}
List<Author> author=session.selectList("selectAuthorJoin2");
for(Author a:author){
	System.out.println(a.getUser().getUserName()+a.getRealName());
}
{% endhighlight java %}

## 子查询
#### 在user.xml建立子查询
{% highlight xml %}
<select id="findById"  parameterType="int"  resultType="cheng.book.pojo.User">
	select * from User where id=#{id}
</select>
{% endhighlight xml %}

#### 配置map文件author.xml
{% highlight xml %}
<!-- 子查询 -->
<resultMap type="Author" id="AuthorSubMap">
	<id property="id" column="author.id"/>
	<result property="realName" column="realName"/>
	<result property="IDCard" column="IDCard"/>
	<association property="user" column="userID"
	javaType="User" select="findById">
	</association>
</resultMap>
<select id="selectAuthor" resultMap="AuthorSubMap">
	select * from author
</select>
{% endhighlight xml %}
#### 测试类
{% highlight java %}
List<Author> author=session.selectList("selectAuthor");
for(Author a:author){
	System.out.println(a.getUser().getUserName()+a.getRealName());
}
{% endhighlight java %}

#### 子查询与联合查询区别：
#### 子查询：N+1次查询，占用资源可大可小
#### 联合查询：一次查询，占用资源大
#### 子查询较适合 懒加载

#### 懒加载例子：第二条语句当用到的时候才执行
#### 开启懒加载（需在设置别名之前）
{% highlight xml %}
<!-- 设置懒加载 -->
<settings>
	<setting name="lazyLoadingEnabled" value="true"/>
	<setting name="aggressiveLazyLoading" value="false"/>
</settings>
{% endhighlight xml %}

#### 测试类
{% highlight java %}
List<Author> author=session.selectList("selectAuthor");
for(Author a:author){
	System.out.println(a.getRealName());
	System.out.println("懒加载应用了");
	System.out.println(a.getUser().getUserName());
}
{% endhighlight java %}
#### 在控制台可以看到，第二条语句当用到的时候才执行

## 集合查询
#### 新建visit表，用来记录用户登录
#### 新建visit类
{% highlight java %}
package cheng.book.pojo;

import java.util.Date;

public class Visit {

	private Integer visitID;
	private User user;
	private Date visitDate;
	private String visitIP;

	public Integer getVisitID() {
		return visitID;
	}
	public void setVisitID(Integer visitID) {
		this.visitID = visitID;
	}
	public User getUser() {
		return user;
	}
	public void setUser(User user) {
		this.user = user;
	}
	public Date getVisitDate() {
		return visitDate;
	}
	public void setVisitDate(Date visitDate) {
		this.visitDate = visitDate;
	}
	public String getVisitIP() {
		return visitIP;
	}
	public void setVisitIP(String visitIP) {
		this.visitIP = visitIP;
	}

}
{% endhighlight java %}
#### User类添加visitList属性
{% highlight java %}
package cheng.book.pojo;

import java.util.List;

public class User {

	private int id;
	private String userName;
	private String password;
	private List<Visit> visitList;

	public User() {
		super();
	}
	public User(String userName, String password) {
		super();
		this.userName = userName;
		this.password = password;
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getUserName() {
		return userName;
	}
	public void setUserName(String userName) {
		this.userName = userName;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}

	public List<Visit> getVisitList() {
		return visitList;
	}
	public void setVisitList(List<Visit> visitList) {
		this.visitList = visitList;
	}
}
{% endhighlight java %}
#### 配置user.xml
{% highlight xml %}
<resultMap type="User" id="visitMap">
	<id property="id" column="id"/>
	<result property="userName" column="userName"/>
	<collection property="visitList" javaType="ArrayList" column="visitID"
			ofType="cheng.book.pojo.Visit">
		<result property="visitID" column="visitID"/>
		<result property="visitIP" column="visitIP"/>
		<result property="visitDate" column="visitDate"/>
	</collection>
</resultMap>
<select id="selectVisit" resultMap="visitMap">
	select * from User inner join visit on user.id=visit.userID
</select>
{% endhighlight xml %}

#### 测试类
{% highlight java %}
List<User> user=session.selectList("selectVisit");
for(User u:user){
	System.out.println(u.getUserName());
	for(Visit v:u.getVisitList()){
		System.out.println(v.getVisitIP());
	}
}
{% endhighlight java %}
#### 鉴别器 discriminator

## 动态SQL
#### 判断读者金币大于某值的会员号，若无金币则为普通会员 if
#### 新建读者表
#### 新建读者类
{% highlight java %}
package cheng.book.pojo;

public class Readers {

	private Integer readerID;
	private User user;
	private Integer money;
	public Integer getReaderID() {
		return readerID;
	}
	public void setReaderID(Integer readerID) {
		this.readerID = readerID;
	}
	public User getUser() {
		return user;
	}
	public void setUser(User user) {
		this.user = user;
	}
	public Integer getMoney() {
		return money;
	}
	public void setMoney(Integer money) {
		this.money = money;
	}
}
{% endhighlight java %}

#### 在配置文件建立select
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="/">

	<select id="selectReaderMoney" resultType="Readers" parameterType="Readers">
		select * from reader
			where 1=1
			<if test="money!=null">
				and money>#{money}
			</if>
	</select>

</mapper>
{% endhighlight xml %}
#### 建立测试类
{% highlight java %}
Readers readers=new Readers();
readers.setMoney(200);
List<Readers> read=session.selectList("selectReaderMoney",readers);
for(Readers r:read){
	System.out.println(r.getReaderID());
}
{% endhighlight java %}
#### 判断用户名，如果不存在就判断id,最后判断密码不为空 choose
#### 建立查询
{% highlight xml %}
<select id="selectUserChoose" resultType="User" parameterType="User">
	select * from User where 1=1
	<choose>
		<when test="userName!=null">
			and userName like #{userName}
		</when>
		<when test="id!=0">
			and id=#{id}
		</when>
		<otherwise>
			and password is not null
		</otherwise>
	</choose>
</select>
{% endhighlight xml %}
#### 测试类
{% highlight java %}
User user=new User();
user.setUserName("%i%");
user.setId(2);
List<User> list=session.selectList("selectUserChoose",user);
for(User u:list){
	System.out.println(u.getUserName());
}
{% endhighlight java %}

#### 判断用户名，如果不存在就判断id,最后判断密码不为空 where标记=智能条件
#### user.xml
{% highlight xml %}
<select id="selectUserWhere" resultType="User" parameterType="User">
	select * from User
	<where>
		<if test="userName!=null">
			and userName like #{userName}
		</if>
		<if test="id!=null">
			and id=#{id}
		</if>
	</where>
</select>
{% endhighlight xml %}
#### 测试类
{% highlight java %}
User user=new User();
user.setUserName("%i%");
user.setId(3);
List<User> list=session.selectList("selectUserWhere",user);
for(User u:list){
	System.out.println(u.getUserName());
}
{% endhighlight java %}

#### set标记=智能赋值
#### user.xml
{% highlight xml %}
<update id="updateUserSet" parameterType="User">
	update User
	<set>
		<if test="userName!=null">userName=#{userName},</if>
		<if test="password!=null">password=#{password},</if>
	</set>
</update>
{% endhighlight xml %}
#### 测试类
{% highlight java %}
User user=new User();
user.setId(1);
user.setUserName("张三丰");
user.setPassword("2222");
session.update("updateUserSet", user);
session.commit();
{% endhighlight java %}

#### trim标记=格式化标记

属性|描述
:--|:--
prefix|前缀增加
suffix|后缀增加
prefixOverrides|自动判断前置
suffixOverrides|自动判断后置

#### user.xml
{% highlight xml %}
<update id="updateUserTrim" parameterType="User">
	update User
	<trim prefix="SET" suffixOverrides="," suffix="WHERE id=#{id}">
		<if test="userName!=null and userName!='' " >
			userName=#{userName},
		</if>
		<if test="password!=null and password !='' ">
			password=#{password},
		</if>
	</trim>
</update>
{% endhighlight xml %}

#### 测试类
{% highlight java %}
User user=new User();
user.setId(1);
user.setUserName("张三丰");
user.setPassword("2322");
session.update("updateUserTrim", user);
session.commit();
{% endhighlight java %}

#### foreach标记

属性|描述
:--|:--
collection|循环集合或指定类型
item|每一次迭代结果
open|开始符号，可选
separator|元素之间的分隔符，可选
close|关闭符号，可选
index|list和数组的序列可选

#### 从指定id集合中查询出会员 foreach
#### user.xml
{% highlight xml %}
<select id="selectUserForeach" resultType="User" parameterType="User">
	select * from User
	<where>
		id in
		<foreach collection="list" item="item"
			index="index" open="(" separator="," close=")">
			#{item}
		</foreach>
	</where>
</select>
{% endhighlight xml %}
#### 测试类
{% highlight java %}
ArrayList<Integer> ides=new ArrayList<>();
ides.add(1);
ides.add(3);
ides.add(2);
List<User> list=session.selectList("selectUserForeach",ides);
for(User u:list){
	System.out.println(u.getUserName());
}
{% endhighlight java %}

#### 循环赋值
#### 使用集合一次添加多个用户
#### User.xml
{% highlight xml %}
<insert id="insertUserForeach">
	insert into User (userName,password) values
	<foreach item="item" index="key" collection="list"
		open="" separator="," close="">
		(#{item.userName},#{item.password})
	</foreach>
</insert>
{% endhighlight xml %}
#### 测试类
{% highlight java %}
ArrayList<User> list=new ArrayList<>();
User u1=new User("chenglong","222311");
User u2=new User("wangfeng","232176");
list.add(u1);
list.add(u2);
session.insert("insertUserForeach",list);
session.commit();
{% endhighlight java %}
#### 代码下载：[百度云盘](http://pan.baidu.com/s/1mhEkAAs) 密码：sxbm
