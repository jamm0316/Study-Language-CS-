[<back](https://www.notion.so/Spring-basic-41cfc2c136874d9896aa115923bd4109?pvs=21)

---

<aside>
📃

Contents

</aside>

---

## 다양한 의존관계 주입 방법

의존관계 주입은 크게 4가지 방법이 있다.

- 생성자 주입
- 수정자 주입(setter 주입)
- 필드 주입
- 일반 메서드 주입

### 생성자 주입

- 이름 그대로 생성자를 통해서 의존관계를 주입받는 방법.
- 특징
    - 생성자 호출 시점에 딱 1번만 호출되는 것이 보장된다.
    - **불변, 필수** 의존관계에 사용

**OrderServiceImpl**

```java
@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
```

**중요! 생성자가 딱 1개만 있다면 @Autowired가 없어도 의존관계가 자동 주입 된다.**

**OrderServiceImpl → @Autowired 미포함**

```java
@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        System.out.println("memberRepository = " + memberRepository);
        System.out.println("discountPolicy = " + discountPolicy);
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
```

**OrderServiceImpl → @Autowired 포함**

```java
@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        System.out.println("memberRepository = " + memberRepository);
        System.out.println("discountPolicy = " + discountPolicy);
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
```

실행결과

```java
memberRepository = hello.core.member.MemoryMemberRepository@5ba88be8
discountPolicy = hello.core.discount.RateDiscountPolicy@56928307
```

### 수정자 주입(setter 주입)

- setter라 불리는 필드의 값을 변경하는 수정자 메서드를 통해서 의존관계를 주입하는 방법.
- 특징
    - **선택, 변경** 가능성이 있는 의존관계에 사용
    - `required = false` 로 **선택여부** 설정 가능
        
        ```java
        @Autowired(required = false)
        public void setMemberRepository(MemberRepository memberRepository) {
        ```
        
    - 자바빈 프로퍼티 규약의 수정자 메서드 방식을 사용하는 방법.

```java
@Component
public class OrderServiceImpl implements OrderService {

    private MemberRepository memberRepository;
    private DiscountPolicy discountPolicy;

    @Autowired
    public void setMemberRepository(MemberRepository memberRepository) {
        System.out.println("memberRepository = " + memberRepository);
        this.memberRepository = memberRepository;
    }

    @Autowired
    public void setDiscountPolicy(DiscountPolicy discountPolicy) {
        System.out.println("discountPolicy = " + discountPolicy);
        this.discountPolicy = discountPolicy;
```

**실행결과**

```java
memberRepository = hello.core.member.MemoryMemberRepository@5ba88be8
discountPolicy = hello.core.discount.RateDiscountPolicy@56928307
```

- **의존관계 주입 순서**
    - 생성자 등록 → 수정자(setter) 등록
    
    ```java
    @Component
    public class OrderServiceImpl implements OrderService {
    
        private MemberRepository memberRepository;
        private DiscountPolicy discountPolicy;
    
        @Autowired
        public void setMemberRepository(MemberRepository memberRepository) {
            System.out.println("memberRepository = " + memberRepository);
            this.memberRepository = memberRepository;
        }
    
        @Autowired
        public void setDiscountPolicy(DiscountPolicy discountPolicy) {
            System.out.println("discountPolicy = " + discountPolicy);
            this.discountPolicy = discountPolicy;
        }
    
        @Autowired
        public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
            System.out.println("1. OrderServiceImpl.OrderServiceImpl");
            this.memberRepository = memberRepository;
            this.discountPolicy = discountPolicy;
        }
    ```
    

**실행결과**

```java
1. OrderServiceImpl.OrderServiceImpl
memberRepository = hello.core.member.MemoryMemberRepository@42039326
discountPolicy = hello.core.discount.RateDiscountPolicy@4f9a2c08
```

> 참고: `@Autowired`의 기본 동작은 주입할 대상이 없으면 오류가 발생한다. 주입할 대상이 없어도 동작하게 하려면 `@Autowired(required = false)`로 지정하면 된다.
> 

> 참고: 자바빈 프로퍼티, 자바에서 과거부터 필드의 값을 직접 변경하지 않고 setXxx, getXxx 라는 메서드를 통해서 값을 읽거나 수정하는 규칙을 만들었는데, 그것이 자바빈 프로퍼티 규약이다. 더 자세한 내용은 자바빈 프로퍼티로 검색해보자.
> 

- **자바빈 프로퍼티 규약**
    
    자바빈(JavaBean) 프로퍼티 규약(JavaBean Property Convention)은 자바에서 객체의 속성(프로퍼티)을 다루는 표준 규칙을 정의하는 방식입니다. 이 규약은 주로 JavaBeans라는 객체를 사용하여 다양한 Java API 및 프레임워크에서 데이터나 속성을 직렬화하고 직렬화된 데이터를 교환하는 데 사용됩니다. 특히 스프링(Spring) 프레임워크나 JSP, JSF 같은 기술에서 많이 사용됩니다.
    
    JavaBean 프로퍼티 규약에는 몇 가지 주요 요소가 포함됩니다:
    
    ### 1. **기본 생성자**
    
    - JavaBean 클래스는 **기본 생성자**(매개변수가 없는 생성자)를 제공해야 합니다.
    - 이는 객체를 인스턴스화할 때 특정 생성자를 호출하지 않고 기본적으로 사용할 수 있도록 합니다.
    
    ```java
    public class Person {
        public Person() {
            // 기본 생성자
        }
    }
    
    ```
    
    ### 2. **프라이빗 필드**
    
    - JavaBean 클래스의 속성(필드)은 **private**로 선언되어야 합니다. 이는 외부에서 직접 접근하는 것을 방지하고, **getter**와 **setter** 메서드를 통해 접근하게끔 하기 위함입니다.
    
    ```java
    public class Person {
        private String name;
    }
    
    ```
    
    ### 3. **Getter 및 Setter 메서드**
    
    - 각 속성에 대해 **getter**와 **setter** 메서드를 제공해야 합니다. 이때 메서드 이름은 속성 이름을 기반으로 정해지며, 메서드명 규칙은 다음과 같습니다:
        - Getter 메서드: `getPropertyName()` 또는 `isPropertyName()` (boolean 타입일 경우 `is` 사용)
        - Setter 메서드: `setPropertyName()`
    
    ```java
    public class Person {
        private String name;
    
        public String getName() {   // Getter
            return name;
        }
    
        public void setName(String name) {   // Setter
            this.name = name;
        }
    }
    
    ```
    
    ### 4. **직렬화 가능성**
    
    - JavaBean 클래스는 직렬화가 가능해야 하기 때문에 `Serializable` 인터페이스를 구현해야 합니다. 이는 객체를 파일로 저장하거나 네트워크를 통해 전송할 수 있도록 하기 위함입니다.
    
    ```java
    import java.io.Serializable;
    
    public class Person implements Serializable {
        private String name;
        private int age;
    
        public Person() {
            // 기본 생성자
        }
    
        public String getName() { return name; }
    
        public void setName(String name) { this.name = name; }
    
        public int getAge() { return age; }
    
        public void setAge(int age) { this.age = age; }
    }
    
    ```
    
    ### 요약
    
    JavaBean 프로퍼티 규약은 다음과 같은 규칙을 따른 클래스 설계를 요구합니다:
    
    - 기본 생성자를 제공해야 한다.
    - 속성(필드)은 `private`이어야 한다.
    - 각 속성에 대해 표준적인 getter/setter 메서드를 제공해야 한다.
    - 클래스는 `Serializable` 인터페이스를 구현해야 한다.
    
    **a.** JavaBean 클래스에 대한 기본 예시 코드를 작성해 볼까요?
    
    **b.** 특정 JavaBean을 만들고 그걸 기반으로 기능을 추가해보고 싶으신가요?
    

**자바빈 프로퍼티 규약 예시**

```java
class Data {
    private int age;
    
    public void setAge(int age) {
        this.age = age;
    }
    
    public void getAge() {
        return age;
    }
}
```

### 필드 주입

- 이름 그대로 필드에 바로 주입하는 방법
- 특징
    - 코드가 간결하지만 외부에서 변경이 불가능해서 테스트하기 힘들다는 치명적 단점이 있다.
    - DI 컨테이너가 없으면 아무것도 할 수 없다.
    - 사용하지 말자!
        - 애플리케이션의 실제 코드와 관계 없는 테스트 코드
        - 스프링 설정을 목적으로 하는 `@Configuration` 같은 곳에만 특별한 용도로 사용

**OrderServiceImpl**

```java
@Component
public class OrderServiceImpl implements OrderService {

    @Autowired private MemberRepository memberRepository;
    @Autowired private DiscountPolicy discountPolicy;
```

**AutoAppConfigTest**

```java
@Test
void basicScan() {
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AutoConfig.class);
    MemberService memberService = ac.getBean(MemberService.class);

    assertThat(memberService).isInstanceOf(MemberService.class);

    OrderServiceImpl orderService = ac.getBean(OrderServiceImpl.class);
    MemberRepository memberRepository = orderService.getMemberRepository();
    System.out.println("memberRepository = " + memberRepository);
}
```

**실행결과**

```java
memberRepository = hello.core.member.MemoryMemberRepository@628c4ac0
```

- `new OrderServiceImpl` 생성 시, `@Autowired` 애노테이션이 붙은 필드는 Spring 컨테이너가 관리하는 빈에만 의존성이 주입된다.
- test 환경에서는 수동으로 `new OrderServiceImpl`를 생성해주어야하는데, 이때는 멤버 변수가 `private`로 막혀있기 때문에 수동으로 의존성을 주입할 수 없다.

**OrderServiceTest**

```java
@Test
void fieldInjectionTest() {
    OrderServiceImpl orderService = new OrderServiceImpl();  //인터페이스 생성 불가
    orderService.createOrder(1L, "itemA", 10000);
}
```

실행결과

```java
java.lang.NullPointException
```

### 일반 메서드 주입

- 일반 메서드를 통해서 주입을 받을 수 있다.
- 특징
    - 한번에 여러 필드를 주입 받을 수 있다.
    - 일반적으로 잘 사용하지 않는다.
    
    ```java
    @Component
    public class OrderServiceImpl implements OrderService {
    
        private MemberRepository memberRepository;
        private DiscountPolicy discountPolicy;
    
        public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
            this.memberRepository = memberRepository;
            this.discountPolicy = discountPolicy;
        }
    
        @Autowired
        public void init(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
            this.memberRepository = memberRepository;
            this.discountPolicy = discountPolicy;
        }
    ```
    

> 참고: 의존관계 자동 주입은 스프링 컨테이너가 관리하는 스프링 빈이어야 동작한다. 스프링 빈이 아닌 `Member` 같은 클래스에서 `@Autowired` 코드를 적용해도 아무 기능도 동작하지 않는다.
> 

- Test 수정
    - 전체 테스트 실행 시 아래와 같은 오류 발생
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f8f841aa-8bf4-4fac-9400-5d3058123cea/image.png)
    
    하나씩 살펴 보자.
    
    1. **ConfigurationSingletonTest**
        1. 오류 메세지 → `OderServiceImpl`의 타입이 실제로 `NullBean`이 반환된다.
            
            ```java
            org.springframework.beans.factory.BeanNotOfRequiredTypeException: Bean named 'orderService' is expected to be of type 'hello.core.order.OrderServiceImpl' but was actually of type 'org.springframework.beans.factory.support.**NullBean**'
            
            	at org.springframework.beans.factory.support.AbstractBeanFactory.adaptBeanInstance(AbstractBeanFactory.java:422)
            	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:403)
            	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:205)
            	at org.springframework.context.support.AbstractApplicationContext.getBean(AbstractApplicationContext.java:1249)
            	**at hello.core.singleton.ConfigurationSingletonTest.configurationTest(ConfigurationSingletonTest.java:20)**
            ```
            
        2. **ConfigurationSingletonTest**
            
            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/94a5f709-c942-41c1-bfa6-4cf6ab0d89bd/image.png)
            
            - `OrderServiceImpl orderService = ac.getBean(”orderService”, OrderServiceImpl.class);`
            위 코드에서 Bean을 가져오는데 오류가 생김
            - 위 Bean은 어디서 생성하느냐?
            → `ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);`
        3. **AppConfig(오류 발견!!)**
            
            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/abf2be67-abc8-4b06-914e-bff8062bb4ae/image.png)
            
            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/2243996a-a9d9-48d9-a92d-9d0beb09d6d7/image.png)
            
            - **해결됨!!**
                
                ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/5108c599-bfc6-4a4e-99f0-bb3bc2730d7d/image.png)
                
    
    1. **CoreApplicationTest**
        1. 오류메세지 → BeanDefinitionOverrideException이 터졌다. → memoryMemberRepository이 유효하지 않고 그 클래스 리소스 주소는 AutoConfig.class다.
            
            ```java
            Caused by: org.springframework.beans.factory.support.BeanDefinitionOverrideException: **Invalid bean definition with name 'memoryMemberRepository' defined in class path resource [hello/core/AutoConfig.class]**: Cannot register bean definition [Root bean: class [null]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=autoConfig; factoryMethodName=memberRepository; initMethodNames=null; destroyMethodNames=[(inferred)]; defined in class path resource [hello/core/AutoConfig.class]] for bean 'memoryMemberRepository' since there is already [Generic bean: class [hello.core.member.MemoryMemberRepository]; scope=singleton; abstract=false; lazyInit=null; autowireMode=0; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=null; factoryMethodName=null; initMethodNames=null; destroyMethodNames=null; defined in file [/Users/evan/Documents/Developer/03_Study/02_Spring/03_basic/core/out/production/classes/hello/core/member/MemoryMemberRepository.class]] bound.
            ```
            
        2. **AutoConfig → 오류 해결!!**
            
            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/ea27d919-56a6-4a8b-b735-e3c9d8acdb8b/image.png)
            
            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/fbfdf2a4-8259-49f8-b558-ec56bd6b556f/image.png)
            
            - 수동 빈 등록 충돌 테스트를 할 때 빈 이름을 자동으로 등록했던 테스트
    2. **그러나 또 오류 발생**
        
        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/12a748b5-1595-4237-b011-eef51d788a50/image.png)
        
        - 이번 오류 메세지도 한번 보자
        → MemberServiceImpl을 생성하는데 스프링 빈에 memoryMemberRepository와 memberRepository라는 2개의 빈이 등록되어 충돌되고 있다고 한다.
            
            ```java
            Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'memberServiceImpl' defined in file [/Users/evan/Documents/Developer/03_Study/02_Spring/03_basic/core/out/production/classes/hello/core/member/MemberServiceImpl.class]: Unsatisfied dependency expressed through constructor parameter 0: No qualifying bean of type 'hello.core.member.MemberRepository' available: expected single matching bean but found 2: memoryMemberRepository,memberRepository
            
            Caused by: org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean of type 'hello.core.member.MemberRepository' available: expected single matching bean but found 2: memoryMemberRepository,memberRepository
            ```
            
        - 그래서 각각의 빈이 어디서 등록되었는지 찾아보았다.
            - 하나는 `MemoryMemberRepository`에 등록된 `@Component`였고, 나머지 하나는 `AppConfig`에 등록된 `@Bean`이였다.
                
                **테스트를 위한 과정**
                
                1. 문제가 있는 부분 이름 바꾸기
                    1. AppConfig에 등록된 멤버 리포지토리
                        
                        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/55ce64c5-6266-44bc-aedd-c8daa26da4d8/image.png)
                        
                    2. 메모리 멤버 리포지토리 클래스의 컴포넌트
                        
                        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/183b1790-a1a7-45cf-9126-885957242911/image.png)
                        
                2. 테스트 결과 다시 보기
                    
                    ```java
                    Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'memberServiceImpl' defined in file [/Users/evan/Documents/Developer/03_Study/02_Spring/03_basic/core/out/production/classes/hello/core/member/MemberServiceImpl.class]: Unsatisfied dependency expressed through constructor parameter 0: No qualifying bean of type 'hello.core.member.MemberRepository' available: expected single matching bean but found 2: 메모리_멤버_리포지토리,컨피그_멤버_리포지토리
                    
                    Caused by: org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean of type 'hello.core.member.MemberRepository' available: expected single matching bean but found 2: 메모리_멤버_리포지토리,컨피그_멤버_리포지토리
                    ```
                    
            - 그렇다면, `ComponentScan`에 의해 `@Component(memoryMemberRepository)` 와 `AppConfig`의 `@Bean` 으로 등록된 `memberRepository` 가 충돌한다는 뜻이다.
            - 이것을 막기위해 `AutoConfig` 에 `Component.class`는 스캔에서 제외하는 필터를 걸어놨다. 그러나 이게 실행되지 않은 듯 하다. 이유는 무엇일까?
            - `CoreApplicationTest` 클래스를 보면 `@SpringBootTest` 이라는 애너테이션이 붙어있다.
            이것은 `@SpringBootApplication` 애너테이션을 찾아 위 환경을 테스트 하겠다는 것이다.
            그리고 문제는 여기 있었다.
            - `@SpringBootApplication`은 자체적으로 `@ComponentScan` 애너테이션을 들고있다.
            **여기에는 `@Configuration` 애너테이션이 붙은 클래스를 제외하겠다는 `excludeFilter`가 포함되어있지 않았다.**
                
                ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/a31871d2-53bf-44af-8c68-57cd7310768e/image.png)
                
            
            - **따라서, `AppConfig` 클래스가 함께 불러와진 것이다.!!!!**
            - 결국, `CoreApplication`클래스에 아래와 같이 `excludeFilter`를 걸어주면 해결!!!
                - `@Configuration` 을 제외한 모든 `@Component`를 스프링 빈에 등록한다. 따라서 중복을 없앨 수 있다.
                
                ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/32532f16-acee-4adc-8033-9183306e6980/image.png)
                
                **해결!!!!**
                
                ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9a6f4a0a-3d60-4724-b284-441b5c274cae/image.png)
                
    

