---
layout: post
title: "MyBatis逆向工程"
date:   2019-01-12 +0800
toc: true
category: MyBatis
tags: MyBatis SSM
excerpt: 本篇文章主要讲解MyBatis逆向工程,根据数据库中的表生成mybatis代码。
---
#### MyBatis逆向工程就是根据数据库中的表生成mybatis代码(mapper文件、bean类、dao层文件)
## 1.引入依赖
{% highlight xml %}
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.4.6</version>
</dependency>
<dependency>
  <groupId>org.mybatis.generator</groupId>
  <artifactId>mybatis-generator-core</artifactId>
  <version>1.3.7</version>
</dependency>
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.13</version>
</dependency>
{% endhighlight xml %}

## 2.编写generatorConfig.xml
        <?xml version="1.0" encoding="UTF-8"?>
        <!DOCTYPE generatorConfiguration
          PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
          "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

        <generatorConfiguration>
          <context id="DB2Tables" targetRuntime="MyBatis3">
            <!-- 设置生成的java文件无注释 -->
            <commentGenerator>
            <property name="suppressAllComments" value="true" />
            </commentGenerator>

            <!-- 配置数据库连接 -->
            <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
            connectionURL="jdbc:mysql://localhost:3306/test?serverTimezone=GMT%2B8"
            userId="root"
            password="root">
            </jdbcConnection>

            <!-- 默认false,把JDBC DECIMAL和NUMERIC类型解析为Integer
            为true时把JDBC和NUMERIC类型解析为java.math.BigDecimal
            -->
            <javaTypeResolver >
              <property name="forceBigDecimals" value="false" />
            </javaTypeResolver>

            <!-- 指定javaBean生成的位置 -->
            <javaModelGenerator targetPackage="com.xingyu.bean" targetProject=".\src\main\java">
              <!-- 是否让schema作为包的后缀 -->
              <property name="enableSubPackages" value="true" />
              <!-- 从数据库返回的值被清理前后的空格 -->
              <property name="trimStrings" value="true" />
            </javaModelGenerator>

            <!-- 指定sql映射文件mapper生成的位置 -->
            <sqlMapGenerator targetPackage="mapper"  targetProject=".\src\main\resources">
              <!-- 是否让schema作为包的后缀 -->
              <property name="enableSubPackages" value="true" />
            </sqlMapGenerator>

            <!-- 指定dao接口生成的位置，mapper接口 -->
            <javaClientGenerator type="XMLMAPPER" targetPackage="com.xingyu.dao"  targetProject=".\src\main\java">
              <!-- 是否让schema作为包的后缀 -->
              <property name="enableSubPackages" value="true" />
            </javaClientGenerator>

            <!-- table指定每个表的生成策略 -->
            <table tableName="t_user" domainObjectName="User"></table>
            <table tableName="t_test" domainObjectName="Test"></table>

            </context>
        </generatorConfiguration>


## 3.编写MyBatisGenerator.java
{% highlight java %}
        public class MybatisGenerator {
        	public static void main(String[] args) throws IOException, XMLParserException, InvalidConfigurationException, SQLException, InterruptedException {
        		List<String> warnings = new ArrayList<String>();
        		boolean overwrite = true;
        		File configFile = new File("generatorConfig.xml");
        		ConfigurationParser cp = new ConfigurationParser(warnings);
        		Configuration config = cp.parseConfiguration(configFile);
        		DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        		MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
        		myBatisGenerator.generate(null);
        	}
        }
{% endhighlight java %}

#### 运行MyBatisGenerator.java后再对应的文件件下生成bean、dao、mapper
