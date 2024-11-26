[<back](https://www.notion.so/basic-_-3ee7f95ac24d43f881b7ee2e8f21c210?pvs=21)

---

<aside>
📄 목차

</aside>

---

## 클래스가 필요한 이유

자바는 클래스와 객체로 이루어져 있다. 그만큼 클래스와 객체라는 개념은 중요하다.

### 문제: 학생 정보 출력 프로그램 만들기

당신은 두 명의 학생 정보를 출력하는 프로그램을 작성해야 한다. 각 학생은 이름, 나이, 성적을 가지고 있다.

**요구 사항:**

1. 첫 번째 학생의 이름은 “학생1”, 나이 15, 성적은 90입니다.
2. 두 번째 학생은 이름은 “학생2”, 나이 16, 성적은 80입니다.
3. 각 학생의 정보를 다음과 같은 형식으로 출력해야 합니다: `“이름: [이름] 나이: [나이] 성적: [성적]”`
4. 변수를 사용해서 학생 정보를 저장하고 변수를 사용해서 학생 정보를 출력해야합니다.

**예시 출력:**

```java
이름: 학생1 나이: 15 성적: 90
이름: 학생2 나이: 16 성적: 80
```

변수를 사용해서 이 문제를 풀어보면 다음과 같다

**ClassStart1 - 변수사용**

```java
package class1;

public class ClassStart1 {
    public static void main(String[] args) {
        String student1Name = "학생1";
        int student1Age = 15;
        int student1Grade = 90;
        
        String student2Name = "학생2";
        int student2Age = 16;
        int student2Grade = 80;

        System.out.println("이름: " + student1Name +" 나이: " + student1Age + " 성적: " + student1Grade);
        System.out.println("이름: " + student2Name +" 나이: " + student2Age + " 성적: " + student2Grade);
        }
    }
```

**실행결과**

```java
이름: 학생1 나이: 15 성적: 90
이름: 학생2 나이: 16 성적: 80
```

학생이 여러명이 될 경우 변수를 새로 선언, 추가해야하고 출력하는 코드도 추가해야해서 번잡하다.

배열로 문제를 해결해보자.

### 문제: 이전문제에 대해 배열을 사용하자

배열을 사용하여 코드 리펙토링

**ClassStart2**

```java
package class1;

public class ClassStart2 {
    public static void main(String[] args) {
        String[] studentName = {"학생1", "학생2", "학생3", "학생4"};
        int[] studentAge = {15, 16, 18, 20};
        int[] studentGrade = {90, 80, 90, 100};

        for (int i = 0; i < studentName.length; i++) {
            System.out.println("이름: " + studentName[i] + " 나이: " + studentAge[i] + " 성적: " + studentGrade[i]);
        }
    }
}
```

**실행결과**

```java
이름: 학생1 나이: 15 성적: 90
이름: 학생2 나이: 16 성적: 80
이름: 학생3 나이: 18 성적: 90
이름: 학생4 나이: 20 성적: 100
```

### 배열 사용의 한계

한 학생의 데이터가 `studentName`, `studentAge`, `studentGrade` 3개의 배열에 나누어져 있다. 따라서 데이터를 변경할 때 매우 조심해서 작업해야한다.

예를 들어 학생2의 데이터를 제거하려면 각 배열마다 학생2의 요소를 정확하게 찾아서 제거해주어야한다.

학생2 제거

```java
String[] studentName = {"학생1", "학생3", "학생4"};
int[] studentAge = {15, 18, 20};
int[] studentGrade = {90, 90, 100};
```

한 학생의 데이터가 3개의 배열에 나누어져 있기 때문에 3개 배열을 각각 변경해야한다. 이렇게 하면 특정 학생의 데이터를 변경할 때 실수할 가능성이 매우 높다. 컴퓨터가 볼 때는 아무 문제 없지만 사람이 관리하기는 복잡한 데이터다.

**정리**

지금처럼 이름, 나이, 성적을 각각 따로 나누어서 관리하는 것은 사람이 관리하기 좋은 방식이 아니다.

사람이 관리하기 좋은 방식은 학생이라는 개념을 하나로 묶는 것이다. 그리고 각각의 학생별로 본인의 이름, 나이, 성적을 관리하는 것이다.

## 클래스 도입

앞서 이야기한 문제를 클래스를 도입해서 해결해보자.

클래스를 사용해서 학생이라는 개념을 만들고, 각각의 학생 별로 본인의 이름, 나이, 성적을 관리하는 것이다.

**Student 클래스**

```java
package class1;

public class Student {
    String name;
    int age;
    int grade;
}
```

`class` 키워드를 사용해서 학생 클래스(`Student`)를 정의. 학생 클래스는 내부에 이름(`name`), 나이(`age`), 성적(`grade`) 변수를 가진다.

이렇게 **클래스에 정의한 변수들을 멤버 변수, 또는 필드**라 한다.

- **멤버 변수(Member Variable)**: 이 변수들은 특정 클래스에 소속된 멤버이기 때문에 이렇게 부른다.
- **필드(Field):** 데이터 항목을 가르키는 전통적인 용어. 데이터베이스, 엑셀 등에서 데이터 각각의 항목을 필드라 한다.
- 자바에서는 멤버 변수, 필드는 같은 뜻이다. 클래스에 소속된 변수를 뜻한다.

**클래스는 관례상 대문자로 시작하고 낙타 표기법을 사용한다.**

예): `Student`, `User`, `MemberService`

