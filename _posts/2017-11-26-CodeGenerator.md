---
layout:     post
title:      代码生成器
subtitle:   三种代码生成的方法
date:       2017-11-26
author:     Static
header-img: img/CodeGenerator/bg.png
catalog: true
tags:
    - 记录
---

### 总结下博主最近学的代码生成器

### 分为三种方法

#### 第一种：
> 利用mybatis-generator-core-1.3.5.jar和mysql-connector-java-5.1.18.jar两个jar包


##### 1. 在我的电脑D盘中创建一个generator目录

![目录结构](/img/CodeGenerator/目录结构.PNG)

##### 2. 修改generator.xml中的代码

```xml
<?xml version="1.0" encoding="UTF-8"?>  
    <!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN" "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">  
    <generatorConfiguration>  
        <!-- 数据库驱动包位置 -->  
        <classPathEntry location="D:\generator\mysql-connector-java-5.1.18.jar" />   
        <!-- <classPathEntry location="C:\oracle\product\10.2.0\db_1\jdbc\lib\ojdbc14.jar" />-->  
        <context id="DB2Tables" targetRuntime="MyBatis3">  
            <commentGenerator>  
                <property name="suppressAllComments" value="true" />  
            </commentGenerator>  
            <!-- 数据库链接URL、用户名、密码 -->  
             <jdbcConnection driverClass="com.mysql.jdbc.Driver" connectionURL="jdbc:mysql://localhost:3306/emp?characterEncoding=utf8" userId="root" password="123456">   
            <!--<jdbcConnection driverClass="oracle.jdbc.driver.OracleDriver" connectionURL="jdbc:oracle:thin:@localhost:1521:orcl" userId="msa" password="msa">-->  
            </jdbcConnection>  
            <javaTypeResolver>  
                <property name="forceBigDecimals" value="false" />  
            </javaTypeResolver>  
            <!-- 生成模型的包名和位置 -->  
            <javaModelGenerator targetPackage="andy.model" targetProject="D:\generator\src">  
                <property name="enableSubPackages" value="true" />  
                <property name="trimStrings" value="true" />  
            </javaModelGenerator>  
            <!-- 生成的映射文件包名和位置 -->  
            <sqlMapGenerator targetPackage="andy.mapping" targetProject="D:\generator\src">  
                <property name="enableSubPackages" value="true" />  
            </sqlMapGenerator>  
            <!-- 生成DAO的包名和位置 -->  
            <javaClientGenerator type="XMLMAPPER" targetPackage="andy.dao" targetProject="D:\generator\src">  
                <property name="enableSubPackages" value="true" />  
            </javaClientGenerator>  
            <!-- 要生成那些表(更改tableName和domainObjectName就可以) -->  
            <table tableName="student" domainObjectName="Student" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false" />  
            <!-- <table tableName="course_info" domainObjectName="CourseInfo" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false" />  
            <table tableName="course_user_info" domainObjectName="CourseUserInfo" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false" /> -->  
        </context>  
</generatorConfiguration>  
```

##### 3.打开cmd，进入mybatis-generator-core-1.3.5.jar所在的目录，输入

```shell
java -jar mybatis-generator-core-1.3.5.jar -configfile generator.xml -overwrite
```

提示*MyBatis Generator finished successfully, there were warnings.* 表示生成成功。

![目录结构](/img/CodeGenerator/cmd提示生成成功.png)

#### 第二种：

> 利用Maven的代码生成器的插件，默认大家已会Maven


##### 1. 所需依赖：

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.34</version>
</dependency>
```

##### 2. Maven插件

```
<plugin>
     <groupId>org.mybatis.generator</groupId>
     <artifactId>mybatis-generator-maven-plugin</artifactId>
     <version>1.3.2</version>
     <configuration>
     <verbose>true</verbose>
     <overwrite>true</overwrite><!--重新生成会覆盖原先的代码-->
     </configuration>
