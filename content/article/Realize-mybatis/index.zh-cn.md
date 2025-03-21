---
title: "实现Mybatis"
description: "模拟mybatis实现过程"
date: 2025-03-21
draft: false
tags: ["Mybatis", "AOP", "JDK动态代理"]
---

## 介绍

Mybatis 是通过jdk的动态代理和反射来实现mapper的方法构建的

## 环境准备

{{< keywordList >}}
{{< keyword >}} JDK21 {{< /keyword >}} 
{{< keyword >}} Maven {{< /keyword >}} 
{{< keyword >}} Mysql {{< /keyword >}} 
{{< /keywordList>}}
## 下载和部署模型

1. **UserMapper**  
   ```java
   public interface UserMapper {
    User selectUserById(@Param("id") int id);

    User selectByName(@Param("name") String name);

    User selectByNameAndAge(@Param("name") String name,@Param("age") int age);
    }
    ```
2. **参数注解 Param**
    ```java
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.PARAMETER)
    public @interface Param {
        String value();
    }
    ```
3. **表名注解 Table**
    ```java
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.TYPE)
    public @interface Table {
        String tableName();
    }
    ```
3. **测试**
    ```java
    public class App
    {
        public static void main(String[] args) {
            MySqlSessionFactory mySqlSessionFactory = new MySqlSessionFactory();
            UserMapper mapper = mySqlSessionFactory.getMapper(UserMapper.class);
            User user = mapper.selectUserById(1);
            System.out.println(user);
        }
    }
    ```

    ```Shell
    select id,name,age from user where id =?
    User(id=1, name=测试1, age=10)
    ```

