[<back](https://www.notion.so/Spring-basic-41cfc2c136874d9896aa115923bd4109?pvs=21)

---

<aside>
📃

목차

</aside>

## 비즈니스 요구사항과 설계

- **회원**
    - 회원 **가입하고 조회** 가능
    - 회원은 **일반과 VIP 두가지 등급**
    - 회원 **데이터는 자체 DB를 구축할 수 있고, 외부 시스템과 연동 가능(미확정)**
- **주문과 할인 정책**
    - 회원 **상품 주문 가능.**
    - 회원 **등급**에 따라 **할인 정책 적용가능.**
    - 할인 정책은 모든 **VIP는 1000원을 할인해주는 고정 금액 할인을 적용해달라(나중에 변경가능)**
    - 할인 정책은 변경 가능성이 높다. 회사의 기본 할인 정책을 아직 정하지 못했고, 오픈 직전까지 고민을 미루고 싶다. 최악의 경우 할인을 적용하지 않을 수 있다.**(미확정)**

요구사항을 보면 회원 데이터, 할인 정책 같은 부분은 지금 결정하기 어려운 부분. 그렇다고 이런 정책이 결정될 때 까지 개발을 무기한 기다릴 수 없음. 객체 지향 설계로 설계하자.

인터페이스를 만들고 구현체를 언제든지 갈아끼울 수 있도록 설계하면 됨.

> **참고**: 프로젝트 환경설정을 편리하게 하려고 스프링 부트 사용. 지금은 스프링 없는 순수한 자바로만 개발을 진행한다.
> 

## 회원 도메인 설계

- 회원 도메인 요구사항
    - 회원 가입, 조회
    - 회원은 일반과 VIP 두가지 등급
    - 회원 데이터는 미확정(자체 DB, 외부 시스템)

**회원 도메인 협력 관계 → 기획자가 볼 수 있는 그림**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/e6504ac1-0f7f-4ed5-920b-77067ee766c1/2197d543-6955-42a0-830c-2475f82e6a62.png)

- 회원 서비스 인터페이스는 회원 저장소 인터페이스를 참조한다.
- 회원 저장소는 ‘메모리 회원 저장소’, ‘DB 회원 저장소’, ‘외부 시스템 연동 회원 저장소’로 구현하고, 필요시 인터페이스에 구현체만 갈아끼우는 형태로 구현한다.

**회원 클래스 다이어그램 → 개발자가 구체화한 그림(정적인 다이어그램)**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/c1e574e0-7ee6-4069-8c69-9241d3e33836/d65775a3-9692-441b-ba53-b17d866ef80d.png)

- `MemberService` 인터페이스를 구현한 `MemberServiceImpl` 은 `MemberRepository` 인터페이스를 참조한다.
- `MemebrRepository` 인터페이스에는 `MemoryMemberRepository` , `DbMemberRepository` 와 같은 구현체가 존재한다.

**회원 객체 다이어그램 → 실제 new로 참조한 인스턴스끼리 그림(동적인 다이어그램)**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/2ba0633a-d7eb-49a7-a03f-eb75d7826882/c5903f67-3f49-4e57-85a6-3126573208f3.png)

회원서비스: `MemebrServiceImpl`

## 회원 도메인 개발

개발 순서: 엔티티(Grade, Member) → 회원 저장소 (회원 저장소(인터페이스) → 메모리 회원 저장소(구현체)) → 서비스(회원 서비스(인터페이스)→ 회원 서비스`Impl`(구현체))

### 회원 엔티티

**Grade**

```java
package hello.core.member;

public enum Grade {
    BASIC,
    VIP
}
```

**Member**

```java
package hello.core.member;

public class Member {
    private Long id;
    private String name;
    private Grade grade;

    public Member1(Long id, String name, Grade grade) {
        this.id = id;
        this.name = name;
        this.grade = grade;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Grade getGrade() {
        return grade;
    }

    public void setGrade(Grade grade) {
        this.grade = grade;
    }
}
```

### 회원 리포지토리

**MemebrRepository(Interface)**

