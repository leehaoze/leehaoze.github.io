---
title: 搭建SpringBoot多模块应用
date: 2020-01-07 15:00:36
tags:
- SpringBoot
---
# 新建项目
- 新建项目文件夹
`mkdir ~/IdeaProjects/spring-boot-behind`
- 在Idea中打开
打开`Intellij Idea`,选择`Open`，打开刚刚新建的文件夹
- 添加`Maven`
右键左侧导航栏中的**项目名**，选择`Add New Framewrok`
![IMAGE](http://image.leehaoze.cn/529DFD82AC31FFF5AEB1FD51F6BF3E06.jpg)
在弹窗中左侧导航栏，选择`Maven`
![](http://image.leehaoze.cn/130F3F2701CF0242C048152630E27FC1.jpg)
点击`OK`,填写一些信息即可。
- 添加`Spring`依赖
  - 编辑`pom.xml`，添加`packaging`条目`<packaging>pom</packaging>`
  - 再引入`Spring`的**pom parent**
```
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.7.RELEASE</version>
    </parent>
- 删除生成的`src`文件夹
```
# 添加base子模块
base模块如其名，都是一些基础的组件，比如异常处理、拦截器、过滤器、切片、返回结果封装、通用工具类等
- 新建`Module`
右击左侧导航栏项目名，选择`New->Module...`
![](http://image.leehaoze.cn/B573462E755842F0D6F2A6A3FEC05ACC.jpg)
在弹窗中选择`Maven`，点击`Next`，在`Name`中填写`base`，点击Finish
![](http://image.leehaoze.cn/87444A44B9D1A0A65917B6A814DBCE0D.jpg)
- 设置版本
子模块的版本号可以单独维护，也可以跟随父模块，这里为了统一，所有子模块都跟随父模块的版本，编辑`pom.xml`添加一条`version`条目即可
![](http://image.leehaoze.cn/4CFF941A1277547D84D8B602BDEBD6E9.jpg)
- 引入依赖
整个项目都要用到的依赖都是在这个模块里引入，比如：`spring-boot-start`、`lombok`、`fastjson`等
编辑`base/pom.xml`
    ```
    <dependencies>
        <!-- Spring Boot Starter-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- json库 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.59</version>
        </dependency>
        <!-- 提供Getter Setter注解等 -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.8</version>
        </dependency>
    </dependencies>
    ```
- 新建包文件夹
    在`base/src/main/java`下新建与`group ID`相同的包
    ![](http://image.leehaoze.cn/B0D46510706DA90D0A0DA949C34EE4D0.jpg)
    ![](http://image.leehaoze.cn/7E3AAF6527DEEA4B11F0C9109FED3507.jpg)

- 新建几个常用文件夹后大概长这样
![](http://image.leehaoze.cn/F37546B6B939312934F2552522C7A28C.jpg)

# 添加bean子模块
bean模块内主要是一些常量、实体类、DTO类
- 新建`Module`
再新建一个名为`bean`的`Module`
- 引入依赖（记得在`pom.xml`里添加此模块的版本号跟随父模块）
bean模块依赖base模块即可，如果有其他关于实体类的依赖，可以在此模块引入，只用于此模块。比如我在这里引入了谷歌的`guava`包，使用其中的`Convert`实现实体类与DTO类的转换
```
    <dependencies>
        <dependency>
            <groupId>com.leehaoze</groupId>
            <artifactId>base</artifactId>
            <version>${parent.version}</version>
        </dependency>
    </dependencies>
```
- 新建包文件夹
- 新建几个常用文件夹
![](http://image.leehaoze.cn/20200107170723.png)

# 添加dao子模块
dao模块内是数据操作，包括与数据库、redis等数据交互都在此模块，包括`MyBatis`的`Mapper`也是在此模块
- 新建`Module`
新建一个名为`dao`的`Module`
- 引入依赖（记得在`pom.xml`里添加此模块的版本号跟随父模块）
`dao`模块依赖于`bean`模块，初次之外使用`MyBatis`的话，还要在此模块 引入`MyBatis`相关依赖。
```
    <artifactId>dao</artifactId>

    <version>${parent.version}</version>

    <dependencies>
        <!-- bean -->
        <dependency>
            <groupId>com.leehaoze</groupId>
            <artifactId>bean</artifactId>
            <version>${parent.version}</version>
        </dependency>
        <!-- mybatis -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.0</version>
        </dependency>
        <!-- 分页工具 -->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.3</version>
        </dependency>
        <!-- MySQL 连接驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.17</version>
        </dependency>
    </dependencies>
```
- 新建包文件夹
- 在包下新建`mapper`文件夹

# 添加manager子模块
manager模块主要负责对数据的加工处理，实现DTO类与实体类的互转、数据的组合查询等。
- 新建`Module`
新建一个名为`manager`的`Module`
- 引入依赖（记得在`pom.xml`里添加此模块的版本号跟随父模块）
`manager`依赖于`dao`模块，
- 新建包文件夹
- 在报下新建`manager`文件夹

# 添加service子模块
service模块主要负责业务实现，包括参数的校验、业务逻辑处理等。
- 新建`Module`
新建一个名为`service`的`Module`
- 引入依赖（记得在`pom.xml`里添加此模块的版本号跟随父模块）
`service`依赖于`manager`子模块
- 新建包文件夹
- 在包下新建`service`文件夹

# 添加web子模块
web子模块主要是controller以及程序入口
- 新建`Module`
新建一个名为`web`的`Module`
- 引入依赖（记得在`pom.xml`里添加此模块的版本号跟随父模块）
`web`模块依赖于`service`模块
另外需要添加`build`插件
```
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```
- 新建包文件夹
- 在包下新建`controller`文件夹
- 创建应用入口
  - 在包下新建`MainApplication`类
  - 为类添加`@SpringBootApplication`注解
  
  ```
  @SpringBootApplication
  public class MainController {
    public static void main(String[] args) {
        SpringApplication.run(MainController.class, args);
    }
  }
  ```
- 创建`Spring-Boot`配置文件
  在`web/src/main.java/resources`中新建`application.properties`
  配置数据库连接配置
  ```
  spring.datasource.url=jdbc:mysql://你的数据库机器名:端口号/数据库名?useUnicode=true&characterEncoding=gbk&zeroDateTimeBehavior=convertToNull&jdbcCompliantTruncation=false
  
  spring.datasource.username=账号
  
  spring.datasource.password=密码
  
  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
  
  mybatis.mapper-locations=classpath*:mapper/*.xml

  server.port=8293
  ```
  *注意配置中`mybatis.mapper-locations`配置项，该项值为`classpath*:mapper/*.xml`,第一个*代表在所有的包路径下都查找，不然的话只会在配置文件所在的包路径下查找*
  
  # 运行
  打开`MainApplication`，可以看到IDEA在编辑区域左侧有一个绿色的小箭头，点击即可运行应用
  ![](http://image.leehaoze.cn/E07737DD52103C03FC5A8B9A953A4A8F.jpg)
