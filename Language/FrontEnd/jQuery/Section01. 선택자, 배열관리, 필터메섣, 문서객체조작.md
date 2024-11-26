[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

목차

</aside>

---

## 개요

모든 브라우저에서 동작하는 클라이언트 자바스크립 라이브러리

### **목적**

- 문서객체 모델과 관련된 처리 쉽게 구현
- 일관된 이벤트 연결 쉽게구현
- 시각적 효과 쉽게 구현
- Ajax 애플리케이션 쉽게 개발

### CDN 또는 다운로드해서 사용

## $(doument).ready()

- window.onload → jquery로 사용하기

```jsx
window.onload = function () {
  document.querySelector("#here").innerHTML += "<h1>JavaScript이용 1</h1>"
};

window.onload = function () {
  document.querySelector("#here").innerHTML += "<h1>JavaScript이용 2</h1>"
};

$(document).ready(function () {
  document.querySelector("#here").innerHTML += "<h1>JQuery 이용1</h1>"
});  //window.onload -> $(document).ready()

$(document).ready(function () {
  document.querySelector("#here").innerHTML += "<h1>JQuery 이용2</h1>"
});  //window.onload -> $(document).ready()

$(function () {
  document.querySelector("#here").innerHTML += "<h1>JQuery 이용3</h1>"
});  //window.onload -> $(document).ready()

$(() => {
  document.querySelector("#here").innerHTML += "<h1>JQuery 이용4</h1>"
});  //window.onload -> $(document).ready()

$(initial);  //window.onload -> $(document).ready()

jQuery(initial);

function initial() {
  document.querySelector("#here").innerHTML += "<h1>JQuery 이용5</h1>"
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/8a8156ed-a5ea-44d6-8ea8-1b5ebc889ea7/image.png)

**jQeury가 먼저 실행되는 이유**

jQuery의 `$(document).ready()` 함수는 DOM이 완전히 로드된 후 실행되며, `window.onload`보다 먼저 실행됨.

이는 `window.onload`가 페이지의 모든 리소스(이미지, 스타일 등)가 로드된 후 실행되는 반면, `$(document).ready()`는 DOM만 준비되면 바로 실행되기 때문.

## 선택자

### 기본 선택자

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/4af23bc9-69f7-4e13-bf00-c8248d22ab2b/image.png)

**선택자를 모두 호출** (= `document.querySelectorAll()`)

```jsx
$(function () {
  $("button:nth-of-type(1)").click(function () {
    $("button").css("background-color", "blue");  //document.querySelectorAll()
    $("button").css("color", "white");  //document.querySelectorAll()
  });
});
```

**JSON형태로 값 입력하기**

```jsx
$("button:nth-of-type(2)").click(function () {
  //json형태로 만들어서 추가하기
  var obj = {
    "background-color":"green",
    "color":"whitesmoke",
    "border": "3px dotted red",
    "padding": "15px"
  }
  $("button").css(obj);

  $("h1").css("color", "blue");
});
```

### 필터선택자

- 필터링할 selector의 조건을 입력해준 후 이벤트와 이벤트 핸들러 연결
    
    ```jsx
    $("button:nth-of-type(3)").click(function () {
      $("input[type='text']").css("background-color", "yellow");
    
    });
    
    $("button:nth-of-type(1)").click(function () {
      $("button").css("background-color", "blue");  //document.querySelectorAll()
      $("button").css("color", "white");  //document.querySelectorAll()
    });
    
    $("button:nth-of-type(2)").click(function () {
      //json형태로 만들어서 추가하기
      var obj = {
        "background-color":"green",
        "color":"whitesmoke",
        "border": "3px dotted red",
        "padding": "15px"
      }
      $("button").css(obj);
    
      $("h1").css("color", "blue");
    });
    ```
    

### 추천하는 initialize하는 법

```jsx
$(f_initial);
  function f_initial() {
    $("button:nth-of-type(1)").click(f_go);
    $("button:nth-of-type(2)").click(f_go2);
  }

  function f_go2() {
    $("ul li a").each(function (index, item) {
      console.log(index,"번째", item)
      console.log("content:" + $(item).text(), "속성: " + $(item).attr("href"));
      console.log("-----------------")
    });
  }
```

## 배열관리

### 배열관리 메서드

**$.each()**

- 기본형
    
    ```jsx
    $.each(arr, callbackFunction)
    ```
    

- 예제
    
    ```jsx
    function f_go2() {
      $("ul li a").each(function (index, item) {
        console.log(index,"번째", item)
        console.log("content:" + $(item).text(), "속성: " + $(item).attr("href"));
        console.log("-----------------")
      });
    }
    
    function f_go() {
      var arr = [{"name":"구글", link: "https://www.google.com"}];
      arr.push({"name":"네이버", link: "https://www.naver.com"});
      arr.push({"name":"다음", link: "https://www.daum.net"});ㅌ
    
      var output = "";
      $.each(arr, function(index, item) {
        output += `<li><a href="${item.link}">${item.name}</a></li>`;
      });
    
      output = `<ul>${output}</ul>`;
      $("#here").html(output);
    }
    ```
    

**addClass()**

- 예제
    - p태그 짝수 번째 요소에 관하여 css 바꾸기

```jsx
<script>
  function f_go3() {
    $("p:nth-of-type(2n)").addClass("high-light")
  }
</script>

<style>
  p {
    border: 1px solid gray;
    padding: 5px;
  }

  .high-light {
    background-color: yellow;
    font-weight: bold;
    font-size: 20px;
  }
</style>
```

### 객체 확장

**$.extend**

- 예제
    
    김밥집 JSON data에 가격, 전화번호, 주소 추가하여 출력해보기.
    
    ```jsx
    function f_go4() {
      var obj = {menu: "김밥"}
      var obj2 = {price: 3600, phone: "010-1234-5678"}
      $.extend(obj, obj2, {address: "마포구 연남동"});
      var output = ""
      $.each(obj, function (key, value) {
        output += `
        <p>${key}....${value}</p>
        `
      });
      $("#here").html(output);
    }
    ```
    
- 객체가 JSON일 경우 `$.each` 는 `key`, `value`를 사용

**$.noComflict()**

- 다른 라이브러리와 함께 사용할 경우 위 명령어를 통해 $가 jQuery로 인식하지않게 된다.
- 따라서, 모든 jQuery 함수를 `jQeury.each()` 와 같이 변경해야함.

# 필터메서드

## 기본 필터 메서드

### 필터 선택자

- **기본형**

`$(selector).filter(function(index) {});`

- 예제
    
    p태그 중 3의 배수로 나오는 것만 필터링 해서 css 적용.
    
    ```jsx
    function f_go5() {
      //$("p:nth-of-type(even)").css("color", "red");
      // $("p").filter(":nth-of-type(odd)").css("color", "blue");
    
      //모든 p중에서 3의 배수인 것만 css을 적용
      $("p").filter(function (index) {
        console.log(index);
        return index % 3 == 0;
      }).css("border", "5px dotted green");
    }
    ```
    

### 문서 객체 탐색 종료

**end()**

- end() 있으면 filter()의 상위를 의미 $("h2")
- end() 없으면 filter(":even") 중에서 filter(":odd") 홀수를 의미

```jsx
function f_go6() {
  $("h2")
    .css("background-color", "orange")
    .filter(":even").css("color", "blue").end()
    .filter(":odd").css("background-color", "green")
}

<body>
	<h2>0</h2>
	<h2>1</h2>
	<h2>2</h2>
	<h2>3</h2>
	<h2>4</h2>
	<h2>5</h2>
	<h2>6</h2>
</body>
```

### 특정 위치의 문서 객체 선택

**eq(): index는 0부터 시작**

**first()**

**last()**

```jsx
function f_go7() {
  $("p").eq(3).css("color", "red");
  $("p").eq(-1).css("color", "red");
  $("p").first().css("background-color", "orange");
  $("p").last().css("background-color", "orange");
}
```

### 문서 객체 추가 선택

**add()**: 문서 객체의 체이닝을 더 유연하게 하기 위해 사ㅛㅇ

**is()**: 특정 객체의 특징 판별

### 특정 태그 선택

**$.parseXML**

```jsx
function f_go8() {
  var xmlStr = `
  <friends>
    <friend>
      <name>홍길동1</name>
      <phone>010-1234-5678</phone>
    </friend>
    <friend>
      <name>홍길동2</name>
      <phone>010-5555-4444</phone>
    </friend>
    <friend>
      <name>홍길동3</name>
      <phone>010-6666-7777</phone>
    </friend>
  </friends>
  `;

  var xmlDoc = $.parseXML(xmlStr);
	var xmlDoc = $.parseXML(xmlStr);
    $(xmlDoc).find("friend").each(function (index, item) {
    console.log($(item).find("name").text());
    console.log($(item).find("phone").text());
  });
}
```

**콘솔창 확인**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/4f1f7326-782f-49f3-afce-b2a395eb227f/image.png)

### 공공데이터 파싱

**fetch().then().then()**

```jsx
function f_go9() {
  fetch("문화체육관광부 대한민국역사박물관_근현대사 구술영상 목록정보_20241008.xml")
    .then((response) => response.text())  //text로 변환
    .then((xmlStr) => {
      var xmlDoc = $.parseXML(xmlStr);
      $(xmlDoc).find("DATA_RECORD")
        .each((index, item) => {
          let subject = $(item).find("SUBJECT").text();
          let info = $(item).find("DATA_COMMENT").text();
          console.log(subject, info)
        });
    });
}
```

### parent()

특정 태그의 부모를 선택할 수 있는 명령어

```jsx
<script>
function f_go10() {
  var txt = $(this).text();
  if (txt === "활성화하기") {
    $(this).text("비활성화하기");
    $(this).parent().css("background-color", "lightgreen")
    $(this).parent().find("span").text("활성화");
  } else {
    $(this).text("활성화하기");
    $(this).parent().css("background-color", "pink")
    $(this).parent().find("span").text("비활성화");
  }
}
</script>

<div>
  <span>비활성화</span>
  <button>활성화하기</button>
</div>
```

# 문서 객체 조작

## 문서 객체 클래스 속성 추가/제거

`addClass()`

`removeClass()`

`attr(”key”, “value”)`

`attr(”key”, function(){})` 

`attr(”key”, {})` 

```jsx
function f_do4() {
  let obj = {
    "width": function (index) {
      return 150 * (index + 1);
    },
    "height": function (index) {
      return 150 * (index + 1);
    }
  };

  $("#here img").attr(obj);
}

function f_do3() {
  $("#here img:nth-of-type(2)").css("border", "10px solid red");
  var obj = {"width": "250px", "height": "250px"};
  $("#here img:nth-of-type(2)").attr(obj);
}

function f_do2() {
  $("#here img:nth-of-type(1)").css("border", "10px solid red")
  $("#here img:nth-of-type(1)").attr("width", "200px")
  $("#here img:nth-of-type(1)").attr("height", "200px")
}

function f_do1() {
  //getter
  let getSrc1 = $("#here img:nth-of-type(1)").attr("src");
  console.log(getSrc1);
}
```

**.css(”style”, function() {})**

- 함수형태로 css 수정하기

```jsx
function f_do5() {
  $("#here img")
    .css("border", function (index) {
      return (index + 1) * 3 + "px dotted blue";
  });
}
```

**.css("backgroundColor", function (index) {**

- 배열의 속성 집어넣기

```jsx
function f_do7() {
  var arr = ["lightgreen", "pink", "orange", "skyblue"];
  $("h1").css("backgroundColor", function (index) {
    return arr[index]
  });
}
```

**.removeAttr('width')**

- 속성 지우기

```jsx
function f_do6() {
  $("#here img").removeAttr('width');
  $("#here img").removeAttr("height");
  $("#here img").removeAttr("src");
}
```

## 문서 객체의 내부검사

html (=innerHTML)

```jsx
function f_do8() {
  $("h1").html(function(index, contents) {
    return contents + `<span>${index + 1}</span>`});
}
```

text

remove(): 객체 지움

empty(): 내용만 지움

```jsx
function f_do10() {
  $("#here").remove();  //객체 지우기
}

function f_do9() {
  $("#here").empty();  //내용 지우기
}
```

## 문서 객체 생성

`$()`로 객체 생성 가능.

appendTo

prependTo

insertAfter

insertBefore

```jsx
function f_do11() {
  //<img scr="" alt="" width="150px"/>
  var arr = ["/resources/images/iu6.jpg", "/resources/images/iu7.jpg", "/resources/images/iu8.jpg", "/resources/images/iu8.jpg"];
  var img1 = $("<img/>").attr({src:arr[0], alt:"아이유", width:"150px"});
  var img2 = $("<img/>").attr({src:arr[1], alt:"아이유", width:"150px"});
  var img3 = $("<img/>").attr({src:arr[2], alt:"아이유", width:"150px"});
  var img4 = $("<img/>").attr({src:arr[3], alt:"아이유", width:"150px"});
  $(img1).appendTo("#here");
  $(img2).prependTo("#here");
  $(img3).insertBefore("#here");
  $(img4).insertAfter("#here");
}
```