이제 학생 클래스를 사용하는 코드를 작성해보자.

**ClassStart3**

```java
package class1;

public class ClassStudent3 {

    public static void main(String[] args) {
        Student student1;
        student1 = new Student();
        student1.name = "학생1";
        student1.age = 15;
        student1.grade = 90;

        Student student2 = new Student();
        student2.name = "학생2";
        student2.age = 16;
        student2.grade = 80;

        System.out.println("이름: " + student1.name + " 나이: " + student1.age + " 성적: " + student1.grade);
        System.out.println("이름: " + student2.name + " 나이: " + student2.age + " 성적: " + student2.grade);
    }
}
```

**실행결과**

```java
이름: 학생1 나이: 15 성적: 90
이름: 학생2 나이: 16 성적: 80
```

**클래스와 사용자 정의 타입**

- 타입은 데이터의 종류나 형태를 나타낸다.
- `int`라고 하면 정수 타입, `String`이라고 하면 문자 타입.
- 학생(`Student`)라는 타입을 만들면 되지 않을까?
- 클래스를 사용하면 `int`, `String`과 같은 타입을 직접 만들 수 있다.
- 사용자가 직접 정의하는 사용자 정의 타입을 만들려면 설계도가 필요하다. 이 **설계도가 바로 클래스**
- 설계도인 **클래스를 사용해서 실제 메모리안에 만들어진 실체를 객체 또는 인스턴스라** 한다.
- 클래스를 통해 사용자가 원하는 종류의 데이터 타입을 마음껏 정의할 수 있다.

**용어: 클래스, 객체, 인스턴스**

클래스는 설계도이고, 이 설계도를 기반으로 실제 메모리에 만들어진 실체를 객체 또는 인스턴스라고 한다. 둘다 같은 의미로 사용됨. 여기서 학생(`Student`) 클래스를 기반으로 학생1(`Student1`), 학생2(`Student2`) 객체 또는 인스턴스를 만들었다.

<aside>
📄 **아래 내용 요약**

`Student student1` ⇒ Student 타입의 student1이라는 변수명을 변수를 선언

`new Student` ⇒ Student 객체 새로 생성

따라서, `Student student1 = new Student`

⇒ `Student` 타입의 **객체, 인스턴스**를 새로 생성해서 바인딩해라. 어디에? `student1` 변수에!(이때 student1는 인스턴스라 칭하고, 타입은 Student)

⇒ `Student` 타입은 해당 클래스에 가서 보니까 `String name`, `int age`, `int grade`라는 **멤버 변수, 필드**를 가지고 있네?

⇒ 자바는 이 메모리를 확보해 놓고 메모리가 있는 위치의 **참조값**을 반환한다.

⇒ 그리고 그 참조값을 `Student student1`에 할당

따라서, `Student student1 = x001` 이라는 참조값을 통해 해당 **멤버 변수**에 접근할 수 있게 됨

