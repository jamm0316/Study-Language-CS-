[<back](https://www.notion.so/Spring-basic-41cfc2c136874d9896aa115923bd4109?pvs=21)

---

<aside>
📃

Content

</aside>

---

## 웹 애플리케이션과 싱글톤

- 스프링은 태생이 기업용 온라인 서비스 기술을 지원하기 위해 탄생.
- 대부분의 스프링 애플리케이션은 웹 애플리케이션이다. 물론 웹이 아닌 애플리케이션 개발도 얼마든지 개발할 수 있다.
- 웹 애플리케이션은 보통 여러 고객이 동시에 요청함.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f45d3ba1-b344-4175-bf18-2aaf766c915a/27c80870-0a08-432f-9c87-0cff4c4d323d.png)

- 고객이 같은 요청을 하면 매번 똑같은 인스턴스를 생성해서 반환해주어야한다.

**스프링 없는 순수한 DI 컨테이너 테스트**

```java
package hello.core.singleton;

import hello.core.AppConfig;
import hello.core.member.MemberService;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThat;

public class SingletonTest {

    @Test
    @DisplayName("스프링이 없는 순수한 DI 컨테이너")
    void pureContainer() {
        AppConfig appConfig = new AppConfig();

        //1. 조회: 호출할 때 마다 객체를 생성
        MemberService memberService1 = appConfig.memberService();  //1번 요청
        MemberService memberService2 = appConfig.memberService();  //2번 요청

        //2. 참조값이 다른 것을 확인
        System.out.println("memberService1 = " + memberService1);
        System.out.println("memberService2 = " + memberService2);

        //memberService1 !== memberService2
        assertThat(memberService1).isNotSameAs(memberService2);
    }
}

```

**실행결과**

```java
memberService1 = hello.core.member.MemberServiceImpl@3c0a50da
memberService2 = hello.core.member.MemberServiceImpl@646be2c3
```

- 우리가 만들었던 스프링 없는 순수한 DI 컨테이너인 AppConfig는 요청을 할 때마다 객체를 새로 생성한다.
- 고객 트래픽이 초당 100이 나오면 초당 100개 객체가 생성되고 소멸된다. → 메모리 낭비가 심하다.
- 해결방안은 해당 객체가 딱 1개만 생성되고, 공유하도록 설계하면 된다. → 싱글톤 패턴

## 싱글톤 패턴

- 클래스의 인스턴스가 딱 1개만 생성되는 것을 보장하는 디자인 패턴이다.
- 그래서 객체 인스턴스를 2개 이상 생하지 못하도록 막아야 한다.
    - `private` 생성자를 사용해서 외부에서 임의로 `new` 키워드를 사용하지 못하도록 막아야한다.

**SingletonService**

```java
package hello.core.singleton;

public class SingletonService {

    //싱글톤 인스턴스를 생성할 때, static을 사용해 클래스 영역에 하나만 생성.
    private static final SingletonService instance = new SingletonService();

    //인스턴스 불러오는 메서드
    public static SingletonService getInstance() {
        return instance;
    }

    //기본 생성자를 private로 숨김
    private SingletonService() {
    }

    public void logic() {
        System.out.println("싱글톤 객체 로직 호출");
    }
}
```

1. `static` 영역에 객체 `instance`를 미리 하나 생성해서 올려둔다.
2. 이 객체 인스턴스 필요하면 오직 `getInstance()` 메서드를 통해서만 조회할 수 있다. 이 메서드를 호출하면 항상 같은 인스턴스를 반환한다.
3. 딱 1개의 객체 인스턴스만 존재해야 하므로 생성자를 `private` 로 막아서 혹시라도 외부에서 new 키워드로 객체 인스턴스가 생성되는 것을 막는다.
- 만약 외부에서 호출하려고 하면 아래와 같은 오류 메세지가 반환됨.
(`SingletonService singletonService = new SingletonService();` )

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/25712447-62aa-493a-a986-8fdc29f908c4/image.png)

**SingletonTest**

```java
@Test
@DisplayName("싱글톤 서비스 객체 호출")
void singletonService() {
    new SingletonService();
}
```

**실행결과**

```java
java: SingletonService() has private access in hello.core.singleton.SingletonService
```

- 외부에서 싱글톤 패턴의 객체를 생성할 수 없게 `private`로 기본 생성자를 막아 놨다.

**SingletonTest**

