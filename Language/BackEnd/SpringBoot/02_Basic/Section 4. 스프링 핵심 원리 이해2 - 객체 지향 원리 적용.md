[<back](https://www.notion.so/Spring-basic-41cfc2c136874d9896aa115923bd4109?pvs=21)

---

<aside>
📃

목차

</aside>

---

## 새로운 할인 정책 개발

**새로운 할인 정책을 확장해보자**

- **악덕 기획자**: 서비스 오플 직전에 할인 정책을 지금처럼 고정 할인이 아니라 좀 더 합리적인 주문 금액당 할인하는 정률% 할인으로 변경하고 싶어요. 예를 들어서 기존 정책은 VIP가 10000원을 주문하든 20000원을 주문하든 항상 1000원을 할인했는데, 이번에 새로나온 정책은 10% 지정해두면 고객이 10000원을 주문하면  1000원을 할인해주고, 20000원을 주문하면 2000원을 할인해주는 거예요.
- **순진 개발자**: 제가 처음부터 고정 할인은 아니라고 했잖아요.
- **악덕 기획자**: 애자일 소프트웨어 개발 선언 몰라요? “계획을 따르기보다 변화에 대응하기를”
- **순진 개발자**: …(하지만 난 유연한 설계가 가능하도록 객체지향 설계 원칙을 준수했지 후후)

> 참고: 애자일 소프트웨어 개발 선언 (https://agilemanifesto.org/iso/ko/manifesto.html)
> 

**RateDiscountPolicy 추가**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/95e639bd-4b1f-42ba-b96a-3501842c101c/dad9f606-3e7f-4efb-9925-338e45810b1d.png)

- 정률할인 구현체 만들기

`core/src/main/java/hello/core/discount/RateDiscountPolicy.java`

**RateDiscountPolicy**

```java
package hello.core.discount;

import hello.core.member.Grade;
import hello.core.member.Member;

public class RateDiscountPolicy implements DiscountPolicy {

    private int discountPercent = 10;

    @Override
    public int discount(Member member, int price) {
        if (member.getGrade() == Grade.VIP) {
            return price * discountPercent / 100;
        } else {
            return 0;
        }
    }
}
```

- 정률할인 구현체 테스트(VIP할인 되는 경우, 안되는 경우, BASIC의 경우)

`core/src/test/java/hello/core/discount/RateDiscountPolicyTest.java`

**RateDiscountPolicyTest**

```java
package hello.core.discount;

import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.*;

class RateDiscountPolicyTest {

    RateDiscountPolicy rateDiscountPolicy = new RateDiscountPolicy();
    MemberService memberService = new MemberServiceImpl();

    @Test
    @DisplayName("VIP는 10%할인이 적용되어야 한다.")
    void vip_o() {
        //given
        Member member = new Member(1L, "memberVIP", Grade.VIP);

        //when
        int discount = rateDiscountPolicy.discount(member, 10000);

        //then
        assertThat(discount).isEqualTo(1000);
    }

    @Test
    @DisplayName("VIP가 아니면 할인이 적용되지 않아야 한다.")
    void vip_x() {
        //given
        Member member = new Member(1L, "memberBASIC", Grade.BASIC);

        //when
        int discount = rateDiscountPolicy.discount(member, 10000);

        //then
        assertThat(discount).isEqualTo(0);
    }
}
```

## 새로운 할인 정책 적용과 문제점

방금 추가한 할인 정책 적용

### 할인 정책 애플리케이션에 적용

할인 정책을 변경하려면 `OrderServiceImpl` 코드를 고쳐야 한다.

```java
public class OrderServiceImpl implements OrderService {

    MemberRepository memberRepository = new MemoryMemberRepository();
    //DiscountPolicy discountPolicy = new FIxDiscountPolicy();
    DiscountPolicy discountPolicy = new RateDiscountPolicy();
...
}
```

**문제점 발견**

- 우리는 역할과 구현을 충실하게 분리했다 → OK
- 다형성도 활용하고, 인터페이스와 구현 객체를 분리함 → OK
- OCP(개방 폐쇄 원칙_Open/Closed Principle), DIP(의존관계 역전 원칙_Dependency Inversion Principle)같은 객체지향 설계 원칙을 충실히 준수했다.
    
    → 그렇게 보이지만 사실 아니다.
    
    - DIP: 주문서비스 클라이언트(`OrderServiceImpl`)는 `DiscountPolicy` 인터페이스를 의존하면서 DIP를 지킨것 같지만,
        - 클래스 의존관계를 분석해보면, 추상(인터페이스) 뿐만 아니라, **구현체(구현) 클래스에도 의존** 하고 있다.
        - 구체(구현) 클래스 : `FixDiscountPolicy`, `RateDiscountPolicy`
    - OCP: 변경에는 닫히고, 확장에는 열려있어야 하는데…
        
        **→ 지금 코드는 기능을 확장하려면, 클라이언트 코드에 영향(변경)을 준다!** 따라서 **OCP위반**
        

**왜 클라이언트 코드를 변경해야할까?**

클래스 다이어그램으로 의존관계를 분석해보자.

**기대했던 의존관계**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/00ea3b84-5591-4465-8402-08b75748ee9f/edc2aef8-e9f1-4333-9d70-399377a22691.png)

