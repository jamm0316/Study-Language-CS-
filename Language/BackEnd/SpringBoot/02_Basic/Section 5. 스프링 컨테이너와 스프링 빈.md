[<back](https://www.notion.so/Spring-basic-41cfc2c136874d9896aa115923bd4109?pvs=21)

---

## 스프링 컨테이너 생성

스프링 컨테이너 생성 과정

```java
//스프링 컨테이너 생성
ApplicationContext applicationContext =
new AnnotationConfigApplicationContext(AppConifg.class)
```

- `ApplicationContext`를 스프링 컨테이너라 한다.
- `ApplicationContext`는 인터페이스다 → 다형성 적용 가능
- 스프링 컨테이너는 XML을 기반으로 만들 수 있고, 애노테이션 기반의 자바 설정 클래스로 만들 수 있다.
- 직전에 `AppConfig`를 사용했던 방식이 애노테이션 기반의 자바 설정 클래스로 스프링 컨테이너를 만든 것이다.
- 자바 설정 클래스를 기반으로 스프링 컨테이너(`ApplicationContext`)를 만들어보자.
    - `new AnnotataionConfigApplicationContext(AppConfig.class);`
    - 이 클래스는 `ApplicationContext` 인터페이스의 구현체다.

> 참고: 더 정확히는 스프링 컨테이너를 부를 때 `BeanFactory`, `ApplicationContext`로 구분해서 이야기한다. `BeanFactory`를 직접 사용하는 경우는 거의 없으므로 일반적으로 `ApplicaitionContext`를 스프링 컨테이너라고 한다.
> 

### 스프링 컨테이너의 생성 과정

1. **스프링 컨테이너 생성**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/0e4d3574-d88c-43ba-879a-1eff7ea34e10/b1cc3b42-133f-4838-97cd-3195a5bfa068.png)

- `new AnnotationConfigApplicationContext(AppConfig.class)`
- 스프링 컨테이너를 생성할 때는 구성 정보를 지정해주어야 한다.
- 여기서는 `AppConfig.class`를 구성 정보로 지정함.

1. 스프링 빈 등록

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/414a9ffc-8689-4d2c-ac09-e27942fcaf12/d60b6bf1-5517-4951-b111-5eff8f21b5c7.png)

- 스프링 컨테이너는 파라미터로 넘어온 설정 클래스 정보를 사용해서 스프링 빈을 등록한다.

**빈 이름**

- 빈 이름은 메서드 이름을 사용한다.
- 빈 이름을 직접 부여할 수 도 있다.
    - `@Bean(name = “memberService2”)`

> **주의: 빈 이름은 항상 다른 이름 부여**해야한다. 같은 이름을 부여하면, 다른 빈이 무시되거나, 기존 빈을 덮어버리거나 설정에 따라 오류가 발생.
> 

1. **스프링 빈 의존관계 설정 - 준비**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/1e3144f2-3074-46cb-b62a-46c607819f7d/48fb1de3-49d3-43bb-a226-c27d68dd9e7e.png)

1. 스프링 빈 의존관계 설정 - 완료

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9ea819f0-e79b-43c3-9880-e3c8a9a25176/c143098b-599a-4b81-8e80-56aa153d4c53.png)

- 스프링 컨테이너는 설정 정보를 참고해서 의존관계를 주입(DI)한다.
- 단순히 자바 코드를 호출하는 것 같지만, 차이가 있다. 이 차이는 뒤에 싱글톤 컨테이너에서 설명.

**참고**

스프링은 빈을 생성하고, 의존관계를 주입하는 단계가 나누어져 있다. 그런데 이렇게 자바코드로 스프링을 등록하면 생성자를 호출하면서 의존관계 주입도 한번에 처리된다. 여기서 이해를 돕기 위해 개념적으로 나누어 설명했따. 자세한 내용은 의존관계 자동 주입에서 다시 설명.

**정리**

