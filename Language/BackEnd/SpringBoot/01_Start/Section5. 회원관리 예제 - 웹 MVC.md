[<back](https://www.notion.so/Spring-2cf44a3f25f542dfbcc88e2390cee6e3?pvs=21)

---

## 회원 웹 기능 - 홈 화면 추가

**홈 컨트롤러 추가**

`hello-spring/src/main/java/hello/hello_spring/controller/HomeController.java` 

**HomeController.java**

```java
package hello.hello_spring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    @GetMapping("/")
    public String home() {
        return "home";
    }
}
```

`hello-spring/src/main/resources/templates/Home.html` 

**Home.html**

```java
<!doctype html>
<html xmlns:th="http://www.thymeleaf.org" lang="en">

<body>
<div class="container">
    <div>
        <h1>Hello Spring</h1>
        <p>회원 기능</p>
        <p>
            <a href="/members/new">회원 가입</a>
            <a href="/members">회원 목록</a>
        </p>
    </div>
</div> <!-- /container -->

</body>
</html>
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/dc82366c-033b-4a58-a3b3-ae3349d5de3b/image.png)

→ ‘Section2. 정적 컨텐츠 실행’에서 보면, **스프링 컨테이너**를 먼저 확인하고 연결시켜줄 것이 없으면 
`hello-static.html`과 연결 시켜준다.

→ 따라서, 위의 경우 스프링 컨테이너(`HomeController`)에 `@GetMapping(“/”)` 이 있으므로 `home.html`과 연결된 것. 

→컨테이너에 `@Contorller`가 없다면 `index.html`이 웰컴페이지로 출력됨.

## 회원 웹 기능 - 등록

### 회원 등록 폼 개발

**회원등록 폼 컨트롤러**

`hello-spring/src/main/java/hello/hello_spring/controller/MemberController.java`

**MemberController.java**

```java
package hello.hello_spring.controller;

import hello.hello_spring.domain.Member;
import hello.hello_spring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

@Controller
public class MemberController {

...

    @GetMapping("/members/new")
    public String createFrom() {
        return "members/createMembersForm";
    }

    @PostMapping("/members/new")
    public String create(MemberForm form) {
        Member member = new Member();
        member.setName(form.getName());

        memberService.join(member);

        return "redirect:/";
    }
}
```

**회원 등록 폼 HTML**

`hello-spring/src/main/resources/templates/members/createMembersForm.html`

**createMembersForm.html**

```java
<!doctype html>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div calss='container'>
    <form action="/members/new" method="post">
        <div class="form-group">
            <label for="name">이름</label>
            <input type="text" id="name" name="name" placeholder="이름을 입력하세요">
        </div>
        <button type="submit">등록</button>
    </form>

</div> <!-- /container-->
</body>
</html>
```

- GET방식은 조회 POST방식은 등록을 뜻함.

**home.html**

```html
<!doctype html>
<html xmlns:th="http://www.thymeleaf.org" lang="en">

<body>
<div class="container">
    <div>
        <h1>Hello Spring</h1>
        <p>회원 기능</p>
        <p>
            **<a href="/members/new">회원 가입</a>  <!-- /members/new로 이동 -->**
            <a href="/members">회원 목록</a>
        </p>
    </div>
</div> <!-- /container -->

</body>
</html>
```

**MembersController**

```java
    **@GetMapping("/members/new")  // -> /members/new 가 호출되면 해당 메서드 실행**
    public String createFrom() {
        **return "members/createMembersForm";  // -> /members/createMembersForm으로 이동로 이동**
    }
```

**createMembersForm.html**

```html
<!doctype html>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div calss='container'>
    **<form action="/members/new" method="post">  <!--버튼 클릭시 post메서드 실행-->
        <div class="form-group">  <!--action button 정의-->**
            <label for="name">이름</label>
            <input type="text" id="name" **name="name"** placeholder="이름을 입력하세요">
        </div>
        <button type="submit">등록</button>
    </form>

</div> <!-- /container-->
</body>
</html>
```

**MembersController**

```java
    **@PostMapping("/members/new")  // -> post 메서드 실행**
    public String create(MemberForm form) {
        Member member = new Member();
        member.setName(form.getName());

        memberService.join(member);

        **return "redirect:/";  //-> 초기화면으로 전환**
    }
```

## 회원 웹 기능 - 조회