지금까지 단순히 `DiscountPolicy` 인터페이스만 의존한다고 생각함.

**실제 의존관계**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/66ea0ea4-d070-4c6a-b95b-56aea8252f94/43db6e65-282e-4508-a006-5a48783070cf.png)

```java
public class OrderServiceImpl implements OrderService {

    MemberRepository memberRepository = new MemoryMemberRepository();
    //DiscountPolicy discountPolicy = new FIxDiscountPolicy();
    DiscountPolicy discountPolicy = new RateDiscountPolicy();
...
}
```

실제로는 `OrderServiceImpl` 이 `DiscountPolicy` 인터페이스 뿐만 아니라 `FixDiscountPolicy` 
(구현체) 클래스도 함께 의존하고 있다. 실제 코드를 보면 → **DIP 위반!!**

**정책 변경**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/2713f842-282f-4efe-bcb8-cff3d64384a3/e3383291-5ef4-4629-9d40-2264a96c068d.png)

**중요!!** 그래서, `FixDiscountPolicy`를 `RateDiscountPolicy`로 변경하는 순간 `OrderServiceImpl`의
소스 코드도 함께 변경해야한다!  → **OCP 위반!!**

### 어떻게 문제를 해결할 수 있을까?

- 클라이언트 코드인 `OrderServiceImpl`은 `DiscountPolicy`의 인터페이스 뿐만 아니라 구체 클래스도 함께 의존한다.
- 그래서 구체 클래스를 변경할 때 클라이언트 코드도 함께 변경해야 한다.
- **DIP위반** → 추상에만 의존하도록 변경(인터페이스에만 의존)
- DIP를 위반하지 않도록 인터페이스에만 의존하도록 의존관계 변경.

**인터페이스에만 의존하도록 설계 변경**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/d2255977-c2dc-4107-ae5b-239476fff560/70b641a3-f362-46a3-9f1b-f905508a7c44.png)

**인터페이스만 의존하도록 코드 변경**

```java
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository = new MemoryMemberRepository();
    private DiscountPolicy discountPolicy;
...
}
```

**실행결과**

```java
java.lang.NullPointerException: Cannot invoke "hello.core.discount.DiscountPolicy.discount(hello.core.member.Member, int)" because "this.discountPolicy" is null

	at hello.core.order.OrderServiceImpl.createOrder(OrderServiceImpl.java:18)
	at hello.core.order.OrderServiceTest.createOrder(OrderServiceTest.java:22)
	at java.base/java.lang.reflect.Method.invoke(Method.java:568)
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1511)
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1511)

Process finished with exit code 255
```