스프링 컨테이너를 생성하고, 설정(구성) 정보를 참고해서 스프링 빈도 등록하고, 의존관계도 설정.

이제 스프링 컨테이너에서 데이터를 조회해보자.

## 컨테이너에 등록된 모든 빈 조회

스프링 컨테이너에 실제 스프링 빈들이 잘 등록 되어있는지 확인

**ApplicationContextInfoTest**

```java
package hello.core.beanfind;

public class ApplicationContextInfoTest {

    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

    @Test
    @DisplayName("모든 빈 출력하기")
    public void findAllBean() {
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            Object bean = ac.getBean(beanDefinitionName);
            System.out.println("name = " + beanDefinitionName + "object = " + bean);
        }
    }

    @Test
    @DisplayName("애플리케이션 빈 출력하기")
    public void findApplicationBean() {
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);

            //Role ROLE_APPLICATION: 직접 등록한 애플리케이션 빈
            //Role ROLE_INFRASTRUCTURE: 스프링 내부에서 사용하는 빈
            if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
                Object bean = ac.getBean(beanDefinitionName);
                System.out.println("name = " + beanDefinitionName + " Object = " + bean);
            }
        }

    }
}
```

- 모든 빈 출력하기
    - 실행하면 스프링에 등록된 모든 빈 정보를 출력할 수 있다.
    - `ac.getBeanDefinitionNames()`: 스프링에 등록된 모든 빈 이름을 조회한다.
    - `ac.getBean()`: 빈 이름으로 빈 객체(인스턴스)를 조회한다.
- 애플리케이션 빈 출력하기
    - 스프링 내부에서 사요하는 빈은 제외하고, 내가 등록한 빈만 출력해보자.
    - 스프링 내부에서 사용하는 빈은 `getRole()`로 구분할 수 있다.
        - `ROLE_APPLICATION`: 일반적으로 상용자가 정의한 빈
        - `ROLE_INFRASTRUCTURE`: 스프링 내부에서 사용하는 빈

## 스프링 빈 조회 - 기본

스프링 컨테이너에서 스프링 빈을 찾는 가장 기본적인 조회 방법

- `ac.getBean(빈 이름, 타입)`
- `ac.getBean(타입)`
- 조회 대상 스프링 빈이 없으면 예외 발생
    - `NoSuchBeanDefinitionException: No bean named ‘xxxx’ available`

**ApplicationContextBasicFindTest**

```java
package hello.core.beanfind;

import hello.core.AppConfig;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import static org.assertj.core.api.Assertions.*;

public class ApplicationContextBasicFindTest {
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

    @Test
    @DisplayName("빈 이름으로 조회")
    public void findBeanByName() {
        MemberService memberService = ac.getBean("memberService", MemberService.class);
        assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
    }

    @Test
    @DisplayName("빈 이름없이 타입으로만 조회")
    public void findBeanByType() {
        MemberService memberService = ac.getBean(MemberService.class);
        assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
    }

    //유연성이 떨어지므로 추천하지 않는 방식
    @Test
    @DisplayName("구체 타입으로 조회")
    public void findBeanByName2() {
        MemberServiceImpl memberService = ac.getBean("memberService", MemberServiceImpl.class);
        assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
    }
```

- 빈 이름이 없는 경우 조회

**ApplicationContextBasicFindTest**

```java
@Test
@DisplayName("빈 이름으로 조회 x")
public void findBeanByNameX() {
    MemberService memberService = ac.getBean("xxxx", MemberService.class);
    assertThat(memberService).isInstanceOf(memberService.getClass());
}
```

**실행결과**

```java
org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'xxxx' available
```

**코드 수정**

```java
@Test
@DisplayName("빈 이름으로 조회 x")
public void findBeanByNameX() {
    assertThrows(NoSuchBeanDefinitionException.class,
            () -> ac.getBean("xxxx", MemberService.class));
}
```

