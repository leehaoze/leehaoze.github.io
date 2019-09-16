---
title: MyBatis-Generator几个问题的解决
date: 2019-09-16 15:59:03
tags:
- Spring
- Java
- Mybatis
---
# 使用MyBatis-Generator生成的Example
Mybatis-Generator生成的Example十分好用，通过Example我们可以随意的组合查询条件而不用去写XML文件。要想在生成时生成Example文件，需要在`table`标签里，去掉这些`*byExample="false"`即可
```
        <!-- 操作的具体表 -->
        <table schema="test" tableName="user" domainObjectName="User" 
            enableCountByExample="false" enableUpdateByExample="false"
            enableDeleteByExample="false" enableSelectByExample="false"
            selectByExampleQueryId="false">     
        </table>
```

# Mybatis-Generator无法覆盖旧的XML文件内容
使用Mabatis-Generator时经常遇到变更了表结构，重新生成xml文件、mapper文件，但经常发现xml文件根本没有被覆盖生成，而是追加内容，直接导致了项目报错，这里需要引入一个插件配置即可
```
 <!--覆盖生成XML文件-->
<plugin type="org.mybatis.generator.plugins.UnmergeableXmlMappersPlugin" />
```

# Mybatis-Generator无法区分同名表
有时开发时数据库中 会有不同库下的同名表，导致Mybatis-Generator 无法识别使用哪张表，这里需要在数据库JDBC连接参数里新增一个参数`nullCatalogMeansCurrent`
```<jdbcConnection driverClass="${jdbc.driver}"
                        connectionURL="${jdbc.url}" userId="${jdbc.user}" password="${jdbc.password}">
            <!--MySQL 不支持 schema 或者 catalog 所以需要添加这个-->
            <!--参考 : http://www.mybatis.org/generator/usage/mysql.html-->
            <property name="nullCatalogMeansCurrent" value="true"/>
        </jdbcConnection>
```

# Mybatis-Generator生成insert语句时，返回自增主键
Mybatis-Generator 默认生成的insert语句是不包含`userGeneratedKeys`和`keyColumn`信息，所以也就无法返回自增主键，这里是由于生成时没有指定主键列，所有不知道哪个是主键，在表信息里天鉴赏相关信息即可
```
<table tableName="user" domainObjectName="User">
    <generatedKey column="id" sqlStatement="MySql" identity="true"/>
</table>
```
