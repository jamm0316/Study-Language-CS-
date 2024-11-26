[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

목차

</aside>

# 서블릿의 필터와 리스너 기능

## 서블릿 속성과 스코프

**서블릿 속성**

- ServletContext, HttpSession, HttpServletRequest 객체에 바인딩 된 저장 객체(정보)
    
    → 상태정보유지기술
    
- setAttribute: 바인딩
- getAttribute: 접근
- removeAttribute: 제거

**서블릿 스코프**

- ServletContext 속성은 애플리케이션 전체에서 접근 가능(애플리케이션 전체)
- HttpSession 속성은 사용자만 접근 가능(브라우저)
- HttpSeervletRequest 속성은 해당 요청/응답에 대해서만 접근 가능(요청과 응답 사이클)
- 각 스코프를 이용해 로그인 상태 유지, 장바구니, MVC의 Model과 View의 데이터 전달 기능 구현

## 서블릿의 여러가지 URL 패턴

### **URL패턴**

- 서블릿 매핑 시 사용되는 가상의 주소
    
    ```java
    @WebServlet("/auto/login.do")
    ```
    
- 클라이언트가 브라우저에서 요청할 떄 사용되며 반드시 /로 시작해야함
- 이름까지 일치, 디렉토리 이름만 일치하는 패턴,

### **패턴 종류**

- 이름까지일치
    
    ```java
    @WebServlet("/auto/login.do")
    ```
    
    - `http://localhost:9090/bananaShop4_war_exploded/auth/login.do` 로 작성하면 해당 서블릿 작동

- 디렉토리 일치
    
    ```java
    @WebServlet("/auth/*")
    ```
    
    - `http://localhost:9090/bananaShop4_war_exploded/auth/anonymous` 로 작성하면 해당 서블릿 작동

- 확장자만 일치
    
    ```java
    @WebServlet("*.do")
    ```
    
    - `http://localhost:9090/bananaShop4_war_exploded/auth/anonymous.do` 로 작성하면 해당 서블릿 작동

- 모든 요청
    
    ```java
    @WebServlet("/*")
    ```
    

## Filter API

### **필터**

브라우저에서 서블릿에 요청하거나 응답할  미리 요청이나 응답과 관련해 여러가지 작업 처리.

요청이나 응답 시 공통적인 작업 처리.

**필터기능 수행 과정**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/73ace11a-b81a-4178-b837-3568a8cba59e/image.png)

**요청필터**

- 서블릿에서 일일이 한글 인코딩을 구현하는 것이 아니라 필터에서 먼저 필터링해서 인코딩 처리
    
    ```java
    request.setCharacterEncoding("utf-8");
    ```
    
    위 코드를 일일히 쓰지 않고 요청 필터를 만들어 모든 서블릿을 불러오기 전에 인코딩한다.
    
    DBUtil과 비슷한 역할을 한다.
    

**응답필터**

- 모든 요청이 끝나고 응답할 때 실행된다.

### 예제

**요청필터**

- 인코딩 필터
    
    ```java
    package com.shinhan4.filter;
    
    import javax.servlet.*;
    import javax.servlet.annotation.WebFilter;
    import javax.servlet.http.HttpFilter;
    import java.io.IOException;
    
    @WebFilter("*.do")
    public class EncodingFilter extends HttpFilter implements Filter {
        public EncodingFilter() {
            System.out.println("EncodingFilter 생성");  //서버 시작
        }
    
        @Override
        public void destroy() {
            System.out.println("EncodingFilter 소멸");  //서버 종료
        }
    
        /**
         * *.do 요청시마다 수행됨
         */
        @Override
        public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
            //servlet 요청 전
            System.out.println("============servlet 요청 before=========");
            req.setCharacterEncoding("utf-8");
            chain.doFilter(req, res);  //다른 필터를 수행하거나, 다른 필터가 없다면 서블릿을 수행하러 간다.
    
            //servlet 수행 후 응답하러 가기 전
            System.out.println("============servlet 요청 after 응답 전=====");
        }
    }
    
    ```
    

**응답필터**