</aside>

1. **변수 선언**

**Student student1  //Student 변수 선언**

- `Student student1`
    - `Studnet` 타입을 받을 수 있는 변수 선언
        - `int`는 정수, `String`은 문자를 담을 수 있듯이 `Student`는 `Student`타입의 객체(인스턴스)를 받을 수 있다.

1. **객체 생성**

**student1 = new Student()  //Student 인스턴스 생성**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/27bb27e4-6f89-4785-a110-d66f1054cdaa/Untitled.png)

student1 = new Student() 코드를 나누어 분석해보자.

- 객체를 사용하려면 먼저 설계도인 클래스를 기반으로 객체(인스턴스)를 생성.
- `new Student()`: `new`는 새로 생성한다는 뜻. `new Student()`는 `Student` 클래스 정보를 기반으로 새로운 객체를 생성하라는 뜻. 이렇게하면 메모리에 실제 `Student` 객체(인스턴스)를 생성.
- 객체를 생성할 때는 `new 클래스명()`을 사용하면 된다. 마지막에 `()`도 추가해야한다.
- `student` 클래스는 `String name`, `int age`, `int grade` 멤버 변수를 가지고 있다. 이 변수를 사용하는데 필요한 메모리 공간도 함께 확보한다.

1. **참조값 보관**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/7df8ffdf-07ef-457a-a5df-e15722bcbf7a/Untitled.png)

- 객체를 생성하고 나면 자바는 메모리 어딘가에 있는 객체에 접근할 수 있는 참조값(주소)(`x001`)을 반환
    - 여기서 `x001` 이라고 표현한 것은 참조값
- `new` 키워드를 통해 객체가 생성되고 나면 참조값을 반환한다. 앞서 선언한 변수인 `Student student1` 에 생성된 객체의 참조값(`x001`)을 보관한다.
- `Student student1` 변수는 이제 메모리에 존재하는 실제 `Student` 객체(인스턴스)의 참조값을 가지고있다.
    - `student1` 변수는 방금 만든 객체에 접근할 수 있는 참조값을 가지고 있다. 따라서 이 변수를 통해서 객체를 접근(참조)할 수 있다. 쉽게 얘기해서 `student1` 변수를 통해 메모리에 있는 실제 객체를 접근하고 사용할 수 있다.

**참조값을 변수에 보관해야하는 이유**

객체를 생성하는 `new Student()` 코드 자체에는 아무런 이름이 없다. 이 코드는 단순히 `Student` 클래스를 기반으로 메모리에 실제 객체를 만드는 것. 따라서 생성한 객체에 접근할 수 있는 방법이 필요함. 이런 이유로 객체를 생성할 때 반환되는 참조값을 어딘가에 보관해두어야한다. 앞서 `Student student1` 변수에 참조값(`x001`)을 저장해 두었으므로 저장한 참조값(`x001`)을 통해 실제 메모리에 존재하는 객체에 접근 가능.

지금까지 설명한 내용을 간단히 풀어보면 다음과 같다

```java
Student student1 = new Student();  //1. Student 객체 생성
Student student1 = x001;  //2. new Student()의 결과로 x001 참조값 반환
student1 = x001;  //3. 최종 결과
```

이후 학생2(`student2`)까지 생성하면 다음과 같이 `Student` 객체(인스턴스)가 메모리에 2개 생성된다. 각각 참조값이 다르므로 서로 구분할 수 있다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/90c5a3ae-cfa2-4e59-a6bf-98dcc0159fb3/Untitled.png)

참조값을 확인하고 싶다면 다음과 같이 객체를 담고 있는 변수를 출력해보면 된다.

```java
System.out.println(student1);
System.out.println(student2);
```

**출력결과**

```java
class1.Student@6acbcfc0
class1.Student@5f184fc6
(패키지.클래스정보)@(참조값)
```

@앞은 패키지 + 클래스 정보를 뜻한다. @뒤에 16진수는 참조값을 뜻한다.

## 객체 사용

클래스를 통해 생성한 객체를 사용하려면 먼저 메모리에 존재하는 객체에 접근해야한다. 객체에 접근하려면 `.`(점, dot)을 사용.

