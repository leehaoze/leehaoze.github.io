---
title: Spring热部署+热部署不生效问题
date: 2019-08-14 20:54:52
tags:
- Spring
- Java
---

开始Spring应用时，经常需要修改一点代码看看效果，这个时候往往需要重启整个Spring应用，感觉还是挺麻烦的，不过好在Spring提供了热重启部署的功能

# 引入依赖
```
<!-- 开发者工具 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
```

引入开发者工具后，当代码重新编译后，Spring便会自动重启

# 配置IDE
Intellij IDEA并不是自动编译修改后的代码，因此需要打开自动编译 勾选上`build project automatically`
![自动编译](http://image.leehaoze.cn/20190814204329_ntG5H5_Screenshot.jpeg)

# 问题
在把项目转为多模块的项目结构后，再使用热部署时，会报错提示
`Illegal access: this web application instance has been stopped already. Could not load [mapper/].`
这是由于热部署再加载Mybatis的xml文件时，没有找到xml文件导致了，这里需要修改配置文件中，Mybatis的xml扫描路径
将`mybatis.mapper-locations=classpath:mapper/*.xml`修改为`mybatis.mapper-locations=classpath*:mapper/*.xml`
`classpath*`代表扫描所有模块下的路径，不带*则只扫描web模块下的路径


# 结束
这样简单配置后，每次修改代码后，按下`command + s`便会自动编译+打包部署了
但是有时候可能感觉自己明明修改了代码了，而且也保存了，但是请求接口不生效，这是因为修改后的代码编译失败了(语法错误等等),导致没有触发热部署，这个时候可以尝试手动编译，看一下是不是有错误
