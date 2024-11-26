[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

목차

</aside>

# 자바 코드를 없애는 액션태그

## 인클루드 액션 태그 사용하기

화면에서 자바코드를 없애는 액션코드

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9e30b270-4504-4ce4-b21c-fe9e4e0946ab/ec09c61f-42cd-4abd-9d0f-e47bd1feddd4.png)

**인클루드 액션 태그**

인클루드 디렉티브 태그처럼 화면을 분할해서 관리하는데 사용되는 태그

**인클루드 액션 태그 처리 과정**

- 기존 가JSP 컴파일 된다.
- 컴파일 된 기존 JSP파일이 include action tag  JSP로 들어온다.
- include action tag JSP 파일이 따로 컴파일 된다.

**기본형**

```java
<jsp:include page="../jsp/common.jsp"></jsp:include>
<jsp:forward page="../jsp/common.jsp"></jsp:forward>
```

## useBean, setProperty, getProperty

## 자바 빈

- Java EE 프로그래밍 시 사용되는 클래스

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/6a8ca7e9-5044-4cc9-9a61-f63267f55af0/image.png)

**자바 빈 예제**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/17c2836f-2947-4756-ae54-c2d3bad2661e/image.png)

### 유즈빈

**유즈빈 액션 태그**

JSP 페이지에서 자바 빈을 대체하기 위한 태그

**기본형**

```java
<jsp:useBean id="member2" class="com.fistzone.member.MemberDTO" scope="request"/>
```

### setProperty/getProperty

setter, getter의 기능 수행

**기본형**

```java
<%-- Setter --%>
<jsp:setProperty name="member2" property="member_id" value="song2 <action tag>"/>
<jsp:setProperty name="member2" property="member_name" value="송재명2 <action tag>"/>
<jsp:setProperty name="member2" property="member_email" value="shin2@han.com <action tag>"/>
<jsp:setProperty name="member2" property="member_pass" value="12341234 <action tag>"/>

<%-- Getter --%>
<p><jsp:getProperty name="member2" property="member_id"/></p>
<p><jsp:getProperty name="member2" property="member_name"/></p>
<p><jsp:getProperty name="member2" property="member_email"/></p>
<p><jsp:getProperty name="member2" property="member_pass"/></p>
```

### forward/include

**기본형**

```java
<jsp:forward page="beanTest2.jsp"></jsp:forward>
<jsp:include page="beanTest2.jsp"/>
```

- forward 실행화면
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/aabcba09-a9fb-4233-ad2d-8124c4fa9b16/image.png)
    
- include 실행화면
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/596048ae-6714-4259-9e2c-1f175d8a4e7d/image.png)
    

# 표현 언어와 JSTL (중요***)

## 표현언어(Expression Language)

자바 코드가 들어가는 표현식을 좀 더 편리하게 사용하기 위한 데이터 출력 기능

**특징**

- 편리하게 값 출력
- 변수 연산자 포함가능
- 자바 빈 속성, 내장 객체 속성 출력
- 표현 언어 자체 내장 객체도 제공
- jsp 페이지 생성 시 기본 설정은 표현언어 사용 불가
- 페이지 디렉티브 태그에서 반드시 반드시 반드시 `isELIgnored = flase` 로 설정해야함.

**기본형**

```java
${표현식 or 값}
```

**파일 읽는 주체**

1. `${}`: 톰캣이 해석(JSP 문법)
2. `$()`: 브라우저가 해석 (jQuery 문법)
3. ``\${}``: 톰캣이 해석 안함 → 브라우저에서 ``\${}`` 해석
    
    ```jsx
    //<javaSript>
    $("h1").css("background-color", "indigo");
    $("h1").css("color", "whitesmoke");
    
    //script안에서 사용했으므로 ${}톰캣이 해석, 문자열이므로 ""로 싸줘야함.
    let contextPath = "${pageContext.request.servletContext.contextPath}";
    
    //${contextPath}를 톰캣이 해석하므로 contextPath가 그대로 나옴
    //그러나 \${}을 사용하면 브라우저가 해석하므로, let contextPath 변수에 할당된 값이 나오게됨.
    let str = `this application path: \${contextPath}`;  // \$는 톰캣이 해석 안함 -> 브라우져가 함.
    console.log(str);
    ```
    

**빈 속성 접근 방법**

```java
${빈객체}.속성
```

## 표현언어 내장 객체(내장변수)

ArrayList

HashMap

## 바인딩 속성 출력하기

표현언어 같은 속성에 대한 우선순위

page > request > session > application

## 커스텀 태그

**커스텀 태그**

액션태그나 표현언어를 사용하더라도 조건식이나 반복문 이용하여 자바코드 제거 하기 위한 기능

