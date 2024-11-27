[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

목차

</aside>

---

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

## Spring Injection 방법

### List 또는 배열

**PersonVO**

```java
public class PersonVO {
    String name;
    int age;
    CarVO car;
    List<String> major;
    List<LicenseVO> licenseList;
}
```

**di4.xml**

```java
<bean id="lamborghini URUS" class="com.shinhan.mavenProject.section4.CarVO">
    <property name="model" value="lamborghini"/>
    <property name="price" value="300000000"/>
    <property name="color" value="yellow"/>
</bean>

<bean id="evan" class="com.shinhan.mavenProject.section4.PersonVO">
    <property name="name" value="evan"/>
    <property name="age" value="32"/>
    <property name="car" ref="lamborghini URUS"/>
    <property name="major">
        <list>
            <value>컴공</value>
            <value>경제</value>
        </list>
    </property>
    <property name="licenseList">
        <list>
            <ref bean="license1"/>
            <ref bean="license2"/>
        </list>
    </property>
</bean>
```

### Map, Properties

**PersonVO**

```java
@NoArgsConstructor
@AllArgsConstructor
@Getter @Setter
@ToString
public class PersonVO {
    String name;
    int age;
    CarVO car;
    List<String> major;
    List<LicenseVO> licenseList;
    Map<String, BookVO> bookMap;
    Properties myProfile;
}
```

- properties도 map이다. key, value 중 value가 무조건 문자일 때 사용

**di4.xml**

```java
<property name="bookMap">
        <map>
            <entry>
                <key>
                    <value>내책1</value>
                </key>
                <ref bean="book1"/>
            </entry>
            <entry>
                <key>
                    <value>내책2</value>
                </key>
                <ref bean="book2"/>
            </entry>
        </map>
</property>
<property name="myProfile">
    <props>
        <prop key="email">jamm0316@naver.com</prop>
        <prop key="phone">010-6682-6308</prop>
    </props>
</property>
```

### Set

**PersonVO**

```java
@NoArgsConstructor
@AllArgsConstructor
@Getter @Setter
@ToString
public class PersonVO {
    String name;
    int age;
    CarVO car;
    List<String> major;
    List<LicenseVO> licenseList;
    Map<String, BookVO> bookMap;
    Set<String> friends;
}
```

**di4.xml**

```java
<property name="friends">
    <set>
        <value>대의찬</value>
        <value>갓서희</value>
        <value>킹신영</value>
        <value>왕시현</value>
    </set>
</property>
```

### 의존관계 자동설정

**byName**

- field과 bean의 id가 같으면 그것을 propertis로 주입시킴.

```java
<bean id="person2" class="com.shinhan.mavenProject.section4.PersonVO" autowire="byName">
```

**byType**

- filed와 type이 같으면 bean이 있으면 그것을 properties로 주입시킴.
    
    ```java
    <bean id="person2" class="com.shinhan.mavenProject.section4.PersonVO" autowire="byType">
    ```
    
- 같은 타입의 bean이 2개 이상 있으면 `UnsatisfiedDependencyException` 예외가 뜬다
→ **만족되지 않은 종속성예외**

## Bean 객체의 범위

기본적으로 컨테이너에 한개의 bean 생성

scope속성을 이용하여 범위 설정 가능

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/6b0843d6-3583-410e-9ed8-fec41077f74e/image.png)

**di4.xml**

```java
<bean id="person2" class="com.shinhan.mavenProject.section4.PersonVO" autowire="byName" scope="singleton">
```

## Annotation 기반 설정

**특징**

- 자바 annotation으로 xml설정을 대신함.
- 설정파일 간결화 가능
- 클래스 필드, 메소드를 관리
- 가독성이 좋음

**주요 Annotation**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/b09adf7c-e500-4808-88ed-73efed57def3/image.png)

## Java 기반 설정

Spring 3.0 부터 자바 클래스에서 `@Configuration`, 메서드에 `@Bean`을 사용

**Appconfig**

```java
//section 4까지는 xml설정을 통해서 bean을 생성함.
//section 5부터는 자바 소스를 이용해서 bean생성.
@Configuration  //설정파일이다.
@ComponentScan  //이 파일을 스캔하도록 애노테이션을 달아놓는다.
public class AppConfig {

    @Bean  //<bean id="getCar" class="com.shinhan.mavenProject.CarVO"></bean>
    public CarVO getCar() {
        System.out.println("AppConfig.getCar");
        return new CarVO("lamborghini", 300000000, "yellow");
    }
}
```

**App**

```java
public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);

        ctx.getBean("getCar", CarVO.class);
    }
}
```

---

