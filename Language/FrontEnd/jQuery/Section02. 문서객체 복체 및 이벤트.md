[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

contents

</aside>

# 문서 객체 복제

## 문서객체 이동

**appendTo()**

- 예제
    
    맨앞에있는 이미지를 맨 뒤로 동적으로 옮기는 버튼 만들기
    
    ```jsx
    function f_call2() {
      //$("img").eq(1) == first()
      $("img").first().appendTo("#here")
    }
    
    <div id="here">
      <img src="/resources/images/iu5.jpg" alt="아이유"/>
      <img src="/resources/images/iu6.jpg" alt="아이유"/>
      <img src="/resources/images/iu7.jpg" alt="아이유"/>
    </div>
    ```
    

## 문서 객체 이동

문서 객체를 복제해서 추가하고 싶을 때

**clone()**: 문서객체 복제

- 예제
    
    ```jsx
    var index = 0;
    function f_call3() {
      //계속 증가하면서 복제하기
    
      let obj = $("img").eq(index).clone();
      obj.appendTo("#here");
      index++
      // var obj = $("img").first().clone();
      // obj.appendTo("#here");
    }
    ```
    

# 이벤트

jQuery를 사용하면 자바스크립트 이벤트를 간편하게 연결 가능.

## 이벤트연결 기본

기본적인 방법은 `on()` 메서드 사용

`.click`과 같은 함수보다 좋은 점: 함수를 object형태(`{key: function}`)로 한번에 줄 수 있다.

### **이벤트 연결**

**기본형**

- `.on(이벤트, 이벤트 핸들러)`
- `$(selector).on(click, function(){})`

**이벤트 연결 범위 한정짓기**

- 문제점
    - html문서를 읽으면서 `alert`기능이 추가되어야하는데 아직 `img`객체가 없으므로 `img`객체가 추가 되었을 때, `alert`기능을 수행할 수 없다.
        
        ```jsx
        $("img").on("click", function () {
          alert($(this).attr("src"));
        });
        
        function f_call4() {
        	let obj = `
        	<img src="/resources/images/iu5.jpg" alt="아이유"/>
        	<img src="/resources/images/iu6.jpg" alt="아이유"/>
        	<img src="/resources/images/iu7.jpg" alt="아이유"/>
        	`
        	$(obj).appendTo("#here");
        }
        ```
        

- 개선사항
    - `$(”부모객체”).on(”이벤트”, “이벤트 적용범위”, “이벤트 핸들러”)` 를 이용하여, 부모에게 이벤트를 주고, 부모 아래에 이벤트 적용 범위가 들어왔을 때 `function`을 실행하도록 수정.
        
        ```jsx
        $("#here").on("click", "img", function () {
          alert($(this).attr("src"));
        });
        
        function f_call4() {
        	let obj = `
        	<img src="/resources/images/iu5.jpg" alt="아이유"/>
        	<img src="/resources/images/iu6.jpg" alt="아이유"/>
        	<img src="/resources/images/iu7.jpg" alt="아이유"/>
        	`
        	$(obj).appendTo("#here");
        }
        ```
        

### **이벤트 연결 제거**

기본형: `.off()` 

### **다중 이벤트 연결**

- on()메서드로 연결
    
    ```jsx
    var eventobj = {
      "mouseenter": function() {
        $(this).addClass("aa")
      },
      
      "mouseleave": function() {
        $(this).removeClass("aa")
      }
    };
    
    $("h1").on(eventobj);
    ```
    

- hover로 동일하게 작동하도록 연결
    
    ```jsx
    $("h1").hover(function () {
      $(this).addClass("aa")
    },
    function () {
      $(this).removeClass("aa")
    });
    ```
    

## 키보드 출력 이벤트

- keyup, keydown, keypress

```jsx
$("input:nth-of-type(1)").on("keyup", function () {
  let str = $(this).val()
  $("#here").text(str + "------" + str.length);  //value속성 얻기
});

<input maxlength="10">
```

## 입력양식 이벤트

**** preventDefault ****

- 로그인 하기 기능의 경우 아이디와 비밀번호가 틀리면 못들어가게 해야함.
- 그 때 preventDefault를 사용하여 이동조건을 막아버림.

```jsx
<script>
$(function () {
  $("form").on("submit", f_SubmissionConstraints)
});

function f_SubmissionConstraints(event) {
 let id = $("#userid").val();
 let pw = $("#userpwd").val();

  if (id === "hr" && pw === "1234") {
    return;
  }
  alert("id 또는 비밀번호가 틀렸습니다.")
  event.preventDefault();
</script>

<form action="2.html" method="get">
  <input id="userid" name="user_id" value="hr">
  <input id="userpwd" name="user_pwd" value="1234">
  <input type="submit" value="로그인하기">
</form>
```