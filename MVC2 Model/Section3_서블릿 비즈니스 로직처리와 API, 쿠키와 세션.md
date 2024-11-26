[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

목차

</aside>

# 서블릿 비즈니스 로직 처리

## 서블릿 비즈니스 로직 처리 방법

서블릿 비즈니스 처리방법

- 서블릿이 클라이언트에서 요청 받으면 그 요청에 대해 작업을 수행하는 것.
- DB연동 관련 작업 외에 다른 서버와 연결해서 데이터 얻는 작업도 수행
- 서블릿의 가장 핵심 기능

서블릿 비즈니스 처리 작업 예

- 웹 사이트 회원 등록 요청처리
- 로그인
- 주문처리 작업

## 서블릿 데이터베이스 연동하기

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/e3a81fb4-168e-43ef-a5d1-6ae1e07f09cc/image.png)

- 서블릿 → DAO → DTO 를 통해  DB와 연결되어 클라이언트에게 jsp, html로 보내줌.
    
    → MVC2모델(jsp, html)로 보내주는 것이면, MVC2모델
    
    →서블릿이 직접 응답하면 MVC1모델
    

### **PreparedStatement 인터페이스 특징**

- Statement를 상속하므로 메서드 그대로 사용가능.
- 컴파일된 SQL문을 DBMS에 그대로 전달할 수 있음.

### **ConnectionPool 등장배경**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/52c0f350-5689-4dad-83d3-24eb982ae456/image.png)

- 필요할 때마다 연결하면 시간이 오래걸리므로, 미리 연결해두는 기술

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/a583e80f-14ab-4c83-8b52-19c2ea09a4a5/image.png)

### DataSource 이용해 데이터베이스 연동

- DBConnectionPool 셋팅하기
    
    breadscrumb: apache > tomcat9 > document > jdbc > oracle
    
    https://tomcat.apache.org/tomcat-9.0-doc/jndi-datasource-examples-howto.html#Oracle_8i,_9i_&_10g
    

**context.xml (META-INF)**

```xml
<Resource name="jdbc/myoracle" auth="Container"
          type="javax.sql.DataSource" driverClassName="oracle.jdbc.OracleDriver"
          url="jdbc:oracle:thin:@127.0.0.1:1521:mysid"
          username="scott" password="tiger" maxTotal="20" maxIdle="10"
          maxWaitMillis="-1"/>
```

**web.xml**

```xml
<resource-ref>
 <description>Oracle Datasource example</description>
 <res-ref-name>jdbc/myoracle</res-ref-name>
 <res-type>javax.sql.DataSource</res-type>
 <res-auth>Container</res-auth>
</resource-ref>
```

**DataUtil.java**

```java
public class DBUtil {
    public static Connection getConnection() {
        Context initContext = null;
        Connection conn = null;
        try {
            initContext = new InitialContext();
            Context envContext  = (Context)initContext.lookup("java:/comp/env");
            DataSource ds = (DataSource)envContext.lookup("jdbc/myoracle");
            conn = ds.getConnection();
        } catch (NamingException e) {
            throw new RuntimeException(e);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
        return conn;
    }
```

# 서블릿 확장 API 사용하기

## 서블릿 포워드 기능 사용하기

### **포워드 기능**

- 요청에 대한 추가작업, 포함된 정보를 다른 서블릿이나 jsp와 공유
- MVC모델2 개발 시 서블릿에서 jsp로 데이터 전달될 때 사용

## 서블릿 여러가지 포워드 방법

### **재요청 방법**

- redirect방법
    - 웹브라우저에 재요청하는 방식(`sendRedireact`)
        
        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/337f18c0-dd31-43e7-b121-52fdba90e1aa/image.png)
        
- refresh방법
    - `response.addHeader(”Refresh”, 경과시간(초); url=””)`
- location방법
    - `location.href=””`

### dispatch를 이용한 포워드 방법

### **포워드 방법**

