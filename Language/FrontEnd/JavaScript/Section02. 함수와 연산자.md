[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

목차

</aside>

# 함수와 이벤트

## 함수

동작해야할 목적대로 명령을 묶어 놓은 것.

자바스크립에는 이미 여러 함수가 만들어져 있음.

### 함수의 선언 및 호출

기본형: `function f1() {}`

- 예약어는 변수로 선언할 수 없다.

```jsx
function f1() {
    var alert = 100;
    alert("aaaa");
}
```

**Error**

```jsx
Uncaught TypeError: alert is not a function
```

### var를 사용한 변수의 특징

**호이스팅**

- 변수를 뒤에 선언하지만 앞에서 미리 선언한 것처럼 인식.

**재선언과 재할당 가능**

- 재선언: 이미 선언된 변수 다시 선언 가능.
- 재할당: 같은 병수에 다른 값을 할당할 수 있음
    
    → 재선언, 재할당을 많이하면 실수할 확률 높아짐.
    

### let과 const의 등장.

**let을 사용한 변수**

- 블록 변수 - 블록{} 안에서만 사용할 수 있다.
    
    → 전역 변수는 변수 이름과 초깃값만 할당하면 됨.
    
- 호이스팅 없다.
- 재선언, 재할당 없다.

```jsx
function f1() {
      //var: 지역변수, 함수 내에서 유효하다.
      //let: block({}) 내에서 유효하다.
      var su = 100;
      var total = 0;  //undefined: 값을 할당하지 않으면 타입이 결정되지 않으므로 이게 뜬다.
      //NaN: Not A Number: 숫자가 아니다. undefined + 1 => NaN
      for (let i = 1; i <= su; i++) {
        //console.log(i, total);
        total += i;
      }
      document.querySelector("#here").innerHTML = (i - 1) + "까지 합: " + total;
    }
```

**Error**

```jsx
Uncaught ReferenceError: i is not defined
```

**const를 사용한 변수**

- 상수
- 변경 불가

```jsx
const name = "신한DS";
      name = "ds";
```

**Error**

```jsx
3Function.html?_ijt=jh2j32pmfged9huk78moh3opdh&_ij_reload=RELOAD_ON_SAVE:13 Uncaught TypeError: Assignment to constant variable.
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/fc8b70b0-3fc9-436a-8266-cd65802ce9ae/image.png)

## 함수

### 익명함수

```jsx
function f4() {
  //2. 익명함수(이름없는 함수): 함수가 변수에 할당됨.
  var add = function (a, b) {
    return a + b;
  };
  var result = add(10, 20);
  document.querySelector("#here").innerText = result;
}
```

### 즉시실행함수

```jsx
function f5() {
  var result = (function (a, b) {
    return a + b;
  })(30, 40);
  document.querySelector("#here").innerText = result;
}
```

### 화살표함수

```jsx
function f6() {
  var f = (a, b) =>a + b;
  document.querySelector("#here").innerText = f(1, 2);
}
```

### 내부함수

내부 함수 선언 시 외부함수에 영향을 받지 않는다.

```jsx
function internalFunction() {
  alert("함수 내의 함수 -> 내부함수");
}

function call() {
  document.querySelector("#here").innerText = "2";

  internalFunction();
  //변수(var)선언만 호이스팅 되고, = 이후로는 다음 절에 와야만 f에 바인딩 된다.
  // f();

  //1. 선억적 내부 함수
  function internalFunction() {
    alert("함수 내의 함수 -> 내부함수");
  }
```

### **오버로딩**

함수 선언 시 인자를 선언하지 않으면, `arguments` 로 배열의 형식으로 받을 수 있음.

```jsx
function f7() {
  var result = sumALl(10, 20, 30, 40, 50, 60, 70, 80, 90, 100);
  document.querySelector("#here").innerText = result;
}

function sumALl() {
  //arguments
  //arguments = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
  var result = 0;
  for (let i = 0; i < arguments.length; i++) {
    result += arguments[i];
  }
  return result;
}
```

자바처럼 만들면 나중함수가 적용됨

```jsx
function sumAll(a, b, c) {
  return a + b + c;
}

function sumAll(a, b) {
  return a + b;
```

### 콜백 함수

**정의**

- 함수를 parameter로 보내고 받은 쪽에서 수행.
- 콜백 함수는 **특정 작업이 완료된 후 실행되거나** **어떤 이벤트에 반응하여 실행**되는 함수입니다. 콜백 함수는 주로 **다른 함수에 인자로 전달되어** 특정 조건이 충족되면 실행됩니다.

**목적**

- **특정 작업을 완료한 후에 실행**되도록 하여 **재사용 가능하게** 하기 위함입니다.
- 예를 들어, 비동기 작업(예: 데이터 요청) 이후에 실행할 작업을 정의하는 경우나, 이벤트가 발생할 때 특정 동작을 수행하게 하려는 경우에 유용합니다.

**콜백함수 예제**

```jsx
function f10() {
  //함수를 정의
  var f = function () {
    document.querySelector("#here").innerText = new Date();
  };

  //함수를 실행
  //Handler =listener: 동작을 주는(뭐해야하는지) 것(함수)
  //timeout: 몇 ms마다 실행할 지
  setInterval(f, 1000);

  //1번만 실행됨
  //함수를 넘긴것이 아니라 함수의 리턴값을 넘긴 것이기 때문에 f()는 즉시 실행되고,
  //setInterval(undefined, 1000)을 실행하게 되고 반복할 매개변수가 없으므로 중단된다.
  // setInterval(f(), 1000);
}
```

### 클로저 함수

**정의**

- 지역변수를 남겨두는 현상
- 클로저는 **함수와 그 함수가 선언될 당시의 렉시컬 환경을 기억하는 메커니즘**입니다. 클로저를 사용하면 **함수 내부의 지역 변수**가 외부에서도 접근 가능하게 됩니다.

**목적**

- 지역변수는 함수가 끝나면 사라진다.
- 그러나 지역변수를 다른 지역에서도 이어서 사용하기 위해 function을 return하여 함수 형태로 받아 다른 지역에서 사용.
- **함수의 지역 변수를 외부에서도 사용할 수 있도록 하기** 위함입니다.
- 보통 클로저는 **데이터를 은닉**하거나 **상태를 유지**하기 위해 사용됩니다.

**주의사항**

- 코드 오류 시 웹 페이지 오류 발생 및 경고창 출력 안함.
- 함수 안의 지역 변수는 함수 외부에서 사용 불가능.
- 지역변수는 함수 실행 시 생성되고 종료 시 사라짐.

```jsx
function f11() {
  var aa = returnFunc();
  var result = aa();
  document.querySelector("#here").innerText = result;

}

function returnFunc() {
  //클로저: 규칙(지역변수는 함수내에서만 사용) 위반
  //지역변수를 함수에 담아 보내면 다른데에서 지역변수 사용가능.
  //지역변수는 함수 종료 후 사라지나, function에 담겨서 다른 함수로 넘어가면 사라지지 않음.
  var score = 100;
  //함수내에서 반환되는 값
  var f = function () {
    alert("나의 점수는" + score)
    return score;
  };

  //함수가 함수를 반환
  return f;
}
```

**클로저 응용**

- global 변수를 선언하지 않고 count를 증가시키는 버튼을 만들고싶다.

**클로저 예제 1**

- 글로벌 변수가 아닌 지역변수로 count를 늘리고 싶다.

```jsx
// var cnt2는 global scope가 되어 바람직하지 않은 변수 선언이다.
var cnt2 = 0;
function f13() {
  cnt2++;
  document.querySelector("#here").innerText = add();
}
```

**리펙터링한 코드**

```jsx
//즉시실행함수: (function(){})()
    //add = function() {var count; return count++;}
    var add = (function () {
      var count = 0;
      console.log("즉시 실행 함수는 1번 실행됨.")
      return function () {
        count++;
        return count;
      };
    })();

function f12() {
  document.querySelector("#here").innerText = add();
}
```

**클로저 예제 2**

- 문제: i가 3만 3번 출력됨

```jsx
function f14() {
  for (var i = 0; i < 3; i++) {
    //setTimeout(): 일정ms 이 지나고 1회만 수행됨.
    //setInterval(): 일정ms 시간 마다 수행됨.
    setTimeout(function () {
      alert(i);
    }, 1);
  }
  //var은 함수 내에서 접근가능한 지역변수, 따라서 함수 종료시점까지 남아있음.
}
```

리펙터링한 코드

```jsx
function f15() {
  //method1: 지역변수를 다른 지역변수에 복사
  for (var i = 0; i < 3; i++) {
    (function (a) {
      setTimeout(function () {
        alert(a);
      }, 1);
    })(i);
  }
}

function f16() {
  //method2: let는 블록 내에서만 유효.
  for (let i = 0; i < 3; i++) {
    //function block 내에서 유효하기 때문에 var처럼 변경될 여기에 할당된 i는 var처럼 변경될 일이 없다.
    setTimeout(function () {
      alert(i);
    }, 1);
  }
}
```

**함수 내에서 자동으로 클로저 만들어 처리하기**

- **forEach**를 사용하면 각 element를 호출할 때마다 클로저 영역이 만들.
    
    ```jsx
    function f17() {
      var arr = [10, 20, 30];
      //arr을 돌면서 f함수를 수행
      arr.forEach(function (element, index, arr2) {
        console.log(element, index, arr2);
        setTimeout(function () {
          alert(index);
        }, 1);
      });
    }
    ```
    

## 자바스크립트 내장함수

### 인코딩과 디코딩

- **인코딩**: 문자를 컴퓨터에 저장하거나 통신 목적으로 부호화.
- **디코딩**: 인코딩을 다시 문자로 변환.

```jsx
function call3() {
  var url = "http://localhost:9090/appleShop4/emp/empRegister.jsp?fname=신한";

  var result = encodeURIComponent(url);
  document.querySelector("#demo").innerHTML = "incode: " +result;

  result = decodeURIComponent(result);
  document.querySelector("#demo").innerHTML += "<br>decode: " + result;
}
```

### 문자 숫자로 변환(parseInt)

```jsx
function call4() {
  var str = "1000원";
  document.querySelector("#demo").innerHTML = parseInt(str) + 2000;
}
```

### 초기값 설정

```jsx
function call5() {
  //document.querySelector("#demo").innerHTML = f_param(10, 20, 30);
  document.querySelector("#demo").innerHTML = f_param(10);
}

//method1. 조건절 이용
function f_param(a, b, c) {
  if (!b) {
    b = 200;
  }
  if (!c) {
    c = 300;
  }
  return a + b + c;
}

//method2. 논리 연산자 이용
function f_param(a, b, c) {
  b = b || 201;
  c = c || 201;
  return a + b + c;
}

//method3. parameter 초기값설정
function f_param(a, b = 202, c = 302) {
  return a + b + c;
}
```

## 전개 연산자

```jsx
function call6() {
  add(1, 2, 3);
  var arr = [10, 20, 30];
  add(...arr);
}

function add() {
  document.querySelector("#demo").innerHTML = arguments[0] + arguments[1] + arguments[2];
}

function add(...arr) {
  document.querySelector("#demo").innerHTML +="<br>" +  (arr[0] + arr[1] + arr[2]) + "!!!!!";
}
```

## 이벤트와 이벤트 처리기

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/977dad48-c8b4-4954-abff-ea7fcd21fbb8/image.png)

**이벤트**: 일어나는 사건

예) click, hover

**이벤트 속성**: 이벤트와 이벤트 핸들러를 연결하는 역할 

예) onclick, onmouseover

**이벤트 핸들러**

### internal javascript 만들기

- 이벤트 속성과 핸들러를 이용해 만든다
- `window.onload`: ‘html이 모두 로드된 이후에’ 라는 이벤트 속성에 function()을 넣는다.
- 따라서 function()은 자동 실행된다.

```jsx
<script>
//window가 로드가 끝나면 function함수를 부른다. = 콜백함수
window.onload = function () {
  var btn1 = document.querySelector("#btn1");
  var btn2 = document.querySelector("#btn2");
  var btn3 = document.querySelector("button:nth-of-type(3)");
  //버튼의 이벤트 속성에 callback함수를 넣는다(바인딩).

  btn1.onclick = f_call1;
  btn2.onclick = f_call2;
  btn3.onclick = f_call3;
};

function f_call1() {
  document.querySelector("#here").innerHTML = "버튼1";
}

function f_call2() {
  document.querySelector("#here").innerHTML = "버튼2";
}

function f_call3() {
  document.querySelector("#here").innerHTML = "버튼3";
}

</script>
</head>
<body>
<h1>이벤트</h1>
<button id="btn1">Event Handler 넣기1</button>
<button id="btn2">Event Handler 넣기2</button>
<button>Event Handler 넣기3</button>
<hr>
<div id="here">
here
</div>
</body>
```