```java
//객체 값 대입
student1.name = :"학생1";
student1.age = 15;
student1.grade = 90;

//객체 값 사용
System.out.println("이름" + student1.name + " 나이: " + student1.age + " 성적: " + student1.grade);
```

### 객체에 값 대입

객체가 가지고 있는 멤버 변수(`name`, `age`, `grade`)에 값을 대입하려면 먼저 객체에 접근해야한다.

객체에 접근하려면 `.` 키워드를 사용하여 변수(`student1`)에 들어있는 참조값 (`x001`)을 일거서 메모리에 존재하는 객체에 접근.

순서를 간단히 풀어보면 다음과 같다.

```java
student1.name = "학생1"  //1. student1 객체의 name 멤버 변수에 값 대입
x001.name = "학생1"  //2. 변수에 있는 참조값을 통해 실제 객체에 접근, 해당 객체의 name 멤버 변수에 값 대입
```

`student1.` (dot)이라고 하면 `student1` 변수가 가지고 있는 참조값을 통해 실제 객체에 접근.

`student1`은 `x001`이라는 참조값을 가지고 있으므로 `x001` 위치에 있는 `Student` 객체에 접근.

**그림으로 자세히 알아보자**

`student1.name = ”학생1”` **코드 실행 전**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/25541461-b451-49a6-bcec-8394a2b0cd1a/Untitled.png)

`student1.name = “학생1”` **코드 실행 후**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/ae7d1495-401a-45a3-9afd-dcd5c72eb6da/Untitled.png)

- `student1.name` 코드를 통해 `.` 키워드가 사용되었다. `student1` 변수가 가지고 있는 참조값을 통해 실제 객체에 접근.
- `x001.name = “학생1”`:`x001` 객체가 있는 곳에 `name` 멤버 변수에 “학생1” 데이터가 저장됨.

### 객체 값 읽기

객체의 값을 읽는 것도 앞서 설명한 내용과 같다. `.` 키워드를 통해 참조값을 사용해서 객체에 접근한 다음에 원하는 작업을 하면 된다.

```java
//1. 객체 값 읽기
System.out.println("이름:" + student1.name);
//2. 변수에 있는 참조값을 통해 실제 객체에 접근하고, name 멤버 변수에 접근.
System.out.println("이름:" + x001.name);
//3. 객체의 멤버 변수에 값을 읽어옴
System.out.println("이름:" + "학생1");
```

**객체 값 읽기 - 그림1**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/5a5f4c6b-ad92-41b8-a6d0-42e3daa7f131/Untitled.png)

**객체 값 읽기 - 그림2**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/258b0303-e90a-4c35-8dc7-42fa8ebfd81e/Untitled.png)

- `x001` 에 있는 `Student` 인스턴스의 `name` 멤버 변수는 “학생1”이라는 값을 가지고 있다.

## 배열 도입 - 시작

클래스와 객체 덕분에 학생 데이터를 구조적으로 이해하기 쉽게 변경할 수 있다.

마치 실제 학생이 있고, 그 안에 각 학생의 정보가 있는 것 같아 사람도 이해하기 편리하다.

배열을 사용해 특정 타입을 연속한 데이터 구조로 묶어서 편리하게 관리할 수 있다.

`Student` 클래스를 사용한 변수들도 `Student` 타입이기 때문에 학생도 배열을 사용해서 하나의 데이터 구조로 묶어서 관리할 수 있다.

**ClassStart4**

```java
package class1;

public class ClassStudent4 {

    public static void main(String[] args) {
        Student student1;  //String student; 와 같은 표
        student1 = new Student();
        student1.name = "학생1";
        student1.age = 15;
        student1.grade = 90;

        Student student2 = new Student();
        student2.name = "학생2";
        student2.age = 16;
        student2.grade = 80;

        Student[] students = new Student[2];
        students[0] = student1;
        students[1] = student2;

        System.out.println("이름: " + students[0].name + " 나이: " + students[0].age + " 성적: " + students[0].grade);
        System.out.println("이름: " + students[1].name + " 나이: " + students[1].age + " 성적: " + students[1].grade);
    }
}
```

**실행결과**

```java
이름: 학생1 나이: 15 성적: 90
이름: 학생2 나이: 16 성적: 80
```