- 각 애너테이션 사용 설명
    
    ### 1. `@Component`, `@Service`, `@Repository`, `@Bean`에 대한 설명
    
    - **`@Component`**, **`@Service`**, **`@Repository`**
        - **모두 클래스**를 스프링 컨테이너에 빈으로 등록하는 역할을 한다.
        - 이들은 역할에 따라 구분되며, 스프링이 특정 목적으로 빈을 구별하거나 처리할 때 사용된다.
            - `@Component`: 범용적으로 사용되는 애너테이션.
            - `@Service`: 서비스 계층(비즈니스 로직)에 사용.
            - `@Repository`: 데이터 액세스 계층(DAO)에 사용하며, 스프링이 예외를 변환하는 추가 기능을 제공.
    - **`@Bean`**
        - 클래스의 **메서드를 빈으로 등록**하는 데 사용된다.
        - 일반적으로 Java Config 클래스(`@Configuration` 붙은 클래스)에서 사용된다.
        - 즉, 메서드 수준에서 동작하며, 메서드 반환 객체가 스프링 컨테이너에 빈으로 등록된다.
    - **개선된 설명**:
        
        > @Component, @Service, @Repository는 클래스 전체를 빈으로 등록하는 데 사용되고,
        > 
        > 
        > `@Bean`은 **메서드 반환 객체를 빈으로 등록**하는 데 사용된다.
        > 
        > 모두 스프링 컨테이너에 빈을 생성하고 관리하는 역할을 한다.
        > 
    
    ---
    
    ### 2. `@Autowired`에 대한 설명
    
    - **정확한 역할**:
        - `@Autowired`는 **스프링 컨테이너에 등록된 빈을 찾아서 의존성을 주입(DI)하는 역할**을 한다.
        - 이는 **필드, 생성자, 또는 세터 메서드**에 사용 가능하다.
    - **추가 설명**:
        - DI는 필드에 값을 "자동으로 넣어준다"고 표현할 수 있지만, **필드에 직접 값을 넣는 것은 권장되지 않는 방식**이다.
        - **생성자 주입**(constructor injection)을 사용하는 것이 더 권장되는 패턴이다.
    - **필드 주입의 한계**:
        - 테스트가 어렵고, 의존성이 명시적으로 보이지 않아서 코드 가독성이 떨어질 수 있다.
    
    ---
    
    ### 3. 코드 분석과 개선
    
    ### 코드:
    
    ```java
    @Service
    public class DeptService {
    
        @Autowired
        DeptDAO deptDao;
    }
    
    ```
    
    - **설명**:
        1. `@Service`는 `DeptService` 클래스를 빈으로 등록한다.
        2. `@Autowired`는 **스프링 컨테이너에서 `DeptDAO` 타입의 빈을 찾아** `deptDao` 필드에 주입한다.
    - **정확한 표현**:
        - `deptDao`에 주입되는 것은 **빈의 참조(reference)**이다.
        따라서 "빈을 ref로 주입한다"는 표현이 맞다.
    - **권장 방식 (생성자 주입)**:
        
        ```java
        @Service
        public class DeptService {
        
            private final DeptDAO deptDao;
        
            @Autowired
            public DeptService(DeptDAO deptDao) {
                this.deptDao = deptDao;
            }
        }
        
        ```
        
        - 이 방식은 의존성을 더 명확히 표현하고 테스트 가능성을 높여준다.
    
    ---
    
    ### 4. DI에 대한 설명
    
    - **DI (Dependency Injection)**:
        - 등록된 빈을 **다른 빈의 필드, 생성자 매개변수, 또는 세터 메서드에 주입**하는 과정이다.
        - `@Autowired`는 **필드, 생성자, 또는 세터에 DI를 실행하는 도구**이다.
    - **추가로 명확히 할 점**:
        - DI는 **필드에 값을 넣어주는 것만을 의미하지 않는다.**
        생성자나 세터 주입도 모두 DI의 일종이다.
    
    ---
    
    ### 5. 최종 정리 (수정된 설명)
    
    1. *`@Component`, `@Service`, `@Repository`*는 **클래스를 빈으로 등록**하고,
        
        **`@Bean`**은 **메서드 반환 객체를 빈으로 등록**한다.
        
        이는 모두 **빈을 생성하는 역할**을 한다.
        
    2. *`@Autowired`*는 **스프링 컨테이너에서 등록된 빈을 찾아** 해당 필드, 생성자, 또는 세터에 **의존성을 주입(DI)**한다.
    3. DI는 등록된 빈의 **참조를 다른 빈에 연결해주는 과정**이며, 이는 **필드, 생성자, 또는 세터**를 통해 이루어진다.
    4. **코드 분석**:
        
        ```java
        @Service
        public class DeptService {
        
            @Autowired
            DeptDAO deptDao;
        }
        ```
        
        - `@Service`로 `DeptService`를 빈으로 등록하고,
        - `@Autowired`를 통해 `deptDao`라는 `DeptDAO` 타입의 빈을 찾아 **주입**한다.
    
    위 애너테이션을 스캔하려면 아래 코드가 di.xml에 있어야한다.
    
    ```xml
    <context:component-scan base-package="com.shinhan.mavenProject.section6"/>
    ```
    
    ### 결론
    
    기본적으로 네 설명은 거의 맞았고, 다만 **DI가 필드 주입만을 의미하지 않으며 생성자나 세터 주입도 포함된다**는 점을 보완하면 더욱 정확한 이해가 될 거야! 😊