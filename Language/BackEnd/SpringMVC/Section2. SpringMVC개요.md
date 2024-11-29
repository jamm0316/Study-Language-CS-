<aside>
📄

목차

</aside>

# SpingMVC 개요

## MVC2모델(Model View Controller)

**사용자 인터페이스와 비즈니스 로직을 분리하여 개발한다.**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/1a508ec5-cf1c-414d-8efa-c9fa01739614/097b559a-3ecb-4e86-ace2-3779531e1a37.png)

## Spring Web MVC

- 서블릿 기반의 MVC Model2 구조를 제공
- **Front Controller** 패턴 적용
    - 전체 애플리케이션을 통합, 관리하기 위해 서버로 들오는 모든 요청을 먼저 받아서 처리
    - Front Controller가 servlet으로 구성되어 있음

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/6e5ca75b-7cf5-424e-a1a5-2f0686e0230d/2fa601f7-bf02-4e27-b487-c632208bf8dc.png)

### Spring MVC 컴포넌트간의 관계와 흐름

- **Dispatcher Servlet: Front Controller**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/69f443ac-6e2d-454b-9d91-b4f715149bde/image.png)

- **Handler Mappings**: @GetMapping()
- **Controller(Handler)**: Controller
- **View Resolver’s**: return “~”;
- **View’s**: Model data → .jsp → Dispatcher Servlet
- **Client(Browser)**: .jsp

### spring MVC 주요 구성요소

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/867dcdf3-2d1d-46ac-adbc-06d5c9e78424/bdacb6c8-8f37-4cfa-9f12-dfc0b0170694.png)

### Dispatcher Servlet

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/d38eb94b-f257-4e66-a2d0-5dae06d96d08/5a70019e-98d5-4084-802c-eacc06c3141e.png)

### Repository

```java
/**
 * <bean id="deptDAO2" class="com.shinhan.myapp.model.DeptDAO"/>
 */

//==Bean
@Repository("deptDAO2")  //DAO역할을 하는 @Componenet
public class DeptDAO {

	//type이 같으면 자동으로 Injection (IoC, DI)
	@Autowired
	DataSource ds;  //getBean과 같은 역할인가??
```

### Service

```java
@Service
public class DeptService {

	@Autowired  //ref
	DeptDAO deptDao;
```

### Controller

```java
@Controller  //요청오면 페이지 return, 다른 요청으로 재요청 가능
public class SampleController {

    @Autowired
    DeptService deptService;
```

### webapp/WEP-INF/views

## Spring Annotation

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/81d36203-c616-474b-834b-511ccc7499ab/68d91d92-cd72-47db-81f4-dcfc4aaa39c8.png)

### @RequestMapping

**기본형**

`@ReqeustMapping(value={””,””}, method=””, params=””)`

**특징**

- 요청에 대한 어떤 메서드를 매핑하기 위한 어노테이션
- 클래스 단위나 메서드 단위로 사용할 수 있다.
    
    ```java
    package com.shinhan.myapp;
    @Controller
    @RequestMapping("/jsptest")  //class level에서 요청주소 작성
    public class SampleController2 {
    
        @GetMapping("/first1.do")  //functnio level에서 요청주소 작성
        public void f1() {
    
        }
    
        @RequestMapping("/first2.do")
        public void f2() {
    
        }
    
        @RequestMapping(value = "/first3.do", method = RequestMethod.GET)
        public void f3() {
    
        }
    }
    ```
    

**다양한 메서드**

- **method= RequestMethod.*POST***
    - **SampleController3**
        
        ```java
        @Controller
        public class SampleController3 {
        
            @RequestMapping(value = {"/second3.do"}, method= RequestMethod.POST)
            public String f2() {
                return "jsptest/second3";
            }
            
            @RequestMapping(value = {"/second1.do","/second2.do"}, method= RequestMethod.GET)
            public String f1() {
                return "/jsptest/first1";
            }
        ```
        
    - **first1**
        
        ```java
        <%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
        <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
        <c:set var="path" value="${pageContext.servletContext.contextPath}"/>
        <html>
        <head>
            <title>Title</title>
        </head>
        <body>
        <h1>first1.jsp열기</h1>
        <form action="${path}/second3.do" method="post">
            id: <input type="text" name="userId" value="jamm"/><br>
            pw: <input type="password" name="userId" value="1234"/><br>
            <input type="submit" value="서버전송(post)"/>
        </form>
        </body>
        </html>
        ```
        