```java
@Test
@DisplayName("싱글톤 패턴을 적용한 객체 사용")
void singletonService() {
    //1. 조회: 호출할 때 마다 같은 객체를 반환
    SingletonService instance1 = SingletonService.getInstance();
    SingletonService instance2 = SingletonService.getInstance();

    //2. 참조값이 같은 것을 확인
    System.out.println("instance1 = " + instance1);
    System.out.println("instance2 = " + instance2);

    assertThat(instance1).isSameAs(instance2);
}
```

**실행결과**

```java
instance1 = hello.core.singleton.SingletonService@146044d7
instance2 = hello.core.singleton.SingletonService@146044d7
```

- `isSameAs`는 객체의 참조값을 비교한다.
- 호출할 때 마다 같은 객체 인스턴스를 반환하는 것을 확인할 수 있다.

> 참고: 싱글톤 패턴을 구현하는 방법은 여러가지가 있다. 여기서는 객체를 미리 생성해두는 가장 단순하고 안전한 방법을 선택했다.
> 

싱글톤 패턴을 적용하면 고객의 요청이 올 때 마다 객체를 생성하는 것이 아니라, 이미 만들어진 객체를 공유해서 효율적으로 사용할 수 있다. 하지만 싱글톤 패턴은 다음과 같은 수많은 문제점을 가지고 있다.

**싱글톤 패턴 문제점**

- 싱글톤 패턴은 구현하는 코드 자체가 많이 들어간다.
- 의존관계상 클라이언트 구체 클래스에 의존한다. → DIP를 위반한다.
- 클라이언트가 구체 클래스에 의존해서 OCP 원칙을 위반할 가능성이 높다.
- 테스트하기 어렵다.
- 내부 속성을 변경하거나 초기화 하기 어렵다.
- private 생성자로 자식 클래스를 만들기 어렵다.
- 결론적으로 유연성이 떨어진다.
- 안티패턴으로 불리기도 한다.

## 싱글톤 컨테이너

스프링 컨테이너는 싱글톤 패턴의 문제점을 해결하면서, 객체 인스턴스를 싱글톤(1개만 생성)으로 관리한다.

지금까지 우리가 학습한 스프링 빈이 바로 싱글톤으로 관리되는 빈이다.

**싱글톤 컨테이너**

- 스프링 컨테이너는 싱글톤 패턴을 적용하지 않아도, 객체 인스턴스를 싱글톤으로 관리한다.
    - 이전에 설명한 컨테이너 생성 과정을 자세히 보자. 컨테이너는 객체를 하나만 생성해서 관리 한다.
        
        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/414a9ffc-8689-4d2c-ac09-e27942fcaf12/d60b6bf1-5517-4951-b111-5eff8f21b5c7.png)
        
- 스프링 컨테이너는 싱글톤 컨테이너 역할을 한다. 이렇게 싱글톤 객체를 생성 하고 관리 하는 기능을 싱글톤 레지스트리 라고 한다.
- 스프링 컨테이너의 이런 기능 덕분에 싱글톤 패턴의 모든 단점을 해결 하면서 객체를 싱글톤으로 유지 할 수 있다.
    - 싱글톤 패턴을 위한 지저분한 코드가 들어 가지 않아도 된다.
    - DIP, OCP, 테스트, private 생성자로부터 자유롭게 싱글톤을 사용할 수 있다.

**스프링 컨테이너를 사용하는 테스트 코드**

**SingletonTest**

```java
@Test
@DisplayName("스프링 컨테이너와 싱글톤")
void springContainer() {
    ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

    //1. 조회: 호출할 때 마다 같은 객체를 반환
    MemberService memberService1 = ac.getBean("memberService", MemberService.class);
    MemberService memberService2 = ac.getBean("memberService", MemberService.class);

    //2. 참조값이 값은 것을 확인
    System.out.println("memberService1 = " + memberService1);
    System.out.println("memberService2 = " + memberService2);

    assertThat(memberService1).isSameAs(memberService2);
}
```

**실행결과**

```java
memberService1 = hello.core.member.MemberServiceImpl@7d446ed1
memberService2 = hello.core.member.MemberServiceImpl@7d446ed1
```

**싱글톤 컨테이너 적용 후**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/067474a5-eb81-45f2-bc39-076793155afe/80bdc19e-5990-4d03-b5fa-a837e30d2098.png)

- 스프링 컨테이너 덕분에 고객의 요청이 올때마다 객체를 생성 하는 것이 아니라 이미 만들어진 객체를 공유해서 효율적으로 재사용 할 수 있다.

> 참고: 스프링 기본 빈 등록 방식은 싱글톤이지만 싱글톤 방식만 지원 하는 것은 아니다.
요청 할 때마다 새로운 객체를 생성 해서 반환 하는 기능도 제공 한다. 자세한 내용은 빈 스코프에서 설명.
> 

