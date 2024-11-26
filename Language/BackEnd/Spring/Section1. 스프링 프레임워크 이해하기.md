[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

목차

</aside>

---

## 스프링 환경설정하기

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
    

**폴더구성**

```
/src
 ∟ /webapp
   ∟ /view
	   ∟ /index
   ∟ /WEB-INF
	   ∟ /applicationContext.xml
	   ∟ /dispatcher-serlvet.xml
	   ∟ /web.xml
```

**web.xml**

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

**dispatcher-serlvet.xml**

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

**applicationContext.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

</beans>
```

## 스프링의 주요기능

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/d7cf4d35-2396-472e-af24-cbb535a09b65/image.png)

## 스프링 프레임워크 특징

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9b440bf3-bece-4e2a-9b75-ab58f67c1c45/image.png)

- POJO
    - 평범한 자바 구조 → 단순한 자바 클래스 형태로 만드는 것
- 경량컨테이너
    - Maven을 통해 jar파일을 지역저장소에 저장되어 라이브러리만 가지고 있으면 스프링 프로젝트 빌드

### 제어 역행(IoC Inversion of Control)

- 객체의 생성에서 소멸까지를 설정 파일이나 애노테이션을 통해서 컨테이너가 관리
- Dependency Injection(DI)을 통해 모듈간의 의존성 주입
- 스프링이 객체를 만들어서 의존성을 주입해주기 때문에 제어가 역전됨
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/5019046c-99e4-4ead-8bee-0dff42e2c494/828ec33c-ffdc-4016-be9b-1fb4892c55aa.png)
    
- IoC: 작은 부품을 먼저 만들어 큰 부품 생성 시 조립
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f0f50922-15fc-49c4-a888-90d928bc1183/image.png)
    
- DI(Dependency injection)
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/6cc0b5ed-1dac-4d65-b443-b893f7947bc2/image.png)
    
    → 조립형의 경우 **B가 변경되어도 A는 그 사실을 알 필요가 없다.**
    

## Maven

**프로젝트 관리 기능**

- 프로젝트 구조와 내용을 기술하는 선언적 접근 방식의 오픈 소스 빌드 툴
- 종속 라이브러리들과 라이브러리 denpendency 의존성 자원까지 관리 가능
- pom.xml설정: 버전 및 서렂ㅇ, 빌드 환경, 라이브러리 저장소 및 의존성 관리

## 스프링 프레임워크 특징-스프링 이전

### 스프링 이전에는 interface 패턴 이용

interface 개발 코드와 객체가 서로 통신하는 접점 역할 다형성 구현에 중요

### Factory패턴

- 클래스 인스턴스를 만드는 것을 서브클래스에서 결정하도록 함.
- new 키워드를 사용하는 부분을 서브 클래스에 위임함으로써 객체 생성을 캡슐화
- 구상클래스에 대한 의존성이 줄어듦.

**TVFactory**

```java
public class TVFactory {

    public static TVInterface makeTv(String brand) {
        if (brand.equals("sam")) {
            return new SamsungTV("랄랄라");
        } else {
            return new LgTV();
        }
    }
}

```

**TVInteface**

```java
package section1;

public interface TVInterface {
    void turnOn();
    void turnOff();
}
```

**SamsungTV**

```java
package section1;

public class SamsungTV implements TVInterface{

    String model;

    public SamsungTV(String model) {
        this.model = model;
    }

    public void turnOn() {
        System.out.println(getClass().getSimpleName() + "전원을 켠다");
    }

    public void turnOff() {
        System.out.println(getClass().getSimpleName() + "전원을 끈다");
    }
}
```

**TVUser**

```java
public class TvUser {
    public static void main(String[] args) {
        f3();
    }

    private static void f3() {
        //interface(규격서)가 있는 경우
//        TVInterface tv = new SamsungTV();  //사용한다. 의존관계가 있다.
        TVInterface tv = TVFactory.makeTv("sam");
        TVInterface tv2 = TVFactory.makeTv("lg");
        tv.turnOn();
        tv.turnOff();

        tv2.turnOn();
        tv2.turnOff();
    }
```

### Spring적용

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/495b8509-ef0b-447a-bbe7-0bd1b0e5b604/image.png)

- TVUser1 은 TVInterface에 의존
- TVUser1 은 TVInterface에 의존
- TVuser3 은 SpringFramework에 의존하므로 다른 객체에 의존하지 않음.

### 스프링 컨테이너

**Bean**

- 스프링이 IoC방식으로 직접 생성과 제어 담당하는 객체

**BeanFactory**

- 스프링 IoC를 담당하는 핵심컨테이너
- 모바일 관련 어플리케이션 개발 시 사용
- 사용할 떄 만들기 때문에 늦지만, 메모리 효율적으로 사용
- 메모리를 효율적으로 사용해야하는 모바일 개발에 적합

**ApplicationContext**

- BeanFactory를 확장한 IoC 컨테이너. Bean을 등록하고 관리하는 것은 BeanFactory와 동일하며 스프링이 제공하는 각종 부가서비스를 추가로 제공
- 사용되기 전에 빈을 미리 로딩
- 일반 어플리케이션 개발 시 사용됨

- **WebApplicationContext**
    - 웹 개발시 사용

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/8c81c2fa-0b9b-4138-a48b-82e14fe2b389/image.png)

- POJO(Plain Old Java Object): 프레임워크에 의존하지 않고, 필드와 메서드만으로 구성된 단순한 Java 객체이다

### pom.xml설정하기

```java
<properties>
      <project.build.sorceEncoding>UTF-8</project.build.sorceEncoding>
      <maven.compiler.source>11</maven.compiler.source>
      <maven.compiler.target>11</maven.compiler.target>
      <org.springframework-version>5.3.5</org.springframework-version>
  </properties>
```

**<bean>태그에 사용되는 여러 가지는 속성들**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/00fb284f-bdce-4494-a14e-22ca6561d009/image.png)

## IOC와 DI

**Constructor Injection**

의존하는 빈 객체를 클래스를 초기화 할 때 컨테이너로 부터 생성자의 파라미터를 통해서 전달받는 방식

- Index 이용 방법(0부터 시작)
- name 이용 방법

### 스프링 컨테이너를 이용하여 Bean생성

**java Beans 문법**

```java
//POJO(Plain Old Java Object): 프레임워크에 의존하지 않고, 필드와 메서드만으로 구성된 단순한 Java 객체이다
//java bean 문법: field는 private modified, 기본생성자, getter/setter
public class Car {
    private String model;
    private int price;
    
    public Car() {
        System.out.println("Car Default Constructor");
    }

    //constructor overloading
    public Car(String model, int price) {
        System.out.println("Car arg2 constructor");
        this.model = model;
        this.price = price;
    }

    public String getModel() {
        return model;
    }

    public void setModel(String model) {
        this.model = model;
    }

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }
}
```

**Person**

```java
package com.shinhan.mavenProject.section3;

import lombok.*;

//POJO(Plain Old Java Object): 프레임워크에 의존하지 않고, 필드와 메서드만으로 구성된 단순한 Java 객체이다
@NoArgsConstructor
@AllArgsConstructor
@Getter @Setter
@ToString
public class Person {
    String name;
    int age;
    Car car;
}
```

**di3.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--default Constructor 만듦-->
    <bean id="car1" class="com.shinhan.mavenProject.section3.Car"/>

    <!--2 argument Constructor - index-->
    <bean id="car2" class="com.shinhan.mavenProject.section3.Car">
        <constructor-arg index="0" value="ABC모델"/>
        <constructor-arg index="1" value="1000"/>
    </bean>

    <!--2 argument Constructor - name-->
    <bean id="car3" class="com.shinhan.mavenProject.section3.Car">
        <constructor-arg name="model" value="SSS모델"/>
        <constructor-arg name="price" value="2000"/>
    </bean>

    <!--2 argument Constructor - setter-->
    <bean id="car4" class="com.shinhan.mavenProject.section3.Car">
        <property name="model" value="setter injection prop(model)"/>
        <property name="price" value="3000"/>
    </bean>

    <bean id="person1" class="com.shinhan.mavenProject.section3.Person">
        <property name="name" value="evan"/>
        <property name="age" value="32"/>
        <property name="car" ref="car4"/>
    </bean>
</beans>
```

- `arg`는 `index` 또는 `name`을 통해 주입할 수 있다.
- field값이 객체일 경우 `ref=””` 를 사용하여 객체 `id` 주입