## 옵션처리

주입할 스프링 빈이 없어도 동작해야 할 때가 있다.

그런데 `@Autowired`만 사용하면 `required` 옵션의 기본값이 `true`로 되어 있어서 자동 주입 대상이 없으면 오류가 발생한다.

자동 주입 대상을 옵션으로 처리하는 방법은 다음과 같다.

- `@Autowired(required = false)`: 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출 안됨.
- `org.springframework.lang.@Nullable`: 자동 주입할 대상이 없으면 `null`이 입력된다.
- `Optional<>:` 자동 주입할 대상이 없으면 `Optional.empty`가 입력됨.

**AutowiredOptionalTest**

```java
package hello.core.Autowired;

import hello.core.member.Member;
import jakarta.annotation.Nullable;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import java.util.Optional;

public class AutowiredOptionalTest {

    @Test
    void AutowiredOption() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(TestBean.class);
    }

    static class TestBean{
        @Autowired(required = false)  // 호출 자체가 안됨
        public void setBean1(Member noBean1) {
            System.out.println("noBean1 = " + noBean1);
        }

        @Autowired
        public void setBean2(@Nullable Member noBean2) {  //null 값으로 호출됨
            System.out.println("noBean2 = " + noBean2);
        }

        @Autowired
        public void setBean3(Optional<Member> noBean3) {  //optional으로 감싸져서 호출됨
            System.out.println("noBean3 = " + noBean3);
        }
    }
}
```