## 싱글톤 방식의 주의점

- 싱글톤 패턴이든, 스프링 같은 싱글톤 컨테이너를 사용하든 객체 인스턴스를 하나만 생성해서 공유하는 싱글톤 방식은 여러 클라이언트가 하나의 같은 객체 인스턴스를 공유하기 때문에 싱글톤 객체는 상태를 유지(stateful)하게 설계하면 안된다.
- 무상태(stateless)로 설계해야한다.
    - 특정 클라이언트에 의존적인 필드가 있으면 안된다.
    - 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다.
    - 가급적 읽기만 가능해야한다.
    - 필드 대신에 자바에서 공유되지 않는 지역변수, 파라미터, TreadLocal등을 사용해야한다.
- 스프링 빈의 필드에 공유 값을 설정하면 정말 큰 장애가 발생할 수 있다.

**상태를 유지할 경우 발생하는 문제점 예시**

**StatefulService**

```java
package hello.core.singleton;

public class StatefulService {

    private int price;

    public void order(String name, int price) {
        System.out.println("name = " + name + " price = " + price);
        this.price = price;  //여기서 문제!!
    }

    public int getPrice() {
        return price;
    }
}
```

**StatefulServiceTest**

```java
package hello.core.singleton;

class StatefulServiceTest {

    @Test
    void statefulServiceSingleton() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
        StatefulService statefulService1 = ac.getBean(StatefulService.class);
        StatefulService statefulService2 = ac.getBean(StatefulService.class);

        //ThreadA: A 사용자 10000원 주문
        statefulService1.order("UserA", 10000);

        //ThreadB: B 사용자 20000원 주문
        statefulService2.order("UserB", 20000);

        //ThreadA: 사용자A 주문 금액 조회
        int price = statefulService1.getPrice();
        System.out.println("price = " + price);

        assertThat(statefulService2.getPrice()).isEqualTo(20000);
    }

    static class TestConfig {

        @Bean
        public StatefulService statefulService() {
            return new StatefulService();
        }
    }
}
```

실행결과

```java
name = UserA price = 10000
name = UserB price = 20000
price = 20000
```

- 최대한 단순히 설명하기 위해, 실제 쓰레드는 사용하지 않았다.
- ThreadA가 사용자A 코드를 호출하고 ThreadB가 사용자B 코드를 호출한다 가정하자.
- `StatefulService`의 `price`필드는 공휴되는 필드인데, 특정 클라이언트가 값을 변경한다.
- 사용자A의 주문금액은 10000원이 되어야하는데, 20000원이라는 결과가 나왔다.
- 실무에서는 이런 경우를 종종 보는데, 이로인해 정말 해결하기 어려운 큰 문제들이 터진다.(몇년에 한번씩은 꼭 만난다.)
- 진짜 공유필드는 조심해야 한다! 스프링 빈은 항상 무상태(stateless)로 설계하자.

**StatelessService 만들기**

- 필드값을 없애도 매개변수(파라미터)를 사용

```java
package hello.core.singleton;

public class StatefulService {

    public int order(String name, int price) {
        System.out.println("name = " + name + " price = " + price);
        return price;
    }
}

```

**StatelessServiceTest**

```java
package hello.core.singleton;

class StatefulServiceTest {

    @Test
    void statefulServiceSingleton() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
        StatefulService statefulService1 = ac.getBean(StatefulService.class);
        StatefulService statefulService2 = ac.getBean(StatefulService.class);

        //ThreadA: A 사용자 10000원 주문
        int userAPrice = statefulService1.order("UserA", 10000);

        //ThreadB: B 사용자 20000원 주문
        int userBprice = statefulService2.order("UserB", 20000);

        //ThreadA: 사용자A 주문 금액 조회
        System.out.println("price = " + userAPrice);
    }

    static class TestConfig {

        @Bean
        public StatefulService statefulService() {
            return new StatefulService();
        }
    }

}
```

**실행결과**

```java
name = UserA price = 10000
name = UserB price = 20000
price = 10000
```

## @Configuration과 싱글톤

**AppConfig**

```java
package hello.core;

@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public static MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    @Bean
    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    @Bean
    public static DiscountPolicy discountPolicy() {
//        return new FIxDiscountPolicy();
        return new RateDiscountPolicy();
    }
}
```

- memberService 빈을 만드는 코드를 보면 `memberRepository()`를 호출한다.
    - 이 메서드를 호출하면 `new MemoryMemberRepository()`를 호출한다.
