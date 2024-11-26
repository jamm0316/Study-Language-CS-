[<back](https://www.notion.so/TIL-87b0cd0d402d49b5aa87ea9615304bb1?pvs=21)

---

<aside>
📄

contents

</aside>

---

# XMLHttpRequest 객체

**사용목적**

- 비동기 통신을 하기 위해서
    - 현재화면은 그대로 있고 호출하면 서버에 갔다가 다시 돌아오는 것.
    - 예시) 아이디 중복 체크 시, 서버에 요청을 보내고 오는 과정이 비동기로 일어남.
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/291841c0-f172-42e1-8c62-853075b338ca/image.png)
    

## XMLHttpRequest

XMLHttpRequest는 자바스크립트가 Ajax를 사용할 때 사용하는 객체

### 동기방식

- request를 보내 놓고 다음 코드를 계속해서 실행한다.
- 예제
    
    현재 페이지에서 버튼을 누르면 1.html을 불러오도록 한다.(동기)
    
    ```jsx
    function f_ajax1() {
      var request = new XMLHttpRequest();
      request.open("get", "/javaScriptPractice/day6/1.html", false);  //동기
      //Ajax 수행
      request.send();  //서버로 간다.
    
      //응답 받기
      $("#here").html(request.responseText);
    }
    ```
    

### 비동기방식

**readyState 속성**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/73c1d407-c1e2-4937-996b-b7d5ea6d8337/image.png)

**http status code**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/8f0144e9-752c-42ce-a9c0-01c8199f5af6/image.png)

- 응답받는 조건을 설정해주어야함.

```jsx
function f_ajax2() {
  var request = new XMLHttpRequest();
  //응답 받기
  request.onreadystatechange = function (event) {
    if (request.readyState == 4 && request.status == 200) {
      $("#here").html("돈이 들어왔다고? 나는 비동기라 기다리지 않아." + request.responseText);
    }
  };
  request.open("get", "/javaScriptPractice/day6/1.html", true);  //비동기

  //Ajax 수행
  request.send();  //서버로 간다.... 비동기이므로 응답을 기다리지 않음.
}
```

# jQuery Ajax

## 기본

### 메서드

**$.ajax()**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/d1198790-2f2f-411d-9150-f621c810fb11/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/0f3257b0-5ad0-4062-a467-adc502f18e05/image.png)

**load**

```jsx
function f_ajax5() {
  let url = "/unnamed2/emp/empAll.jsp"
  $("#here").load(url);
}
```

**get**

```jsx
function f_ajax6() {
  let url = "/unnamed2/emp/empAll.jsp"
  let data = {};
  $.get(url, data, callbackMethod);
}

function callbackMethod(data, status, xhr) {
  $("#here").html(data);
  console.log(status);
  console.log(xhr);
}
```

**post**

```jsx
function f_ajax8() {
  let url = "/unnamed2/emp/empAll.jsp";
  let data = {};
  $.ajax({
    "url": url,
    type: "get",
    data: {},
    success: function (response, status, xhr) {
      console.log(response, status, xhr);
      $("here").html(response);
    }
  });

  $.ajax({
    "url": "DeepQA_data.json",
    type: "post",
    data: {},
    success: function (response, status, xhr) {
      console.log(response, status, xhr);
      $("here").before(response.data[2].title);
    }
  });
}
```

---

향상된 Ajax data 사용

```jsx

```