</plugin>
```

##### 3. 代码生成的配置文件generatorConfig.xml,根据项目来修改配置文件。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd" >
<generatorConfiguration>

    <!--classPathEntry:数据库的JDBC驱动 -->
    <classPathEntry
            location="C:\Users\qilixiang\.m2\repository\mysql\mysql-connector-java\5.1.34\mysql-connector-java-5.1.34.jar"/>

    <context id="MysqlTables" targetRuntime="MyBatis3">

        <!-- 注意这里面的顺序确定的，不能随变更改 -->
        <!-- 自定义的分页插件 <plugin type="com.deppon.foss.module.helloworld.shared.PaginationPlugin"/> -->

        <!-- 可选的（0 or 1） -->
        <!-- 注释生成器 -->
        <commentGenerator>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>

        <!-- 必须的（1 required） -->
        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/work_attendance"
                        userId="root" password="123456">
        </jdbcConnection>

        <!-- 可选的（0 or 1） -->
        <!-- 类型转换器或者加类型解析器 -->
        <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer true，把JDBC DECIMAL 和
            NUMERIC 类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>


        <!-- 必须的（1 required） -->
        <!-- java模型生成器 -->
        <!-- targetProject:自动生成代码的位置 -->
        <javaModelGenerator targetPackage="top.qilixiang.attendence.entity"
                            targetProject="D:\code\project\do-call\src\main\java"
        >
            <!-- TODO enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="true"/>
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!-- 必须的（1 required） -->
        <!-- map xml 生成器 -->
        <sqlMapGenerator targetPackage="top.qilixiang.attendence.dao"
                         targetProject="D:\code\project\do-call\src\main\java">
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>

        <!-- 可选的（0 or 1） -->
        <!-- mapper 或者就是dao接口生成器 -->
        <javaClientGenerator targetPackage="top.qilixiang.attendence.dao"
                             targetProject="D:\code\project\do-call\src\main\java"
                             type="XMLMAPPER">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>

        <table tableName="attendence" domainObjectName="Attendence"
               enableInsert="true" enableCountByExample="false" enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false" selectByExampleQueryId="false">
            <!-- 忽略字段 可选的（0 or 1） -->
            <!-- <ignoreColumn column="is_use" /> -->
            <!--无论字段是什么类型，生成的类属性都是varchar。 可选的（0 or 1） 测试无效 -->
            <!-- <columnOverride column="city_code" jdbcType="VARCHAR" /> -->
        </table>

    </context>
    
</generatorConfiguration>
```

##### 4. 进入当前项目的根目录，在cmd中输入

```
mvn mybatis-generator:generate -e //-e表示在cmd中显示日志
```

![方法二cmd命令](/img/CodeGenerator/方法二cmd.png)

也可以配置idea命令

![方法二idea命令](/img/CodeGenerator/方法二idea命令.png)


cmd提示：

![方法二idea生成成功](/img/CodeGenerator/方法二idea生成成功.png)

生成成功，目录结构：

![方法二生成成功目录结构](/img/CodeGenerator/方法二生成成功目录结构.png)


### 第三种

> 代码生成器，根据数据表名称生成对应的Model、Mapper、Service、Controller简化开发,主要利用freemarker作为模版生成Service和Controller。