- dispatch 방법
    - RequestDispatcher를 이용.
    - `rd.forward(request, response)`
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/0e68641a-9180-4702-87e6-26393b51e22a/image.png)
    

### 바인딩

- 웹 프로그래밍 실행 시 자원을 서블릿 관련 객체에서 저장하는 방법
- 주로 HttpServletRequest, HttpSession, ServletContext 객체에서 사용
- ServletContext 객체에 저장하면 모든 서블릿 접근 가능.
- 저장된 자원은 프로그램 실행 시 서블릿이나 JSP에서 공유해서 사용

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/06ad214c-acc1-4304-b463-122af31c43fa/078375d0-ea63-456c-89c1-5ed8d30032d7.png)

- 토큰형: HttpServletRequest
- static형: ServletContext(application)
- 1개의 세션형: HttpSession
- page에서만 사용: PageContext

## ServletContext와 ServletConfig 사용법

### **ServletContext 특징**

- 컨테이너 간의 연동을 위해 사용
- 컨텍스트마다 하나의 ServletContext 생성
- 서블릿 끼리 자원 공유
- 컨테이너 생성 시 생성, 종료 시 소멸

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/8d6f7f18-3077-434b-8960-40da6656ce60/image.png)

**브라우저 storage에 정보 저장하기**

- **LocalStorage**
    - local storage에 저장 → 브라우저가 죽어도 살아있음.
- **SessionStorage**
    - 브라우저Session storage에 저장 → 브라우져가 종료되면(세션이 끝나면) 내용 소멸

### **ServletContext에서 제공하는 메서드**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/dccb4a7a-2ed1-4082-9faf-1b0212f45413/image.png)

### **ServeltContext에 매개변수 설정**

- web.xml에 매개변수 설정
    
    전체 어플리케이션에서 설정을 사용해야할 때 사용
    
    **web.xml**
    
    ```xml
    <context-param>
        <param-name>username</param-name>
        <param-value>hr</param-value>
    </context-param>
    <context-param>
        <param-name>userpass</param-name>
        <param-value>hrpass</param-value>
    </context-param>
    ```
    
    **ServletContextTest**
    
    ```java
    package com.shinhan4.contorller2;
    
    import javax.servlet.*;
    import javax.servlet.http.*;
    import javax.servlet.annotation.WebServlet;
    import java.io.IOException;
    
    @WebServlet("/ServletContextTest")
    public class ServletContextTest extends HttpServlet {
        private static final long serialVersionUID = 1L;
    
        @Override
        protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            ServletContext app = getServletContext();
            String id = app.getInitParameter("username");
            String pass = app.getInitParameter("userpass");
            System.out.println("id = " + id);
            System.out.println("pass = " + pass);
        }
    }
    ```
    
    **실행결과**
    
    ```java
    id = hr
    pass = hrpass1234
    ```
    

- **파일로 매개변수 설정**
    
    **WEB-INF > resource > menu.text**
    
    ```
    직원조회, 직원입력, 부서등록, 부서조회
    ```
    
    **ServletContextTest**
    
    ```java
    InputStream is = app.getResourceAsStream("/WEB-INF/resource/menu.txt");
        BufferedReader br = new BufferedReader(new InputStreamReader(is));
        String line = null;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }
    ```
    

### **ServletConfig**

서블릿 마다 있으며, 서블릿에 대한 초기화 작업 기능

**ServletContextTest**

```java
@WebServlet(urlPatterns = {"/ServletContextTest", "/servlet/config"},
initParams = {@WebInitParam(name = "email", value = "jamm0316@naver.com")})
public class ServletContextTest extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. servlet 접근
        String email = getInitParameter("email");
        System.out.println("email = " + email);
```

실행결과

```
email = jamm0316@naver.com
```

위와 같은 방법으로는 잘 사용하지 않음.

web.xml에서 서블릿을 등록할 때 사용할 수 있음 

**web.xml**