**태그 종류**

- **JSTL (JSP Standard Tag Library)**
    
    JSTL라이브러리 따로 설치해서 사용, JSP에서 가장 많이 사용
    

- **개발자 만든 커스텀 태그**
    
    스트러츠나 스프링 프레임워크에서 미리 만들어서 제공
    

**표준 태그 라이브러리(JSP Standard Tag Library)**

커스텀 태그 중 가장 많이 사용되는 태그를 표준화하여 라이브러리로 제공

- 라이브러리 주입방법

```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

## Core태그 라이브러리 사용하기

### **값 저장 및 사용**

```java
<c:set var="path" value="${pageContext.servletContext.contextPath}" scope="application"/>
<c:set var="major" value="컴퓨터공학과" scope="page"/>
<c:set var="path2" value="${pageContext.servletContext.contextPath}" scope="page"/>

2. getAttribute(값을 읽음):
2. getAttribute: ${major}
2. getAttribute: ${path2}
```

### **조건문**

```java
<c:if test="조건문">
```

**예제**

```java
<c:if test="${loginMember == null}" scope="session">
    <h2>로그인 안된 사용자</h2>
    <ul>
        <li><a href="${path}/dept/deptSelectAll.do">부서조회</a></li>
        <li><a href="${path}/dept/deptInsert.do">부서입력</a></li>
    </ul>
</c:if>

<c:if test="${loginMember != null}">
    <h2>로그인된 사용자</h2>
    <ul>
        <li><a href="${path}/emp/emplist.do">직원조회</a></li>
        <li><a href="${path}/emp/empInsert.do">직원입력</a></li>
        <li><a href="${path}/dept/deptSelectAll.do">부서조회</a></li>
        <li><a href="${path}/dept/deptInsert.do">부서입력</a></li>
    </ul>
</c:if>
```

### **Choose문**

```java
<c:choose>
    <c:when test="${90<=score && score<=100}">
        <p>A학점</p>
    </c:when>
    <c:when test="${80<=score && score<90}">
        <p>B학점</p>
    </c:when>
    <c:otherwise>
        <p>노력요망</p>
    </c:otherwise>
</c:choose>
```

### forEach문

```java
<ul>
<c:forEach items="${empDatas}" var="emp" varStatus="loopstatus">
    <li>${loopstatus.index} / ${loopstatus.count} / ${loopstatus.first} ---> ${emp.first_name} </li>
</c:forEach>
</ul>
```

- c:if 문과 조합

```java
<c:forEach items="${empDatas}" var="emp" varStatus="status">
        <tr>
            <td>
                    ${status.count}
                <c:if test="${status.first}">
                    첫번째
                </c:if>
                <c:if test="${status.last}">
                    마지막
                </c:if>
                <c:if test="${status.count mod 2 == 0}">
                    / 짝수
                </c:if>
            </td>
```

### url 문

- JSTL의 url태그는 절대경로 사용 시, default로 context path가 사용한다.
- 상대경로일 경우에는 들어가지 않는다.
    
    ```java
    <c:url var="empDetail" value="/emp/detail.do">
        <c:param name="empid" value="100"/>
    </c:url>
    <a href="${empDetail}">100번직원 상세보기</a>
    ```
    
    - `value="/emp/detail.do"`  → `contextPath/emp/detail.do`

### redirect 태그

- response.sendRedirect() 기능을 대체함.

### out 태그

- 현재 문서에 내용 출력

# HTML5와 제이쿼리

## 시멘틱 웹을 위한 구성요소

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/46d381e8-b6bb-43be-aca5-f8c102f2e896/image.png)

## Ajax 동작방식

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/212e98cb-509b-426b-bb83-ce0f2eeb336d/image.png)

### Ajax 사용형식

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f7766949-cc7c-4f0e-a1c5-ce3a63e08ba7/image.png)

## 제이쿼리에서 JSON 사용하기

**JSON정의**

- name/value 쌍으로 이루어진 데이터 객체를 전달하기 위해 인간이 읽을 수 있는 텍스트를 사용하는 개방형 표준 데이터 형식
- 비동기 브라우저/서버 통신(Ajax)을 위해 XML을 대체하는 데이터 전송 형식
- 자바스크립트에서 파생된 것이므로 자바스크립트 구문 형식을 따르지만 프로그래밍 언어나 플랫폼에 독립적이어서 쉽게 사용할 수 있음.

Maven에 의존성 추가하기

**pom.xml**

```xml
<dependency>
    <groupId>com.googlecode.json-simple</groupId>
    <artifactId>json-simple</artifactId>
    <version>1.1.1</version>
</dependency>
```