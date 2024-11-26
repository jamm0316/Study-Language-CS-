[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

목차

<aside>
📄

content

</aside>

## 배열의 자료형

- 자바크립트의 기본 자료형

### 객체의 사용

- 객체의 키: 식별자 또는 문자열 모두 가능
- 식별자가 아닌 문자를 키로 설정하는 경우 []로 접근

### 속성

- 요소: 배열 내부
- 속성: 객체 안에 있는 값

```jsx
var obj = {"key": "value", "user name": "hong"};
```

- 함수에서 속성을 접근할 때
    - function의 범위를 고려하여 반드시 this사용.
    - this가 없으면 local varible → global variable → window variable 순으로 찾음.
        
        ```jsx
        var person = {
            name2: "송재명",
            eat: function (food) {
              var hereObj = document.querySelector("#here");
              hereObj.innerHTML = `name(atrribute): ${this.name2} <br> food이름(parameter): ${food}`;
            }
          };
          person.eat("바나나");
        }
        ```
        

## 객체관련 키워드

### in 키워드

- 이름 속성: true
- 성별 속성: false

### with 키워드

- 복잡하게 사용하는 코드를 짧게 줄여줌.

## 객체와 배열을 사용한 데이터 관리

### 배열을 통한 객체 관리

```jsx
var studentArr = [];
  studentArr.push({이름: "송재명1", 자바: 100, SQL: 100, WEB: 100});
  studentArr.push({이름: "송재명2", 자바: 80, SQL: 70, WEB: 55});
  studentArr.push({이름: "송재명3", 자바: 98, SQL: 88, WEB: 79});
  studentArr.push({이름: "송재명4", 자바: 87, SQL: 98, WEB: 69});
  studentArr.push({이름: "송재명5", 자바: 97, SQL: 76, WEB: 98});

  //for(var 'variable' in 'object')
  //for(var 'index' in 'array')
  for (var i in studentArr) {
    studentArr[i].getSum = function () {
      return this.자바 + this.SQL + this.WEB;
    };

    studentArr[i].getAvg = function () {
      return (this.자바 + this.SQL + this.WEB) / (studentArr.length - 1);
    };

    console.log(studentArr[i].이름 + "......" + studentArr[i].getAvg());
  }
```

### 함수를 사용해서 객체 생성

- 객체의 반영
    - 하나씩 만들어 배열에 사용: 서로 다른 형태의 객체를 배열 안에 넣을 수 있는 장점
    - `toString` 함수를 만들어서 화면에 출력 가능
        
        ```jsx
        for (var i in studentArr) {
            studentArr[i].getSum = function () {
              return this.자바 + this.SQL + this.WEB;
            };
        
            studentArr[i].getAvg = function () {
              return (this.자바 + this.SQL + this.WEB) / (studentArr.length - 1);
            };
        
            console.log(studentArr[i].이름 + "......" + studentArr[i].getAvg());
            console.log(studentArr[i]);  //toString이 자동으로 호출됨. [Object object]
        
            studentArr[i].toString = function () {
              return `이름은 ${this.이름}, 총점은 ${this.getSum()}, 평균은 ${this.getAvg()}`
            };
            var hereObj = document.querySelector("#here");
            hereObj.innerHTML += `<p>${studentArr[i]}</p>`
          }
        ```
        

## 생성자 함수

- new 키워드를 사용해서 생성하는 함수
- 첫 키워드를 대문자로 설정

### this키워드

- 생성자 함수로 생성될 객체의 속성 지정

### 메서드 생성

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/b0cfe18e-b38c-4ad2-8589-8e7d0f363f09/image.png)

예제

```jsx
function Student(name, java, sql, web) {
  this.이름 = name;
  this.자바 = java;
  this.SQL = sql;
  this.WEB = web;
  this.getSum = function () {
    return this.자바 + this.SQL + this.WEB;
  };

  this.getAvg = function () {
    return (this.자바 + this.SQL + this.WEB) / 3;
  };

  this.toString = function () {
    return `이름은 ${this.이름}, 총점은 ${this.getSum()}, 평균은 ${this.getAvg()}`
  };
}

function f_call4() {
  var studentArr = [];
  studentArr.push(new Student("송재명1", 100, 100, 100));
  studentArr.push(new Student("송재명2", 80, 70, 55));

  for (var i in studentArr) {
    var hereObj = document.querySelector("#here");
    hereObj.innerHTML += `<p>${studentArr[i]}</p>`
  }
}
```

### 프로토타입

동일한 함수 생성에 따른 비효율적 메모리 이용 해결하기 위해 생성자 함수로 생성된 **객체가 공통으로 가진 공간을 만드는 것.**

```jsx
function Student(name, java, sql, web) {
  this.이름 = name;
  this.자바 = java;
  this.SQL = sql;
  this.WEB = web;
}

//Prototype: 생성자 함수를 이용해서 만든 객체들의 공유공간.
Student.prototype.getSum = function () {
  return this.자바 + this.SQL + this.WEB;
};

Student.prototype.getAvg = function () {
  return (this.자바 + this.SQL + this.WEB) / 3;
};

Student.prototype.toString = function () {
  return `이름은 ${this.이름}, 총점은 ${this.getSum()}, 평균은 ${this.getAvg()}`
};
```