```xml
<servlet>
    <servlet-name>servletconfig</servlet-name>
    <servlet-class>com.shinhan4.contorller2.ServletContextTest</servlet-class>
    <init-param>
        <param-name>email</param-name>
        <param-value>jamm0316@naver.com</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>servletconfig</servlet-name>
    <url-pattern>/servlet/config</url-pattern>
</servlet-mapping>
```

### loda-on-strartup 기능 사용하기

톰캣 컨테이너가 실행되면 미리 서블릿을 실행함.

따라서, 요청에 따라 서블릿이 실행되는 것이 아니므로 최초 서블릿 요청 시 빠르게 처리할 수 있음.

**web.xml**

```xml
<servlet>
    <servlet-name>servletconfig</servlet-name>
    <servlet-class>com.shinhan4.contorller2.ServletContextTest</servlet-class>
    <init-param>
        <param-name>email</param-name>
        <param-value>jamm0316@naver.com</param-value>
    </init-param>
    **<load-on-startup>1</load-on-startup>**
</servlet>
```

**ServletContextTest**

```java
@WebServlet(urlPatterns = {"/ServletContextTest", "/servlet/config"},
initParams = {@WebInitParam(name = "email", value = "jamm0316@naver.com")})
public class ServletContextTest extends HttpServlet {
    private static final long serialVersionUID = 1L;

    public ServletContextTest() {
        System.out.println("ServletContextTest Constructor");
    }
```

**실행결과**

```
ServletContextTest Constructor
```

---

# 9. 쿠키와 세션 알아보기

## 웹 페이지 연결 기능

### 세션 트래킹

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/2305af1e-63a5-4821-a5d0-397055e0cd40/image.png)

- HTTP 프로토콜은 서버-클라이언트 통신 시 stateless 방식으로 통신
- 즉, 브우저에서 새 웹 페이지를 열면 기존 웹페이지의 서블릿 정보를 알 수 없음.
- 따라서, 세션 트랙킹이용해서 웹 페이지 간의 연결 기능을 구현

**세션 트래킹 구현법**

<hidden>태그 와 URL ReWriting

쿠키와 세션

- 클라이언트에 저장: 쿠키
- 서버에 저장: 세션

## <hidden> 태그와 URL Rewriting 이용

**문제점**

- 웹페이지가 많아지면 일일이 로그인 정보 전송해야함

## 쿠키를 이용한 웹 페이지 연동 기능

### **쿠키**

- 웹 페이지들 사이의 공유 정보를 클라이언트 PC에 저장해 놓고 사용

**특징**

- 정보가 클라이언트 pc에 저장
- 보안에 취약
- 용량 제한
- 클라이언트가 사용유무 설정
- 도메인당 쿠키가 만들어짐(웹 사이트당 하나)

**종류**

- persistance쿠키
- session쿠키

**쿠키 기능 실행 과정**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/29fca8ec-6379-4e87-8dc9-b025153cdf36/image.png)

**쿠키 API**

`addCookie()`: 클라이언트 브라우저에 전송 후 저장

`getCookie()`: 쿠키를 서버로 가져옴.

**쿠키 만들기 예제**

1. 쿠키 셋팅
    
    **SetCookieServlet**
    
    ```java
    package com.shinhan4.contorller2;
    
    import javax.servlet.*;
    import javax.servlet.http.*;
    import javax.servlet.annotation.WebServlet;
    import java.io.IOException;
    import java.net.URLEncoder;
    
    @WebServlet("/cookie/set")
    public class SetCookieServlet extends HttpServlet {
        private static final long serialVersionUID = 1L;
    
        @Override
        protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            Cookie cookie1 = new Cookie("userid", "jamm");
            Cookie cookie2 = new Cookie("userpass", "1234");
    
            //parameter로 들어온 값 쿠키에 저장.
            Cookie cookie3 = new Cookie("phone", request.getParameter("phone"));
            Cookie cookie4 = new Cookie("major", URLEncoder.encode("토목", "UTF-8"));
            
            cookie4.setMaxAge(60);
            cookie4.setMaxAge(2 * 60);
            cookie4.setMaxAge(4 * 60);
            cookie4.setMaxAge(5 * 60);
    
            response.addCookie(cookie1);
            response.addCookie(cookie2);
            response.addCookie(cookie3);
            response.addCookie(cookie4);
    
            response.setContentType("text/html;charset='utf-8'");
    //        response.setCharacterEncoding("utf-8");
            response.getWriter().append("<h1><쿠키저장 완료/h1>");
        }
    }
    
    ```
    