- 걸린 시간 측정 필터
    
    ```java
    package com.shinhan4.filter;
    
    import javax.servlet.*;
    import javax.servlet.annotation.WebFilter;
    import javax.servlet.http.HttpFilter;
    import javax.servlet.http.HttpServletRequest;
    import java.io.IOException;
    
    @WebFilter("*.do")
    public class TimerFilter extends HttpFilter implements Filter {
    
        public TimerFilter() {
            System.out.println("TimerFilter.TimerFilter 서버 시작시 생성");
        }
    
        public void destroy() {
            System.out.println("TimerFilter.destroy 서버 종료시 생성");
        }
    
        @Override
        //ServletRequest은 아직 Http가 아니므로 HttpServletRequest를 사용하지 않는다.
        public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws ServletException, IOException {
    //        long start = System.currentTimeMillis();
            long start = System.nanoTime();
            chain.doFilter(request, response);
            long end = System.nanoTime();
    
            //캐스트다운
            HttpServletRequest req = (HttpServletRequest) request;
            System.out.println(req.getRequestURI() + "...Request");
            System.out.println("걸린 시간: " + (end - start) + "ns");
        }
    }
    ```
    

### Web.xml에 필터 등록하기

**web.xml (=**`@WebFilter("/*")`)

```xml
<filter>
    <filter-name>timer</filter-name>
    <filter-class>com.shinhan4.filter.TimerFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>timer</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

- 주의사항: 둘 중 하나만 있어야한다.

## 여러 가지 서블릿 관련 ListenerAPI

### Listener API

서블릿에서 발생하는 이벤트에 대해서 처리 할 수 있는 기능.

**각각의 이벤트들**

```java
package com.shinhan4.listener;

import javax.servlet.*;
import javax.servlet.annotation.WebListener;
import javax.servlet.http.*;

@WebListener
public class ServletListener implements ServletContextListener, HttpSessionListener, HttpSessionAttributeListener, ServletRequestListener {

    public ServletListener() {
        System.out.println("[LISTENER]>> Alistener Constructor");
    }

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("[LISTENER]>> ServletContext contextInitialized");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("[LISTENER]>> ServletContext contextDestroyed");    }

    @Override
    public void sessionCreated(HttpSessionEvent se) {
        System.out.println("[LISTENER]>> sessionCreated");
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        System.out.println("[LISTENER]>> sessionDestroyed");
    }

    @Override
    public void attributeAdded(HttpSessionBindingEvent sbe) {
        System.out.println("[LISTENER]>> Session attributeAdded");
    }

    @Override
    public void attributeRemoved(HttpSessionBindingEvent sbe) {
        /* This method is called when an attribute is removed from a session. */
    }

    @Override
    public void attributeReplaced(HttpSessionBindingEvent sbe) {
        System.out.println("[LISTENER]>> Session attributeReplaced");
    }

    @Override
    public void requestInitialized(ServletRequestEvent sre) {
        System.out.println("[LISTENER]>> requestInitialized");
    }

    @Override
    public void requestDestroyed(ServletRequestEvent sre) {
        System.out.println("[LISTENER]>> requestDestroyed");
    }
}
```

- 서버 시작 → 로그인 화면 접속 → 로그인 → empLIst.do 접속

**console Log**

```java
Connected to server
[LISTENER]>> Alistener Constructor
[LISTENER]>> ServletContext contextInitialized
[FILTER]>> TimerFilter.TimerFilter 서버 시작시 생성
[FILTER]>> EncodingFilter 생성
ServletContextTest Constructor
[LISTENER]>> requestInitialized
[FILTER]>> /bananaShop4_war_exploded/...Request
[FILTER]>> 걸린 시간: 2697792ns
[LISTENER]>> requestDestroyed
[LISTENER]>> requestInitialized
doGet
[LISTENER]>> sessionCreated
[FILTER]>> /bananaShop4_war_exploded/auth/login.do...Request
[FILTER]>> 걸린 시간: 489555833ns
[LISTENER]>> requestDestroyed
[LISTENER]>> requestInitialized
doPost
[LISTENER]>> Session attributeAdded
[LISTENER]>> Session attributeAdded
[FILTER]>> /bananaShop4_war_exploded/auth/login.do...Request
[FILTER]>> 걸린 시간: 496812000ns
[LISTENER]>> requestDestroyed
[LISTENER]>> requestInitialized
108건
[FILTER]>> /bananaShop4_war_exploded/emp/emplist.do...Request
[FILTER]>> 걸린 시간: 210047750ns
[LISTENER]>> requestDestroyed
```

# JSP정의와 구성요소

## JSP등장 배경

서블릿에서 자바코드를 기반으로 HTML과 자바스크립트로 화면을 만들었다.

그러나 역할과 책임을 분리시켜HTML, CSS, JavaScript를 기반으로 JSP요소들을 사용해 동적 화면 구현.

**기존문제점**

- 서블릿에 비즈니스 로직과 화면 기능이 같이 있다보니 개발 후 유지보수 어려움

**해결책**

- 비즈니스 로직과 결과를 보여주는 화면 기능 분리
- 이로 인해 개발자는 비즈니스 로직 구현에 집중, 디자이너는 화면기능 구현에 집중
- 개발 후 유지보수, 재사용 수월

**구성요소**

- HTML, CSS 태그
- JSP기본, 액션 태그
- 프레임워크에서 제공하는 커스텀 태그

**톰캣 컨테이너에서 JSP 변환과정**

1. 변환단계(Translation): `.java`자바로 변환
2. 컴파일 단계(Compile): `.class` 변환
3. 실행단계(Execute): `.class`를 실행하여 HTML로 표현됨.

**주의사항**

HTML 주석(`<!-- 주석 -->`)은 JSP 전처리 단계에서 주석으로 처리하지 않고 해석된다.

## JSP페이지 구성요소

### 디렉티브 태그

- JSP 여러가지 속성을 정의하는 태그

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ include file="../jsp/header.jsp"%>
```