- **Member는 스프링 빈이 아니다.**
- `setNoBean1()`은 `@Autowired(required = false)` 이므로 호출 자체가 안된다.

**실행결과**

```java
noBean2 = null
noBean3 = Optional.empty
```

> 참고: @Nullable, Optional은 스프링 전반에 걸쳐서 지원된다. 예를 들어서 생성자 자동 주입에서 특정 필드에만 사용해도 된다.
> 

## 생성자 주입을 선택하라!

과거에는 수정자 주입과 필드 주입을 많이 사용했지만, 최근에는 스프링을 포함한 DI 프레임워크 대부분이 생성자 주입을 권장한다. 그 이유는 다음과 같다.

1. **불변**
- 대부분의 의존관계 주입은 한번 일어나면 애플리케이션 종료시점까지 의존관계를 변경할 일이 없다. 오히려 대부분의 의존관계는 애플리케이션 종료 전까지 변하면 안된다.(불변해야 한다.)
- 수정자 주입을 사용하면 setXxx 메서드를 public으로 열어두어야 한다.
- 누군가 실수로 변경할 수 도 있고, 변경하면 안되는 메서드를 열어두는 것은 좋은 설계방법이 아니다.
- 생성자 주입은 객체를 생성할 때 딱 1번만 호출되므로 이후 호출될 일이 없다. 따라서 불변하게 설계 가능.

