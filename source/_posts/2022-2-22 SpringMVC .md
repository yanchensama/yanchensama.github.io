---
title: SpringMVC
tags:
  - spring
  - springmvc
  - javaweb
  - java
  - web
categories:
- Web
abbrlink: ae0f95e0
date: 2022-02-17 21:52:47
---

##  第一个springMVC程序

>该篇是讲使用maven，tomcat的springmvc程序

pom文件添加springMVC框架和commons-logging（spring依赖）

```xml
 <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.16</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/commons-logging/commons-logging -->
    <dependency>
      <groupId>commons-logging</groupId>
      <artifactId>commons-logging</artifactId>
      <version>1.2</version>
    </dependency>
```

先是在web.xml文件下修改,成使用DispatcherServlet类分发

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.4" xmlns="http://java.sun.com/xml/ns/j2ee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee
http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">

  <servlet>
    <servlet-name>MyFirstServletName</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
<!--      springMVC配置位置参数-->
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:MyFirstServletName-servlet.xml</param-value>
    </init-param>
<!--    启动顺序 ，数字越小，优先级越高-->
    <load-on-startup>1</load-on-startup>
  </servlet>
<!--拦截所有访问-->
  <servlet-mapping>
    <servlet-name>MyFirstServletName</servlet-name>
    <url-pattern>/*</url-pattern>
  </servlet-mapping>
</web-app>
```

然后是在**resources**目录中创建**MyFirstServletName-servlet.xml**(注意后缀名是-servlet.xml)，添加你的控制器文件到beans列表中，springMVC才能找到controller文件开始初始化

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean class="com.yc.controller.MyFirstSpringController"/>
</beans>
```

在**src/java/com.xxx.controller**目录下创建你的controller文件

```java
package com.yc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class MyFirstSpringController {
    @RequestMapping(value = "/hello", method = RequestMethod.GET)
    public @ResponseBody String Hello() {
        return "Hello, SpringMVC.";
    }
}
```

使用Annotation简化，@Controller标记为控制器文件。通过 `@RequestMapping` 和 `@ResponseBody` 我们将一个普通函数标记成**可以处理 GET 请求，同时返回字符串的 Handler。**

使用tomcat访问 .../hello。

