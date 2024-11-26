[<back](https://www.notion.so/Spring-2cf44a3f25f542dfbcc88e2390cee6e3?pvs=21)

---

<aside>
📃 목차

</aside>

---

## AOP가 필요한 상황

- 모든 메소드의 호출 시간을 측정하고 싶다면?
- 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern)
- 회원 가입 시간, 회원 조회 시간을 측정하고 싶다면?

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/28505ef5-52df-434d-8e03-0e7719d0a978/36458b27-7ba9-44ee-a5ba-f6f4706bfc7c.png)

**문제**

- 회원가입, 회원 조회에 시간을 측정하는 기능은 핵심 관심 사항이 아니다.
- 시간을 측정하는 로직은 공통 관심 사항이다.
- 시간을 측정하는 로직과 핵심 비즈니스 로직이 섞여서 유지보수가 어렵다.
- 시간을 측정하는 로직을 별도의 공통 로직으로 만들기 매우 어렵다.
- 시간을 측정하는 로직을 변경할 때 모든 로직을 찾아가면서 변경해야한다.

`hello-spring/src/main/java/hello/hello_spring/service/MemberService.java` 

**MemberService.java**

```java
    /*
     * 회원가입
     * */

    public Long join(Member member) {
        //같은 이름이 있는 중복 회원 x
        long start = System.currentTimeMillis();

        try {
            validateDuplicateMember(member);  //중복회원 검증
            memberRepository.save(member);
            return member.getId();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("join = " + timeMs + "ms");
        }

    }

    /*
     * 전체 회원 조회
     * */
    public List<Member> findMembers() {
        long start = System.currentTimeMillis();
        try {
            return memberRepository.findAll();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("findmembers = " + timeMs + "ms");
        }
    }
```

- 위 같이 시간을 측정하는 로직을 모든 메서드마다 적용시켜주어야함.
- 너무 반복적이고 생산적이지 못한 작업임…

## AOP 적용

- AOP: Aspect Oriented Programming(관점 지향 프로그래밍)
    
    → aspect: 측면, 면(ex. consider every aspect) /  oriented: 지향
    
- 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern)분리

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/1a237cfb-e7b2-4e0d-8db1-25ef7c5302a3/9e6d5a6e-6c84-41d3-9e9c-ec76e4bdce6a.png)

`hello-spring/src/main/java/hello/hello_spring/aop/TimeTraceAop.java`

**TimeTraceAop.java**

```java
package hello.hello_spring.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class TimeTraceAop {

    @Around("execution(* hello.hello_spring..*(..))")
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        System.out.println("START: " + joinPoint.toLongString());
        try {
            return joinPoint.proceed();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("END: " + joinPoint.toLongString() + " " + timeMs + "ms");
        }
    }
}
```

**해결**

- 회원가입, 회원, 조회 등 핵심 관심사항과 시간을 측정하는 공통 관심 사항을 분리.
- 시간을 측정하는 로직을 별도의 공통 로직으로 만듦.
- 핵심 관심 사항을 깔끔하게 유지 가능.
- 변경이 필요하면 이 로직만 변경.
- 원하는 적용 대상을 선택 가능.

### 스프링의 AOP 동작 방식 설명

**AOP 적용 전 의존관계**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/01700909-bde0-4594-931c-d5bf1c958189/c06a1ffb-7a1c-481b-a3f9-fcf2e7ff318c.png)

**AOP 적용 후 의존관계**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/a71f0aa1-5063-434c-8be1-e353c219e4aa/26fb48ea-f799-4eb5-84f0-03c79a0fb1d2.png)

**AOP 적용 전 전체 그림**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9d6f8845-2718-4447-b7f8-9f62f49b6e16/58d16cc4-cfd5-45ca-9366-6085f05d06c7.png)

**AOP 적용 후 전체 그림**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/c7bd639e-2890-42eb-a5c0-dfbfdbf1952b/f6d89c6d-7a2e-42b1-9126-9ba23bdddbf2.png)

- 실제 proxy가 주입되는지 콘솔에서 확인

`hello-spring/src/main/java/hello/hello_spring/controller/MemberController.java`

**MemberController.java**

```java
@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
        **System.out.println("memberService.getClass() = " + memberService.getClass());**
    }
```

**실행 결과**

```java
memberService.getClass() = class hello.hello_spring.service.MemberService$$SpringCGLIB$$0
```