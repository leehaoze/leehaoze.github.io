---
title: Spring使用分页查插件
date: 2019-08-19 23:14:33
tags:
- Spring
---

分页查询是最经常做的工具了，Java里提供了很好用的分页工具可以让我们不再关心如何实现分页

# 引入依赖
```
<!-- 分页工具 -->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.3</version>
        </dependency>
```

# 注册插件
```
@Configuration
public class PageHelperBean {

    /**
     * 将分页插件注入到容器中
     * @return
     */
    @Bean
    public PageHelper pageHelper() {
        //分页插件
        PageHelper pageHelper = new PageHelper();
        Properties properties = new Properties();
        properties.setProperty("reasonable", "true");
        properties.setProperty("supportMethodsArguments", "true");
        properties.setProperty("helperDialect", "mysql");
        properties.setProperty("params", "count=countSql");
        pageHelper.setProperties(properties);
        return pageHelper;
    }

}
```

# 使用
```
PageHelper.startPage(page,perPage);
List<ApplicationListForm> result = applicationMapper.queryAppsInPage();
```

在你要做的查询前使用`PageHelper.startPage(page,perPage);`即可