1. **누락**
- 프레임워크 없이 순수한 자바 코드는 단위 테스트 하는 경우에 다음과 같이 **수정자 의존관계인 경우 런타임 오류**가 나고 **생성자 주입의 경우 컴파일 오류**를 낼 수 있다.
- **final** 키워드를 사용할 수 있다.
    - **final 키워드 사용의 장점**
        - 멤버 변수로 `final` 선언 시, 생성자에서 누락하면 컴파일 오류가 난다.
        - `private final` 키워드를 사용하면, `private`를 통해 외부 접근을 막고, `final`을 통해 생성자 초기화 시, 정의 된 이후 변경을 방지 하여, 객체 생성의 일관성과 불변성을 유지할 수 있다.
            
            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f76ca203-be5a-4ece-ac46-9b6665eeb601/image.png)
            
        
        > 참고: 수정자 주입을 포함한 나머지 주입 방식은 모두 생성자 이후에 호출되므로, 필드에 `final` 키워드를 사용할 수 없다. 오직 생성자 주입 방식만 `final` 키워드를 사용할 수 있다.
        > 

**컴파일 오류와 런타임 오류 확인**

**OrderServiceImpl**

- 생성자 주입의 경우

```java
package hello.core.order;

import com.sun.source.tree.UsesTree;
import hello.core.discount.DiscountPolicy;
import hello.core.member.Member;
import hello.core.member.MemberRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;

@Component
public class OrderServiceImpl implements OrderService {

    private MemberRepository memberRepository;
    private DiscountPolicy discountPolicy;

    @Autowired
    public void setMemberRepository(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Autowired
    public void setDiscountPolicy(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }

...

    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int discountPrice = discountPolicy.discount(member, itemPrice);

        return new Order(memberId, itemName, itemPrice, discountPrice);
    }
}

```

**OrderServiceImplTest**

```java
package hello.core.order;

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

class OrderServiceImplTest {

    @Test
    void createOrder() {
        OrderServiceImpl orderService = new OrderServiceImpl();
        orderService.createOrder(1L, "itemA", 10000);
    }
}
```

**실행결과**

```java
java.lang.NullPointerException: Cannot invoke "hello.core.member.MemberRepository.findById(java.lang.Long)" because "this.memberRepository" is null
```

- 생성자 주입의 경우

**OrderServiceImpl**

```java
@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }

    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int discountPrice = discountPolicy.discount(member, itemPrice);

        return new Order(memberId, itemName, itemPrice, discountPrice);
    }
}
```

**실행결과**

```java
java: constructor OrderServiceImpl in class hello.core.order.OrderServiceImpl cannot be applied to given types;
```

<aside>
💡

**위 오류메시지를 통해 알 수 있는것!!**

**컴파일 오류**의 경우 런타임 오류 보다 더욱 **정확하게 문제점을 파악**할 수 있어, **정확하고 빠른 디버깅이 가능**하다.

---

**런타임 오류**의 경우 `NullPointerException` 오류를 발생시켜, `OrderServiceImpl.createOrder` 메서드에서 오류가 발생했는지, `MemberRepository.findById` **메서드에서 오류가 발생**했는지, **생성자 주입에서 문제가 발생**했는지 **일일히 찾아보아야한다.**

**컴파일 오류**의 경우 `Constructor OrderServiceImpl cannot be applied to given types` 오류를 발생시켜, 생성자 `OrderServiceImpl`은 주어진 인수 타입으로 적용될 수 없다는 것을 알 수 있음.

따라서, **생성자에 주입에 문제가 있음을 바로 알 수 있음.**

</aside>

- **런타임 오류와 컴파일 오류**
    - **런타임 오류**: 프로그램이 실행되는 도중 발생하는 오류, 컴파일은 정상적으로 진행되었으나, 실행시점에서 예상치 못한 오류가 발견되었을 때 발생
    - **컴파일 오류**: 코드 작성 단계에서, 컴파일러가 문법적, 구조적 오류를 발견하여 컴파일을 실패시키는 오류
- **런타임 오류의 경우(수정자 주입)**
    - **오류메시지**
        
        ```java
        java.lang.**NullPointerException**: Cannot invoke "hello.core.member.MemberRepository.findById(java.lang.Long)" because **"this.memberRepository" is null**
        ```
        
    - **개발자가 얻을 수 있는 단서**
        1. `NullPointerException`이 발생했다는 것은 프로그램 실행 중 null 값을 참조하려고 했다는 것을 의미함.
        2. 오류 메시지에서 `“this.memberRepository” is null` 이라는 부분을 통해 `memberRepository` 필드가 초기화되지 않았음을 알 수 있음.
        3. 그러나 왜 `memberRepository`가 `null`인지, 어떤 부분에서 초기화가 누락되었는지 즉시 알기 어려움.
        4. 문제의 근원이 객체 생성시점이 아니라, 해당 메서드 호출 시점에서야 드러나므로 디버깅 범위가 넓어짐.

- **컴파일 오류의 경우(생성자 주입)**
    - **오류메시지**
        
        ```java
        java: constructor OrderServiceImpl in class hello.core.order.OrderServiceImpl cannot be applied to given types;
        ```
        
    - **개발자가 얻을 수 있는 단서**
        1. 컴파일러가 `OrderServiceImpl` 클래스의 생성자가 제공된 인수로는 호출될 수 없음을 알려줌.
        2. 즉, `OrderServiceImpl`의 생성자가 특정한 인자를 필요로 하지만, 인자가 제공되지 않았다는 것을 의미.
        3. 오류는 컴파일 단계에서 발생하므로, 코드 작성 시점에서 문제를 바로 인지할 수 있음.
        4. 개발자는 생성자 파라미터를 확인하고, 필요한 의존성을 주입하지 않았음을 빠르게 알아챌 수 있다.