- **params= {}**
    - **SampleController3**
        
        ```java
        @Controller
        public class SampleController3 {
        
            Logger logger = LoggerFactory.getLogger(SampleController3.class);
        
            //요청의 주소 같고, 넘어오는 파라메터도 확인
            //(userid라는 파라메터의 값은 꼭 같아야함. userpw는 존재하기만 하면됨, email은 존재안함)
            @RequestMapping(value="/second4.do", params = {"userid=jamm", "userpw", "!email"})
            public String f3(String userid, String userpw) {
                logger.info("id= " + userid);
                logger.info("pw= " + userpw);
                return "jsptest/second3";
            }
        ```
        

### **@RequestParam**

- 목적: submit으로 인해 들어온 parameter를 argument로 받을 때 사용.
- 받는 parameter이름과 할당할 변수의 이름이 다를 때 사용.
- 만약 두개가 같다면 해당 어노테이션 사용할 필요 없음.

```java
@GetMapping("/jsptest2/coffee2.do")
    public void f2(@RequestParam("department_id") int id,
                   @RequestParam("department_name") String name,
                   @RequestParam("manager_id") int mid,
                   @RequestParam("location_id") int locid) {
        logger.info("id = " + id);
        logger.info("name = " + name);
        logger.info("mid = " + mid);
        logger.info("locid = " + locid);
    }
```

### **@ModelAttribute**

- 목적: 모델에 객체를 추가하여 뷰(JSP 등)에 데이터를 전달하기 위해 모델에 객체 추가.

```java
@GetMapping("/jsptest2/coffee3.do")
    public String f3 (@ModelAttribute DeptDTO dept) {
        logger.info((dept.toString()));
        return "jsptest2/coffe2;";
    }
```

- **DTO형태로 인수 받기**
    - 사용방법
        - 빈에 등록된 DTO와 submit으로 들어오는 parameter의 이름이 같으면 자동으로 DTO를 생성해줌.
        
        **DeptInsert**
        
        ```java
        <%@ page contentType="text/html; charset=UTF-8" language="java" %>
        
        <!doctype html>
        <html lang="en">
        <head>
            <meta charset="utf-8">
            <meta name="viewport" content="width=device-width, initial-scale=1">
        </head>
        <body class="p-3 m-0 border-0 bd-example m-0 border-0">
        <%@include file="../jsp/header.jsp" %>
        <!-- Example Code -->
        
        <div class="input-group mb-3">
            <form class="insertCard" action="insert.do" method="post">
                <h2>부서등록</h2>
                <div class="input-group mb-3">
                    <span class="input-group-text">부서번호</span>
                    <input type="number" required="required" class="form-control" placeholder="숫자입력" name="department_id"
                           aria-describedby="basic-addon1">
                </div>
                <div class="input-group mb-3">
                    <span class="input-group-text">부서이름</span>
                    <input type="text" required="required" class="form-control" placeholder="부서입력" name="department_name"
                           aria-describedby="basic-addon1">
                </div>
                <div class="input-group mb-3">
                    <span class="input-group-text">부서장</span>
                    <input type="number" required="required" class="form-control" value="101" placeholder="101"
                           name="manager_id"
                           aria-describedby="basic-addon1">
                </div>
                <div class="input-group mb-3">
                    <span class="input-group-text">지역번호</span>
                    <input type="number" required="required" class="form-control" value="1700" placeholder="1700"
                           name="location_id"
                           aria-describedby="basic-addon1">
                </div>
                <input type="hidden" name="phone" value="010-1234-5678"/>
                <button type="submit" class="btn btn-primary">신규부서 등록</button>
                <button id="btn_ajax" type="button" class="btn btn-warning">신규부서 등록(Ajax)</button>
            </form>
        </div>
        <script>
          $("#btn_ajax").on("click", f_jsonInsert)
        
          function f_jsonInsert() {
            let obj = {
              "deptid": $('[name="department_id"]').val(),
              "deptname": $('[name="department_name"]').val(),
              "mid": $('[name="manager_id"]').val(),
              "locid": $('[name="location_id"]').val()
            };
            let jsonStr = JSON.stringify(obj);
            console.log(obj);
            console.log(jsonStr)
            $.ajax({
              url: "${path}/json.do",
              type: "get",
              data: {"jsonInfo": jsonStr},
              success: function (responseData) {
                alert(responseData);
              },
              error: function (err) {
                alert(err);
              },
              complete: function () {
                alert("완료!");
              }
        
            });
          }
        </script>
        </body>
        </html>
        ```
        
        **DeptController**
        
        ```java
        @PostMapping("/dept/insert.do")
            public String insertPost(DeptDTO dept) {
                deptService.insertService(dept);
                return "redirect:/dept/list.do";
            }
        ```
        