```java
package hello.core.member;

public interface MemberRepository {
    //member 객체를 인수로 받아 저장하는 기능 수행(return값 없으므로 void 사용)
    void save(Member member);
    
    //memberId를 인수로 받아 객체를 불러와서 Id를 반환.
    Member findById(Long MemberId);
}
```

**MemoryMemberRepository(Implementation)**

```java
package hello.core.member;

import java.util.HashMap;
import java.util.Map;

public class MemoryMemberRepository implements MemberRepository {

    Map<Long, Member> store = new HashMap<>();
    
    @Override
    //멤버 객체를 인수로 받으면, 맴버 객체의 아이디(key)와 객체(value)를 쌍으로 저장
    public void save(Member member) {
        store.put(member.getId(), member);
    }

    @Override
    //Long 타입의 숫자를 숫자를 받으면 store에서 해당 key값에 대응하는 객체 가져옴
    public Member findById(Long memberId) {
        return store.get(memberId);
    }
}
```

### 회원 서비스

**MemberService(Interface)**

```java
package hello.core.member;

public interface MemberService {
    //비즈니스 용어 사용: join
    void join(Member member);
    
    //비즈니스 용어 사용: findMember
	    Member findMember(Long memberId);
}
```

**MemberService(Implementation)**

```java
package hello.core.member;

public class MemberServiceImpl implements MemberService {

    //MemberRepository Interface 참조 -> implementation = MemoryMemberRepository
    MemberRepository memberRepository = new MemoryMemberRepository();

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```

## 회원 도메인 실행과 테스트

- **자바에서 테스트 하기**

`core/src/main/java/hello/core/member`

**MemberApp**

```java
package hello.core.member;

public class MemberApp {
    public static void main(String[] args) {
        MemberService memberService = new MemberServiceImpl();
        Member newMember = new Member(1L, "evan", Grade.VIP);
        memberService.join(newMember);

        Member findMember = memberService.findMember(1L);
        System.out.println("newMember = " + newMember.getName());
        System.out.println("findMember = " + findMember.getName());
    }
}
```

**실행결과**

```java
newMember = evan
findMember = evan
```

- **Junit으로 테스트 하기**

`core/src/test/java/hello/core/member`

**MemberServiceTest**

```java
package hello.core.member;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

public class MemberServiceTest {

    MemberService memberService = new MemberServiceImpl();

    @Test
    void join() {
        //given -> 메소드 실행을 위해 주어지는 값
        Member newMember = new Member(1L, "evan", Grade.VIP);

        //when -> 실제 메소드 실행
        memberService.join(newMember);
        Member findMember = memberService.findMember(1L);

        //then -> 메소드가 제대로 작동하는 지 확인
        Assertions.assertThat(newMember).isEqualTo(findMember);
    }
}
```

**실행결과**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9d412a00-1ea9-49f6-995d-244265b91be8/image.png)

만일 when을 아래와 같이 변경한다면?

```java
//when
Member findMember = memberService.findMember(2L);
```

**실행결과**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/2d97e757-971e-44f5-9902-ecb5be2136fc/image.png)

- 위처럼 오류가 발생함. 일일히 눈으로 값을 확인해가며, 대조해보지 않아도 테스트를 편리하게 진행가능

### 회원 도메인 설계의 문제점

- 이 코드의 설계상 문제점은 아래 두가지를 충족하지 못함
    - **OCP 원칙 위반**
        - 확장에 열려있고 **변경에 닫혀있어야한다.**
        - 그러나 `MemberServiceImpl`**의 경우,**
            
            `MemberRepository`의 구현체 변경 시, `new MemoryMemebrRepository`를 참조하고 있는 모든 객체의 코드를 변경해야함.
            
            ```java
            package hello.core.member;
            
            public class MemberServiceImpl implements MemberService {
            
                //MemberRepository Interface 참조 -> implementation = MemoryMemberRepository
                MemberRepository memberRepository = new MemoryMemberRepository();
            ```
            
    - **DIP 원칙 위반**
        - **역할(인터페이스)에 의존**해야한다.
        인터페이스에 의존해야지 구현 클래스에 의존하면 안된다.
        - 그러나 `MemberServiceImpl`**의 경우,**
            
            `MemberRepository(interface)`와 `MemoryMemeberRepository(implementation)` 모두 의존하고 있음.
            
            ```java
            package hello.core.member;
            
            public class MemberServiceImpl implements MemberService {
            
                //MemberRepository Interface 참조 -> implementation = MemoryMemberRepository
                MemberRepository memberRepository = new MemoryMemberRepository();
            ```
            
        - 의존관계가 인터페이스 뿐만 아니라 구현까지 모두 의존하는 문제가 있음.
    
    **→ 주문까지 만들고나서 문제점과 해결방안 설명**
    

