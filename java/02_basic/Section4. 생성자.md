[<back](https://www.notion.so/basic-_-3ee7f95ac24d43f881b7ee2e8f21c210?pvs=21)

---

<aside>
📃 목차

</aside>

---

## 생성자 - 필요한 이유

객체를 생성하는 시점에 어떤 작업을 하고 싶다면 생성자(construct) 이용.

**MemberInit**

```java
package construct;

public class MemberInit {
    String name;
    int age;
    int grade;
}
```

**MethodInitMain1**

```java
package construct;

import java.lang.reflect.Member;

public class MethodInitMain {
    public static void main(String[] args) {

        MemberInit member1 = new MemberInit();
        member1.name = "user1";
        member1.age = 15;
        member1.grade = 90;

        MemberInit member2 = new MemberInit();
        member2.name = "user2";
        member2.age = 16;
        member2.grade = 80;

        MemberInit[] members = {member1, member1};

        for (MemberInit member : members) {
            System.out.println("이름: " + member.name + ", 나이: " + member.age + ", 성적: " + member.grade);
        }
    }
}
```

**실행결과**

```java
이름: user1, 나이: 15, 성적: 90
이름: user2, 나이: 16, 성적: 80
```

회원 객체를 생성하고 나면 `name`, `age`, `grade` 같은 변수에 초기값을 설정.

이 코드에는 회원 초기값을 설정하는 부분이 계속해서 반복된다. 메서드를 이용해 반복을 제거해보자.

**MethodInitMain2**

```java
package construct;

public class MethodInitMain2 {
    public static void main(String[] args) {

        MemberInit member1 = new MemberInit();
        initMember(member1, "user1", 15, 90);

        MemberInit member2 = new MemberInit();
        initMember(member2, "user2", 16, 80);

        MemberInit[] members = {member1, member2};
        for (MemberInit member : members) {
            System.out.println("이름: " + member.name + ", 나이: " + member.age + ", 성적: " + member.grade);
        }
    }
    static void initMember(MemberInit member, String name, int age, int grade) {
        member.name = name;
        member.age = age;
        member.grade = grade;
    }
}
```

**실행결과**

```java
이름: user1, 나이: 15, 성적: 90
이름: user2, 나이: 16, 성적: 80
```

`initMember(…)` 메서드를 사용해서 반복 제거.

그런데 이 메서드는 대부분 `Member` 객체의 멤버 변서를 사용한다. 우리는 앞서 객체 지향에 대해 학습함.

이 경우 속성과 기능을 한 곳에 두는 것이 더 나은 방법이다.

따라서 `MemberInit`이 자기 자신의 데이터를 변경하는 기능(메서드)를 제공하는 것이 좋다.

## this

**MemberInit - intiMember() 추가**

```java
package construct;

public class MemberInit {
    String name;
    int age;
    int grade;

    void initMember(String name, int age, int grade) {
        this.name = name;
        this.age = age;
        this.grade = grade;
    }
}
```

**MethodInitMain3**

```java
package construct;

public class MethodInitMain3 {
    public static void main(String[] args) {

        MemberInit member1 = new MemberInit();
        member1.initMember("user1", 15, 90);

        MemberInit member2 = new MemberInit();
        member2.initMember("user2", 16, 80);

        MemberInit[] members = {member1, member2};
        for (MemberInit member : members) {
            System.out.println("이름: " + member.name + ", 나이: " + member.age + ", 성적: " + member.grade);
        }
    }
}
```

**실행결과**

```java
이름: user1, 나이: 15, 성적: 90
이름: user2, 나이: 16, 성적: 80
```

`initMember(…)`는 `Member`에 초기값을 설정하는 기능을 제공하는 메서드.

다음과 같이 메서드를 호출하면 객체의 멤버 변수에 인자로 넘어온 값을채운다.

`member1.initMember(”user1”, 15, 90)`

### this

`initMember(String name…)`의 코드를 보면 메서드의 매개변수에 정의한 `String name`과 `Member`의 멤버 변수의 이름이 `String name`으로 똑같다. 나머지 변수들도 `name`, `age`, `grade`로 모두 같다.

- 이 경우 멤버변수가 코드 블럭의 더 안쪽에 있기 때문에 **매개변수가 우선순위**를 갖음.
    - 따라서 `initMember(String name, …)` 메서드 안에서 `name`이라고 적으면 매개변수에 접근.
    - 멤버 변수에 접근하려면 앞에 `this.`라고 해주면, `this`는 인스턴스 자신의 참조값을 가리킴

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/3e573424-8610-4462-9f5d-78c02895ac8d/54d24a6a-b719-494c-adc6-292d4b96a741.png)

