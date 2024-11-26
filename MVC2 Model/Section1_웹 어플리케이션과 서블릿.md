[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

목차

</aside>

## 1. 프로그램 발전과정

### 클라이언트 PC기반 프로그램

**문제점**

- 프로그램이 변경될 떄마다 일일이 다시 설치해야함.
- 데이터베이스 접속 정보와 같은 정보가 쉽게 노출 될 수 있어 보안에 취약.

### 클라이언트/서버 프로그램

**특징**

- 클라이언트 프로그램은 화면 기능과 데이터 송수신만 제공
- 모든 기능은 서버에서 수행
- 기능이 변경되어도 모두 서버에서 처리하므로 클라이언트 프로그램은 수정할 필요 없음
- 보안측면에서 우수함

**구조**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/a24c0c7b-8ae0-4678-ab0d-a95a4cb2c165/image.png)

**문제점**

- **그러나 사용자 관련 화면 기능이 바뀌면 클라이언트 프로그램도 수정해서 다시 설치해야함.**

### 서버프로그램

**구조**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/5df83581-12b9-4601-b159-86bbeb87e931/image.png)

**특징**

- 클라이언트가 가지고 있는 것이 없으므로 변경에 유연하다.

## 2. 웹 프로그래밍과 JSP

### 웹 프로그래밍 기본

**정적(static) 웹프로그래밍**

- HTML, CSS, JavaScript 처럼 미리 보여줄 파일을 저장해두고 브라우저에서 요청할 경우 그대로 전달
- 환율정보나 주가 정보등 실시간 정보를 표시하는데 적합하지 않음

**정적(static) 웹프로그래밍 구성요소**

- 웹 서버: 각 클라이언테에게 서비스를 제공하는 컴퓨터
- 클라이언트: 네트워크로 서버 접속한 후 서버로부터 서비스 제공받는 컴퓨터
- HTTP 프로토콜: www서비스를 제공하는 통신 규약
- HTML: www 서비스를 제공하기 위한 표준언어
- CSS: HTML 디자인
- JavaScript: HTML 동적기능 제공

### 정적 웹 프로그래밍

**문제점**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/200c67bb-6905-417d-bc43-8be37ff082f7/image.png)

### 동적 웹 프로그래밍

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f4a58209-ae07-4a3b-8655-668b38bda43e/image.png)

**특징**

- 정적 웹프로그래밍에서 관리자의 역할을 웹 애플리케이션 서버(Web Application Sever, WAS)가 수행
- 처음에 동적인 방식으로 프로그램을 제공하는 기능은 CGI(Common Gate Interface, 공용게이트웨이)였음
- WAS가 해석하고 나면 HTML반환 → 웹서버로 전달 → 실시간 정보 전달 가능.

**CGI동작방식**

- 초기 웹 프로그램에서 사용하는 방식
- 프로세스 방식으로 실행됨
    - 요청자 수만큼 프로세스가 실행됨.
    - 따라서 많은 사용자가 요청을 보내면 프로그램이 다운됨.
- 서버 부하 심함

**문제점**

- 처음부터 메모리에 로드해서 수행하며 메모리 과부하가 걸림
    - JSP, ASP, PHP로 발전함

### JSP 프로그램 특징

**특징**

- 브라우저 요청시 스레드방식으로 실행하므로 CGI보다 효율적.
- 클라이언트 요구를 처리하는 기능은 최초 한번만 메모리에 로드됨.
- 클라이언트가 동일한 기능을 요구하면 기존에 사용한 기능 재사용
- 프로세스 방식보다 훨씬 빠름.

## 3. 개발환경 설정

- JDK
- 톰캣(Tomcat)
- 이클립스 or **InteliJ**
- java EE API
- 비주얼 스튜디오(VS code)
- Oracle DBMS
- SQL Developer
- exERD or **ERDCloud** ro ERDwin

## 4. 웹 애플리케이션 이해하기

### 웹 애플리케이션

기존의 **정적 웹애플리케이션 사용**하며,
서블릿, JSP, 자바 클래스들을 추가하여 사용자에게 **동적인 서비스를 제공**하는 프로그램

**웹 애플리케이션 기본 구조**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/7203d29d-4a33-4823-ae32-9cf711e50fc2/image.png)

- class: java, servlet
- lib: 필요한 라이브러리
- web: 웹 설정

### 컨테이너(=서버)

**개요**

**컨텍스트**: server.xml에 등록하는 웹 애플리케이션

즉 톰캣 입장에서 인식하는 한 개의 웹 애플리케이션(appleShop….)

**특징**

- 웹 애플리케이션당 하나의 컨텍스트가 등록됨
- 웹 애플리케이션 이름과 같을 수도 다를 수도 있음.
- 이름 중복 불가
- 명사로 지정
- 대소문자 구분
- server.xml에 등록

### 웹 애플리케이션의 기본 구조

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/2330e81e-8942-4ce3-b850-084a793c42c6/image.png)

1. 웹 브라우저에서 **컨텍스트 이름**으로 요청
2. 요청을 받은 톰캣 컨테이너는 요청한 컨텍스트 이름이 있는지 확인 후 브라우저에 전송
3. 브라우저에 표시

## 서블릿 이해하기

### 서블릿이란

서버 쪽에서 실행되면서 클라이언트 요청에 따라 동적으로 서비스를 제공하는 자바 클래스.

- 서블릿 동작과정

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/1017bdef-2d3f-4de3-bb53-9234adb042c1/image.png)

- 웹 서버: 정적 자원 전달.(정적)
- WAS: 서버 프로그램 실행가능.(동적)
- 서블릿에서 스레드를 통해 요청 실행 후 html을 보내줌.

**특징**

- 서버쪽에서 실행.
- 스레드 방식.
- 자바로 만들어짐.
- 컨테이너에서 실행.
- 플랫폼 독립적.
- 보안기능 적용 쉬움.
- 엡 브라우저에서 요청 시 기능을 수행.

**서블릿 계층 구조**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/35a805a4-862b-400a-bfb8-235b326463ac/image.png)

- GenericSevlet: 추상클래스
- Servelt, ServletConfig: 인터페이스

**요청 과정**

클라이언트 요청 → public service() 호출 → protected service() 호출 → doGet, doPost() 호출

### 서블릿 생명주기

서블릿 실행단계마다 호출되어 기능을 수행하는콜백 메서드

- `init()`, `destroy()` 메서드는 생략가능 하나 `doGet()`, `doPost()`는 반드시 구현해야함.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f6febeab-a451-4a24-944a-fd771f5510bb/image.png)