코드를 분석해보자

```java
Student student1;
student1 = new Student();
student1.name = "학생1";
student1.age = 15;
student1.grade = 90;

Student student2 = new Student();
student2.name = "학생2";
student2.age = 16;
student2.grade = 80;
```

`Student` 클래스를 기반으로 `student1`, `student2` 인스턴스 생성. 그리고 필요한 값 채움.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/1ade3e83-24d6-4e1a-8779-e5ca4d7fdcd3/5a62e2ca-745a-4185-a368-507ac86b55be.png)

### 배열에 참조값 대입

이번에는 `Student`를 담을 수 있는 배열을 생성하고, 해당 배열에 `student1`, `student2` 인스턴스를 보관.

```java
Studnent[] students = new Student[2];
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/0aa4fc6b-21d6-4227-a016-24b402888853/d6e68bb2-f4d2-4767-b738-1b9ba2726c47.png)

- `Student` 변수를 2개 보관할 수 있는 사이즈 2의 배열을 만든다.
- `Student` 타입의 변수는 `Student` 인스턴스의 참조값을 보관한다. `Student` 배열의 각각의 항목도 `Student`타입의 변수일 뿐. 따라서 `Student` 타입의 참조값을 보관
    - `student1`, `student2` 변수를 생각해보면 `Student` 타입의 참조값을 보관한다.
- 배열에는 아직 참조값을 대입하지 않았기 때문에 참조값이 없다는 의미의 `null`값으로 초기화

이제 배열에 객체를 보관.

```java
students[0] = student1;
students[1] = student2;

//자바에서 대입은 항상 변수에 들어 있는 값을 복사한다.
students[0] = x001;
students[1] = x002;
```

**자바에서 대입은 항상 변수에 들어 있는 값을 복사.**

`student1`, `student2` 에는 참조값이 보관되어 있다. 따라서 이 참조값이 배열에 저장된다. 또는 `student1`, `student2`에 보관된 참조값을 읽고 복사해서 배열에 대입.

**배열에 참조값을 대입한 이후 배열 그림**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/b892acf5-cddd-4786-9389-1549fcfefc5f/24a40e5b-e337-405d-bc9c-ccad6f0a2320.png)

이제 배열은 `x001`, `x002`의 참조값을 가진다. 참조값을 가지고 있기 때문에 `x001`(학생1), `x002`(학생2), `Student` 인스턴스에 모두 접근가능

**배열에 참조값을 대입한 이후 최종 그림**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/dcd16e96-da68-4871-8894-e9f9fa80b56d/0183eeb3-7558-4561-accc-15bc24ed5c3b.png)

**자바에서 대입은 항상 변수에 들어 있는 값을 복사해서 전달.**

```java
students[0] = student1;
students[1] = student2;

//자바에서 대입은 항상 변수에 들어 있는 값을 복사한다.
students[0] = x001;
students[1] = x002;
```

자바에서 변수의 대입(`=`)은 모두 변수에 들어잇는 값을 복사해서 전달하는 것.

이 경우 오른쪽 변수인 `student1`, `student2`에 참조값이 들어있다. 그래서 이 값을 복사해서 왼쪽에 있는 배열에 전달.

따라서 기존 `student1`, `student2`에 들어있던 참조값은 그대로 유지됨

**주의!**

변수에는 인스턴스 자체가 들어있는 것이 아니다. 인스턴스의 위치를 가리키는 참조값이 들어있을 뿐!

따라서 대입(`=`)시에 인스턴스가 복사되는 것이 아니라 참조값만 복사됨

### 배열에 들어있는 객체 사용

배열에 들어있는 객체를 사용하려면 먼저 배열에 접근하고, 그 다음에 객체에 접근하면 된다.

**학생1 예제**

```java
System.out.println(students[0].name);  //배열에 접근 시작
System.out.println(x005[0].name);  //[0]을 사용해서 x005 배열의 0번 요소에 접근
System.out.println(x001.name);  //.(dot)을 사용해서 참조값으로 객체에 접근
System.out.println("학생1");
```

**학생2 예제**

```java
System.out.println(student[1].name);  //배열에 접근 시작
System.out.println(x005[1].name);  //[0]을 사용해서 x005 배열의 0번 요소에 접근
System.out.println(x002.name);  //.(dot)을 사용해서 참조값으로 객체에 접근
System.out.println("학생2");
```

## 배열 도입 - 리펙토링

배열 덕분에 출력에서 다음과 같이 for문을 도입할 수 있다.

**ClassStart5**

```java
package class1;