## 주문과 할인 도메인 설계

- 주문과 할인 정책
    - 회원은 상품을 주문할 수 있다.
    - 회원 등급에 따라 할인 정책을 적용할 수 있다.
    - 할인 정책은 모든 VIP는 1000원 할인해주는 고정 금액 할인을 적용해달라.(나중에 변경 가능)
    - 할인 정책은 변경 가능성이 높다. 회사의 기본 정책을 아직 정하지 못했고, 오픈 직전까지 고민을 미루고있다. 최악의 경우 할인을 적용하지 않을 수 있다.(미확정)

**주문 도메인 협력, 역할, 책임**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/1c2fd19f-7eae-4af3-92e7-a1b7c3f301bf/ec9cf562-fe86-4b42-8087-25dc10c53a24.png)

1. **주문 생성**: 클라이언트는 주문 서비스에 주문 생성을 요청
2. **회원 조회**: 할인을 위해서는 회원 등급 필요. 그래서 주문 서비스는 회원 저장소에서 회원 조회
3. **할인 적용**: 주문 서비스는 회원 등급에 따른 할인 여부를 할인 정책에 위임.
4. **주문 결과 반환**: 주문 서비스는 할인 결과를 포함한 주문 결과를 반환

> 참고: 실제로는 주문 데이터를 DB에 저장하나, 예제가 너무 복잡해질 수 있어서 생략, 단순 주문 결과 반환
> 

**주문 도메인 전체**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/6b3ac91f-90b5-451b-bb03-9b73d4d5b1b9/05aa6e2d-b86f-4786-8b97-9baa3ab563e8.png)

**역할과 구현을 분리**해서 자유롭게 구현 객체를 조립할 수 있게 설계.
덕분에 회원 저장소는 물론, 할인 정책도 유연하게 변경할 수 있다.

(회원 저장소 역할 interface 아래, 메모리 회원 저장소 implementation, DB 회원 저장소 implementation)

(할인 정책 역할 interface 아래, 정액 할인 정책 implementation, 정률 할인 정책 implementation)

따라서, 기획자의 니즈에 맞추서 바꿔 끼우기만 하면 되게끔 설계함.

→ 객체 지향 프로그래밍!!!!

**주문 도메인 클래스 다이어그램**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/84819c87-9601-4cd1-9bd1-1bc7b9b6a955/2708114b-5e76-44d5-8249-f1a8b466f281.png)

- `OrderService(interface)` 아래 `OrderServiceImpl(implementation)`
- `OrderServiceImpl` 은 `MemberRepository(interface)`, `DiscountPolicy(interface)` 참조
    - interface `MemberRepository`
        - implementation 
        `MemoryMemberRepository`
        `DbMemberRepository implementation`
    - interface `DiscountPolicy`
        - implementation
        `FixDiscountPolicy`
            
            `RateDiscountPolicy`
            

**주문 도메인 객체 다이어그램1**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f9681745-6601-48ef-bce1-ea75a611944a/dee28f2e-8d58-4a11-8934-3c9ed31ff7c2.png)

회원을 메모리에서 조회, 정액 할인 정책(고정 금액)을 지원해도 주문서비스를 변경하지 않아도 된다.

역할들의 협력 관계를 그대로 재사용 가능.

**주문 도메인 객체 다이어그램2**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/25ef053a-1dda-4588-b1eb-ec665808ea04/0d0214f1-b276-41fb-b4bb-970dcff23553.png)