- orderService  빈을 만드는 코드도 동일하게 `memberRepository()`를 호출한다.
    - 이 메서드를 호출하면 `new MemoryMemberRepository()`를 호출한다.

결과적으로 각각 다른 2개의 `MemoryMemberRepository()`가 생성되면 싱글톤 패턴이 깨지는 것처럼 보인다.

스프링 컨테이너는 이것을 어떻게 해결할까?

**ConfigurationSingletonTest**

```java
package hello.core.singleton;

public class ConfigurationSingletonTest {

    @Test
    void configurationTest() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

        MemberServiceImpl memberService = ac.getBean("memberService", MemberServiceImpl.class);
        OrderServiceImpl orderService = ac.getBean("orderService", OrderServiceImpl.class);
        MemberRepository memberRepository = ac.getBean("memberRepository", MemberRepository.class);

        MemberRepository memberRepository1 = memberService.getMemberRepository();
        MemberRepository memberRepository2 = orderService.getMemberRepository();

        System.out.println("memberService -> memberRepository1 = " + memberRepository1);
        System.out.println("oderService -> memberRepository2 = " + memberRepository2);
        System.out.println("memberRepository = " + memberRepository);

        assertThat(memberRepository1).isSameAs(memberRepository2);
        assertThat(memberRepository2).isSameAs(memberRepository);
    }
}
```

**실행결과**

```java
memberService -> memberRepository1 = hello.core.member.MemoryMemberRepository@be68757
oderService -> memberRepository2 = hello.core.member.MemoryMemberRepository@be68757
memberRepository = hello.core.member.MemoryMemberRepository@be68757
```

- 확인해보면 `memberRepository` 인스턴스는 모두 같은 인스턴스가 공유되어 사용된다.
- `AppConfig`의 자바 코드를 보면 분명히 각각 2번 `new MemoryMemebrRepository` 호출해서 다른 인스턴스가 생성되어야 하는데 어떻게 같은 참조값을 반환하는 것일까?
- 실험을 통해 확인해보자.

**AppConfig**

```java
package hello.core;

@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService() {
        System.out.println("Call AppConfig.memberService");
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository() {
        System.out.println("Call AppConfig.memberRepository");
        return new MemoryMemberRepository();
    }

    @Bean
    public OrderService orderService() {
        System.out.println("Call AppConfig.orderService");
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    @Bean
    public DiscountPolicy discountPolicy() {
//        return new FIxDiscountPolicy();
        return new RateDiscountPolicy();
    }
}
```

- 실행될 때마다 `Call Appconfig.~` 이 출력되도록 하였다.
- 만약 `memberService`, `memberRepository`, `orderService`를 호출하면,
- 아래와 같이 `Call Appconfig.memberRepository` 가 3번 호출 되어야 한다.
`Call AppConfig.memberservice` 
`Call Appconfig.memberRepository`  → 1
`Call Appconfig.memberRepository`  → 2
`Call AppConfig.orderService`
`Call Appconfig.memberRepository`  → 3

**ConfigurationSingletonTest**

```java
package hello.core.singleton;

public class ConfigurationSingletonTest {

    @Test
    void configurationTest() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

        MemberServiceImpl memberService = ac.getBean("memberService", MemberServiceImpl.class);
        OrderServiceImpl orderService = ac.getBean("orderService", OrderServiceImpl.class);
        MemberRepository memberRepository = ac.getBean("memberRepository", MemberRepository.class);

        MemberRepository memberRepository1 = memberService.getMemberRepository();
        MemberRepository memberRepository2 = orderService.getMemberRepository();

        System.out.println("memberService -> memberRepository1 = " + memberRepository1);
        System.out.println("oderService -> memberRepository2 = " + memberRepository2);
        System.out.println("memberRepository = " + memberRepository);

        assertThat(memberRepository1).isSameAs(memberRepository2);
        assertThat(memberRepository2).isSameAs(memberRepository);
    }
}
```

**실행결과**

```java
Call AppConfig.memberService
Call AppConfig.memberRepository
Call AppConfig.orderService
```

- 예상과 다르게 각 메서드들이 한번 호출되었다.

## @Configuratrion과 바이트코드 조작의 마법

스프링 컨테이너는 싱글톤 레지스트리다. 따라서 빈이 싱글톤이 되도록 보장해주어야 한다. 그런데 스프링이 자바 코드까지 어떻게 하기는 어렵다. 아래 코드는 분명 3번 호출이 되어야 하는 것이 맞다.

