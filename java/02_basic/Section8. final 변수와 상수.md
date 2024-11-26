[<back](https://www.notion.so/basic-_-3ee7f95ac24d43f881b7ee2e8f21c210?pvs=21)

---

<aside>
📃 목차

</aside>

---

## final 변수와 상수 1

변수에 `final` 키워드가 붙으면 더는 값을 변경 할 수 없다.

참고로 `final`은 `class`, `method`를 포함한 여러곳에 붙을 수 있다. 지금 변수에 붙은 `final` 키워드를 알아보자. 나머지는 상속을 설명한 이후 설명.

### final - 지역변수

**FinalLocalMain**

```java
package final1;

public class FinalLocalMain {

    public static void main(String[] args) {
        //final 지역 변수
        final int data1;
        data1 = 10;
        data1 = 20;  //컴파일 오류
        //final 지역변수
        final int data2 = 20;
        data2 = 10;  //컴파일 오류
    }

    static void method(final int parameter) {
        parameter = 10;  //컴파일 오류
    }
}
```

**실행결과**

```java
java: variable data1 might already have been assigned
java: cannot assign a value to final variable data2
java: final parameter parameter may not be assigned
```

- `final`을 지역 변수에 설정할 경우 최초 한번만 할당할 수 있다. 이후 변수의 값을 변경하려면 컴파일 오류 발생.
- `final`을 지역 변수 선언 시 바로 초기화 한 경우 이미 값이 할당되었기 때문에 값을 할당할 수 없다.
- 매개변수에 `final`이 붙으면 메서드 내부에서 매개변수의 값을 변경할 수 없다. 따라서 메서드 호출 시점에 사용된 값이 끝까지 사용됨.

### final - 필드(멤버 변수)

**ConstructInit**

```java
package final1;

public class ConstructInit {

    final int value;  //초기값이 없는 경우

    public ConstructInit(int value) {
        this.value = value;  //생성자를 통해 할당 가능
    }
}
```

- `final` 멤버 변수가 초기값이 설정 되어 있지 않은 경우 생성자를 통해 변수를 초기화할 수 있다.

**FieldInit**

```java
package final1;

public class FieldInit {

    static final int CONST_VALUE = 10;
    final int value = 10;  //초기값이 설정되어 있는 경우

    public FieldInit(int vlaue) {
        //this.value = vlaue;  //생성자를 통해 할당 불가
    }
}
```

- `final` 멤버 변수가 초기값 설정이 되어있는 경우 생성자를 통해 할당이 불가하다.

**FinalFieldMain**

```java
package final1;

public class FinalFieldMain {
    public static void main(String[] args) {
        //final 필드 - 생성자 초기화
        System.out.println("생성자 초기화");
        ConstructInit constructInit1 = new ConstructInit(10);
        ConstructInit constructInit2 = new ConstructInit(20);
        System.out.println("constructInit1 = " + constructInit1.value);
        System.out.println("constructInit2 = " + constructInit2.value);

        //final 필드 - 필드 초기화
        System.out.println("필드 초기화");
        FieldInit fieldInit1 = new FieldInit();
        FieldInit fieldInit2 = new FieldInit();
        FieldInit fieldInit3 = new FieldInit();
        System.out.println("fieldInit1 = " + fieldInit1.value);
        System.out.println("fieldInit2 = " + fieldInit2.value);
        System.out.println("fieldInit3 = " + fieldInit3.value);

        //상수
        System.out.println("상수");
        System.out.println(FieldInit.CONST_VALUE);
    }
}
```

실행결과

```java
생성자 초기화
constructInit1 = 10
constructInit2 = 20
필드 초기화
fieldInit1 = 10
fieldInit2 = 10
fieldInit3 = 10
상수
10
```

`ConstructInit` 과 같이 생성자를 사용해서 `final` 필드를 초기화 하는 경우, 각 인스턴스마다 `final` 필드에 다른 값을 할당할 수 있다. `final`을 사용했기 때문에 생성 이후에 이 값을 변경하는 것이 불가.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/10061235-b5e1-4674-83ab-ca5238df2e35/7cfefb28-d3f1-46e8-99aa-1eb1408b2387.png)

- `FieldInit` 과 같이 final 필드를 필드에서 초기화 하는 경우, 모든 인스턴스가 다음 오른쪽 그림과 같이 같은 값을 가진다.
- 여기서는 `FieldInit` 인스턴스의 모든 `value` 값은 `10`이 된다.
- 모든 인스턴스가 같은 값을 가사용하기 때문에 결과적으로 메모리를 낭비하게 된다.(물론 JVM에 따라서 내부 최적화를 시도할 수 있다.)또 메모리 낭비를 떠나서 같은 값이 계속 생성되는 것은 개발자가 보기에 명확한 중복이다. 이럴 때 사용하면 좋은것이 바로 `static` 영역이다.

**static final**

- `FieldInit.MY_VALUE` 는 `static` 영역에 존재. 그리고 `final` 키워드를 사용해서 초기화 값이 변하지 않는다.
- static 영역은 단 하나만 존재하는 영역. `MY_VALUE`변수는 JVM에서 하나만 존재하므로 앞서 설명한 중복과 메모리 비효율 해결 가능

이런 이유로 필드에 `final` + 필드 초기화를 사용하는 경우 `static`을 붙혀서 사용하는 것이 효과적.

## final 변수와 상수 2

**상수(Constant)**

상수는 변하지 않고, 항상 일정한 값을 갖는 수를 말한다. 자바에서 보통 단 하나만 존재하는 변하지 않는 고정된 값을 상수라 한다.

이런 이유로 상수는 `static final` 키워드를 사용.

**자바 상수 특징**