**진행 과정**

```java
this.name = name;  //1. 오른쪽의 name은 매개변수에 접근
this.name = "user1";  //2. name 매개변수의 값 사용
x001.name = "user1";  //3. this.은 인스턴스 자신의 참조값을 뜻함. 따라서 인스턴스 멤버 변수에 접근
```

**this 제거**

```java
this.name = name;
name = name;
```

둘다 매개변수를 가르키기 때문에 초기값이 출력되게 된다.

**실행 결과**

```java
이름: null, 나이: 0, 성적: 0
이름: null, 나이: 0, 성적: 0
```

**정리**

- 매개변수의 이름과 멤버 변수의 이름이 같은 경우 this를 사용해서 둘을 명확하게 구분.
- this 인스턴스는 자신을 가리킨다.

### this의 생략

this는 생략가능.

이 경우 변수를 찾을 때 가까운 지역변수(매개변수도 지역변수)를 먼저 찾고 없으면 그 다음으로 멤버변수를 찾음.

멤버 변수도 없을 시 오류발생.

필드 이름(멤버 변수)와 매개변수의 이름이 서로 다른 경우

```java
package construct;

public class MemberThis {
    String nameField;

    void initMember(String nameParameter) {
        nameField = nameParameter;
    }
}
```

nameField는 앞에 this가 없어도 멤버 변수에 접근

- `nameField`는 먼저 지역변수(매개변수)에서 같은 이름이 있는지 찾음. 없으므로 멤버 변수에서 찾음.
- `nameParameter`는 먼저 지역변수(매개변수)에서 같은 이름이 있는지 찾음. 있으므로 사용.

### this와 코딩 스타일

다음과 같이 멤버 변수에 접근하는 경우 항상 `this`를 사용하는 코딩 스타일도 있다.

```java
package construct;

public class MemberThis {
    String nameField;

    void initMember(String nameParameter) {
        this.nameField = nameParameter;
    }
}
```

`this.nameField`를 보면 `this`를 생략할 수 있지만, 생략하지 않고 사용해도 된다.

이렇게 this를 사용하면 이 코드가 멤버 변수를 사용한다는 것을 눈으로 쉽게 확인 가능.

- 그러나 최근 스타일은 `this`를 쓰지 않는다.
    - 왜?
        - this를 쓰면 코드가 지저분해진다.
        - IDE에서 색깔로 구분 해주기 때문.

      ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/e1c82f25-eaa0-490d-b97c-7f7e8e30e85f/image.png)

    - 따라서,
        - 이름이 중복되는 경우에만 this를 사용한다.

## 생성자 - 도입

프로그래밍을 하다보면 생성하고 이후 바로 초기값을 할당해야하는 경우가 많다.

따라서 앞서 `initMember(…)`과 같은 메서드를 매번 만들어야함.

그래서 대부분 객체 지향 언어는 객체를 생성하자마자 즉시 필요한 기능을 좀 더 편리하게 수행할 수 있도록 생성자라는 기능 제공

생성자를 사용하면 객체를 생성하는 시점에 즉시 필요한 기능을 수행할 수 있음.

앞서 살펴면 `initMember(…)`와 같이 메서드와 유사하지만 몇가지 다른 특성.

**MemberConstruct**

```java
package construct;

public class MemberConstruct {
    String name;
    int age;
    int grade;

    MemberConstruct(String name, int age, int grade) {
        System.out.println("생성자 호출 name= " + name + ", age= " + age + ", grade= " + grade);
        this.name = name;
        this.age = age;
        this.grade = grade;
    }
}
```