**NullPointerException이 나오는 이유**

- `OderService`는 인터페이스이기 때문에 `createOrder` 메서드가 `null` 값을 갖고 있기 때문.

`core/src/main/java/hello/core/order/OrderService.java`

**OrderService**

```java
package hello.core.order;

public interface OrderService {
    Order createOrder(Long memberId, String itemName, int itemPrice);
}
```

- **즉, 인터페이스에만 의존하도록 설계와 코드를 변경했는데 구현체가 없어서 실행이 불가.**

**해결방안**

- 이 문제를 해결하려면 누군가가 클라이언트인 `OderServiceImpl`에 `DiscountPolicy`의 구현 객체를 대신 생성하고 주입해주어야 한다.

## 관심사의 분리

- 애플리케이션을 하나의 공연이라 생각해보자. 각각의 인터페이스를 배역(배우 역할)이라 생각하자. 그런데! 실제 배역에 맞는 배우를 선택하는 것은 누가 하는가?
- 로미오와 줄리엣 공연을 하면 로미오 역할을 누가 할지 줄리엣 역할을 누가할지 배우들이 정하는 것이 아니다. 이전 코드는 마치 **로미오 역할(인터페이스)을 하는 라이언 고슬링(구현체, 배우)**가 **줄리엣 역할(인터페이스)을 하는 여자 주인공(구현체 , 배우)를 직접 초빙**하는 것과 같다.
- 라이언 고슬링은 공연도 해야하고 동시에 여자 주인공도 공연에 직접 초빙해야하는 **다양한 책임**을 가지고 있다.
    
    ```java
    //OrderServiceImpl -> 라이언 고슬링
    public class OrderServiceImpl implements OrderService {
        //라이언 고슬링이 줄리엣 역할(인터페이스) 선택 = 맥아담스(구현체)
        private final MemberRepository memberRepository = new MemoryMemberRepository();
        //라이언 고슬링이 아버지 역할(인터페이스) 선택 = 드웨인 존슨(구현체)
        private DiscountPolicy discountPolicy = new FixdiscountPolicy();
    ...
    }
    ```
    
- 위와 같은 설계는 **DIP(Dependency Inversion Policy_의존관계 역전 원칙)** 를 위반.
    
    > **DIP: 역할(추상, 인터페이스)에 의존해야지 구현(구체, 구현체)에 의존해서는 안된다.**
    > 
    

**관심사를 분리하자**

- 배우는 본인의 역할인 배역을 수행하는 것에만 집중해야 한다.
- 라이언 고슬링은 어떤 여자 주인공이 선택되더라도 똑같이 공연을 할 수 있어야 한다.
- 공연을 구성하고, 담당 배우를 섭외하고, 역할에 맞는 배우를 지정하는 책임을 담당하는 별도의 **공연 기획자**가 나올 시점
- 공연 기획자를 만들고, 배우와 공연 기획자의 책임을 확실히 분리.

### AppConfig 등장

> Configuration(Config): 구성, 설정
> 
- 애플리케이션의 전체 동작 방식을 구성(config)하기 위해, **구현 객체를 생성**하고 **연결**하는 책임을 가지는 별도의 설정 클래스 만들기

**AppConfig**

```java
public class AppConfig {
    public MemberService memberService() {
        return new MemberServiceImpl(new MemoryMemberRepository());
    }

    public OrderService orderService() {
        return new OrderServiceImpl(new MemoryMemberRepository(), new FIxDiscountPolicy());
    }
}
```

- AppConfig는 애플리케이션의 실제 동작에 필요한 **구현 객체를 생성** 한다.
    - `MemberServiceImpl`
    - `MemoryMemberRepository`
    - `OrderServiceImpl`
    - `FixdiscountPolicy`