## array 관련 메서드

### sort()

```jsx
var arr = [100, 40, 50, 20, 88];
  var beforeStr = arr.join("**")

  //함수를 만들어서 sort함수에 parameter로 넘겨준다. sort함수가 부른다.
  // < = >
  // var ascStr = arr.sort(function (a, b) {return a-b;}).join("**");
  //화살표 함수(람다식)
  var ascStr = arr.sort((a, b) => a - b).join("**");
  var dscStr = arr.sort(function (a, b) {return b-a;}).join("**");

  var studentArr = f_call4();
  //학생 평균으로 ascending
  studentArr.sort((a, b) => a.getAvg() - b.getAvg());

  //학생 이름 순으로 descending
  studentArr.sort((a, b) => b.이름.localeCompare(a.이름));

  var hereObj = document.querySelector("#here")
  hereObj.innerHTML = `
  before: ${beforeStr} <br>
  after(asc): ${ascStr} <br>
  after(dsc): ${dscStr}<br>
 학생들의 정보:<br> ${studentArr.join("<br>")}
  `;
```

## ECMAScript 5 Array 객체

### 반복메서드

**forEach()**

```jsx
var output = "";
  arr.forEach(function (element, index, arrObj) {
    output += `<p>${index}번째 값은...${element}...전체 배열${arrObj}</p>`;
    output += `<p>${arguments[1]}번째 값은...${arguments[0]}...전체 배열${arguments[2]}</p><hr>`;
  });
  hereObj.innerHTML = output
```

**map()**

```jsx
var output = "";
var arr2 = arr.map(function (item, index, arrObj) {
  // return item / 10;
  return "변경값" + item / 10;
});
hereObj.innerHTML = `원래 array ${arr}, 변형 array ${arr2}`;
```

**filter()**

```jsx
var arr2 = arr.filter(f_filter);
  hereObj.innerHTML = `filter 결과는 ${arr2}`;
function f_filter(item) {
  return item % 20 == 0;
}
```

**every()**

```jsx
var result = arr.every(function (item, index) {
  return item % 10 == 0;
});
hereObj.innerHTML = `${result}`;
```

**some()**

```jsx
var result = arr.some(function (item, index) {
  return item == 5;
});
hereObj.innerHTML = `${result}`;
```

### JSON(JavaScript Object Notation)

**JSON → String**

- `*JSON*.stringify(obj)`

String → JSON

- `*JSON*.parse(str);`

오픈api를 이용해 JSON 데이터 가공하기

- 가상화폐 종목 출력

```jsx
const options = {method: 'GET', headers: {accept: 'application/json'}};

fetch('https://api.bithumb.com/v1/market/all?isDetails=false', options)
  .then(response => response.json())
  .then(response => {
    var rowObj = "";
    for (let i in response) {
      //{market: 'KRW-BTC', korean_name: '비트코인', english_name: 'Bitcoin'}
      rowObj += `
        <tr>
          <td>${response[i].market}</td>
          <td>${response[i]["korean_name"]}</td>
          <td>${response[i]["english_name"]}</td>
	      </tr>`;
    }

var output = `
	<table border: 1px solid gray>
	<tr>
	  <th>마켓</th>
	  <th>한국이름</th>
	  <th>영어</th>
	</tr>
	<tr>
	  <td>
		 ${rowObj}
	  </td>
	</tr>
	</table>
	`;

hereObj.innerHTML = output;
  })
  //응답된 object를 접근해서 테이블로 출력하기
  .catch(err => console.error(err));
```

- 종목별 실시간 종가, 호가, 최대, 최저가 확인

```jsx
const options = {method: 'GET', headers: {accept: 'application/json'}};

fetch('https://api.bithumb.com/public/ticker/all', options)
  .then(response => response.json())
  .then(response => {
    console.log(response.data);
    var rowObj = "";
    var tableDate = "";
    for (let prop in response.data) {
      //
      if (prop === "date") {
        tableDate = new Date(parseInt(response.data[prop])).toISOString() ;
      }
      rowObj += `
        <tr>
          <td>${prop}</td>
          <td>${response.data[prop]["opening_price"]}</td>
          <td>${response.data[prop]["acc_trade_value_24H"]}</td>
          <td>${response.data[prop]["min_price"]}</td>
          <td>${response.data[prop]["max_price"]}</td>
      </tr>`;
    }

    var output = `
      <table border: 1px solid gray>
      <caption>${tableDate}</caption>
      <tr>
        <th>종목명</th>
        <th>시작가</th>
        <th>종가</th>
        <th>최소</th>
        <th>최대</th>
      </tr>
      <tr>
        <td>
       ${rowObj}
        </td>
      </tr>
    </table>
    `;

    hereObj.innerHTML = output;
  })
  .catch(err => console.error(err));
```

### toJSON()

메서드 유무에 따라 JSON.stringify결과가 다르다.