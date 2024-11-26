[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

content

</aside>

# 객체 알아보기

## 객체

- 프로램에서 인식할 수 있는 모든 대상

**자바스크립트 객체**

- dom객체: 문서 뿐만아니라 웹 문서 안에 포하된 이미지, 링크,텍스트 필드
- 브러우저 관련 객체: 웹 브라우저 정보를 객체로 관리
- 내장객체: 웹 프로그래밍에서 자주 사용하는 요소

**사용자 정의 객체**

**객체의 인스턴스 만들기**

- new 객체

**프로퍼티와 메서드**

- 프로퍼티: 객체의 특징이나 속성
- 메서드: 객체에서 할 수 있는 동작

### 내장 객체 - Array 객체

**인스턴스 만들기**

1. 초기값 없 경우
    
    `var arr = new array(4)`
    
2. 초기값 있는 경우
    
    `var arr = [10, 20, 30]`
    

**메서드**

- **concat()**
- **join()**

- shift(): 앞에서 하나 뺴기
- unshift: 앞에 하나 추가

- pop(): 뒤에서 하나 빼기
- push(): 뒤에 하나 추가

- splice() : 기존 배열 변경하며, 배열 자르기
    - `splice(index, [갯수], [넣을 요소])`
    - 갯수 안정하면 끝까지
- slice(): **기존 배열은 건드리지 않고,** 배열 자르기
    - 끝까지 가져오기

### 내장객체 - Date

인스턴스 만들기

`new Date();`

### 내장객체 - Math

수학 계산 관련된 메서드 포함.

## 브라우저 객체

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f74b5ca3-6a4e-4e65-8458-72343dfb5811/image.png)

# DOM

## 문서객체모델

자바스크립트를 이용하여 웹 문서에 접근하고 제어할 수 있도록 객체를 사용해 웹 문서를 체계적으로 정리하는 방법

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/3ae73a33-32db-4da0-abcb-b90119aeb81a/image.png)

### DOM 트리

- 노드(node): DOM트리에서 가지가 갈라져 나간 항목

### 구성원칙

- 모든 HTML 태그는 요소
- 텍스트: 텍스트(text) 노드
- 태그의 속성: 속성(attribute) 노드
- 주석

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f79b6297-bdaa-4b97-8585-617048a47c8a/image.png)

### DOM 요소에 접근하기

`getElementByID()`

`getElementsByClassName()` : 반환값 2개이상, HTMLCollection 객체에 저장됨.

`getElementsByTagName()`: 반환값 2개이상, HTMLCollection 객체에 저장됨.

**더 선호됨.**

`querySelector()`

`querySelectorAll()`

`innerText`

`innerHTML`

`getAttribute(”속성명”)`

`setAttribute(”속성명”, “값”)`

### DOM 요소에 직접 연결하기

- 익명함수로 직접연결
- 선언적 함수로 간접연결

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/e2cfce54-c83b-4c99-bf88-706a7d1fb3d1/image.png)

event 객체

**this**

이벤트가 발생한 대상에 접근하는 예약어

# 이벤트

## 이벤트 관련 용어

### 이벤트 연결

**이벤트 모델**

**이벤트 모델 분류**

- DOM Level 단계에 따라 두가지로 분류

### 고전 이벤트 모델

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/4f21f037-a0b4-45ff-aa6d-5c3153f1def8/image.png)

**이벤트 제거**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/08b08c01-253f-4b9f-95f0-9ce490707bb0/image.png)

**이벤트 강제 실행**

### 인라인 이벤트 모델

html 태그 안에 속성 넣는 방법.

### 디폴트 이벤트

- `summit`의 경우 `form`에 `action`이 없어도 간다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/27e41513-06df-45ff-a657-0bdc684704b9/image.png)

- 이벤트 리스너에 `return false;`를 하면 디폴트 이벤트를 제거한다.

### 이벤트 전달

**이벤트 버블링**

자식노드에서 부모노드까지 이벤트. 발생하는 것

**이벤트 캡쳐링**

부모노드에서 자식노드까지 이벤트가 발생하는 것.

`stopPropagation()`메서드로 이벤트 전파를 막는다.

## 표준 이벤트 모델

**기본형**

`*btn1*.addEventListener("mouseout", f_mouseout, false);`

`*btn2*.addEventListener("mouseover", f_mouseover2);`

**CSS속성에 접근하기**

`document.요소명.style.속성명`

중간에 - 이 있으면 카멜표기법이나 문자열로 싸야함

`background-color` → `backgroundColor`

**노드 리스트**

`querySelectorAll()` 메서드로 가져온 여러개 노드

텍스트 노드를 사용하는 새로운 요소 추가

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/ef8d6ac3-a762-4571-8ba7-d3b07289720a/image.png)

```jsx
var element = document.createElement("h3");  //<h3></>h3>
var txt = document.createTextNode("치킨치킨치킨 먹으러 가자~");
element.appendChild(txt);
document.querySelector(".here_obj").appendChild(element);
```