- AppConfig는 생성한 객체 인스턴스의 참조(레퍼런스)를 **생성자를 통해서 주입(연결)** 해준다.
    - `MemberServiceImpl` → `MemoryMemberRepository`
    - `OrderServiceImpl` → `MemoryMemberRepository` , `DiscountPolicy`

> 참고: 지금은 각 클래스에 생성자가 없어서 컴파일 오류가 발생한다.
> 

**MemberServiceImpl - 생성자 주입**

```java
package hello.core.member;

public class MemberServiceImpl implements MemberService {

    //회원 가입과 조회를 하려면 MemberRepository interface 필요
    private final MemberRepository memberRepository;

    public MemberServiceImpl(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long MemberId) {
        return memberRepository.findById(MemberId);
    }
}
```

- 설계 변경으로 `MemberServiceImpl`은 `MemoryMemberRepository`를 의존하지 않는다.
- 단지 `MemberRepository` 인터페이스만 의존.
- `MemberServiceImpl` 입장에서 생성자를 통해 어떤 구현 객체가 들어올지(주입될지)알 수 없다.
- `MemberServiceImpl`의 생성자를 통해 어떤 구현 객체를 주입할 지 오직 외부(’`AppConfig`’)에서 결정된다.
- `MemberServiceImpl`은 이제부터 의존관계에 대한 고민은 외부에 맡기고 실행에만 집중!

**그림 - 클래스 다이어그램**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/46b8e9c3-6600-48d0-9e9d-d0211b5c034a/105ca856-7a3d-48dd-8d06-61d222114fef.png)

- 객체의 생성과 연결은 AppConfig가 담당
- **DIP완성**: MemberServiceImpl은 MemberReopository인 추상에만 의존. 이제 구체 클래스를 몰라도 된다.
- **관심사 분리**: 객체를 생성하고 연결하는 역할과 실행하는 역할이 명확히 분리됨.

**그림 - 회원 객체 인스턴스 다이어그램**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/09f469bd-e3e9-472f-ae2e-02cffa60a6ee/image.png)

- `appConfig` 객체는 `memoryMemberRepository` 객체를 생성하고 그 참조값을 `memberServiceImpl` 인터페이스를 생성하면서 생성자로 전달.
- 클라이언트인 `memberServiceImpl` 입장에서 보면 의존관계를 마치 외부에서 주입해주는 것과 같다고 해서 **DI(Dependency injection)** 우리말로 의존관계 주입, 의존성 주입 이라고 한다.

**OderServiceImpl - 생성자 주입**

```java
package hello.core.order;

public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

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

- 설계변경으로 `OderServiceImpl`은 `FixDiscountPolicy`를 의존하지 않는다.
- 단지 `DiscountPolicy` 인터페이스만 의존.
- `OrderServiceImpl` 입장에서 생성자를 통해 어떤 구현 객체가 들어올지(주입될지) 알 수 없다.
- `OrderServiceImpl`의 생성자를 통해서 어떤 구현 객체를 주입할지는 오직 외부(’`AppConfig`’)에서 결정한다.
- `OrderServiceImpl`은 이제부터 실행에만 집중하면 된다.
- `OrderServiceImpl`에는 `MemoryMemberRepository`, `FixDiscountPolicy` 개체의 의존관계가 주입된다.

## AppConfig 실행

**사용클래스 - MemberApp**

```java
package hello.core.member;

public class MemberApp {

    public static void main(String[] args) {
        AppConfig appConfig = new AppConfig();
        MemberService memberService = appConfig.memberService();
        //MemberService memberService = new MemberServiceImpl();
        Member newMember = new Member(1L, "evan", Grade.VIP);
        memberService.join(newMember);

        Member findMember = memberService.findMember(1L);
        System.out.println("newMember = " + newMember.getName());
        System.out.println("findMember = " + findMember.getName());
    }
}
```

**사용클래스 - OrderApp**

```java
package hello.core;

