---
title: Spring Boot多环境配置
date: 2019-08-15 14:30:51
tags:
- Spring
- Java
---
项目开发中，无可避免的需要正式、开发、测试环境等，不同环境的数据源之类的可能会不一样，因此需要区分不同环境下的配置，Spring Boot提供了这样的支持

在项目的`resource`文件夹下，Spring默认读取`application.properties`文件，但还可以建立`application-dev.properties`、`application-test.properties`、`application-prod.properties`三个不同场景(开发、测试、生产)的配置文件，然后再默认的`application.properties`中，选择激活使用哪个环境即可，在配置文件中配置`spring.properties.active=dev`即可使用开发环境