위 오류를 통해 Test코드 수정

**OrderServiceImplTest**

```java
class OrderServiceImplTest {

    @Test
    void createOrder() {
        //given
        MemoryMemberRepository memberRepository = new MemoryMemberRepository();
        memberRepository.save(new Member(1L, "evan", Grade.VIP));

        //when
        OrderServiceImpl orderService = new OrderServiceImpl(memberRepository, new FIxDiscountPolicy());
        Order order = orderService.createOrder(1L, "itemA", 10000);
        
        //then
        Assertions.assertThat(order.getDiscountPrice()).isEqualTo(1000);
    }
}
```

**정리**

- 생성자 주입 방식을 선택하는 이유는 여러가지가 있지만, 프레임워크에 의존하지 않고, 순수한 자바 언어의 특징을 잘 살리는 방법이기도 하다.
- 기본적으로 생성자 주입을 사용하고, 필수 값이 아닌 경우 수정자 주입 방식을 옵션으로 부여하면 된다. 생성자 주입과 수정자 주입을 동시에 사용할 수 있다.
- 항상 생성자 주입을 선택하라! 그리고 가끔 옵셔이 필요하면 수정자 주입을 선택하라! 필드 주입은 사용하지 않는게 좋다.

## 롬복과 최신 트렌드

막상 개발을 해보면, 대부분이 불변이고 그래서 다음과 같이 생성자에 final 키워드를 사용하게 된다.

**그런데 생성자도 만들어야하고, 주입 받는 값을 대입하는 코드도 만들어야하고…**

필드 주입처럼 좀 편리하게 사용하는 방법은 없을까해서 **롬복**을 사용하게 됨.

[롬복 설치 방법](https://www.notion.so/lombok-106cda6b86ff802bab89f9a155928ef6?pvs=21) ([lombok 설치 방법](https://www.notion.so/lombok-106cda6b86ff802bab89f9a155928ef6?pvs=21))

`lombok` 사용 예

HelloLombok

```java
package hello.core.order;

import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
public class helloLombok {

    private String name;
    private int age;

    public static void main(String[] args) {
        helloLombok helloLombok = new helloLombok();
        helloLombok.setName("evan");

        String name = helloLombok.getName();
        System.out.println("name = " + name);
        System.out.println("helloLombok = " + helloLombok);
    }
}
```

**실행결과**

```java
name = evan
helloLombok = helloLombok(name=evan, age=0)
```

기존 코드

**OrderServiceImpl**

```java
@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
```

위 코드의 경우 코드로 작성하여, **생성자 주입 Constructor** 메서드가 존재
(`cmd` + `f12` 입력시 해당 클래스의 멤버변수, 메서드 모두 볼 수 있음)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/4557017b-a25d-473b-820e-4e7949eae2b4/e86af6f6-2ccc-42bc-bcb1-91dd55de9512.png)

**수정된 코드**

**OrderServiceImpl**

```java
@Component
@RequiredArgsConstructor
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;
```

위 경우에도 생성자 주입 Contructor 메서드 존재.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/515ce85f-e225-4606-8020-00a4172b5e26/1ddfa616-f108-4173-bbac-2c614a440503.png)

- lombok 라이브러리가 제공하는 `@RequiredArgsConstructor` 애노테이션을 사용하면, final이 붙은 필드를 모아서 생성자를 자동으로 만들어 준다.

이 최종결과 코드와 이전 코드는 완전히 동일하다. lombok이 자바의 애노테이션 프로세서라는 기능을 이용해서 컴파일 시점에 생성자 코드를 자동으로 생성해준다. 실제 class를 열어보면 다음 코드가 추가되었은 것을 확인할 수 있다.

`core/out/production/classes/hello/core/order/helloLombok.class`

**helloLombok**

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//

package hello.core.order;

import lombok.Generated;

public class helloLombok {
    private String name;
    private int age;

    public helloLombok() {
    }

    public static void main(String[] args) {
        helloLombok helloLombok = new helloLombok();
        helloLombok.setName("evan");
        String name = helloLombok.getName();
        System.out.println("name = " + name);
        System.out.println("helloLombok = " + helloLombok);
    }

    @Generated
    public String getName() {
        return this.name;
    }

    @Generated
    public int getAge() {
        return this.age;
    }

    @Generated
    public void setName(String name) {
        this.name = name;
    }

    @Generated
    public void setAge(int age) {
        this.age = age;
    }

    @Generated
    public String toString() {
        String var10000 = this.getName();
        return "helloLombok(name=" + var10000 + ", age=" + this.getAge() + ")";
    }
}
```

**정리**

최근에는 생성자를 딱 1개 두고, `@Autowired`를 생략하는 방법을 주로 사용한다. 여기에 Lombok 라이브러리의 `@RequiredArgsConstructor` 함께 사용하면 기능은 다 제공하면서, 코드는 깔끔하게 사용할 수 있다.

## 조회 빈이 2개이상 - 문제

@Autowired 는 타입(Type)을 조회하는 것과 똑같다.

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy)
```

타입으로 조회하기 때문에, 마치 다음 코드와 유사하게 동작한다.(실제로는 더많은 기능을 제공한다.)

`ac.getBean(DiscountPolicy.class)`

스프링 빈 조회에서 학습했듯이 조회하면 선택된 빈이 2개 이상일 때 문제가 발생한다.

`DiscountPolicy`의 하위 타입인 `FixDiscountPolicy`, `RateDiscountPolicy` 둘다 스프링 빈으로 선언해보자.

```java
@Component
public class FIxDiscountPolicy implements DiscountPolicy {
```

```java
@Component
public class RateDiscountPolicy implements DiscountPolicy {
```

그리고 이렇게 의존관계를자동 주입을 실행하면,

`NoUniqueBeanDefinitionException` 오류 발생

**오류 메시지**

```java
org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'orderServiceImpl' defined in file [/Users/evan/Documents/Developer/03_Study/02_Spring/03_basic/core/out/production/classes/hello/core/order/OrderServiceImpl.class]: Unsatisfied dependency expressed through constructor parameter 1: No qualifying bean of type 'hello.core.discount.DiscountPolicy' available: **expected single matching bean but found 2: FIxDiscountPolicy,rateDiscountPolicy**

Caused by: org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean of type 'hello.core.discount.DiscountPolicy' **available: expected single matching bean but found 2: FIxDiscountPolicy,rateDiscountPolicy**
```

오류 메시지를 통해 하나의 빈을 기대했는데 `fixDiscountPolicy`, `rateDiscountPolicy` 2개가 발견됐다고 알려준다.