- `bean`이름이 `xxxx`인 `MemberService.class`를 찾았을 때, `NoSuchBeanDefinitionExceoption.class`가 반환되어야 통과.

## 스프링 빈 조회 - 동일한 타입이 둘 이상

- 타입으로 조회 시 같은 타입의 스프링 빈이 두 이상이면 오류가 발생. 이때 빈 이름을 지정.
- `ac.getBeanOfType()`을 사용하면 해당 타입의 모든 빈을 조회할 수 있다.

**ApplicationContextSameBeanFindTest**

```java
package hello.core.beanfind;

public class ApplicationContextSameBeanFindTest {

    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SameBeanConfig.class);

    @Test
    @DisplayName("타입으로 조회 시 같은 타입이 둘 이상 있으면, 중복 오류가 발생한다.")
    void findByTypeDuplicate() {
        MemberRepository bean = ac.getBean(MemberRepository.class);
    }

    @Configuration
    static class SameBeanConfig {
        @Bean
        public MemberRepository memberRepository1() {
            return new MemoryMemberRepository();
        }

        @Bean
        public MemberRepository memberRepository2() {
            return new MemoryMemberRepository();
        }
    }
}
```

**실행결과**

```java
org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean of type 'hello.core.member.MemberRepository' available: expected single matching bean but found 2: memberRepository1,memberRepository2
```

**코드 수정**

```java
package hello.core.beanfind;

public class ApplicationContextSameBeanFindTest {

    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SameBeanConfig.class);

    @Test
    @DisplayName("타입으로 조회 시 같은 타입이 둘 이상 있으면, 중복 오류가 발생한다.")
    void findByTypeDuplicate() {
        **assertThrows(NoUniqueBeanDefinitionException.class,
                () -> ac.getBean(MemberRepository.class));**
    }

    @Configuration
    static class SameBeanConfig {
        @Bean
        public MemberRepository memberRepository1() {
            return new MemoryMemberRepository();
        }

        @Bean
        public MemberRepository memberRepository2() {
            return new MemoryMemberRepository();
        }
    }
}
```

- 특정 타입을 모두 조회하기

**ApplicationContextSameBeanFindTest**

```java
@Test
@DisplayName("특정 타입을 모두 조회하기")
void findAllBeanByType() {
    Map<String, MemberRepository> beansOfType = ac.getBeansOfType(MemberRepository.class);
    for (String key : beansOfType.keySet()) {
        System.out.println("key = " + key + " value = " + beansOfType.get(key));
    }
    System.out.println("beansOfType = " + beansOfType);
    assertThat(beansOfType.size()).isEqualTo(2);
}
```

**실행결과**

```java
key = memberRepository1 value = hello.core.member.MemoryMemberRepository@1583741e
key = memberRepository2 value = hello.core.member.MemoryMemberRepository@5b367418
beansOfType = {memberRepository1=hello.core.member.MemoryMemberRepository@1583741e, memberRepository2=hello.core.member.MemoryMemberRepository@5b367418}
```

## 스프링 빈 조회 - 상속관계

- 부모 타입으로 조회하면, 자식 타입도 함께 조회한다.
- 그래서 모든 자바 객체의 최고 부모인 Object 탑입으로 조회하면, 모든 스프링 빈을 조회한다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/74b3de89-61e5-41c2-a402-5ad9384892f3/b32aee30-cac2-46f1-a05a-72ff99dd57a7.png)

**ApplicationContextExtendsFindTest**