- **인클루드 디렉티브 태그**
    - 재사용성이 높다
    - JSP 페이지의 유지관리가 쉽다.
    - 예시
        
        **header.jsp**
        
        ```java
        <%@ page contentType="text/html;charset=UTF-8" language="java" %>
        
        <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
        <c:set var="path" value="${pageContext.servletContext.contextPath}"></c:set>
        <meta name="viewprot" content="width=device-width, initiial-scale=1"/>
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
        <link href="https://getbootstrap.com/docs/5.3/assets/css/docs.css" rel="stylesheet">
        <style>
          #loginInfo {
            display: flex;
            position: relative;
          }
        </style>
        <div id="loginInfo">
            <p>
                <c:if test="${loginMember.member_name == null}"/>
                손닙이시군요
                </c:if>
                <c:if test="${loginMember.member_name != null}"/>
                ${loginMember.member_name} 님이 접속중입니다.(session)
                </c:if>
            </p>
            <a href="${path}/auth/logout.do" class="btn btn-danger">로그아웃</a>
        </div>
        
        ```
        
        **empLIst.jsp**
        
        ```java
        <%@ page contentType="text/html;charset=UTF-8" language="java" %>
        
        <html>
        <head>
            <title>Title</title>
            <%--    <%@ include file="../jsp/header.jsp"%>--%>
        
        </head>
        <body>
        <div class="container">
            <%--include 디렉티브 태그는 jsp를 합쳐서 컴파일 한다.--%>
            **<%@include file="../jsp/header.jsp" %>**
        ....
                            <button class="btn btn-success" onclick="**location.href='${path}/emp/detail.do?empid=${emp.employee_id}**'">수정
                            </button>
        ...
        </html>
        ```
        

### 스크립트 요소: 주석문, 표현식, 선언식, 스크립트릿 → `<% %>`

`<!%` : 선언문 (instance field, instance method)

`<%=` : 출력문 (out.write)

`<%` : 자바코드

### 표연언어(Expression Language)

- Expression Language를 사용하여 내장객체 조회

```java
1. ${company}<br>
2. ${requestScope.company}<br>
3. ${sessionScope.company}<br>
4. ${applicationScope.company}<br>

<%=request.getAttribute("company")%><br>
<%=session.getAttribute("company")%><br>
<%=application.getAttribute("company")%>
```

### 내장객체

JSP가 서블릿으로 변환될 때 자동으로 생성되는 객체들

**JSP에서 제공하는 내장 객체들**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/2c232bdc-d7da-4101-b933-a310e0829638/7f5c7b5b-56fb-4333-b2a1-9fcd2e904548.png)

**내장 객체들의 스코프**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/379e81a8-c5c5-4dab-903c-5d61afb50dd1/036cebe6-71f4-4032-9cc9-2f3cc865b12c.png)

## JSP 페이지 예외 처리

### JSP 페이지 예외 처리 과정

**error500.jsp**

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" **isErrorPage="true"** %>
<html>
<head>
    <title>오류정보</title>
</head>
<body>
<h1>오류 정보</h1>
**message : <%=exception.getMessage()%>**
</body>
</html>
```

**web.xml**

```java

```