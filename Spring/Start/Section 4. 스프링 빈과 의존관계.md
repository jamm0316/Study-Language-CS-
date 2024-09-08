[<back](https://www.notion.so/Spring-2cf44a3f25f542dfbcc88e2390cee6e3?pvs=21)

---

<aside>
📃 목차

</aside>

---

## 스프링 빈을 등록하고, 의존관계 설정하기

회원 컨트롤러가 회원서비스와 회원 리포지토리를 사용할 수 있게 의존관계를 준비.

**회원 컨트롤러에 의존관계 추가**

```java
package hello.hello_spring.controller;

import hello.hello_spring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}
```

- 생성자에 `@Autowired` 가 있으면, 스프링이 연관 객체를 스프링 컨테이너에서 찾아서 넣어준다.
이렇게 객체 의존 관계를 외부에서 넣어주는 것을 DI(Dependency Injection), 의존성 주입이라 한다.
- 이전 테스트에서 개발자가 직접 주입ㅈ했고, 여기서는 `@Autowired`에 의해 스프링이 주입해준다.

**오류 발생**

```java
Consider defining a been of type 'hello.hellospring.sevice.MemberService' in your configuration.
```

**memberService가 스프링 빈으로 등록되어 있지 않다.**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/96d25bc1-8769-4aec-a600-3b7507d318d7/872ceee2-de9b-47d9-975e-ad89e33524ad.png)

> 참고: `helloController`는 스프링이 제공하는 컨트롤러여서 스프링 빈으로 자동 등록된다.
`@Controller` 가 있으면 자동 등록됨.
> 

**스프링빈을 등록하는 2가지 방법**

- 컴포넌트 스캔과 자동 의존관계 설정
    - `@Controller` , `@Service` , `@Repository`에 들어가보면 `@Component` 가 등록되어 있음.
        
        **@Controller**
        
        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/befeab90-c499-4071-a1f3-96e0b4fe3248/4cd64c46-bede-447a-a569-5560766bb044.png)
        
- 자바 코드로 직접 스프링 빈 등록

### 컴포넌트 스캔과 자동 의존관계 설정

- `@Component` 애노테이션이 있으면 스프링 빈으로 자동 등록
- `@Controller` 컨트롤러가 스프링 빈으로 자동 등록된 이유도 컨포넌트 스캔 때문.

- `@Component`를 포함하는 다음 애노테이션도 스프링 빈으로 자동 등록된다.
    - `@Controller`
    - `@Service`
    - `@Repository`

**회원 서비스 스프링 빈 등록**

```java
@Service
public class MemberService {

    private final MemberRepository memberRepository;
    
    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
```

> 참고: 생성자 `@Autowired`를 사용하면 객체 생성 시점에 스프링 컨테이너에서 해당 스프링 빈을 찾아서 주입한다. 생성자가 1개만 있으면 `@Autowired`는 생략할 수 있다.
> 

**회원 리포지토리 스프링 빈 등록**

```java
@Repository
public class MemoryMemberRepository implements MemberRepoistory {}
```

**스프링 빈 등록 이미지**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/df47aeba-3d74-4e74-841f-a4f64b07a8f0/a221a62f-4a4d-4032-9fb1-d5fc947862cd.png)

- `meberService`와 `memberRepository`가 스프링 컨테이너에 스프링 빈으로 등록되어있다.

> 참고: 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 싱글톤으로 등록한다.(유일하게 하나만 등록해서 공유) 따라서 같은 스프링 빈이면 모두 같은 인스턴스다. 설정으로 싱글톤이 아니게 할수 있지만, 특별한 경우를 제외하면 싱글톤으로 사용.
> 

## 자바 코드로 직접 스프링 빈 등록하기

- 회원 서비스와 회원 리포지토리의 `@Service`, `@Repository`, `@Autowired` 애노테이션을 제고하고 진행.

`hello-spring/src/main/java/hello/hello_spring/SpringConfig.java`

**SpringConfig**

```java
package hello.hello_spring;

import hello.hello_spring.repository.MemoryMemberRepository;
import hello.hello_spring.service.MemberService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }

    @Bean
    public MemoryMemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
}
```

**여기서는 향후 메모리 리포지토리를 다른 리포지토리로 변경할 예정이므로, 컴포넌트 스캔 방식 대신에 자바 코드로 스프링 빈을 설정.**

> 참고: DI에는 필드주입, setter주입, 생성자 주입. 3가지가 있다. 의존관계가 실행중에 동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장
> 

> 참고: 실무에서는 주로 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드 컴포넌트 스캔을 사용. 그리고 정형화되지 않거나, 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록
> 

> 주의: `@Autowired`를 통한 `DI`는 `helloController`, `MemberService`등과 같이 스프링이 관리하는 객체에서만 동작한다. 스프링 빈으로 등록하지 않고 내가 직접 생성한 객체에서는 동작하지 않는다.
>