```java
package hello.core.beanfind;

public class ApplicationContextExtendsFindTest {

    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);

    @Test
    @DisplayName("부모 타입으로 조회 시, 자식이 둘 이상 있으면, 중복 오류가 발생한다.")
    void findByBeanByParentTypeDuplicate() {
        assertThrows(NoUniqueBeanDefinitionException.class,
                () -> ac.getBean(DiscountPolicy.class));
    }

    @Test
    @DisplayName("부모 타입으로 조회 시, 자식이 둘 이상 있으면, 빈 이름을 지정하면 된다.")
    void findByBeanByParentTypeBeanName() {
        DiscountPolicy bean = ac.getBean("rateDiscountPolicy", DiscountPolicy.class);
        assertThat(bean).isInstanceOf(DiscountPolicy.class);
    }

    @Test
    @DisplayName("특정 하위 타입으로 조회.")
    void findByBeanBySubType() {
        DiscountPolicy bean = ac.getBean("rateDiscountPolicy", DiscountPolicy.class);
        assertThat(bean).isInstanceOf(RateDiscountPolicy.class);
    }

    @Test
    @DisplayName("부모 타입으로 모두 조회하기.")
    void findByBeanByParentAllType() {
        Map<String, DiscountPolicy> beansOfType = ac.getBeansOfType(DiscountPolicy.class);
        for (String key : beansOfType.keySet()) {
            System.out.println("key = " + key + " value = " + beansOfType.get(key));
        }
        System.out.println("beansOfType = " + beansOfType);
        assertThat(beansOfType.size()).isEqualTo(2);
    }

    @Configuration
    static class TestConfig {
        @Bean
        public DiscountPolicy rateDiscountPolicy() {
            return new RateDiscountPolicy();
        }

        @Bean
        public DiscountPolicy fixDiscountPolicy() {
            return new FIxDiscountPolicy();
        }
    }
}
```

- Object타입으로 모두 조회하기

**ApplicationContextExtendsFindTest**

```java
@Test
@DisplayName("부모 타입 모두 조회하기 - Object")
void findByBeanByObjectAllType() {
    Map<String, Object> beansOfType = ac.getBeansOfType(Object.class);
    for (String key : beansOfType.keySet()) {
        System.out.println("key = " + key + " value = " + beansOfType.get(key));
    }
}
```

**실행결과**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/e922e831-c672-45dd-b5bf-0059d1a79b05/image.png)

- 자바 객체는 모든게 Object 타입이므로 스프링에 등록된 모든 객체 타입이 다 출력된다.

## BeanFactory와 ApplicationContext

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/87a44cfa-df52-48c6-b538-d7edd5eabf13/0205c462-2a2d-4abf-9042-b1a99f560923.png)

**BeanFactory**

- 스프링 컨테이너 최상위 인터페이스.
- 스프링 빈을 관리하고 조회하는 역할을 담당.
- `getBean()`을 제공한다.
- 지금까지 우리가 사용했던 대부분의 기능은 BeanFactory가 제공하는 기능이다.

**ApplicationContext**

- BeanFactory 기능을 모두 상속받아 제공한다.
- 빈을 관리하고 검색하는 기능을 BeanFactory가 제공해주는데, 둘의 차이는?
- 애플리케이션을 개발할 때 빈을 관리하고 조회하는 외에 수많은 부가기능이 필요하다.

**ApplicationContext가 제공하는 부가기능**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/89a78749-b906-4bed-b578-247e20d9dd11/236e6830-1f31-44f7-ba8d-d52877afeb65.png)

- **메시지소스를 활용한 국제화 기능**
    - 한국에서 들어오면 한국어로, 영어권에서 들어오면 영어로 출력
- **환경변수**
    - 로컬(local), 개발(test), 운영(production)등을 구분해서 처리
- **애플리케이션 이벤트**
    - 이벤트를 발행하고 구독하는 모델 편리하게 지원
- **편리한 리소스 조회**
    - 파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회

**정리**

- ApplicationContext는 BeanFactory의 기능을 상속받는다.
- ApplicationContext는 빈 관리기능 + 편리한 부가 기능을 제공한다.
- BeanFactory를 직접 사용할 일은 거의 없다. 부가기능이 포함된 ApplicationContext를 사용한다.
- BeanFactory나 ApplicationContext를 스프링 컨테이너라고 한다.

## 다양한 설정 형식 지원 - 자바 코드, XML

