[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

목차

</aside>

---

<aside>
📄

**서블릿을 한마디로 표현한다면?**

서블릿의 고도화된 추상화 클래스가 스프링의 `Controller`

서블릿은 받은 요청을 서버에서 처리할 수 있도록 다리 역할을 해주는 객체(class)이며, ajax는 서블릿으로 요청을 보내는 메서드이다.

ajax가 직접 서버와 통신하지 않는이유는 보안, 비즈니스 로직, 데이터베이스 연동, 안됨. 유지보수 및 확장성 서블릿은 중앙관리자 역할

</aside>

- 디테일
    
    좋은 질문이에요! AJAX가 서버와 직접 통신하지 않고, **서블릿을 통해 서버와 소통**하는 이유는 여러 가지 중요한 역할 때문입니다.
    
    **1. 보안**
    
    •	AJAX가 서버에 직접 접근하는 방식은 보안상 위험할 수 있습니다. 서버의 민감한 데이터나 내부 로직이 노출될 가능성이 커지죠.
    
    •	서블릿은 이러한 요청을 중간에서 처리하며, 보안 설정을 통해 권한 검사를 하거나 특정 데이터만 허용하는 등 **보안 필터** 역할을 할 수 있습니다.
    
    **2. 비즈니스 로직 처리**
    
    •	서블릿은 단순히 데이터 전달만 하는 것이 아니라, **비즈니스 로직을 처리**하는 중요한 역할을 합니다. 클라이언트는 데이터를 단순히 요청할 뿐, 복잡한 연산이나 데이터베이스와의 상호작용은 서버에서 이루어지죠.
    
    •	예를 들어, 로그인 요청이 들어오면, 서블릿이 데이터베이스를 조회해 사용자 인증을 처리하고 결과를 반환합니다.
    
    **3. 데이터베이스와의 연동**
    
    •	일반적으로 AJAX는 데이터베이스에 직접 접근할 수 없습니다. **데이터베이스 접근은 서버 측에서 이루어지는 작업**이므로, AJAX는 데이터베이스를 직접 다룰 수 없고, 서블릿이 이 역할을 대신합니다.
    
    •	서블릿은 클라이언트로부터 받은 요청을 바탕으로 데이터베이스와 상호작용하여 데이터를 가져오거나 저장할 수 있습니다.
    
    **4. 통신 표준화와 응답 포맷 관리**
    
    •	서블릿은 다양한 클라이언트 요청을 통일된 형태로 처리하고, 필요한 응답 포맷(JSON, XML 등)을 생성하여 반환하는 **중앙 관리 역할**을 합니다.
    
    •	이를 통해 클라이언트는 응답 형태에 상관없이 일관되게 데이터를 사용할 수 있습니다.
    
    **5. 유지보수 및 확장성**
    
    •	비즈니스 로직을 클라이언트 코드에 넣으면 코드가 복잡해지고 유지보수가 어려워집니다. 대신 서블릿을 통해 서버 측에서 로직을 처리하면, 변경이나 확장 시 클라이언트 코드를 건드릴 필요가 없으므로 유지보수가 더 쉬워집니다.
    
    **결론**
    
    서블릿은 AJAX 요청을 받아 서버가 해야 할 작업을 수행하고, 안전하게 클라이언트에게 필요한 데이터만 돌려주는 역할을 합니다. **AJAX가 직접 서버에 접근할 수 있다 하더라도, 보안, 데이터 처리, 응답 관리 등의 이유로 서블릿과 같은 서버 측 처리가 필수적**입니다.
    

---

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/eb896a88-8cbf-49a6-8739-48e6fc606948/image.png)

## 서블릿의 세가지 기본기능

### 서블릿 기본 기능 수행과정

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/3b7beeff-37ae-44e5-9610-21488f758065/8d2ce87d-7eb0-4428-b604-6f285adc2089.png)

1. 클라이언트로부터 요청을 얻음
2. 데이터베이스 연동과 같은 비즈니스 로직 처리
3. 처리된 결과를 크라이언트에 응답

**HttpServletRequest 메서드**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/0fa51f9f-d508-4332-b646-4691e930d311/image.png)

- request, response는 Token의 형태로 한번 사용되고나면 사라짐.

### <form> 태그로 서블릿 요청

1. 브라우저에서 요청

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/2adc355c-8a1e-4e4b-b924-dc8489e10dc0/image.png)

1. parameter가 서블릿으로 넘어감.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/c2d741b8-a75c-4402-8b16-7f990acebb75/image.png)

# 경로를 제대로 알아야 요청을 주고받을 수 있다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/b6c4ac01-8973-46de-b0db-4e02933244b1/image.png)

**오류번호**

- 404: 요청주소 잘못됨(주소없음)
- 500: 서버프로그램 오류(IDE console창 확인)
- 405: 요청한 방식의 함수 없음(post방식 없음)
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/48e5f049-2081-41fc-ae79-ade3264ced87/image.png)
    

### sendRedirect()

```java
//재요청 -> Browser의 요청 주소가 바뀐다.
response.sendRedirect("emplist.do");
```

- result페이지를 보여준 후, redirect 페이지를 호출하려면 `result.jsp`에서 `meta data`를 넣어준다.
    
    ```java
    <meta http-equiv="refresh" content="3;url=deptSelectAll.do"/>
    ```
    
    3초 뒤에 url이 `deptSelectAll.do` 로 가라는 뜻.
    
    여기서 url은 `context/현재 폴더 위치/` 가 `default`값이다.
    따라서, 현재 폴더가 `webapp/dept` 라면, `context/dept/deptSelectAll.do` 가 호출되고, 이는 웹 요청으로 해석되어, 서블릿이 이 요청을 받아 `deptList.jsp`를 호출한다.
    
- `sendRedirect()` 개념도
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/c7b29813-9946-4a6b-b99c-afe077779929/image.png)
    

### 웹 - 서블릿 - 모델 - DB의 개념도

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/a22a3b51-9e30-432e-ab82-1d97b75253bd/image.png)