public class OrderApp {
    public static void main(String[] args) {
        AppConfig appConfig = new AppConfig();
        OrderService orderService = appConfig.orderService();
        MemberService memberService = appConfig.memberService();

        //MemberService memberService = new MemberServiceImpl(null);
        //OrderService orderService = new OrderServiceImpl(null, null);

        Long memberId = 1L;
        Member member = new Member(memberId, "memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "itemA", 10000);

        System.out.println("order = " + order);
        System.out.println("order.calculatePrice = " + order.calculatePrice());
    }
}
```

**Test - MemberServiceTest**

```java
package hello.core.member;

public class MemberServiceTest {

    MemberService memberService;

    @BeforeEach
    public void beforeEach() {
        AppConfig appConfig = new AppConfig();
        memberService = appConfig.memberService();
    }

    @Test
    void join() {
        //given
        Member newMember = new Member(1L, "evan", Grade.VIP);

        //when
        memberService.join(newMember);
        Member findMember = memberService.findMember(1L);

        //then
        Assertions.assertThat(newMember).isEqualTo(findMember);
    }
}
```

**Test - OrderServiceTest**

```java
package hello.core.order;

public class OrderServiceTest {
    MemberService memberService;
    OrderService orderService;

    @BeforeEach
    void beforeEach() {
        AppConfig appConfig = new AppConfig();
        memberService = appConfig.memberService();
        orderService = appConfig.orderService();
    }

    @Test
    void createOrder() {
        //given
        Long memberId = 1L;
        Member member = new Member(memberId, "memberA", Grade.VIP);

        //when
        memberService.join(member);
        Order order = orderService.createOrder(memberId, "itemA", 10000);

        //then
        Assertions.assertThat(order.getDiscountPrice()).isEqualTo(1000);
    }
}
```

테스트 코드에서 `@BeforeEach`는 각 테스트를 실행하기 전에 호출됨.

**정리**

- `AppConfig`를 통해 관심사를 확실하게 분리
- 배역, 배우를 생각해보자.
- `AppConfig`는 공연 기획자.
- `AppConfig`는 구체 클래스를 선택, 배역에 맞는 담당 배우를 선택. 애플리케이션이 어떻게 동작해야할지 전체 구성을 책임진다.
- 이제 각 배우들은 담당 기능을 실행하는 책임만 지면 된다.
- `OrderServiceImpl`은 기능을 실행하는 책임만 지면 된다.

## AppConfig 리펙터링

현재 `AppConfig`를 보면 **중복**이 있고, **역할**에 따른 **구현**이 잘 안보인다.

- 아래 기대하는 그림처럼 코드에서 역할과 구현이 분리되어 역할이 드러나는 게 가장 중요.

**기대하는 그림**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/308374e6-5713-46ce-828b-b3fe97845cff/2bb05efa-5699-4a28-afe9-1056c5399175.png)

**리팩터리 전**

- `AppConfig`의 역할이 불분명함.

```java
package hello.core;

public class AppConfig {
    public MemberService memberService() {
        return new MemberServiceImpl(new MemoryMemberRepository());
    }