- static final 키워드 사용
- 대문자를 사용하고 구분은 `_`(언더스코어) 사용(관례)
  - 일반적인 변수와 상수를 구분하기 위해
- 필드를 직접 접근해서 사용한다.
  - 상수는 기능이 아니라 고정된 값 자체를 사용하는 것이 목적.
  - 상수는 값을 변경할 수 없다. 따라서 필드에 직접 접근해도 데이터가 변하는 문제 발생 안함

**Constant**

```java
package final1;

public class Constant {
    //수학상수
    public static final double PI = 3.14;

    //시간상수
    public static final int HOURS_IN_DAY = 24;
    public static final int MINUTES_IN_HOUR = 60;
    public static final int SECONDS_IN_MINUTE = 60;

    //애플리케이션 설정 상수
    public static final int MAX_USERS = 1000;
}
```

- 보통 이러너 상수들은 애플리케이션 전반에서 사용되기 때문에 public을 자주 사용.
- 상수는 중아에서 값을 하나로 관리할 수 있다는 장점
- 상수는 런타임 변경 할 수 없다. 상수를 변경하려면 프로그램 종료하고, 코드를 변경한 다음 다시 프로그램을 실행해야함.

**ConstantMain1 - 상수 없음**

```java
package final1;

public class ConstantMain1 {

    public static void main(String[] args) {
        System.out.println("프로그램 최대 참여자수 " + 1000);
        int currentUserCount = 999;
        proccess(currentUserCount++);
        proccess(currentUserCount++);
        proccess(currentUserCount++);
        proccess(currentUserCount++);
        proccess(currentUserCount++);
    }

    private static void proccess(int currentUSerCount) {
        System.out.println("참여자 수: " + currentUSerCount);
        if (currentUSerCount > 1000) {
            System.out.println("대기자로 등록합니다.");
        } else {
            System.out.println("게임에 참여합니다.");
        }
    }
}
```

**ConstantMain2**

```java
package final1;

public class ConstantMain2 {

    public static void main(String[] args) {
        System.out.println("프로그램 최대 참여자수 " + Constant.MAX_USERS);
        int currentUserCount = 999;
        proccess(currentUserCount++);
        proccess(currentUserCount++);
        proccess(currentUserCount++);
        proccess(currentUserCount++);
        proccess(currentUserCount++);
    }

    private static void proccess(int currentUSerCount) {
        System.out.println("참여자 수: " + currentUSerCount);
        if (currentUSerCount > Constant.MAX_USERS) {
            System.out.println("대기자로 등록합니다.");
        } else {
            System.out.println("게임에 참여합니다.");
        }
    }
}
```

- Constant.MAX_USER 상수를 사용했을 때 장점
  - 상수 값(`MAX_USER`)만 변경하면 프로그램을 간단하게 수정할 수 있다.
  - 매직 넘버 문제 해결 → 1000이아니라 사람이 인지할 수 있게 `MAX_USER`라는 변수명으로 코드를 이해할 수 있게 됐다.

## final 변수와 참조

final은 변수의 값을 변경 못함.

- 변수는 크게 2가지로 나뉨(기본형, 참조형)
- 기본형 변수 `10`, `20` … 참조형 변수: `x001`, `x002`
  - `final`을 기본형 변수에 사용하면 값 변경 불가
  - `final`을 참조형 변수에 사용하면 참조값 변경 불가

**Data**

```java
package final1;

public class Data {
    public int value;
}
```

**FinalRefMain**

```java
package final1;

public class FinalRefMain {
    public static void main(String[] args) {
        final Data data = new Data();
        // data = new Data();

        // 참조 대상의 값은 변경 가능
        data.value = 10;
        System.out.println("data.value = " + data.value);
        data.value = 20;
        System.out.println("data.value = " + data.value);
    }
}
```

**실행 결과**

```java
final Data data = new Data();
data = new Data();

java: cannot assign a value to final variable data- 
```

- 참조형 변수 `data`에 `final`이 붙어있으므로, 변수의 참조값을 더 이상 변경할 수 없다.

**실행 결과**

```java
data.value = 10
data.value = 20
```

그러나 참조 대상의 객체 값은 변경 가능

- 참조형 변수 `data`에 `final`이 붙었다. 이 경우 참조값은 변경 못하지만, 참조값 내부의 객체는 변경할 수 있다.
- `Data.value`는 `final`이 아니기 때문에 변경 가능한 것임.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/5a717999-9cb2-450f-811d-3356033fa4fc/85876d90-0a38-4f0c-bbff-880cd58591da.png)

참조형 변수 final이 붙으면 참조 대상 자체를 다른 대상으로 변경 못한다는 것. (`x001` -x→ `x002`)

참조 대상의 변수는 변경 가능 (`x001.value1` → `x001.value2`)

## 응용

### id는 못바꾸게 하는 기능

**Member**

```java
package final1.ex;

public class Member {

    private final String id;
    private String name;

    public Member(String id, String name) {
        this.id = id;
        this.name = name;
    }

    public void changeData(String name) {
        //this.id = id;  //컴파일 오류
        this.name = name;
    }

    public void print() {
        System.out.println("id = " + id + ", name = " + name);
    }
}
```

**MemberMain**

```java
package final1.ex;

public class MemberMain {
    public static void main(String[] args) {
        Member member1 = new Member("jamm0316", "송재명");
        member1.print();
        member1.changeData("곽소영");
        member1.print();
    }
}
```

**실행결과**

```java
id = jamm0316, name = 송재명
id = jamm0316, name = 곽소영
```

---

[⬆️ Top](https://www.notion.so/Section8-final-db7416ad25924f21813d8059d9279e53?pvs=21)