회원을 메모리가 아닌 실제 DB에서 조회하고, 정률 할인 정책(주문 금액에 따라 % 할인)을 지원해도 주문 서비스를 변경하지 않아도 된다.

**협력 관계를 그대로 재사용할 수 있다.**

> 즉, 주문 서비스 구현체가 참조하는 `interface`에서 사용되는 메서드나 코드를 바꿀 필요없이 단순히 각 `interface`들의 구현체만 갈아끼우면 주문 서비스 구현체가 동작하고 클라이언트는 해당 결과를 받아볼 수 있음.
> 

## 주문과 할인 도메인 개발

**Order**

```java
package hello.core.order;

public class Order {
    private Long MemberId;
    private String itemName;
    private int itemPrice;
    private int discountPrice;

    public Order(Long memberId, String itemName, int itemPrice, int discountPrice) {
        MemberId = memberId;
        this.itemName = itemName;
        this.itemPrice = itemPrice;
        this.discountPrice = discountPrice;
    }

    public int calculatePrice() {
        return itemPrice - discountPrice;
    }

    public Long getMemberId() {
        return MemberId;
    }

    public void setMemberId(Long memberId) {
        MemberId = memberId;
    }

    public String getItemName() {
        return itemName;
    }

    public void setItemName(String itemName) {
        this.itemName = itemName;
    }

    public int getItemPrice() {
        return itemPrice;
    }

    public void setItemPrice(int itemPrice) {
        this.itemPrice = itemPrice;
    }

    public int getDiscountPrice() {
        return discountPrice;
    }

    public void setDiscountPrice(int discountPrice) {
        this.discountPrice = discountPrice;
    }

    @Override
    public String toString() {
        return "Order{" +
                "MemberId=" + MemberId +
                ", itemName='" + itemName + '\'' +
                ", itemPrice=" + itemPrice +
                ", discountPrice=" + discountPrice +
                '}';
    }
}
```

**OrderService**

```java
package hello.core.order;

public interface OrderService {
    Order createOrder(Long memberId, String itemName, int itemPrice);
}
```

**OrderServiceImpl**

```java
package hello.core.order;

import hello.core.discount.DiscountPolicy;
import hello.core.member.Member;
import hello.core.member.MemberRepository;

public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository = new MemoryMemberRepository;
    private final DiscountPolicy discountPolicy = new FixDiscountPolicy;

    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int discountPrice = discountPolicy.discount(member, itemPrice);

        return new Order(memberId, itemName, itemPrice, discountPrice);
    }
}
```

`core/src/main/java/hello/core/OrderApp.java`

**OrderApp**

```java
package hello.core;

import hello.core.member.*;
import hello.core.order.Order;
import hello.core.order.OrderService;
import hello.core.order.OrderServiceImpl;

public class OrderApp {
    public static void main(String[] args) {
        MemberService memberService = new MemberServiceImpl();
        OrderService orderService = new OrderServiceImpl();

        Long memberId = 1L;
        Member member = new Member(memberId, "memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "itemA", 10000);

        System.out.println("order = " + order);
        System.out.println("order.calculatePrice = " + order.calculatePrice());
    }
}
```

`core/src/test/java/hello/core/order/OrderServiceTest.java`

**OrderServiceTest**

```java
package hello.core.order;

import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

public class OrderServiceTest {
    MemberService memberService = new MemberServiceImpl();
    OrderService orderService = new OrderServiceImpl();

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

**DiscountPolicy**

```java
package hello.core.discount;

import hello.core.member.Member;

public interface DiscountPolicy {
    /**
     * @return 할인 대상 금액
     */
    int discount(Member member, int price);
}
```

**FixedDiscountPolicy**

```java
package hello.core.discount;

import hello.core.member.Grade;
import hello.core.member.Member;

public class FIxDiscountPolicy implements DiscountPolicy {

    private int DiscountFixAmount = 1000;

    @Override
    public int discount(Member member, int price) {
        if (member.getGrade() == Grade.VIP) {
            return DiscountFixAmount;
        } else {
            return 0;
        }
    }
}
```