    public OrderService orderService() {
        return new OrderServiceImpl(new MemoryMemberRepository(), new FIxDiscountPolicy());
    }
}
```

**리팩터링 후**

```java
package hello.core;

public class AppConfig {
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    private static MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    private static DiscountPolicy discountPolicy() {
        return new FIxDiscountPolicy();
    }
}
```

- 각 인터페이스들의 역할이 드러나고, 설정 변경 시 필요한 부분만 변경하면 된다.
    - ex) `MemberRepository`의 경우,
    `return new MemoryMemberRepository();` 로 어떤 `Repository`를 사용하고 있는지 확인 가능
- `new MemoryMemberRepository` 중복이 있었는데 이 부분이 제거됨.
- `MemoryMemberRepository`를 다른 구현체로 변경할 때 부분만 변경하면 된다.
- `AppConfig`를 보면 역할과 구현 클래스가 한눈에 들어온다. 애플리케이션 전체 구성이 어떻게 되어있는지 빠르게 파악할 수 있다.

## 새로운 구조와 할인 정책 적용

- 처음으로 돌아가서 정액 할인 정책을 정률% 할인 정책으로 변경.
- FixDiscountPolicy → RateDiscountPolicy
- 어떤 부분만 변경하면 되겠는가?

**AppConfig의 등장으로 애플리케이션이 크게 사용 영역과, 객체를 생성하고 구성(Configuration)하는 영역으로 분리되었다.**

**그림 - 사용, 구성의 분리**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/adfb19ae-5371-49b7-87e4-731f4df81a67/461a301b-9436-482a-9f95-4eaf36e43018.png)

**그림 - 할인 정책의 변경**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/8f1080e0-d039-40f3-ae87-ece816099ace/bcb9a36d-b025-44b8-9897-28a13b4bf7e6.png)

- `FixDiscountPolicy` → `RateDiscountPolicy`로 변경해도 구성 영역만 영향을 받고, 사용 영역은 전혀 영향을 받지 않는다.

**할인 정책 변경 구성 코드**

**AppConfig**

```java
package hello.core;

public class AppConfig {
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    private static MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    private static DiscountPolicy discountPolicy() {
//        return new FIxDiscountPolicy();
        return new RateDiscountPolicy();
    }
}
```

- `AppConfig`에서 할인 정책 역할을 담당하는 구현을 `FixDiscountPolicy` → `RateDiscountPolicy` 객체로 변경했다.
- 이제 할인 정책을 변경해도, 애플리케이션의 구성 역할을 담당하는 `AppConfig`만 변경하면 된다.
클라이언트 코드인 `OrderServiceImpl`를 포함해서 사용영역의 어떤 코드도 변경할 필요가 없다.
- 구성영역은 당연히 변경된다. 구성 역할을 담당하는 `AppConfig`를 애플리케이션이라는 공연의 기획자로 생각하자. 공연 기획자는 공연 참여자인 구현 객체를 모두 알아야한다.

> 이로인해 OCP(확장에는 열려있고 변경에는 닫혀있음), DIP(추상에 의존)을 모두 만족하는 코드를 완성함.
OCP → 기획 변경 시 AppConfig에서 변경된 인터페이스로 생성하는 것만 바꿔주면 됨.
DIP → `impl` 형태의 객체들은 모두 추상(인터페이스)에 의존하고 있음.
> 

## 좋은 객체지향 설계의 5가지 원칙의 적용

여기서 3가지 SRP, DIP, OCP 적용

### SRP 단일 책임 원칙

**한 클래스는 하나의 책임만 가져야 한다.**

- 클라이언트 객체는 직접 구현 객체를 생성하고, 연결하고, 실행하는 다양한 책임을 가지고 있음.
- SRP 단일 책임 원칙을 따르면서 관심사를 분리함
- 구현 객체를 생성하고 연결하는 책임은 AppConfig가 담당
- 클라이언트 객체는 실행하는 책임만 담당

### DIP 의존관계 역전 원칙

**프로그래머는 “추상화에 의존해야지, 구체화에 의존하면 안된다.” 의존성 주입은 이 원칙을 따르는 방법 중 하나다.**

- 새로운 할인 정책을 개발하고, 적용하려고 하니 클라이언트 코드도 함께 변경해야 했다. 왜냐하면 기존 클라이언트 코드(`OrderServiceImpl`)는 DIP를 지키며 `DiscountPolicy` 추상화 인터페이스에 의존하는 것 같았지만, `FixDiscountPolicy` 구체화 구현 클래스에도 함께 의존.
- 클라이언트 코드가 `DiscountPolicy` 추상화 인터페이스에만 의존하도록 코드를 변경
- 하지만 클라이언트 코드는 인터페이스만으로 아무것도 할 수 없다.
- `AppConfig`가 `FixDiscountPolicy` 객체 인스턴스를 클라이언트 코드 대신 생성해서 클라이언트 코드에 의존관계를 주입했다. 이렇게해서 DIP 원칙을 따르면서 문제도 해결.

### OCP 확장/폐쇄 원칙

**소프트웨어 요소는 확장에 열려 있으나 변경에는 닫혀 있어야 한다.**

- 다형성 사용하고 클라이언트가 DIP를 지킴.
- 애플리케이션을 사용 영역과 구성 영역으로 나눔.
- `AppConfig`가 의존관계를 `FixDiscountPolicy` → `RateDiscountPolicy`로 변경해서 클라이언트 코드에 주입하므로 클라이언트 코드는 변경하지 않아도 됨.
- **소프트웨어 요소를 새롭게 확장해도 사용 영역의 변경은 닫혀있다.**

## IoC, DI, 그리고 컨테이너

### 제어의 역전 IoC(Inversion of Control)

- 기존 프로그램은 크라이언트 구현 객체가 스스로 필요한 서버 구현 객체를 생성, 연결, 실행했다.
한마디로 구현 객체가 프로그램의 제어 흐름을 스스로 조종. 개발자 입장에서는 자연스러운 흐름.
- 반면 AppConfig 등장으로 구현 객체는 자신의 로직을 실행하는 역할만 담당.
프로그램의 제어 흐름은 이제 AppConfig가 가져간다.
예를 들어 `OrderServiceImpl` 은 필요한 인터페이스들을 호출하지만 어떤 구현 객체들이 실행될지 모른다.
- 프로그램에 대한 제어 흐름에 대한 권한은 모두 AppConfig가 가지고 있다.
심지어 `OrderServiceImpl` 도 AppConfig가 생성한다.
그리고 AppConfig는 `OrderServiceImpl` 이 아닌 `OrderService` 인터페이스의 다른 구현 객체를 생성하고 실행할 수 도 있다.
그런 사실도 모른체 `OrderserviceImpl`은 묵묵히 자신의 로직을 실행할 뿐.
- 이렇듯 프로그램 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것을 제어의 역전(IoC)라 한다.

**프레임워크 vs 라이브러리**

- 기준: 제어권이 누구에게 있는가?
    - 제어권이 나한테 있다 → 라이브러리
    - 제어권이 나한테 없다 → 프레임워크
- 프레임워크 → JUnit: `@Test`애너테이션을 이용해 코드를 작성하면 JUnit 제어권은 프레임 워크에 있고, 프레임워크가 실행되면서 해당 애너테이션을 보고 로직을 콜백하여 실행시켜줌.
- 라이브러리 → 데이터를 XML, JSON으로 바꾸기 위해 라이브러리를 이용한다면 해당 라이브러리를 직접 호출하여 사용하므로 제어권이 나에게 있음.

### 의존관계 주입 DI

- `OrderServiceImpl`은 `DiscountPolicy` 인터페이스에 의존.
실제 어떤 구현체가 사용될지 모른다.
- 의존관계는 정적인 클래스 의존 관계와, 실행 시점에 결정되는 동적인 객체(인스턴스) 의존 관계 둘을 분리해서 생각해야한다.

**정적인 클래스 의존관계**

클래스가 사용하는 `import` 코드만 보고 의존관계를 쉽게 판단할 수 있다. 정적인 의존관계는 애플리케이션을 실행하지 않아도 분석할 수 있다. 클래스 다이어그램을 보자.

`OrderService`은 `MemberRepository`, `DiscountPolicy`에 의존한다는 것을 알 수 있다.

그런데 이러한 클래스 의존관계만으로 실제 실행 시점에서 어떤 객체가 `OrderServiceImpl`에 주입 될지 알 수 없다.

**클래스 다이어그램(정적인 클래스 의존관계)**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/bd5dfe43-ceef-4851-9e42-ee52febfd1c2/e80ea22d-5855-46b1-9f21-d5a21ab72384.png)

**동적인 객체 인스턴스 의존관계**

애플리케이션 실행 시점에 실제 생성된 객체 인스턴스의 참조가 연결된 의존 관계다.

**객체 다이어그램(동적인 클래스 의존관계)**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/502f991f-9350-421a-9fcf-d501054f8e9f/9ea678e3-8751-4b70-b07d-790fd8149b05.png)

- 애플리케이션 **실행 시점(런타임)**에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결 되는 것을 **의존관계 주입**이라 한다.
- 객체 인스턴스를 생성하고, 그 참조값을 전달해서 연결된다.
- 의존관계 주입을 사용하면 클라이언트 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있다.
- **의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다.**

**IoC 컨테이너, DI 컨테이너**

- AppConfig 처럼 객체를 생성하고 관리하면서 의존관계를 연결해 주는 것을 IoC 컨테이너 또는 **DI 컨테이너** 라 한다.
- 의존관계 주입에 초점을 맞추어 최근에는 주로 DI 컨테이너라 한다.
- 또는 어셈블러, 오브젝트 팩토리 등으로 불리기도 한다.

## 스프링으로 전환하기

지금까지 순수한 자바 코드만으로 DI를 적용했다. 이제 스프링을 사용해보자.

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

**MemberApp**

```java
package hello.core.member;

public class MemberApp {