**ConstructMain1**

```java
package construct;

public class ConstructMain1 {
    public static void main(String[] args) {

        MemberConstruct member1 = new MemberConstruct("user1", 15, 90);
        MemberConstruct member2 = new MemberConstruct("user2", 16, 80);

        MemberConstruct[] membesrs = {member1, member2};
        for (MemberConstruct member : membesrs) {
            System.out.println("이름: " + member.name + ", 나이: " + member.age + ", 성적: " + member.grade);
        }
    }
}
```

**실행결과**

```java
생성자 호출 name= user1, age= 15, grade= 90
생성자 호출 name= user2, age= 16, grade= 80
이름: user1, 나이: 15, 성적: 90
이름: user2, 나이: 16, 성적: 80
```

**생성자 호출**

생성자는 인스턴스를 생성하고 나서 즉시 호출된다. 새성자를 호출하는 방법은 다음 코드와 같이 new 명령어 다음에 생성자 이름과 매개변수에 맞추어 인수를 전달.

```java
new 생성자 이름(생성자에 맞는 인수 목록)
new 클래스 이름(생성자에 맞는 인수 목록)
```

참고로 생성자 이름이 클래스 이름.

```java
new MenberConstruct("user1", 15, 90);
```

위 코드는 인스턴스를 생성하고 즉시 해당  생성자를 호출.