##### 1. 首先来看看pom.xml所需依赖的jar包

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>top.qilixiang</groupId>
  <artifactId>CodeGeneratorThree</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>CodeGeneratorThree</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <freemarker.version>2.3.23</freemarker.version>
    <mybatis-generator.version>1.3.5</mybatis-generator.version>
    <mysql-connector-java.version>5.1.6</mysql-connector-java.version>
    <commons-lang3.version>3.5</commons-lang3.version>
    <slf4j-api.version>1.7.25</slf4j-api.version>
    <spring.baseline.version>4.3.10.RELEASE</spring.baseline.version>
    <logback.version>1.2.3</logback.version>
    <mybatis.mapper.version>3.3.9</mybatis.mapper.version>
    <mybatis.pagehelper.version>5.0.4</mybatis.pagehelper.version>
    <mybatis.version>3.4.2</mybatis.version>
    <junit.version>4.12</junit.version>
  </properties>

  <dependencies>

    <!--代码生成器-->
    <dependency>
      <groupId>org.freemarker</groupId>
      <artifactId>freemarker</artifactId>
      <version>${freemarker.version}</version>
    </dependency>
    <dependency>
      <groupId>org.mybatis.generator</groupId>
      <artifactId>mybatis-generator-core</artifactId>
      <version>${mybatis-generator.version}</version>
    </dependency>

    <!--MyBatis-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>${mybatis.version}</version>
    </dependency>
    <dependency>
      <groupId>tk.mybatis</groupId>
      <artifactId>mapper</artifactId>
      <version>${mybatis.mapper.version}</version>
    </dependency>
    <dependency>
      <groupId>com.github.pagehelper</groupId>
      <artifactId>pagehelper</artifactId>
      <version>${mybatis.pagehelper.version}</version>
    </dependency>

    <!--db-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>${mysql-connector-java.version}</version>
    </dependency>

    <!-- logger dependencies-->
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>${slf4j-api.version}</version>
    </dependency>
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-core</artifactId>
      <version>${logback.version}</version>
    </dependency>
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>${logback.version}</version>
    </dependency>

    <!--commons-->
    <dependency>
      <groupId>org.apache.commons</groupId>
      <artifactId>commons-lang3</artifactId>
      <version>${commons-lang3.version}</version>
    </dependency>

    <!-- springFramework dependencies -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${spring.baseline.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.baseline.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.baseline.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring.baseline.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.baseline.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
      <version>${spring.baseline.version}</version>
    </dependency>

    <!--junit-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>${junit.version}</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>

```

##### 2. 需要两个配置文件application-dev.properties和generatorConfig.xml

> application-dev.properties


```
#-------项目所在的根目录---------
project.path=D:/code/project/CodeGeneratorThree

java.path=/src/main/java
resources.path=/src/main/resources
db.obj.Delimiter=`

gen.basepackage=top.qilixiang.crud
gen.context.id=crud
gen.author=qilixiang
rest=true

#-------Mapper接口所在的包---------
mappers=top.qilixiang.crud.base.Mapper

#-------数据库的配置---------
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/school?useUnicode=true&characterEncoding=utf8
jdbc.username=root
jdbc.password=123456
```
> generatorConfig.xml


```xml
<?xml version="1.0" encoding="UTF-8"?> <!DOCTYPE generatorConfiguration PUBLIC
    "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
    "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd"> <!-- 配置生成器 -->
<generatorConfiguration>
  <properties resource="application-dev.properties"/>

  <context id="crud" defaultModelType="hierarchical" targetRuntime="MyBatis3Simple">

    <property name="autoDelimitKeywords" value="false"/>
    <property name="javaFileEncoding" value="UTF-8"/>
    <property name="javaFormatter" value="org.mybatis.generator.api.dom.DefaultJavaFormatter"/>
    <property name="xmlFormatter" value="org.mybatis.generator.api.dom.DefaultXmlFormatter"/>
    <property name="beginningDelimiter" value="${db.obj.Delimiter}"/>
    <property name="endingDelimiter" value="${db.obj.Delimiter}"/>
    <!-- mergeable 为true时，可合并，为false，重新生成的时采用覆盖-->
    <property name="mergeable" value="false"></property>
    <!--通用mapper的插件-->
    <plugin type="tk.mybatis.mapper.generator.MapperPlugin">
      <property name="mappers" value="${mappers}"/>
    </plugin>

    <jdbcConnection driverClass="${jdbc.driver}" connectionURL="${jdbc.url}"
                    userId="${jdbc.username}" password="${jdbc.password}"/>
    <javaTypeResolver type="org.mybatis.generator.internal.types.JavaTypeResolverDefaultImpl">
      <property name="forceBigDecimals" value="false"/>
    </javaTypeResolver>
    <!--model-->
    <javaModelGenerator targetPackage="${gen.basepackage}.entity" targetProject="${project.path}${java.path}">
      <property name="constructorBased" value="false"/>
      <property name="enableSubPackages" value="true"/>
      <property name="immutable" value="false"/>
      <property name="trimStrings" value="true"/>
    </javaModelGenerator>
    <!--mapper xml file-->
    <sqlMapGenerator targetPackage="mapper" targetProject="${project.path}${resources.path}">
      <property name="enableSubPackages" value="false"/>
    </sqlMapGenerator>
    <!--Mapper interface-->
    <javaClientGenerator targetPackage="${gen.basepackage}.repository" type="XMLMAPPER"
                         targetProject="${project.path}${java.path}">
      <property name="enableSubPackages" value="false"/>
    </javaClientGenerator>

    <!--配合CodeGenerator的专用写法，带*的会认为tableName是前缀-->
    <table tableName="crud_*"  >
      <!--主键生成方式-->
      <generatedKey column="id" sqlStatement="Mysql" identity="true"/>
    </table>

  </context>
</generatorConfiguration>
```

**注意：**

- application-dev.properties中gen.context.id=crud要和generatorConfig.xml中<context id="crud */>"一样
- CodeGenerator要求数据库的表名前缀要一致

##### 3. 需要创建freemarker模版来生成Service和Controller的格式

> service.ftl


```ftl
package ${basePackage}.service;

import com.github.pagehelper.ISelect;
import com.github.pagehelper.PageHelper;
import com.github.pagehelper.PageInfo;
import org.apache.ibatis.exceptions.TooManyResultsException;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.transaction.annotation.Transactional;
import tk.mybatis.mapper.entity.Condition;
import ${basePackage}.entity.${modelNameUpperCamel};
import ${basePackage}.repository.${modelNameUpperCamel}Mapper;

import java.lang.reflect.Field;
import java.lang.reflect.ParameterizedType;
import java.util.List;

/**
* 基于通用MyBatis Mapper插件的Service接口的实现
*/
public class ${modelNameUpperCamel}Service{
    @Autowired
    protected ${modelNameUpperCamel}Mapper mapper;

    private Class<${modelNameUpperCamel}> modelClass;    // 当前泛型真实类型的Class

    public ${modelNameUpperCamel}Service() {
        ParameterizedType pt = (ParameterizedType) this.getClass().getGenericSuperclass();
        modelClass = (Class<${modelNameUpperCamel}>) pt.getActualTypeArguments()[0];
    }

    @Transactional(readOnly = false)
        public void save(${modelNameUpperCamel} model) {
        mapper.insertSelective(model);
    }

    @Transactional(readOnly = false)
    public void save(List<${modelNameUpperCamel}> models) {
        mapper.insertList(models);
    }

    @Transactional(readOnly = false)
        public void deleteById(Integer id) {
        mapper.deleteByPrimaryKey(id);
    }

    @Transactional(readOnly = false)
        public void deleteByIds(String ids) {
        mapper.deleteByIds(ids);
    }

    @Transactional(readOnly = false)
    public void update(${modelNameUpperCamel} model) {
        mapper.updateByPrimaryKeySelective(model);
    }

    public ${modelNameUpperCamel} findById(Integer id) {
        return mapper.selectByPrimaryKey(id);
    }


    public ${modelNameUpperCamel} findBy(String property, Object value) throws TooManyResultsException {
        // 手动设置数据源
        // DataSourceKeyHolder.setDbType(DataSourceKeyHolder.DataSourceType.MASTER);
        try {
            ${modelNameUpperCamel} model = modelClass.newInstance();
            Field field = modelClass.getDeclaredField(property);
            field.setAccessible(true);
            field.set(model, value);
            return mapper.selectOne(model);
        } catch (ReflectiveOperationException e) {
            throw new RuntimeException(e.getMessage(), e);
        }
    }

    public List<${modelNameUpperCamel}> findByIds(String ids) {
        return mapper.selectByIds(ids);
    }

    public List<${modelNameUpperCamel}> findByCondition(Condition condition) {
        return mapper.selectByCondition(condition);
    }


    public List<${modelNameUpperCamel}> findAll() {
        return mapper.selectAll();
    }

    public int count() {
        return mapper.selectCount(null);
    }


    public PageInfo findAll(Integer pageNumber, Integer pageSize) {
        return PageHelper.startPage(pageNumber, pageSize).doSelectPageInfo(new ISelect() {
            @Override
            public void doSelect() {
                mapper.selectAll();
            }
        });
    }
}

```

> controller-restful.ftl


```java
package ${basePackage}.api;

import ${basePackage}.entity.${modelNameUpperCamel};
import ${basePackage}.service.${modelNameUpperCamel}Service;
import com.github.pagehelper.PageInfo;
import org.springframework.web.bind.annotation.*;

import javax.annotation.Resource;

@RestController
@RequestMapping("${baseRequestMapping}")
public class ${modelNameUpperCamel}API {

    /**
     * 操作成功
     */
    private static final String DEFAULT_SUCCESS_MESSAGE = "SUCCESS";

    @Resource
    private ${modelNameUpperCamel}Service ${modelNameLowerCamel}Service;

    @PostMapping
    public String add(${modelNameUpperCamel} ${modelNameLowerCamel}) {
        ${modelNameLowerCamel}Service.save(${modelNameLowerCamel});
        return DEFAULT_SUCCESS_MESSAGE;
    }

    @DeleteMapping("/{id}")
    public String delete(@PathVariable Integer id) {
        ${modelNameLowerCamel}Service.deleteById(id);
        return DEFAULT_SUCCESS_MESSAGE;
    }

    @PutMapping
    public String update(${modelNameUpperCamel} ${modelNameLowerCamel}) {
        ${modelNameLowerCamel}Service.update(${modelNameLowerCamel});
        return DEFAULT_SUCCESS_MESSAGE;
    }
    @GetMapping("/{id}")
    public ${modelNameUpperCamel} detail(@PathVariable Integer id) {
        ${modelNameUpperCamel} ${modelNameLowerCamel} = ${modelNameLowerCamel}Service.findById(id);
        return ${modelNameLowerCamel};
    }

    @GetMapping
    public PageInfo list(Integer pageNumber, Integer pageSize) {
        PageInfo pageInfo = ${modelNameLowerCamel}Service.findAll(pageNumber,pageSize);
        return pageInfo;
    }
}

```

##### 4. 创建CodeGenerator类

```java
package top.qilixiang;

import freemarker.template.TemplateExceptionHandler;
import org.apache.commons.lang3.StringUtils;
import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.Context;
import org.mybatis.generator.config.GeneratedKey;
import org.mybatis.generator.config.TableConfiguration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.exception.InvalidConfigurationException;
import org.mybatis.generator.internal.DefaultShellCallback;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.core.io.ClassPathResource;
import org.springframework.core.io.Resource;

import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.sql.*;
import java.text.SimpleDateFormat;
import java.util.*;
import java.util.Date;

/**
 * 代码生成器，根据数据表名称生成对应的Model、Mapper、Service、Controller简化开发。<br/>
 * 该工具类需要MBG的配置文件和properties属性文件
 * 使用方法参考http://qilixiang1118.top/2017/11/26/%E4%BB%A3%E7%A0%81%E7%94%9F%E6%88%90%E5%99%A8/
 *
 * @author qilixiang
 */
public class CodeGenerator {
    private static final Logger LOGGER = LoggerFactory.getLogger(CodeGenerator.class);

    //from constructor or keep default
    private String propertiesPath = "/application-dev.properties";
    private String configPath = "/generatorConfig.xml";

    //fields with default value
    private static final String BLANK_STRING = "";
    private final String DATE = new SimpleDateFormat("yyyy/MM/dd").format(new Date());//@date
    private final List<String> WARNINGS = new ArrayList<String>();
    private final DefaultShellCallback CALLBACK = new DefaultShellCallback(true);

    // from configuration
    private String AUTHOR;//@author
    private String CONTEXTID;
    private String PROJECT_PATH;
    //private final String TEMPLATE_FILE_PATH;
    private String BASE_PACKAGE;
    private String CONTROLLER_FTL;
    private String JAVA_PATH;
    private String RESOURCES_PATH;
    private String BASE_PACKAGE_PATH;
    private String PACKAGE_PATH_SERVICE;
    // private final String PACKAGE_PATH_SERVICE_IMPL;
    private String PACKAGE_PATH_CONTROLLER;
    private String JDBC_DIVER_CLASS_NAME;
    private String JDBC_URL;
    private String JDBC_USERNAME;
    private String JDBC_PASSWORD;

    //mbg config
    private Configuration config;
    private Context context;

    //freemarker config
    private freemarker.template.Configuration freemarkerCfg;
    private boolean need_rest;
    private boolean dealed = false;

    public CodeGenerator() {
        init();
    }

    public CodeGenerator(String propertiesPath, String mgbXmlPath) {
        this.propertiesPath = propertiesPath;
        this.configPath = mgbXmlPath;
        init();
    }

    private void init() {
        Resource resource = new ClassPathResource(propertiesPath);//ClassPathResource()加载是的resources目录下的配置文件
        Properties props = new Properties();
        try {
            props.load(resource.getInputStream());
            AUTHOR = props.getProperty("gen.author");
            CONTEXTID = props.getProperty("gen.context.id").trim();
            PROJECT_PATH = props.getProperty("project.path").trim();
            File projPathFile = new File(PROJECT_PATH);
            if (projPathFile.exists() == false) {
                projPathFile.mkdirs();
            }
            //TEMPLATE_FILE_PATH = PROJECT_PATH + "/src/test/resources/generator/template";
            BASE_PACKAGE = props.getProperty("gen.basepackage").trim();
            need_rest = Boolean.parseBoolean(props.getProperty("rest").trim());
            CONTROLLER_FTL = "controller" + (need_rest ? "-restful" : "") + ".ftl"; // controller.ftl
            JAVA_PATH = props.getProperty("java.path").trim();
            File javaPathFile = new File(PROJECT_PATH, JAVA_PATH);
            if (javaPathFile.exists() == false) {
                javaPathFile.mkdirs();
            }
            RESOURCES_PATH = props.getProperty("resources.path").trim();
            File resourcePathFile = new File(PROJECT_PATH, RESOURCES_PATH);
            if (resourcePathFile.exists() == false) {
                resourcePathFile.mkdirs();
            }
            BASE_PACKAGE_PATH = "/" + BASE_PACKAGE.replaceAll("\\.", "/") + "/";//项目基础包路径
            PACKAGE_PATH_SERVICE = BASE_PACKAGE_PATH + "service/";//生成的Service存放路径;
            // PACKAGE_PATH_SERVICE_IMPL = BASE_PACKAGE_PATH + "/service/impl/";//生成的Service实现存放路径 ;
            if (need_rest)
                PACKAGE_PATH_CONTROLLER = BASE_PACKAGE_PATH + "/api/";//生成的API存放路径;
            else
                PACKAGE_PATH_CONTROLLER = BASE_PACKAGE_PATH + "/web/";//生成的Controller存放路径;

            JDBC_DIVER_CLASS_NAME = props.getProperty("jdbc.driver").trim();
            JDBC_URL = props.getProperty("jdbc.url").trim();
            JDBC_USERNAME = props.getProperty("jdbc.username").trim();
            JDBC_PASSWORD = props.getProperty("jdbc.password").trim();


            ConfigurationParser cp = new ConfigurationParser(WARNINGS);
            config = cp.parseConfiguration(new ClassPathResource(configPath).getInputStream());
            context = config.getContext(CONTEXTID);//CONTEXTID与generatorConfig.xml中的<context id=""/>一致

            //load jdbc driver
            Class.forName(JDBC_DIVER_CLASS_NAME);
        } catch (Exception e) {
            throw new RuntimeException();//直接抛出运行时异常
        }
    }

    public void generateMapper() {
        dealTables();
        try {
            MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, CALLBACK, WARNINGS);
            myBatisGenerator.generate(null);
            if (LOGGER.isDebugEnabled()) {
                LOGGER.debug("代码成功生成，路径：" + PROJECT_PATH);
            }
        } catch (InvalidConfigurationException | SQLException | IOException | InterruptedException e) {
            throw new RuntimeException();//直接抛出运行时异常
        }
    }


    /**
     * 解析所有table配置，正确地和数据库中的表对应起来，形成新的TableConfiguration列表
     */
    private void dealTables() {
        if (!dealed) {
            List<TableConfiguration> tcs = context.getTableConfigurations();
            Set<TableConfiguration> newTcs = new HashSet<>();
            Set<String> dbTableNameSet = getDbTableNames();
            for (TableConfiguration tc : tcs) {
                dealTable(newTcs, dbTableNameSet, tc);
            }
            tcs.clear();
            tcs.addAll(newTcs);
            dealed = true;
        }
    }

    /**
     * 模板文件的路径
     *
     * @param templateDir
     */
    public void genServiceAndController(String templateDir) {
        dealTables();
        List<TableConfiguration> tableConfigs = context.getTableConfigurations();
        for (TableConfiguration conf : tableConfigs) {
            String domainName = conf.getDomainObjectName();
            try {
                //build config
                buildFreemarkerConfiguration(templateDir);
                //prepare data used in template
                Map<String, Object> data = new HashMap<>();
                data.put("date", DATE);
                data.put("author", AUTHOR);
                data.put("baseRequestMapping", tableNameConvertMappingPath(conf.getTableName()));
                data.put("modelNameUpperCamel", domainName);//??
                String modelNameLowerCamel = domainName.substring(0, 1).toLowerCase() + domainName.substring(1);
                data.put("modelNameLowerCamel", modelNameLowerCamel);
                data.put("basePackage", BASE_PACKAGE);

                String javaFileName = domainName + "Service.java";
                File file = new File(PROJECT_PATH + JAVA_PATH + PACKAGE_PATH_SERVICE + javaFileName);
                System.out.println(file);
                if (!file.getParentFile().exists()) {
                    file.getParentFile().mkdirs();
                }
                freemarkerCfg.getTemplate("service.ftl").process(data, new FileWriter(file));
                if (LOGGER.isDebugEnabled()) {
                    LOGGER.debug("路径：" + PROJECT_PATH);
                    LOGGER.debug(javaFileName + "生成成功");
                }
                // File file1 = new File( PROJECT_PATH + JAVA_PATH + PACKAGE_PATH_SERVICE_IMPL + domainName + "ServiceImpl.java" );
                // if (!file1.getParentFile().exists()) {
                //   file1.getParentFile().mkdirs();
                // }
                // freemarkerCfg.getTemplate( "service-impl.ftl" ).process( data,
                //     new FileWriter( file1 ) );
                // System.out.println( domainName + "ServiceImpl.java 生成成功" );

                File file2 = new File(PROJECT_PATH + JAVA_PATH + PACKAGE_PATH_CONTROLLER + domainName + (need_rest ? "API" : "Controller") + ".java");
                if (!file2.getParentFile().exists()) {
                    file2.getParentFile().mkdirs();
                }
                //数据库表明前最好加一个标识符，这是CodeGenerator的固定写法
                freemarkerCfg.getTemplate(CONTROLLER_FTL).process(data, new FileWriter(file2));
                if (LOGGER.isDebugEnabled()) {
                    LOGGER.debug("路径：" + PROJECT_PATH);
                    LOGGER.debug(domainName + (need_rest ? "API" : "Controller") + ".java 生成成功");
                }
            } catch (Exception e) {
                throw new RuntimeException();//直接抛出运行时异常
            }
        }
    }


    private void buildFreemarkerConfiguration(String TEMPLATE_FILE_PATH) throws IOException {
        if (freemarkerCfg != null)
            return;
        freemarkerCfg = new freemarker.template.Configuration(freemarker.template.Configuration.VERSION_2_3_23);
        freemarkerCfg.setDirectoryForTemplateLoading(new File(TEMPLATE_FILE_PATH));
        freemarkerCfg.setDefaultEncoding("UTF-8");
        freemarkerCfg.setTemplateExceptionHandler(TemplateExceptionHandler.IGNORE_HANDLER);
    }

    /**
     * 只连一次数据库获得所有的表名，获取表名后关闭数据库连接
     *
     * @return
     */
    private Set<String> getDbTableNames() {
        Set<String> dbTableNameSet = new HashSet<>();
        try (Connection connection = DriverManager.getConnection(JDBC_URL, JDBC_USERNAME, JDBC_PASSWORD);) {
            DatabaseMetaData dbMetaData = connection.getMetaData();
            ResultSet rs = dbMetaData.getTables(null, null, null, null);
            while (rs.next()) {
                final String tableName = rs.getString("TABLE_NAME");
                dbTableNameSet.add(tableName);
            }
        } catch (Exception e) {
            LOGGER.error(JDBC_URL + "::" + JDBC_USERNAME + "::" + JDBC_PASSWORD, e);
            throw new RuntimeException();//直接抛出运行时异常
        }
        return dbTableNameSet;
    }

    /**
     * 根据原Table配置中是否有通配符来解析数据库中的表名，将符合要求的表配置后加入新配置容器
     *
     * @param newTcs         新Table配置容器
     * @param dbTableNameSet 数据库表名容器
     * @param oldTc          原Table配置
     */
    private void dealTable(Set<TableConfiguration> newTcs, Set<String> dbTableNameSet, TableConfiguration oldTc) {
        String tcTableName = oldTc.getTableName();
        // 分两类，带通配符和不带通配符
        if (tcTableName.contains("*")) {
            Iterator<String> dbTableNameIter = dbTableNameSet.iterator();
            while (dbTableNameIter.hasNext()) {
                String dbTableName = dbTableNameIter.next();
                // 配置中表名为*，则所有表都符合要求
                if (tcTableName.equals("*")) {
                    newTcs.add(configTable(dbTableName, null, oldTc.getGeneratedKey()));
                } else {
                    int starIndex = tcTableName.indexOf("*");
                    //截取前缀
                    String prefix = tcTableName.substring(0, starIndex);
                    //满足前缀要求
                    if (dbTableName.startsWith(prefix)) {
                        newTcs.add(configTable(dbTableName, prefix, oldTc.getGeneratedKey()));
                    }
                }
            }
        } else { // xml中配置的就是要使用的表名和domain
            if (newTcs.contains(tcTableName)) {
                newTcs.add(oldTc);
            } else {
                LOGGER.warn("数据库中不存在此表：%s", tcTableName);
            }
        }

    }

    /**
     * 配置新的TableConfiguration
     *
     * @param tableName    原生表名
     * @param tablePrefix  待去除的前缀
     * @param generatedKey 主键生成方式
     * @return
     */
    private TableConfiguration configTable(String tableName, String tablePrefix, GeneratedKey generatedKey) {
        TableConfiguration tableConfiguration = new TableConfiguration(context);
        tableConfiguration.setTableName(tableName);
        tableConfiguration.setGeneratedKey(generatedKey);
        // 表前缀
        if (StringUtils.isNotEmpty(tablePrefix)) {
            String domainObjectName = tableName.replaceFirst(tablePrefix, BLANK_STRING);
            tableConfiguration.setDomainObjectName(tableNameConvertUpperCamel(domainObjectName));
        }
        return tableConfiguration;
    }

    private static String tableNameConvertUpperCamel(String tableName) {
        String camel = tableNameConvertLowerCamel(tableName);
        return camel.substring(0, 1).toUpperCase() + camel.substring(1);

    }

    private static String tableNameConvertLowerCamel(String tableName) {
        StringBuilder result = new StringBuilder();
        if (tableName != null && tableName.length() > 0) {
            boolean flag = false;
            for (int i = 0; i < tableName.length(); i++) {
                char ch = tableName.charAt(i);
                if ("_".charAt(0) == ch) {
                    flag = true;
                } else {
                    if (flag) {
                        result.append(Character.toUpperCase(ch));
                        flag = false;
                    } else {
                        result.append(ch);
                    }
                }
            }
        }
        return result.toString();
    }


    private static String tableNameConvertMappingPath(String tableName) {
        return "/" + (tableName.contains("_") ? tableName.replaceAll("_", "/") : tableName);
    }

    private class TableInfo {
        String tableName;
        String tablePrefix;
        String columnPrefix;

        public TableInfo(String tableName) {
            this.tableName = tableName;
        }

        public TableInfo(String tableName, String tablePrefix, String columnPrefix) {
            this.tableName = tableName;
            this.tablePrefix = tablePrefix;
            this.columnPrefix = columnPrefix;
        }

    }

    public static void main(String[] args) throws Exception {
        CodeGenerator codeGenerator = new CodeGenerator();
        codeGenerator.generateMapper();
        codeGenerator.genServiceAndController("D:\\code\\project\\CodeGeneratorThree\\src\\main\\resources\\ssm_template_demo");
    }
}

```

##### 5. 运行CodeGenerator


![方法三生成成功](/img/CodeGenerator/方法三生成成功.png)

既生成成功。

项目生成后的目录结构：

![方法三成功目录结构](/img/CodeGenerator/方法三成功目录结构.png)

第三种方法的项目已上传到我的github -> [https://github.com/codeFarmer1118/CodeGeneratorThree](https://github.com/codeFarmer1118/CodeGeneratorThree)

### *代码生成器协助我们不用再写最基本的crud代码，可以大大提高工作效率。*