- 스프링 컨테이너는 다양한 형식의 설정 정보를 받아드릴 수 있게 유연하게 설계되어있다.
    - 자바 코드, XML, Groovy 등등

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/02615472-4c17-4bb4-93cf-be45248e6a93/1e2d1579-3192-401e-acb0-0f56cbb49e5e.png)

### 애노테이션 기반 자바 코드 설정 사용

- 지금까지 했던것.
- `new AnnotationConfigApplicationContext(AppConfig.class)`
- `AnnotationConfigApplicationContext` 클래스를 사용하면서 자바 코드로된 설정 정보를 넘기면 된다.

### XML 설정 사용

- 최근에는 스프링 부트를 많이 사용하면서 XML기반 설정은 잘 사용하지 않는다. 아직 많은 레거시 프로젝트 들이 XML로 되어있고, 또 XML을 사용하면 컴파일 없이 빈 설정 정보를 변경할 수 있는 장점도 있으므로 한번쯤 배워두는 것도 괜찮다.
- `GenericXmlApplicationContext` 를 사용하면 `xml` 설정 파일을 넘기게 된다.

`core/src/main/resources/appConfig.xml`

**appConfig.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="memberService" class="hello.core.member.MemberServiceImpl">
        <constructor-arg name="memberRepository" ref="memberRepository"/>
    </bean>

    <bean id="memberRepository" class="hello.core.member.MemoryMemberRepository"/>

    <bean id="orderService" class="hello.core.order.OrderServiceImpl">
        <constructor-arg name="memberRepository" ref="memberRepository"/>
        <constructor-arg name="discountPolicy" ref="discountPolicy"/>
    </bean>

    <bean id="discountPolicy" class="hello.core.discount.RateDiscountPolicy"/>
</beans>
```

XmlAppContext

```java
package hello.core.xml;

public class XmlAppContext {

    ApplicationContext ac = new GenericXmlApplicationContext("appConfig.xml");

    @Test
    void xmlAppContext() {
        MemberService memberService = ac.getBean("memberService", MemberService.class);
        assertThat(memberService).isInstanceOf(MemberService.class);
    }
}
```

- `xml` 기반의 `appConfig.xml` 스프링 설정 정보와 자바 코드로 된 `AppConfig.java` 설정 정보를 비교해보며 거의 비슷하다는 것을 알 수 있다.
- `xml` 기반으로 설정하느 것은 최근에 잘 사용하지 않으므로 자세한 내용은 스프링 공식 레퍼런스 문서 확인
    - https://docs.spring.io/spring-framework/reference/core/beans/basics.html#beans-factory-xml

## 스프링 빈 설정 메타 정보 - Bean Definition

- 스프링은 어떻게 이런 다양한 설정 형식을 지원 하는 것일까? 그 중심에는 `BeanDefinition` 이라는 추상화가 있다.
- 쉽게 이야기해서 역할과 구현을 개념적으로 나는 것이다.
    - XML을 읽어서 `BeanDefinition`을 만들면 된다.
    - 자 바코드를 읽어서 `BeanDefinition`을 만들면 된다
    - 스프링 컨테이너에 자바코드인지 XML인지 몰라도 된다. 오직 `BeanDefinition` 만 알면 된다.
- `Beandefinition`을 빈 설정 메타 정보란다.
    - `@Bean`, `<bean>` 당 각각 하나씩 메타정보가 생성된다.
- 스프링 컨테이너는 이 메타 정보를 기반으로 스프링 빈을 생성 한다

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/e1fbaa11-7fe5-445b-8bd2-28fbe331001c/1b7b693e-f8e2-4c6c-b53a-9d6560cda6a9.png)

**코드 레벨로 조금 더 깊이 있게 들어가보자**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/0f89efb8-a71a-41c4-8c0d-fc2e54729430/a1fa3f2c-4515-4d1e-9df4-51bd5e4e3293.png)

- `AnnotationConfigApplicationContext`는 `AnnotationBeanDefinitionReader`를 사용해서 `AppConfig.class`를 읽고, `BeanDefinition`을 생성한다.
- `GenericXmlApplicationContext`는 `XmlBeanDefinitionReader`를 사용해서 `appConfig.xml` 설정 정보를 읽고 `BeanDefinition`을 생성한다.
- 새로운 형식의 설정 정보가 추가되면, `XxxBeanDefinitionReader`를 만들어서 `BeanDefinition`을 생성하면 된다.

### BeanDefinition 살펴보기

**BeanDefinition 정보**

- BeanClassName: 생성할 빈의 클래스 명(자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)
- factoryBeanName: 팩토리 역할의 빈을 사용할 경우. 예) appConfig
- factoryMethodName: 핀을 생성 할 팩토리 메소드 지정 예)memberService
- Scope: 싱글톤(기본값)
- lazyInit: 스프링 컨테이너를 생성 할 때 빈 생성 하는 것이 아니라 실제 빈을 사용할 때까지 최대한 생성을 지연 처리 하는지 여부
- InitMethodName: 빈을 생성 하고 의존 관계를 적용한 뒤에 호출 되는 초기화 메서드 명
- DestroyMethodName: 빈의 생명 주기가 끝나서 제거하기 직전에 호출 되는 메서드 명
- Constructor arguments, Properties: 의존 관계 주입에서 사용한다(자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)

**BeanDefinitionTest**

```java
package hello.core.beandefinition;