    public static void main(String[] args) {

        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
        MemberService memberService = applicationContext.getBean("memberService", MemberService.class);

        Member newMember = new Member(1L, "evan", Grade.VIP);
        memberService.join(newMember);

        Member findMember = memberService.findMember(1L);
        System.out.println("newMember = " + newMember.getName());
        System.out.println("findMember = " + findMember.getName());
    }
}
```

**OrderApp**

```java
package hello.core;

public class OrderApp {
    public static void main(String[] args) {

        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
        OrderService orderService = applicationContext.getBean("orderService", OrderService.class);
        MemberService memberService = applicationContext.getBean("memberService", MemberService.class);

        Long memberId = 1L;
        Member member = new Member(memberId, "memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "itemA", 20000);

        System.out.println("order = " + order);
        System.out.println("order.calculatePrice = " + order.calculatePrice());
    }
}
```

- 두 코드를 실행하면 스프링 관련 로그가 몇줄 실행되면서 기존과 동일한 결과가 출력된다.

**스프링 컨테이너**

- `ApplicationContext`를 스프링 컨테이너라 한다.
- 기존에는 개발자가 `AppConfig`를 사용해서 직접 객체를 생성하고 DI를 했지만, 이제부터는 스프링 컨테이너를 통해서 사용한다.
- 스프링 컨테이너는 `@Configuration`이 붙은 `AppConfig`를 설정(구성) 정보로 사용한다. 여기서 `@Bean`이라 적힌 메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록한다. 이렇게 스프링 컨테이너에 등록된 객체를 스프링 빈이라 한다.
- 스피링 빈은 `@Bean`이 붙은 메서드의 명을 스프링 빈의 이름으로 사용한다(`memberService`, `orderService`)
- 이전에는 개발자가 필요한 객체를 `AppConfig`를 사용해서 직접 조회했지만, 이제부터는 스프링 컨테이너를 통해서 필요한 스프링 빈(객체)를 찾아야한다. 스프링 빈은 `appicationContext.getBean()` 메서드를 사용해서 찾을 수 있다.
- 기존에는 개발자가 직접 자바코드로 모든 것을 했다면 이제부터 스프링 컨테이너에 객체를 스프링 빈으로 등록하고, 스프링 컨테이너에서 스프링 빈을 찾아서 사용하도록 변경되었다.

- 코드가 약간 더 복잡해진 것 같은데, 스프링 컨테이너를 사용하면 어떤 장점이 있을까?