이 때, 하위 타입으로 지정할 수도 있지만, 하위 타입으로 지정하는 것은 DIP를 위배하고 유연성이 떨어진다. 그리고 이름만 다르고, 완전히 똑같은 타입의 스프링 빈이 2개 있을 때 해결이 안된다.

스프링 빈을 수동 등록해서 문제를 해결해도 되지만, 의존 관계 자동 주입에서 해결하는 여러 방법이 있다.

## @Autowired 필드명, @Qulifier, @Primary

조회 대상이 2개 이상일 때 해결 방법

- @Autowired 필드 명 매칭
- @Qulifier → @Qulifier 끼리 매칭 → 빈 이름 매칭
- @Primary 사용

### @Autowried 필드 명 매칭

`@Autowired`는 타입 매칭을 시도하고, 이때 여러 빈이 있으면 필드 이름, 파라미터 이름으로 빈 이름을 추가 매칭한다.

**기존코드**

**OrderServiceImpl**

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy)
```

변경된 코드

**OrderServiceImpl**

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy ratediscountPolicy)
```

<aside>
💡

코드를 변경해도 계속해서 오류가 날떄

- 코드 변경
    
    ```java
    @Autowired
        public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy rateDiscountPolicy) {
            this.memberRepository = memberRepository;
            this.discountPolicy = rateDiscountPolicy;
    ```
    
- 실행결과
    
    ```java
    org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'orderServiceImpl' defined in file [/Users/evan/Documents/Developer/03_Study/02_Spring/03_basic/core/out/production/classes/hello/core/order/OrderServiceImpl.class]: Unsatisfied dependency expressed through constructor parameter 1: No qualifying bean of type 'hello.core.discount.DiscountPolicy' available: expected single matching bean but found 2: fixDiscountPolicy,rateDiscountPolicy
    ```
    
- 이유
    - 스프링 부트 3.2 파라미터 이름 인식 문제
        - 3.2 버전부터는 자바 컴파일러에 -parameters 옵션을 넣어주어야 애노테이션의 이름을 생략할 수 있다.
- 해결방안
    - 컴파일 시점에 -parameters 옵션 적용
        - IntelliJ IDEA > File > Settings > Build, Executions, Deployment > Compiler > JavaCompiler > Additional command line parameters 에 다음 추가
            
            `-parameters`
            
        - out 폴더를 삭제하고 다시 실행
        - (권장) Gradle을 사용해 빌드하고 실행함.
</aside>

- `AutoAppConfigTest`는 통과하는데 `XmlAppContext`는 실패하는 이유
    - 오류 메시지
        
        ```java
        org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'orderService' defined in class path resource [appConfig.xml]: Unsatisfied dependency expressed through constructor parameter 1: Ambiguous argument values for parameter of type [hello.core.discount.DiscountPolicy] - did you specify the correct bean references as arguments?
        
        해석: 클래스 경로 리소스 [appConfig.xml]에 정의된 이름이 'orderService'인 Bean을 생성하는 중 오류가 발생했습니다. 생성자 매개변수 1을 통해 표현된 충족되지 않은 종속성: [hello.core.discount 유형의 매개변수에 대한 모호한 인수 값 .DiscountPolicy] - 올바른 Bean 참조를 인수로 지정했습니까?
        ```
        
    - appConfig.xml 파일 중
        
        ```java
        <bean id="orderService" class="hello.core.order.OrderServiceImpl">
            <constructor-arg name="memberRepository" ref="memberRepository"/>
            <constructor-arg name=**"discountPolicy"** ref="discountPolicy"/>
        </bean>
        ```
        
        `name = “discountPolicy”`를 `rateDiscountPolicy`로 바꾸었기 때문에 오류가 발생한다.
        

**@AutoWired 매칭 정리**

1. 타입 매칭
2. 타입 매칭의 결과가 2개 이상일 때 필드 명, 파라미터 명으로 빈 이름 매칭

### @Qualifier 사용

@Qualifier는 추가 구분자를 붙여주는 방법. 주입시 추가적인 방법을 제공하는 것이지 빈 이름을 변경하는 것이 아니다.

**빈 등록시 @Quilifier를 붙여준다.**

**생성자 자동 주입 예시**

**FixDiscountPolicy**

```java
@Component
@Qualifier("fixDiscountPolicy")
public class FixDiscountPolicy implements DiscountPolicy {
```

**RateDiscountPolicy**

```java
@Component
@Qualifier("mainDiscountPolicy")
public class RateDiscountPolicy implements DiscountPolicy {
```

**OrderServiceImpl**

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, @Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
```

**수정자 자동 주입 예시**

```java
@Autowired
public DiscountPolicy setDiscountPolicy (@Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
    return discountPolicy;
}
```

@Qualifier로 주입할 때 @Qualifier(”mainDiscountPolicy”)를 못찾으면 mainDiscountPolicy라는 이름의 스프링 빈을 추가로 찾는다. 하지만 경험상 `@Qualifier` 는 `@Qualifier` 를 찾는 용도로만 사용하는게 명확하고 좋다.

`@Component`와 같이 빈을 자동 주입하는 것 외에 다음과 같이 직접 빈 등록시에도 `@Qualifier`를 동일하게 사용할 수 있다.

```java
@Bean
@Qualifier("mainDiscountPolicy")
public DiscountPolicy discountPolicy() {
    return new ...
}
```

**@Qualifier 정리**

1. `@Qualifier`끼리 매칭
2. 빈 이름 매칭
3. `NoSuchBeanDefinitionException` 예외 발생

### @Primary 사용

`@primary`는 우선순위를 정하는 방법이다. `@Autowired`시에 여러 빈이 매칭되면 `@Primary`가 우선권을 가진다.

`rateDiscountPolicy`가 우선권을 가지도록 하자.

**RateDiscountPolicy**

```java
@Component
@Primary
public class RateDiscountPolicy implements DiscountPolicy {}
```

**사용코드**

**OrderServiceImpl**

```java
//생성자
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
}

//수정자
@Autowired
public DiscountPolicy discountPolicy(DiscountPolicy discountPolicy) {
    return new discountPolicy;
}
```

코드를 실행해보면 문제없이 `@Primary`가 잘 동작하는 것을 확인할 수 있다.

여기까지 보면 `@Primary`와 `@Qualifier` 중 어떤것을 사용할지 고민이 될 것이다. `@Quailfier`의 단점은 주입 받을 때 다음과 같이 모든 코드에 `@Qualifier`를 붙여주어야한다는 것이다.

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, @Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
```

반면에 `@Primary`는 이렇게 붙일 필요가 없다.

**@Primary, @Qualifier 활용**

