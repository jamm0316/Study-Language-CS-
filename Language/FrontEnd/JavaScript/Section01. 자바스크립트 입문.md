[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

목차

</aside>

## 자바스크립트로 무엇을 할까

**웹 요소 제어**

- 웹 UI부부에 많이 활용

**웹 어플리케이션 제작**

- 실시간 정보를 주고 받는 애플리케이션

**다양한 라이브러리**

- jQuery, React등 사용가능

**서버를 구성하고 서버용 프로그램 만들기**

- node.js: 프론트엔드 개발에서 사용하던 자바스크립을 백엔드에서도 사용 가능

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/137591d6-6492-433a-be2a-d42027f6a1ec/image.png)

### 작성방법

- head, body(최상단, 최하단)에 작성할 수 있고 위에서 아래로 흐른다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/a1444686-4b2b-4f87-bf10-b9d7223ec344/image.png)

### 웹 브라우저에서 스트립트 해석과정

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/a553b06a-b2e3-4a11-bd6c-9da4d2cead4e/image.png)

### 간단한 입출력 방법

- `alert`: 알림창
- `confim`: 확인과 취소 선택지가 있는 창
- `prompt`: 입력한것 들어가는창
- `document.write`: HTML에 문자열 입력
    - 지정한 곳에 출력가능한 방법
        
        `*document*.querySelector("#here").innerHTML = "here에 문자열 출력";`
        
- `console.log()`: 개발자 도구에서 확인가능

## 스타일 가이드

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/3c4b3cb3-c250-40b9-9eb0-f884b83dfe0b/image.png)

### 기본규칙

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/0fd2badd-d228-415d-a7b8-316c12f46207/image.png)

# 변수, 연산자, 자료명

## 변수

### 변수 선언 규칙

**변수 선언하기**

- var, let, const
- 변수 선언 방법
- var을 붙히면 지역변수, 없으면 전역변수

## 자료형

**6가지 자료형**

- `undefined`, `Number`, `String`, `boolean`, `object`, `function`
- 값이 할당되면 타입이 결정된다.

```jsx
//var score;  --> Hoisting
console.log(score + "====>" + typeof(score));  //type: undefined

var score = 100;
console.log(score + "====>" + typeof(score));  //type: Number

score = 'A학점'
console.log(score + "====>" + typeof(score));  //type: String

score = 10>20;
console.log(score + "====>" + typeof(score));  //type: boolean

score = null;
console.log(score + "====>" + typeof(score));  //type: object(null)

score = [10, 20, 30, "A", 'B', 10>20, function(){}, [1, 2, 3]];
console.log(score + "====>" + typeof(score));  //type: object

score = {};  //JavaScript Object
console.log(score + "====>" + typeof(score));  //type: object

score = function(){ };  //JavaScript Object
console.log(score + "====>" + typeof(score));  //type: function
```

### 호이스팅

`var`이 붙어있는 변수를 가장 위로 올려서 먼저 수행함.

- JavaScript: 호이스팅을 제일 먼저 거친 후 순서대로 실행한다.

### 배열

하나의 변수 이름으로 여러가지의 타입의 값을 묶은 것

index를 이용하여 접근.

## 연산자

### 비교연산자

`===` : 값과 타입이 모두 같은지 비교

`!==` : 값과 타입이 모두 같지 않은지

```jsx
// document.querySelector("#here")
// document.getElementById("here")
// id는 바로 접근 가능
here.innerHTML = 3 == '3';  //값 비교(true)
here.innerHTML += 3 === '3';  //값과 타입을 비교(false)
var a = 100;
var b = "100";
here.innerHTML += a == b;  //값 비교(true)
here.innerHTML += a === b;  //값과 타입을 비교(false)
here.innerHTML += a !== b;  //값과 타입을 비교(true)
```

## 조건문

### if 문과 if~ else문

### 삼항연산자

### 논리연산자

### Switch문

## 반복문