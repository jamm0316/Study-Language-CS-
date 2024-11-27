> [!tip]
> 안보이는 이미지는 아래 링크확인
> https://jamm0316.notion.site/SpringMVC-IntelliJ-14bcda6b86ff8091beb2d3c7c0cc290f?pvs=4

[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

목차

</aside>

---

## 스프링 MVC 환경설정하기

- **Java Development Kit (JDK)**: JDK 11 version
- **Spring Framework**: Spring Framework 5.3.29
- **Spring Web MVC**: Spring Web MVC 5.3.29
- **Lombok Library**: Lombok 1.18.24
- **Apache Tomcat Server**: Tomcat 9.0.96

**참고자료**

https://www.youtube.com/watch?v=YyQq3y8FIHo

- Source Code: xml
    
    https://github.com/jerry10004/SpringProject/tree/master/src/main/webapp/WEB-INF
    
- Maven dependency: spring Framework-webMVC
    
    https://mvnrepository.com/artifact/org.springframework/spring-webmvc
    

### **폴더구성**

```
/src
 ∟ /webapp
   ∟ /views
	   ∟ /index
   ∟ /WEB-INF
	   ∟ /applicationContext.xml
	   ∟ /dispatcher-serlvet.xml
	   ∟ /web.xml
```

### Maven으로 프로젝트 생성

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9dd34c9b-cf9d-4401-bdc5-67e84f19c26c/image.png)

### maven에 spring-webmvc의존성 추가

**pom.xml**

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.29</version>
</dependency>
<dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>2.0.9</version>
</dependency>
```

### web.xml 수정

**WEB-INF/web.xml**

```xml
<!DOCTYPE web-app PUBLIC
        "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
        "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/applicationContext.xml</param-value>
  </context-param>
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

### dispatcher-servlet 추가

**WEB-INF/dispatcher-servlet.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/cache http://www.springframework.org/schema/cache/spring-cache.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <mvc:annotation-driven />
    <context:component-scan base-package="com.example" />
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" >
        <property name="prefix" value="/WEB-INF/views/" />
        <property name="suffix" value=".jsp" />
    </bean>
</beans>
```

### applicationContext 추가

1. web.xml에서 applicationContext → Create file applicationContext.xml → src/main/webapp
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/2b0d39f3-a800-4a19-9b9c-a3a2658251b1/image.png)
    
2. applicationContext.xml 셋팅
    
    **WEB-INF/applicationContext.xml**
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    </beans>
    ```
    

### WEB-INF/views/index.jsp 디렉토리 생성

**WEB-INF/views/index.jsp**

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ page session="false" %>

<html>
<head>
    <meta charset="UTF-8">
</head>
<body>
<h2>Hello World!</h2>
<p>${myName}</p>
<p>${serverTime}</p>
</body>
</html>

```

### Spring 추가 및 LanguageLevel 설정

1. ProjectStructures(`cmd + ;`) → Project Settings → Module → `+` 클릭 → Spring 추가
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/3b0c0112-5e58-467c-822a-179719711943/image.png)
    
2. Language level: 11-Local variable syntax for lamda parameters 설정
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/7856e0e6-f14d-480f-b1aa-32d9e96f4e5c/image.png)
    

### **src/main/java/HomeController 생성**

1. src/main/java 디렉토리 생성
2. com.shinhan.myapp 디렉토리 생성
3. HomeController 생성
    
    **HomeController**
    
    ```java
    package com.shinhan.myapp;
    
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestMethod;
    
    import java.text.DateFormat;
    import java.util.Date;
    import java.util.Locale;
    
    /**
     * Spring은 POJO방식
     */
    @Controller
    public class HomeController {
    
        private static final Logger logger = LoggerFactory.getLogger(HomeController.class);
    
        @RequestMapping(value = "/", method = RequestMethod.GET)
        public String home(Locale locale, Model model) {
            Date date = new Date();
            DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);
    
            String formattedDate = dateFormat.format(date);
            model.addAttribute("serverTime", formattedDate);
            model.addAttribute("myName", "송재명");
    
            return "index";
        }
    }
    ```
    

### 톰캣 설정

1. Edit Configurations
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/40d211b1-6cea-4890-80bb-0d6330e418d7/image.png)
    
2. Add new → Tomcat Server → Local
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/a67873a5-e1f3-4516-914d-ca3cd442b259/image.png)
    
3. Deployment → `+` 클릭 → Artifact → 
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/1f451e2a-5aef-4a31-b72c-e08b220ea0b3/image.png)
    
4. ~:war exploded
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/10112f11-4f19-4efc-8b73-5126a1eb782d/image.png)
    
5. Application context 경로 수정
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/34c5c086-103f-4fbc-a76e-8452b7ae5b54/image.png)
    

### 실행

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f93918b3-a6d6-4d7d-9c0a-b28f437e8fd8/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/92e44261-4399-4384-969d-bb7764ee5162/image.png)

## 다음에 해볼것

https://velog.io/@meong/IntelliJ%EC%97%90%EC%84%9C-Spring-MVC-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0