코드에서 자주 사용하는 메인 데이터베이스의 커넥션을 획득하는 스프링 빈이 있고, 코드에서 특별한 기능으로 가끔 사용하는 서브 데이터베이스의 커넥션을 획득하는 스프링 빈이 있다고 가정하자. 메인 데이터베이스의 커넥션을 획득하는 스프링 빈은 `@Primary`를 적용해서 조회하는 곳에서 `@Qualifier` 지정 없이 편리하게 조회하고, 서브 데이터베이스 커넥션 빈을 획득 할 때는 `@Qualifier`를 지정해서 명시적으로 획득 하는 방식으로 사용하면 코드를 깔끔하게 유지할 수 있다. 물론 이때 메인 데이터베이스의 스프링 빈을 등록할 때 `@Qualifier`를 지정해주는것은 상관 없다.

**우선순위**

`@Primary`는 기본 값 처럼 동작하는 것이고, `@Qualifier`는 매우 상세하게 동작한다. 스프링은 자동보다는 수동이, 넓은 범위보다는 좁은 범위의 선택권이 우선순위가 높다 따라서 여기서도 `@Qualifier`가 우선권이 높다.

## 애노테이션 직접 만들기

`@Qualifier(”mainDiscountPolicy”)` 이렇게 문자를 직접 적으면 컴파일 시 타입 체크가 안된다. 다음과 같은 애노테이션을 만들어서 문제를 해결할 수 있다.

`core/src/main/java/hello/core/annotation/MainDiscountPolicy.java`

**MainDiscountPolicy**

```java
package hello.core.annotation;

import org.springframework.beans.factory.annotation.Qualifier;

import java.lang.annotation.*;

@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.TYPE, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
@Qualifier("mainDiscountPolicy")
public @interface MainDiscountPolicy {
}
```

**RateDiscountPolicy**

```java
@Component
@MainDiscountPolicy
public class RateDiscountPolicy implements DiscountPolicy {
...
}
```

**OrderServiceImpl**

```java
@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository, @MainDiscountPolicy DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
```

애노테이션에는 상속이라는 개념이 없다. 이렇게 여러 애노테이션을 모아서 사용하는 기능은 스프링이 지원해주는 기능이다. `@Quailfier`뿐만 아니라 다른 애노테이션들도 함께 조합해서 사용할 수 있다. 단적으로 `@Autowired`도 재정의 할 수 있다. 물론 스프링이 제공하는 기능을 뚜렷한 목적없이 무분별하게 재정의하는 것은 유지보수에 더 혼란만 가중할 수 있다.

## 조회한 빈이 모두 필요할 때, List, Map

의도적으로 정말 해당 타입의 스프링 빈이 다 필요한 경우도 있다.

예를 들어서 할인 서비스를 제공하는데, 클라이언트가 할인의 종류(rate, fix)를 선택할 수 있다고 가정, 스프링을 사용하면 소위 말하는 전략 패턴을 매우 간단하게 구현할 수 있다.

`core/src/test/java/hello/core/Autowired/AllBeanTest.java`

**AllBeanTest**

```java
public class AllBeanTest {

    @Test
    void findAllBean() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(DiscountService.class);
    }

    static class DiscountService {
        private final Map<String, DiscountPolicy> policyMap;
        private final List<DiscountPolicy> policies;

        public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {
            this.policyMap = policyMap;
            this.policies = policies;
            System.out.println("policyMap = " + policyMap);
            System.out.println("policies = " + policies);
        }
    }
}
```

**실행결과**

```java
policyMap = {}
policies = []
```

- 해당 생성자에 아무것도 주입되지 않았기 때문에 policyMap, policies는 비어있다.

**AllBeanTest**

```java
public class AllBeanTest {

    @Test
    void findAllBean() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class, DiscountService.class);    }

    static class DiscountService {
        private final Map<String, DiscountPolicy> policyMap;
        private final List<DiscountPolicy> policies;

        public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {
            this.policyMap = policyMap;
            this.policies = policies;
            System.out.println("policyMap = " + policyMap);
            System.out.println("policies = " + policies);
        }
    }
}
```

**실행결과**

```java
policyMap = {fixDiscountPolicy=hello.core.discount.FixDiscountPolicy@26be6ca7, rateDiscountPolicy=hello.core.discount.RateDiscountPolicy@6ea1bcdc}
policies = [hello.core.discount.FixDiscountPolicy@26be6ca7, hello.core.discount.RateDiscountPolicy@6ea1bcdc]
```

- `AutoAppConfig.class` 에 의해 Map은 빈에 등록된 DiscountPolicy 이름(key), 해당 객체 위치(value)가 반환되었으며, List에는 해당객체 위치(value)만 반환되었다.

- TDD를 이용하여, 아래 코드를 작성한다.

```java
DiscountService dicountService = ac.getBean(DiscountService.class);
Member member = new Member(1L, "evan", Grage.VIP);
discountService.discount(member, 10000, "fixDiscountPolicy")
```

여기서, `discountService.discount` 에서 `cmd` + `opt` + `V` 를 활용해, method를 만들어준다.

```java
public int discount(Member member, int price, String discountCode) {
    DiscountPolicy discountPolicy = policyMap.get(discountCode);
    return discountPolicy.discount(member, price)
```

> **참고: discountPolicy.discount(인터페이스)**
> 

```java
package hello.core.discount;

import hello.core.member.Member;

public interface DiscountPolicy {
    /**
     * @return 할인 대상 금액
     */
    int discount(Member member, int price);
```

이후, 아래와 같이 테스트 코드를 작성한다.

**AllBeanTest**

```java
public class AllBeanTest {

    @Test
    void findAllBean() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class, DiscountService.class);

        //given
        DiscountService discountService = ac.getBean(DiscountService.class);
        Member member = new Member(1L, "evan", Grade.VIP);

        //when
        int fixDisCountPrice = discountService.discount(member, 10000, "fixDiscountPolicy");
        int rateDiscountPrice = discountService.discount(member, 20000, "rateDiscountPolicy");

        //then
        assertThat(discountService).isInstanceOf(DiscountService.class);
        assertThat(fixDisCountPrice).isEqualTo(1000);
        assertThat(rateDiscountPrice).isEqualTo(2000);

    }

    static class DiscountService {
        private final Map<String, DiscountPolicy> policyMap;
        private final List<DiscountPolicy> policies;
        
        @Autowired
        public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {
            this.policyMap = policyMap;
            this.policies = policies;
            System.out.println("policyMap = " + policyMap);
            System.out.println("policies = " + policies);
        }

        public int discount(Member member, int price, String discountCode) {
            DiscountPolicy discountPolicy = policyMap.get(discountCode);
            return discountPolicy.discount(member, price);
        }
    }
}
```

**로직분석**