### redirect

**servlet의 response.sendRedirect와 같은 메서드**

**개요**

- 브라우저에서 새로운 요청을 보내는것
    - 따라서, request에 있는 data는 모두 날아감.
- controller의 메서드를 처리하고 다른 페이지로 돌려주고 싶을 때 사용
    
    ```java
    @PostMapping("/dept/insert.do")
        public String insertPost(DeptDTO dept) {
            deptService.insertService(dept);
            return "redirect:/dept/list.do";
        }
    ```
    

**다양한 기능**

- RedirectAttributes
    
    redirect 하는 페이지에 attribute를 추가하고 싶을 때, 인수로 사용
    
    **DeptController**
    
    ```java
    @PostMapping("/dept/insert.do")
    public String insertPost(DeptDTO dept, RedirectAttributes attr) {
        int result = deptService.insertService(dept);
        String message = "입력건수:" + result;
        logger.info(message);
        attr.addFlashAttribute("resultMessage", message);
        return "redirect:/dept/list.do";
    }
    
    @RequestMapping("/dept/list.do")
    public String  selectAllList(Model model, HttpServletRequest request) {
        Map<String, ?> map = RequestContextUtils.getInputFlashMap(request);
        if (map != null) {
            String message = (String) map.get("resultMessage");
            model.addAttribute("result", message);
        } 
        List<DeptDTO> deptList = deptService.selectAllService();
        model.addAttribute("deptList", deptList);
        return "dept/deptList";  //forward, include
    }
    ```
    
    - 여기서, HttpServletRequest의 경우 servlet-api 4.0.* 이상이 필요

### Exception Handling

Spring MVC에서 @RequestMapping이 선언된 메서드는 클라이언트로부터 들어오는 요청을 처리하는 주요 역할 담당. 하지만 요청 처리 중 예외가 발생할 경우, 애플리케이션의 안정성과 사용자 경험을 위해 이러한 예외를 효율적으로 처리하는 것이 중요.

- 404 오류의 경우
    - 404는 에러가 아니기 때문에 Handler를 찾지 못함.
    - 따라서, throwExceptionIfNoHandlerFound 예외를 front-servlet에 param으로 추가
    
    **web.xml**
    
    ```java
    <servlet>
        <servlet-name>appServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>
                /WEB-INF/spring/appServlet/servlet-context.xml
                /WEB-INF/spring/appServlet/servlet-contextDB.xml
            </param-value>
        </init-param>
        <init-param>
            <param-name>throwExceptionIfNoHandlerFound</param-name>
            <param-value>true</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    ```
    

**ExceptionAdvice**

```java
@ControllerAdvice
@Slf4j
public class ExceptionAdvice {

    @ExceptionHandler(Exception.class)
    public String f1(Exception ex) {
        log.info("예외발생 class: " + ex.getClass().getName());
        log.info("예외발생 메세지: " + ex.getMessage());
        return "/error/error500";
    }

    @ExceptionHandler(NoHandlerFoundException.class)
    public String f2(HttpServletRequest request) {
        log.info("요청주소: "+ request.getRequestURI());
        return "/error/error404";
    }
}
```

**error/error500**

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" isErrorPage="true" %>
<html>
<head>
    <title>오류정보</title>
</head>
<body>
<h1>오류 정보</h1>
message : <%=exception.getMessage()%>
</body>
</html>
```