(`Member` 인스턴스 생성 후 바로 `MemberConstruct(String name, int age, int grade)` 생성자 호출.

참고로 new 키워드를 사용해서 객체를 생성할 때 마지막 `()` 도 포함해야 하는 이유가 바로 생성자 때문.

객체를 생성하면서 동시에 생성자를 호출.

### 생성자 장점

**중복 호출 제거**

생성자가 없던 시절에는 생성 직후 어떤 작업을 수행하기 위해 다음과 같이 메서드를 직접 한번 더 호출해야 했다.

생성자 덕분에 객체를 생성하면서 동시에 생성 직후에 필요한 작업을 한번에 처리할 수 있음.

```java
//생성자 등장 전
MemberInit member = new MemberInit();
member.initMember("user1", 15, 90);

//생성자 등장 후
MemberInit member = new MemberInit("user1", 15, 90);
```

**제약 - 생성자 호출 필수**

생성자 등장 전 코드의 경우 `initMember(…)`를 실수로 호출하지 않는다면?

```java
MemberInit member1 = new MemberInit();
initMember(member1, "user1", 15, 90);

MemberInit member2 = new MemberInit();
```

**실행결과**

```java
이름: user1, 나이: 15, 성적: 90
이름: null, 나이: 0, 성적: 0
```

member2 인스턴스에는 초기화된 값이 등록되고 오류가 발생하지 않는다.

결국 아무 정보가 없는 유령 회원이 시스템 내부에 등장하게 됨.

그러나 클래스에 생성자를 넣어두면 아래와 같이 실수로 누락하더라도…

```java
MemberConstruct member1 = new MemberConstruct("user1", 15, 90);
MemberConstruct member2 = new MemberConstruct();
```

**실행결과**

```java
java: constructor MemberConstruct in class construct.MemberConstruct cannot be applied to given types;
  required: java.lang.String,int,int
  found:    no arguments
  reason: actual and formal argument lists differ in length
```

위 처럼 인자를 찾을 수 없다는 에러 메세지가 뜬다.

생성자의 진짜 장점은 객체를 생성할 때 직접 정의한 생성자가 있다면 **직접 정의한 생성자를 반드시 호출**해야함.

참고로 생성자를 메서드 오버로딩 처럼 여러개 정의할 수 있는데, 이 경우에는 하나만 호출

따라서, 생성자 덕분에 이름, 나이, 성적 필수 입력!!

아무 정보 없는 유령 회원이 시스템 내부에 등장할 가능성을 원천 차단!!

**생성자를 사용하면 필수값 입력을 보장할 수 있다.**

> 참고: 좋은 프로그램은 무한한 자유도가 주어지는 프로그램이 아니라 적절한 제약이 있는 프로그램이다.
>

## 기본 생성자

생성자를 만든적 없는데, 생성자를 호출한적 있음.

**MemberInit**

```java
package construct;

public class MemberInit {
    String name;
    int age;
    int grade;
}
```

**MemberInitMain1**

```java
package construct;

public class MethodInitMain1 {
    public static void main(String[] args) {

        MemberInit member1 = new MemberInit();
        ...
    }
}
```

여기서 `new MemberInit()`  이 부분은 분명히 매개변수가 없는 다음과 같은 생성자가 필요할 것.

```java
package construct;

public class MemberInit {
    String name;
    int age;
    int grade;
    
    MemberInit() {  //생성자 필요
    }
}
```

**기본 생성자**

- 매개변수 없는 생성자를 기본 생성자라함.
- 클래스에 생성자가 하나도 없으면 자바 컴파일러는 매개변수가 없고, 작동하는 코드가 없는 기본 생상자를 자동으로 만듦.
- **생성자가 하나라도 있으면 자바는 기본 생성자를 만들지 않는다.**

`MemberInit` 클래스의 경우 생성자를 만들지 않았으므로 자바가 자동으로 기본 생성자를 만들어준 것.

참고: 자바가 자동으로 생성해주는 기본 생성자는 클래스와 같은 접근 제어자르 가진다.(`public`)

**MemberDefault**

```java
package construct;

public class MemberDefault {
    String name;

    MemberDefault() {
        System.out.println("생성자 호출");
    }
}
```

위 처럼 기본 생성자를 직접 만들어도 된다.

**실행결과**

```java
생성자 호출
```

**기본 생성자를 왜 자동으로 만들어 줄까?**

만약 자바에서 기본 생성자를 만들어주지 않는다면, 생성자 기능이 필요하지 않은 경우데도 모든 클래스에 개발자가 직접 기본 생성자를 정의해야한다. 생성자 기능을 사용하지 않는 경우도 많기에 이런 편의기능 제공.

**정리**

- 생성자는 반드시 호출.
- 생성자가 없으면 기본 생성자 제공.
- 생성자가 하나라도 있으면 기본 생성자 제공 안되며, 이 경우 개발자가 정의한 생성자 직접 호출.

## 생성자 - 오버로딩과 this()

생성자도 메서드 오버로딩처럼 매개변수만 다르게 해서 여러 생성자 제공 가능

**MemberConstruct - 생성자 추가**

```java
package construct;

public class MemberConstruct {
    String name;
    int age;
    int grade;

    //추가
    MemberConstruct(String name, int age) {
        this.name = name;
        this.age = age;
        grade = 50;
    }

    MemberConstruct(String name, int age, int grade) {
        System.out.println("생성자 호출 name= " + name + ", age= " + age + ", grade= " + grade);
        this.name = name;
        this.age = age;
        this.grade = grade;
    }
}
```

기존 `MemberConstruct`에 생성자 하나 추가해서 생성자가 2개가 되었다.

새로 추가한 생성자는 `grade`를 받지 않는다. 대신 `grade`는 `50`점

**ConstructMain2**

```java
package construct;

public class ConstructMain2 {
    public static void main(String[] args) {

        MemberConstruct member1 = new MemberConstruct("user1", 15, 90);
        MemberConstruct member2 = new MemberConstruct("user2", 16);

        MemberConstruct[] membesrs = {member1, member2};
        for (MemberConstruct member : membesrs) {
            System.out.println("이름: " + member.name + ", 나이: " + member.age + ", 성적: " + member.grade);
        }
    }
}
```

**실행결과**

```java
생성자 호출 name= user1, age= 15, grade= 90
이름: user1, 나이: 15, 성적: 90
이름: user2, 나이: 16, 성적: 50
```

생성자를 오버로딩 한 덕분에 성적 입력이 꼭 필요한 경우에는 `grade`가 있는 생성자를 호출하고 그렇지 않은 경우에는 `grade`가 없는 생성자를 호출하면 된다.

### this()

두 생성자를 보면 중복되는 부분이 있다.

```java
this.name = name;
this.age = age;
```

이 때 `this()`라는 기능을 사용하면 생성자 내부에서 자신의 생성자를 호출할 수 있다. 참고로 `this`는 인스턴스 자신의 참조값을 가리킨다. 그래서 자신의 생성자를 호출한다.

**MemberConstruct - this() 사용**

```java
package construct;

public class MemberConstruct {
    String name;
    int age;
    int grade;

    //추가
    MemberConstruct(String name, int age) {
        this(name, age, 50);
    }

    MemberConstruct(String name, int age, int grade) {
        System.out.println("생성자 호출 name= " + name + ", age= " + age + ", grade= " + grade);
        this.name = name;
        this.age = age;
        this.grade = grade;
    }
}
```

**ConstructMain2**

```java
package construct;

public class ConstructMain2 {
    public static void main(String[] args) {

        MemberConstruct member1 = new MemberConstruct("user1", 15, 90);
        MemberConstruct member2 = new MemberConstruct("user2", 16);

        MemberConstruct[] membesrs = {member1, member2};
        for (MemberConstruct member : membesrs) {
            System.out.println("이름: " + member.name + ", 나이: " + member.age + ", 성적: " + member.grade);
        }
    }
}
```

**실행결과**

```java
생성자 호출 name= user1, age= 15, grade= 90
생성자 호출 name= user2, age= 16, grade= 50
이름: user1, 나이: 15, 성적: 90
이름: user2, 나이: 16, 성적: 50
```

이 코드는 첫번째 생성자 내부에서 두번째 생성자를 호출한다.

```java
MemberConstruct(String name, int age) -> MemberConstruct(String name, int age, int grade)
```

`this()` 를 사용하면 생성자 내부에서 다른 생성자를 호출할 수 있다. 이부분을 잘 활용하면 지금과 같이 중복을 제거할 수 있다.

**this() 규칙**

- `this()`는 생성자 코드의 첫줄에만 작성할 수 있다.

다음은 규칙 위반. 컴파일 오류 발생

```java
MemberConstruct(String name, int age) {
    System.out.println();
    this(name, age, 50);
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/a6c8ece0-ec19-4f60-a2a5-2788fc73467a/image.png)

```java
Error:Call to 'this()' must be first statement in constructor body
'this()'에 대한 호출은 생성자 본문의 첫 번째 문이어야 합니다.
```

## 문제와 풀이

### 문제 - Book과 생성자

`BookMain` 코드가 작동하도록 `Book` 클래스를 완성하세요

특히 `Book` 클래스는 생성자 중복을 피할 것.

**Book**

```java
package construct.ex;

public class Book {
    String title;
    String author;
    int page;
    
    //TODO 코드를 완성하세요.
}
```

**BookMain**

```java
package construct.ex;

public class BookMain {
    public static void main(String[] args) {
        // 기본 생성자 사용
        Book book1 = new Book();
        book1.displayInfo();

        // title과 author를 매개변수로 받는 생성자
        Book book2 = new Book("Hello Java", "Seo");
        book2.displayInfo();

        // 모든 필드를 매개변수로 받는 생성자
        Book boo3 = new Book("JPA 프로그래밍", "Kim", 700);
        boo3.displayInfo();
    }
}
```

**실행결과**

```java
제목: , 저자: , 페이지: 0
제목: Hello Java, 저자: Seo, 페이지: 0
제목: JPA 프로그래밍, 저자: Kim, 페이지: 700
```

**Book**

```java
package construct.ex;

public class Book {
    String title;
    String author;
    int page;

    Book() {
        this("", "", 0)
    }

    Book(String title, String author) {
        this(title, author, 0);
    }

    Book(String title, String author, int page) {
        this.title = title;
        this.author = author;
        this.page = page;
    }

    void displayInfo() {
        System.out.println("제목: " + title + ", 저자: " + author + ", 페이지: " + page);
    }
}
```