- `DiscountService`는 `Map`으로 모든 `DiscountPolicy`를 주입받는다. 이때 `fixDiscountPolicy`, `rateDiscountPolicy`가 주입된다.
- `discount()` 메서드는 `discountCode`로 “fixDiscountPolicy”가 넘어오면 `map`에서 `fixDiscountPolicy` 스프링 빈을 찾아서 실행한다.

**주입분석**

- `Map<String, DiscountPolicy>`: map의 키에 스프링 빈의 이름을 넣어주고, 그 값으로 `DiscountPolicy` 타입으로 조회한 모든 스프링 빈을 담아준다.
- `List<DiscountPolicy>`: `DiscountPolicy` 타입으로 조회한 모든 스프링 빈을 담아준다.
- 만약 해당하는 타입의 스프링 빈이 없으면, 빈 컬렉션이나 Map을 주입한다.

## 자동, 수동의 올바른 실무 운영 기준

**편리한 자동 기능을 기본으로 사용하자**

그러면 어떤 경우 컴포넌트 스캔과 자동 주입을 사용하고, 어떤 경우에 설정 정보를 통해서 수동으로 빈을 등록하고, 의존관계도 수동으로 주입할까?

결론부터 이야기하면 스프링이 나오고 시간이 갈수록 점점 자동을 선호 하는 추세다. 스프링은 `@Component` 뿐만 아니라 `@Controller`, `@Service`, `@Repository`  처럼 계층에 맞춰 일반적인 애플리케이션 로직을 자동으로 스캔 할 수 있도록 지원 한다. 거기에 너의 최근 스프링 노트는 컴포넌트 스캔을 기본으로 사용하고 스프링 부트의 다양한 스프링 빈들도 조건이 맞으면 자동으로 등록하도록 설계했다.

설정 정보를 기반으로 애플리케이션을 구성하는 부분과 실제 동작 하는 부분을 명확하게 나누는 것이 이상적이지만 개발자 입장에서 스프링 핀을 하나 등록할 때 `@Component` 만 넣어주면 끝나는 일을 @Configuration 설정 정보에 가서 @Bean을 적고, 객체를 생성 하고 주입할 대상을 일 일이 적어 주는 과정은 상당히 번거롭다.
또 관리 할 뿐이 많아서 설정 정보가 커지면 설정 정보를 관리 하는 것 자체가 부담이 된다.
그리고 결정적으로 자동 빈 등록을 사용해도 OCP, DIP를 지킬 수 있다.

**그러면 수동 빈 등록은 언제 사용할까?**

애플리케이션은 크게 업무 로직과 기술 지원 로직으로 나눌 수 있다.

- **업무 로직 빈**: 웹을 지원하는 컨트롤러, 핵심 비즈니스 로직이 있는 서비스, 데이터 계층의 로직을 처리하는 리포지토리등이 모두 업무 로직이다. 보통 비즈니스 요구사항을 개발할 때 추가되거나 변경된다.
- **기술 지원 빈**: 기술적인 문제나 공통 관심사(AOP)를 처리할 때 주로 사용된다. 데이터베이스 연결이나, 공통 로그 처리 처럼 업무 로직을 지원하기 위한 하부 기술이나 공통 기술들이다.

- 업무 로직은 숫자도 매우 많고, 한번 개발해햐 하면 컨트롤러, 서비스, 리포지토리 처럼 어느정도 유사한 패턴이 있다. 이런경우 자동 기능을 적극 사용하는 것이 좋다. 보통 문제가 발생해도 어떤 곳에서 문제가 발생했는지 명확하게 파악하기 쉽다.
- 기술 지원 로직은 업무 로직과 비교해서 그 수가 매우 적고, 보통 애플리케이션 전반에 걸쳐 광범위하게 영향을 미친다. 그리고 업무 로직은 문제가 발생했을 때 어디서 문제인지 명확하게 잘 들어나지만, 기술 지원 로직은 적용이 잘 되고 있는지 아닌지 조차 파악하기 어려운 경우가 많다. 그래서 이런 기술 지원 로직들은 가급적 수동 빈 등록을 사용해서 명확하게 들어내는게 좋다.

**애플리케이션에 광범위 하게 영향을 미치는 기술 지원 객체는 수동 빈으로 등록해서 딱! 설정 정보에 바로 나타나게 하는 것이 유지보수 하기 좋다.**

**비즈니스 로직 중에서 다양성을 적극 활용 할 때**
의존 관계 자동 주입 조회한 빈 모두 필요할 때, List, Map을 다시보자.
DiscountService가 의존관계 자동 주입으로 Map<String, DiscountPolicy>에 주입을 받은 상황을 생각해 보자. 여기에 어떤 빈들이 주입 될지, 각 빈들의 이름은 무엇일지 코드만 보고 한 번에 쉽게 파악할 수 있을까 내가 개발 했으니 크게 관계가 없지만 만약 이 코드들을 다른 개발자가 개발해서 나에게 준 것이라면 자동 등록을 사용하고 있기 때문에 파악하려면 여러 코드를 찾아 봐야 한다.

이런 경우 수동 빈으로 등록하거나 또는 자동으로 하면 특정 패키지에 같이 묶어 두는 게 좋다. 핵심은 딱 보고 이해가 돼야 한다.

이 부분을 별도의 설정 정보로 만들고 수동으로 등록하면 다음과 같다.

```java
@Configuration
public class DiscountPolicyConfig {
    
    @Bean
    public DiscountPolicy rateDiscountPolicy() {
        return new RateDiscountPolicy;
    }
    
    @Bean
    public DiscountPolicy fixDiscountPolicy() {
        return new FixDiscountPolicy;
    }
}
```

이 설정 정보만 봐도 한눈에 빈의 이름은 물론이고, 그래도 빈 자동 등록을 사용하고 싶으면, 파악하기 좋게 `DiscountPolicy`의 구현 빈들만 따로 모아서 특정 패키지에 같이 묶어두자.

참고로 스프링과 스프링 부트가 자동으로 등록하는 수많은 빈들은 예외다. 이런 부분들은 스프링 자체를 잘 이해하고 스프링의 의도대로 잘 사용하는게 중요하다. DataSource같은 데이터베이스에 연결에 사용하는 기술 지원 로직까지 내부에서 자동으로 등록하는데, 이런 부분은 메뉴얼을 잘 참고해서 스프링 부트가 의도한 대로 편리하게 사용하면 된다. 반면에 **스프링 부트가 아니라 직접 기술 지원 객체를 스프링 빈으로 등록한다면 수동으로 등록해서 명확하게 들어내는 것이 좋다.**

**정리**

편리한 자동 기능을 기본으로 사용하자.

직접 등록하는 기술 지원 객체는 수동 등록.

다형성을 적극 활용하는 비즈니스 로직은 수동 등록을 고민해보자.