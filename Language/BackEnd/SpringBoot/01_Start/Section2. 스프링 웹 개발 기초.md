[<back](https://www.notion.so/Spring-2cf44a3f25f542dfbcc88e2390cee6e3?pvs=21)

---

<aside>
📃 목차

</aside>

---

## 정적 컨텐츠

- 스프링부트는 기본적으로 정적 컨텐츠를 제공한다.
    - https://docs.spring.io/spring-boot/reference/web/reactive.html#web.reactive.webflux.static-content

**resources/static/hello-static.html**

```java
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
정적 컨텐츠 입니다.
</body>
</html>
```

**실행결과**

**http://localhost:8080/hello-static.html**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/30cf323b-af9d-44a9-a3c9-c5570e2ea974/image.png)

**정적 컨텐츠 실행 이미지**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9289a01d-1b46-423c-b084-365c233e39d6/b2a05128-dcfc-4b61-9761-e3ef0b85e072.png)

---

## MVC와 템플릿 엔진

- MVC: Model, View(화면), Controller(비즈니스 로직)

**Controller**

```java
package hello.hello_spring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class HelloController {

    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model) {
        model.addAttribute("name", name);
        return "hello-template";
    }
}
```

**View**

**resources/template/hello-template.html**

```java
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```

**실행결과**

**http://localhost:8080/hello-mvc?name=spring!!!!**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/909bb6d2-a354-443c-aaf3-012e36687531/image.png)

**MVC, 템플릿 엔진 이미지 설명**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/cf814e3c-84cc-456f-a041-56e01e2e71c4/b9088794-70d6-4c65-a054-d9c40d598341.png)

---

## API

**@ResponseBody 문자 반환**

**hello-string**

```java
package hello.hello_spring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {
    @GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }
}
```

- `@ResponseBody` 를 사용하면 뷰 리졸버(’viewResolver’)를 사용하지 않음
- 대신 HTTP의 BODY에 문자 내용을 직접 반환(HTML BODY TAG를 말하는 것 아님)

**실행결과**

http://localhost:8080/hello-string?name=evan

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/564696ee-1228-4f3c-a763-f1d9a6527344/image.png)

**소스코드**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/afba3669-a963-4af4-8073-0b3ec59ac0b3/image.png)

**hello-api**

```java
package hello.hello_spring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {
    @GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name) {
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }

    static class Hello {
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
}
```

- `@ResponseBody`를 사용하고, 객체를 반환하면 객체가 JSON으로 반환됨

**실행결과**

[**http://localhost:8080/hello-api?name=evan**](http://localhost:8080/hello-api?name=evan) 

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/dfb8df8b-008d-4483-a5fe-43abce1013e7/image.png)

**@ResponseBody 사용원리**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/641a0e4e-ed2b-4ecc-9ab5-99a6503910cd/86850fc9-3514-40ac-964f-606425fe51c1.png)

- `@ResponseBody` 를 사용
    - HTTP의 BODY에 문자 내용을 직접 반환
    - `viewResolver` 대신에 `HttpMessageConverter`가 동작
    - 기본 문자처리: `StringHttpMessageConverter`
    - 기본 객체처리: `MappingJackson2HttpMessageConverter`
    - byte 처리 등등 기타 여러 HttpMessageConverter 가 기본으로 등록되어있음

> 참고: 클라이언트 HTTP Accept 해더와 서버의 컨트롤러 반환 타입 정보 둘을 조합해서
`HttpMessageConverter`가 선택된다.
>