import hello.core.AppConfig;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class BeanDefinitionTest {

    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
//    GenericXmlApplicationContext ac = new GenericXmlApplicationContext("appConfig.xml");

    @Test
    @DisplayName("빈 설정 메타정보 확인")
    void findApplicationBean() {
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);
            if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
                System.out.println("beanDefinitionName = " + beanDefinitionName +
                        "beanDefinition" + beanDefinition);
            }
        }
    }
}

```

**실행결과**

```java
beanDefinitionName = appConfigbeanDefinitionGeneric 
bean: class [hello.core.AppConfig$$SpringCGLIB$$0]; scope=singleton; abstract=false; lazyInit=null; autowireMode=0; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=null; factoryMethodName=null; initMethodNames=nul
l; destroyMethodNames=null

beanDefinitionName = memberServicebeanDefinitionRoot 
bean: class [null]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=appConfig; factoryMethodName=memberService; initMethodNames=null; destroyMethodNames=[(inferred)]; defined in hello.core.AppConfig

beanDefinitionName = memberRepositorybeanDefinitionRoot 
bean: class [hello.core.AppConfig]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=null; factoryMethodName=memberRepository; initMethodNames=null; destroyMethodNames=[(inferred)]; defined in hello.core.AppConfig

beanDefinitionName = orderServicebeanDefinitionRoot 
bean: class [null]; scope=; 
abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=appConfig; factoryMethodName=orderService; initMethodNames=null; destroyMethodNames=[(inferred)]; defined in hello.core.AppConfig

beanDefinitionName = discountPolicybeanDefinitionRoot 
bean: class [hello.core.AppConfig]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=null; factoryMethodName=discountPolicy; initMethodNames=null; destroyMethodNames=[(inferred)]; defined in hello.core.AppConfig
```

**정리**

- BeanDefinition을 직접 생성해서 스프링 컨테이너에 등록할 수 도 있다. 하지만 실무에서 BeanDefinition을 직접 정의하거나 사용할 일은 거의 없다.
- BeanDefinition에 대해서는 너무 깊이있게 이해하기 보다는, 스프링이 다양한 형태의 설정 정보를 BeanDefinition으로 추상화 해서 사용하는 것 정도만 이해.
- 가끔 스프링 코드나 스프링 관련 오픈 소스의 코드를 볼 때, BeanDefinition이라는 것이 보일 때가 있다. 이 때 이러한 메커니즘을 떠올리면 된다.