그래서 스프링은 클래스의 바이트코드를 조작하는 라이브러리를 사용한다.

모든 비밀은 `@Configuration`을 적용한 `AppConfig`에 있다.

**ConfigurationSingletonTest**

```java
@Test
void configurationDeep() {
    ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
    AppConfig bean = ac.getBean(AppConfig.class);

    //클래스 타입 조회
    System.out.println("bean.getClass() = " + bean.getClass());
}
```

- 순수한 클래스라면 다음과 같이 출력되어야 한다

`class hello.core.AppConfig` 

**실행결과**

```java
bean.getClass() = class hello.core.AppConfig$$SpringCGLIB$$0
                                   클래스 타입 // 
```

- 예상과 다르게 클래스 명에 `SpringCGLIB`이 붙으면서 상당히 복잡해졌다.
이것은 내가 만든 클래스가 아니라 스프링이 `CGLIB`라는 바이트코드 조작 라이브러리를 사용해서 `AppConfig` 클래스를 상속받은 임의의 다른 클래스를 만들고, 그 다른 클래스를 스프링 빈으로 등록한 것!

**그림**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f1908d4f-0cb6-4e76-8ff8-ffc7a98d8cbd/b1a60977-87e7-42ea-8f43-b2b87790d8e3.png)

그 임의의 다른 클래스가 바로 싱글톤이 되도록 보장해준다. 아마도 다음과 같은 바이트 코드를 조작해서 작성되어있을 것이다.

(실제로는 CGLIB의 내부 기술을 사용하는데 매우 복잡하다.)

**AppConfig@CGLIB 예상 코드**

```java
@Bean
public MemberRepository memberRepository() {

    if (memoryMemberRepository가 이미 스프링 컨테이너에 등록 되어 있으면?) {
        return 스프링 컨테이너에서 찾아서 반환;
    } else { //스프링 컨테이너에 없으면
        기존 로직을 호출해서 MemoryMemberRepository를 생성하고 스프링 컨테이너에 등록
        return 반환
    }
}
```

- @Bean이 붙은 메서드마다 이미 스프링 빈이 존재하면 존재하는 빈을 반환하고, 스프링 빈이 없으면 생성해서 스프링 빈으로 등록하고 반환하는 코드가 동적으로 만들어진다.
- 덕분에 싱글톤이 보장됨.

> 참고: AppConfig@CGLIB은 AppConfig의 자식 타입으로, AppConfig 타입으로 조회 할 수 있다.
> 

**`@Configuration`을 적용하지 않고, `@Bean`만 적용하면 어떻게 될까?**

`@Configruration`을 붙이면 바이트코드를 조작하는 CGLIB 기술을 사용해서 싱글톤을 보장하지만, 만약 `@Bean`만 적용하면?

**AppConfig**

```java
package hello.core;

//@Configuration
public class AppConfig {
...    }
}
```

**ConfigurationSingletonTest**

`@Test`
`void configurationDeep()` **실행결과**

```java
bean.getClass() = class hello.core.AppConfig
```

- 이 출력 결과를 통해서 `AppConfig`가 `CGLIB` 기술 없이 순수한 `AppConfig`로 스프링 빈에 등록된 것을 확인할 수 있다.

`@Test`
`void configurationTest()` **실행결과**

```java
Call AppConfig.memberService
Call AppConfig.memberRepository
Call AppConfig.memberRepository
Call AppConfig.orderService
Call AppConfig.memberRepository
```

- `MemberRepository`가 총 3번 호출되어 1번은 `@Bean`에 의해 스프링 컨테이너에 등록하기 위해서이고, 2번은 각각 `memberRepository()`를 호출하면서 발생한 코드다.
- `@Configuration`을 제거함으로 싱글톤이 깨졌다.

**인스턴스가 같은지 테스트 결과**

```java
memberService -> memberRepository1 = hello.core.member.MemoryMemberRepository@1f760b47
oderService -> memberRepository2 = hello.core.member.MemoryMemberRepository@18ece7f4
memberRepository = hello.core.member.MemoryMemberRepository@3cce57c7
```

- 각각 다른 `MemoryMemberRepository` 인스턴스를 가지고 있다.

**정리**

- `@Bean`만 사용해도 스프링 빈으로 등록되지만, 싱글톤을 보장하지 않는다.
    - `memberRepository()`처럼 의존관계 주입이 필요해서 메서드를 직접 호출할 때 싱글톤을 보장하지 않는다.
- 설정정보는 `@Configuration`을 사용해야 싱글톤 보장이 된다.