1. 쿠키 받기
    
    **GetCookieServlet**
    
    ```java
    package com.shinhan4.contorller2;
    
    import javax.servlet.*;
    import javax.servlet.http.*;
    import javax.servlet.annotation.WebServlet;
    import java.io.IOException;
    
    @WebServlet("/cookie/get")
    public class GetCookieServlet extends HttpServlet {
        private static final long serialVersionUID = 1L;
    
        @Override
        protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            Cookie[] cookies = request.getCookies();
            for (Cookie cookie : cookies) {
                System.out.println(cookie.getName());
                System.out.println(cookie.getValue());
                System.out.println("----------------------------");
            }
        }
    }
    ```
    

**문제점**

- 쿠키를 누구나 볼 수 있기 때문에 보안에 취약하다.

## 세션을 이용한 웹 페이지 연동 기능

### **세션**

**특징**

- 쿠키보다 보안 유리
- 정보가 서버의 메모리에 저장
- 서버 부하 줄일 수 있음
- 세션 유효시간 기본 30분
- 브라우저 당 한개의 세션 생성
- 로그인 상태 유지 기능, 쇼핑몰 장바구니 담기 기능에 주로 사용됨.

**실행과정**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/94306f6f-9134-4289-bd71-23a1df7f7b4c/image.png)

**각 브라우저에 대한 세션 생성 상태**

- 브라우저 마다 다름.

**세션 API특징과 기능**

- 서블릿에서 객체 생성
- 기존 세션 객체가 있으면 생성하지 않고, 없으면 생성함.

**세션 만들기 예제**

**SetSessionTest**

```java
package com.shinhan4.contorller2;

import com.fistzone.member.MemberDTO;
import com.fistzone.member.MemberService;

import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;
import java.lang.reflect.Member;

@WebServlet("/session/set")
public class SetSessionTest extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //boolean-> 있으면 얻고 없으면 새로 생성
        HttpSession session = request.getSession(true);

        MemberService memberService = new MemberService();
        MemberDTO memberDTO = memberService.loginService("jamm", "1234");
        session.setAttribute("member", memberDTO);

        System.out.println("isNew?>> " + session.isNew());
        System.out.println("Id?>> " + session.getId());
        System.out.println("CreateTime?>> " + session.getCreationTime());
        System.out.println("lastTime?>> " + session.getLastAccessedTime());
        System.out.println("InactiveInterval?>> " + session.getMaxInactiveInterval());

        response.setContentType("text/html;charset=utf-8");
        response.getWriter().append("<h1>세션 저장 완료</h1>");
    }
}
```

**GetSessionTest**

```java
package com.shinhan4.contorller2;

import com.fistzone.member.MemberDTO;
import com.fistzone.member.MemberService;

import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;

@WebServlet("/session/get")
public class GetSessionServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //boolean-> 있으면 얻고 없으면 새로 생성
        HttpSession session = request.getSession(true);

        MemberDTO member = (MemberDTO) session.getAttribute("member");

        System.out.println("isNew?>> " + session.isNew());
        System.out.println("Id?>> " + session.getId());
        System.out.println("CreateTime?>> " + session.getCreationTime());
        System.out.println("lastTime?>> " + session.getLastAccessedTime());
        System.out.println("InactiveInterval?>> " + session.getMaxInactiveInterval());

        response.setContentType("text/html;charset=utf-8");
        response.getWriter().append("<h1>세션 얻기</h1>").append("<p>" + member + "</p>");
    }
}
```

## encodeURL

## 세션을 이용한 로그인 예제