public class ClassStudent5 {

    public static void main(String[] args) {
        Student student1;  //String student; 와 같은 표현
        student1 = new Student();
        student1.name = "학생1";
        student1.age = 15;
        student1.grade = 90;

        Student student2 = new Student();
        student2.name = "학생2";
        student2.age = 16;
        student2.grade = 80;

        Student[] students = {student1, student2};

        for (int i = 0; i < students.length; i++) {
            System.out.println("이름: " + students[i].name + " 나이: " + students[i].age + " 성적: " + students[i].grade);
        }
    }
}
```

**실행결과**

```java
이름: 학생1 나이: 15 성적: 90
이름: 학생2 나이: 16 성적: 80
```

### 배열 선언 최적화

우리가 직접 정의한 `Student` 타입도 일반적인 변수와 동일하게 배열을 생성할 때 포함될 수 있다.

```java
Studnet[] students = new Student[]{studens1, students2};
```

생성과 선언을 동시에 하는 경우 다음과 같이 더 최적화 할 수 있다.

```java
Studnet[] students = {studnets1, students2}
```

### for문 최적화

**for문 도입**

```java
for (int i = 0; i < students.length; i++) {
    System.out.println("이름: " + students[i].name + " 나이: " + students[i].age + " 성적: " + students[i].grade);
```

**for문 - 반복 요소를 변수에 담아서 처리하기**

```java
for (int i = 0; i < students.length; i++) {
    Student s = students[i];
    System.out.println("이름: " + s.name + " 나이: " + s.age + " 성적: " + s.grade);
```

`students[i].name`, `students[i].age` 처럼 `sutdents[i]`를 자주 접근하는 것이 번거롭다면 반복해서 사용하는 객체를 `Student s`와 같은 변수에 담아두고 사용해도 된다.

이 경우 향상된 for문(for-each문)을 사용하는 것이 가장 깔끔하다.

**향상된 for문(Enhanced For Loop)**

```java
for (Student s : students) {
    System.out.println("이름: " + s.name + " 나이: " + s.age + " 성적: " + s.grade);
```

## 문제와 풀이

### 문제: 영화 리뷰 관리하기1 → 2 (배열, for문 사용)

**문제설명**

당신은 영화 리뷰 정보를 관리하려고 한다. 먼저, 영화 리뷰 정보를 담을 수 있는 `MovieReview` 클래스를 만들어보자.

**요구사항**

1. `MovieReview` 클래스는 다음과 같은 멤버 변수를 포함해야한다.
    - 영화 제목(`title`)
    - 리뷰 내용(`review`)
2. `MovieReviewMain` 클래스 안에 `main()`메서드를 포함하여, 영화 리뷰 정보를 선언하고 출력하자.

**예시 코드 구조**

```java
package class1.Ex;

public class MovieReview {
    String title;
    String review;
}
```

```java
package class1.Ex;

public class MovieReviewMain {
    public static void main(String[] args) {
    // 영화 리뷰 정보 선언
    // 영화 리뷰 정보 출력
}
```

**출력 예시**

```java
영화 제목: 인셉션, 리뷰: 인생은 무한 루프
영화 제목: 어바웃 타임, 리뷰: 인생 시간 영화!
```

**MovieReview**

```java
package class1.Ex;

public class MovieReview {
    String title;
    String review;
}
```

**MovieReviewMain**

```java
package class1.Ex;

public class MovieReviewMain {
    public static void main(String[] args) {
        MovieReview movie1 = new MovieReview();  // 영화 리뷰 정보 선언
        movie1.title = "인셉션";  // 영화 제목
        movie1.review = "인생은 무한 루프";  // 영화 리뷰

        MovieReview movie2 = new MovieReview();  // 영화 리뷰 정보 선언
        movie2.title = "어바웃 타임";  // 영화 제목
        movie2.review = "인생 시간 영화!";  // 영화 리뷰

        MovieReview[] movies = {movie1, movie2};

        for (MovieReview movie : movies) {
            System.out.println("영화 제목: " + movie.title + ", 리뷰: " + movie.review);
        }
    }
}
```

**실행 결과**

```java
영화 제목: 인셉션, 리뷰: 인생은 무한 루프
영화 제목: 어바웃 타임, 리뷰: 인생 시간 영화!
```

### 문제: 상품 주문 시스템 개발

**문제 설명**

온라인 상점의 주문 관리 시스템 개발

상품 주문 정보를 담을 수 있는 `ProductOrder` 클래스 만들기

**요구사항**

1. `ProductOrder` 클래스는 다음과 같은 멤버 변수를 포함
    - 상품명(`ProductName`)
    - 가격(`price`)
    - 주문수량(`quantity`)
2. `ProductOrderMain` 클래스 안에 `main()` 메서드를 포함하여, 여러 상품의 주문 정보를 배열로 관리하고, 그 정보들을 출력하고, 최종 결제 금액 계산하여 출력

**예시 코드 구조**

```java
package class1.Ex;

public class ProductOrder {
    String ProductName;
    int price;
    int quantity;
}
```

```java
package class1.Ex;

public class ProductOrderMain {
    public static void main(String[] args) {
        // 여러 상품의 주문 정보를 담는 배열 생성
        // 상품 주문 정보를 'ProductOrder' 타입의 변수로 받아 저장
        // 상품 주문 정보와 최종 금액 출력
    }
}
```

**출력예시**

```java
상품명: 두부, 가격: 2000, 수량: 2
상품명: 김치, 가격: 5000, 수량: 1
상품명: 콜라, 가격: 1500, 수량: 2
총 결제 금액: 12000
```

**ProductOrder**

```java
package class1.Ex;

public class ProductOrder {
    String ProductName;
    int price;
    int quantity;
}
```

**ProductOrderMain_1 (내가 작성한 코드)**

```java
package class1.Ex;

public class ProductOrderMain_1 {

    public static void main(String[] args) {

        ProductOrder product1 = new ProductOrder();
        product1.ProductName = "두부";
        product1.price = 2000;
        product1.quantity = 2;

        ProductOrder product2 = new ProductOrder();
        product2.ProductName = "김치";
        product2.price = 5000;
        product2.quantity = 1;

        ProductOrder product3 = new ProductOrder();
        product3.ProductName = "콜라";
        product3.price = 1500;
        product3.quantity = 3;

        ProductOrder[] products = {product1, product2, product3};
        int totalPrice = 0;
        for (ProductOrder product : products) {
            System.out.println("상품명: " + product.ProductName + ", 가격: " + product.price + ", 수량: " + product.quantity);
            totalPrice += product.price;
        }
        System.out.println("총 결제 금액: " + totalPrice);
    }
}
```

**ProductOrderMain_2 (제안 코드)**

```java
package class1.Ex;

public class ProductOrderMain_2 {

    public static void main(String[] args) {
        ProductOrder[] orders = new ProductOrder[3];

        ProductOrder order1 = new ProductOrder();
        order1.ProductName = "두부";
        order1.price = 2000;
        order1.quantity = 2;
        orders[0] = order1;

        ProductOrder order2 = new ProductOrder();
        order2.ProductName = "김치";
        order2.price = 5000;
        order2.quantity = 1;
        orders[1] = order2;

        ProductOrder order3 = new ProductOrder();
        order3.ProductName = "콜라";
        order3.price = 1500;
        order3.quantity = 2;
        orders[2] = order3;

        int totalAmount = 0;
        for (ProductOrder order : orders) {
            System.out.println("상품명: " + order.ProductName + ", 가격: " + order.price + ", 수량: " + order.quantity);
            totalAmount += order.price * order.quantity;
        }
        System.out.println("총 결제 금액: " + totalAmount);
    }
}
```

**실행결과**

```java
상품명: 두부, 가격: 2000, 수량: 2
상품명: 김치, 가격: 5000, 수량: 1
상품명: 콜라, 가격: 1500, 수량: 2
총 결제 금액: 12000
```