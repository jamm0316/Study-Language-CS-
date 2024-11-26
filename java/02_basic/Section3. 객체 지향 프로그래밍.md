[<back](https://www.notion.so/basic-_-3ee7f95ac24d43f881b7ee2e8f21c210?pvs=21)

---

<aside>
📃 목차

</aside>

---

## 절차 지향 프로그래밍1 - 시작

**절차 지향 프로그래밍 vs 객체 지향 프로그래밍**

프로그래밍 방식은 크게 절차 지향 프로그래밍과 객체 지향 프로그래밍으로 나눌 수 있다.

**절차 지향 프로그래밍**

- 실행 순서를 중요하게 생각하는 방식.
- 프로그램의 흐름을 순차적으로 따르며 처리하는 방식.
- 즉, “어떻게”를 중심으로 프로그래밍 한다.

**객체 지향 프로그래밍**

- 객체를 중요하게 생각하는 방식.
- 실제 세계의 사물이나 사건을 객체로 보고, 객체들간의 상호작용을 중심으로 프로그래밍 하는 방식.
- 즉, “무엇을” 중심으로 프로그래밍 한다.

**둘의 중요한 차이**

- 절차 지향은 데이터와 해당 데이터의 처리방식이 분리.
- 객체 지향은 데이터와 그 데이터에 대한 행동이나 하나의 ‘객체’ 안에 함께 포함됨.

**단순히 클래스와 객체를 사용해서 관련 데이터를 묶는 방법은 절차 지향 프로그래밍이다.**

### 문제: 음악 플레이어 만들기

**요구 사항:**

1. 음악 플레이어를 켜고 끌 수 있어야 한다.
2. 음악 플레이어의 볼륨을 증가, 감소할 수 있어야 한다.
3. 음악 플레이어의 상태를 확인할 수 있어야 한다.

**예시 출력:**

```java
음악 플레이어를 시작합니다.
음악 플레이어 볼륨: 1
음악 플레이어 볼륨: 2
음악 플레이어 볼륨: 1
음악 플레이어 상태 확인
음악 플레이어 ON, 볼륨: 1
음악 플레이어를 종료합니다.
```

**MusicProductMain1**

```java
package oop1;

public class MusicPlayerMain1 {
    public static void main(String[] args) {
        int volume = 0;
        boolean isOn = false;

        //음악 플레이어 켜기
        isOn = true;
        System.out.println("음악 플레이어를 시작합니다.");

        // 볼륨증가
        volume++;
        System.out.println("음악 플레이어 볼륨: " + volume);

        // 볼륨증가
        volume++;
        System.out.println("음악 플레이어 볼륨: " + volume);

        // 볼륨감소
        volume--;
        System.out.println("음악 플레이어 볼륨: " + volume);

        // 시스템 확인
        if (isOn == true) {
            System.out.println("음악 플레이어: ON, 볼륨: " + volume);
        } else {
            System.out.println("음악 플레이어: OFF);");
        }
        // 종료
        isOn = false;
        System.out.println("음악 플레이어를 종료합니다.");
    }
}
```

**실행 결과**

```java
음악 플레이어를 시작합니다.
음악 플레이어 볼륨: 1
음악 플레이어 볼륨: 2
음악 플레이어 볼륨: 1
음악 플레이어: ON, 볼륨: 1
음악 플레이어를 종료합니다.
```

## 절차 지향 프로그래밍2 - 데이터 묶음

앞서 작성한 코드에 클래스를 도입하자. `MusicPlayerData` 라는 클래스를 만들고, 음악 플레이어에 사용되는 데이터들을 여기에 묶어서 멤버 변수로 사용.

### 절차 지향 음악 플레이어2 - 데이터 묶음

```java
package oop1;

public class MusicPlayerData { ;
    int volume;
    boolean isOn;
}
```

음악 플레이어에 사용되는 `volum`, `isOn` 속성을 `MusicPlayerData`의 멤버 변수에 포함.

**MusicPlayerMain2**

```java
package oop1;

public class MusicPlayerMain2 {

    public static void main(String[] args) {
        MusicPlayerData data = new MusicPlayerData();

        //음악 플레이어 켜기
        data.isOn = true;
        System.out.println("음악 플레이어를 시작합니다.");

        // 볼륨증가
        data.volume++;
        System.out.println("음악 플레이어 볼륨: " + data.volume);

        // 볼륨증가
        data.volume++;
        System.out.println("음악 플레이어 볼륨: " + data.volume);

        // 볼륨감소
        data.volume--;
        System.out.println("음악 플레이어 볼륨: " + data.volume);

        // 시스템 확인
        if (data.isOn == true) {
            System.out.println("음악 플레이어: ON, 볼륨: " + data.volume);
        } else {
            System.out.println("음악 플레이어: OFF);");
        }
        // 종료
        data.isOn = false;
        System.out.println("음악 플레이어를 종료합니다.");
    }
}
```

**실행결과**

```java
음악 플레이어를 시작합니다.
음악 플레이어 볼륨: 1
음악 플레이어 볼륨: 2
음악 플레이어 볼륨: 1
음악 플레이어: ON, 볼륨: 1
음악 플레이어를 종료합니다.
```

## 절차 지향 프로그래밍3 - 메서드 추출

중복되는 부분 및 재사용 가능 부분

```java
//음악 플레이어 켜기
data.isOn = true;
System.out.println("음악 플레이어를 시작합니다.");

// 볼륨증가
data.volume++;
System.out.println("음악 플레이어 볼륨: " + data.volume);

// 볼륨감소
data.volume--;
System.out.println("음악 플레이어 볼륨: " + data.volume);

// 시스템 확인
if (data.isOn == true) {
    System.out.println("음악 플레이어: ON, 볼륨: " + data.volume);
} else {
    System.out.println("음악 플레이어: OFF);");
    
// 종료
data.isOn = false;
System.out.println("음악 플레이어를 종료합니다.");
```

- 음악 플레이어 켜기, 끄기
- 볼륨 증가, 감소
- 음악플레이어 상태 출력

**MusicPlayerMain3**

```java
package oop1;

public class MusicPlayerMain3_2 {

    public static void main(String[] args) {
        MusicPlayerData data = new MusicPlayerData();

        //음악 플레이어 켜기
        on(data);

        // 볼륨증가
        volumeup(data);

        // 볼륨증가
        volumeup(data);

        // 볼륨감소
        volumedown(data);

        // 시스템 확인
        showStatus(data);

        // 종료
        off(data);
    }

    public static void on(MusicPlayerData data) {
        data.isOn = true;
        System.out.println("음악 플레이어를 시작합니다.");
    }

    public static void off(MusicPlayerData data) {
        data.isOn = false;
        System.out.println("음악 플레이어를 종료합니다.");

    }

    public static void volumeup(MusicPlayerData data) {
        data.volume++;
        System.out.println("음악 플레이어 볼륨: " + data.volume);
    }

    public static void volumedown(MusicPlayerData data) {
        data.volume--;
        System.out.println("음악 플레이어 볼륨: " + data.volume);

    }

    public static void showStatus (MusicPlayerData data) {
        if (data.isOn == true) {
            System.out.println("음악 플레이어: ON, 볼륨: " + data.volume);
        } else {
            System.out.println("음악 플레이어: OFF");
        }
    }
}
```

**실행 결과**

```java
음악 플레이어를 시작합니다.
음악 플레이어 볼륨: 1
음악 플레이어 볼륨: 2
음악 플레이어 볼륨: 1
음악 플레이어: ON, 볼륨: 1
음악 플레이어를 종료합니다.
```

각각의 기능을 메서드로 만든 덕분에 기능이 모듈화 됨.

**모듈화의 장점**

- **중복 제거**: 로직 중복이 제거됨. 같은 로직이 필요하면 해당 메서드를 여러번 호출하면 됨.
- **변경 영향 범위**: 기능을 수정할 때 해당 메서드 내부만 변경.
- **메서드 이름 추가**: 메서드 이름을 통해 코드를 더 쉽게 이해 가능.

<aside>
💡 **모듈화란?**

레고 블럭, 필요한 블럭을 가져다 꼽아서 사용할 수 있는 것 처럼 음악플레이어에 필요한 기능을 메서드 호출만으로 쉽게 사용가능. 이제 음악플레이어와 관련된 메서드를 조립해서 프로그램 작성 가능.

</aside>

**절차 지향 프로그래밍의 한계**

- 한계: 데이터와 기능이 분리되어있음.
    - 음악 플레이어의 데이터 → `MusicPlayerData`
    - 데이터를 사용하는 기능 → `MusicPlayerMain3`에 있는 각각의 메서드에 분리
- 데이터와 데이터를 사용하는 기능은 매우 밀접하게 연관 되어 있으며, 이후 관련 데이터가 변경되면 MisicPlayerMain3 부분의 메서드들도 함께 변경해야한다.
- 이로 인해 유지보수 관점에서도 관리 포인트가 2곳으로 늘어남.

- **객체지향 프로그래밍을 통해 데이터와 기능을 온전히 하나로 묶어서 사용가능**

## 클래스와 메서드

클래스는 데이터인 멤버 변수 뿐만 아니라 기능 역할을 하는 메서드도 포함될 수 있다.

**ValueData**

```java
package oop1;

public class ValueData {
    int value;
}
```

**ValueDataMain**

```java
package oop1;

public class ValueDataMain {
    public static void main(String[] args) {
        ValueData data = new ValueData();
        add(data);
        add(data);
        add(data);
        System.out.println("최종 숫자= " + data.value);
    }

    static void add(ValueData data) {
        data.value++;
        System.out.println("숫자 증가 value= " + data.value);
    }
}
```

**실행 결과**

```java
숫자 증가 value= 1
숫자 증가 value= 2
숫자 증가 value= 3
최종 숫자= 3
```

`ValueData`라는 인스턴스를 생성하고 외부에서 `ValueData.value`에 접근해 숫자를 하나씩 증가시키는 코드.

코드는 `value`와 그 값을 증가시키는 `add()` 메서드가 서로 분리되어 있음.

자바 같은 객체 지향 언어는 클래스 내부에 속성(데이터)과 기능(메서드)을 포함할 수 있다.

클래스 내부에 멤버 변수 뿐만아니라 메서드도 함께 포함할 수 있다는 뜻.

**ValueData**

```java
package oop1;

public class ValueData {
    int value;
    
    void add() {
        value++;
        System.out.println("숫자 증가 value= " + value);
    }
}
```

위 클래스는 데이터인 `value`와 기능인 `add()` 메서드를 함께 정의함.

참고: 여기서 만드는 `add()` 메서드에는 `static` 키워드를 사용하지 않는다.

메서드는 객체를 생성해야 호출 할 수 있다. 그런데 `static`이 붙으면 객체를 생성하지 않고도 메서드를 호출할 수 있다.

**ValueObjectMain**

```java
package oop1;

public class ValueObjectMain {

    public static void main(String[] args) {
        ValueData data = new ValueData();
        data.add();
        data.add();
        data.add();
        System.out.println("최종 숫자= " + data.value);
    }
}
```

**실행결과**

```java
숫자 증가 value= 1
숫자 증가 value= 2
숫자 증가 value= 3
최종 숫자= 3
```

**인스턴스 생성**

```java
ValueData data = new ValueData();
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/2241d1ab-4f5e-484c-a499-d9dc4f58f8d9/a456b185-8a21-4867-8556-80bffe4b6edc.png)

`valueObject` 라는 객체 생성. 이 객체는 멤버 변수 뿐만 아니라 내부에 기능을 수행하는 `add()` 메서드 존재

**인스턴스 메서드 호출**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/49b23e7a-6c4c-42a0-a72e-75d605326b5d/288be694-7410-468b-893d-4c1216c21d26.png)

인스턴스의 메서드를 호출하는 방법은 멤버 변수를 사용하는 방법과 동일.

`.` (dot)을 찍어서 객체에 접근한 다음에 원하는 메서드 호출.

```java
ValueObject.add();  //1
x002.add();  //2. x002 ValueObject 인스턴스에 있는 add() 메서드 호출
```

1. `add()` 메서드를 호출하면 메서드 내부에서 `value++`  호출.
   이때, `value` 에 접근해야 하는데, 기본적으로 본인 인스턴스에 있는 멤버변수에 접근.
   `x002` 참조값을 사용하므로 자기 자신인 `x002.value` 에 접근.
2. `++` 연산으로 `value` 의 값을 하나 증가.

**정리**

- 클래스 속성(데이터, 멤버 변수)과 기능(메서드)을 정의할 수 있다.
- 객체는 자신의 메서드를 통해 자신의 멤버 변수에 접근 가능.
    - 객체의 메서드 내부에서 접근하는 멤버 변수는 객체 자신의 멤버 변수이다.

## 객체 지향 프로그래밍

지금까지는 데이터와 기능이 분리된 음악 플레이어를 개발함. 이제 데이터와 기능을 하나로 묶어서 음악 플레이어라는 개념을 온전히 하나의 클래스에 담아볼 것임.

프로그램을 작성하는 절차도 중요하지만 지금은 음악 플레이어라는 개념을 객체로 온전히 만드는 것이 중요.

그러기 위해서는 프로그램 실행 순서 보다는 음악 플레이어 클래스를 만드는 것에 집중.

음악 플레이어가 어떤 속성(데이터)를 가지고 어떤 기능(메서드)을 제공하는지 이 부분에 초점.

지금부터 우리는 음악 플레이어를 개발하는 개발자가 될 것임. 이것을 어떻게 사용할지는 분리해서 생각.

음악 플레이어를 만들어서 제공하는 개발자와 음악 플레이어를 사용하는 개발자가 분리되어 있음.

### 객체란?

세상의 모든 사물을 단순하게 추상화해보면 속성(데이터)과 기능 딱 2가지로 설명가능.

**자동차**

- 속성: 차량 색상, 현재 속도
- 기능: 엑셀, 브레이크, 문 열기, 문 닫기

**동물**

- 속성: 색상, 키, 온도
- 기능: 먹는다, 걷는다

**게임 캐릭터**

- 속성: 레벨 경험치, 소유한 아이템들
- 기능: 이동, 공격, 아이템 획득

객체 지향 프로그래밍은 모든 사물의 속성과 기능을 가진 객체로 생각하는 것으로, 객체에는 속성과 기능만 존재.

이렇게 단순화 하면 세상에 있는 객체들을 컴퓨터 프로그램으로 쉽게 설계 가능.

이런 장점들 덕분에 지금은 객체 지향 프로그래밍이 가장 많이 사용됨.

**객체 지향의 특징**

- 속성과 기능 하나로 묶기
- 캡슐화
- 상속
- 다형성
- 추상화
- 메시지 전달

**음악 플레이어**

- 속성: `volum`, `isOn`
- 기능: `on()`, `off()`, `volumeUp()`, `volumeDown()`, `showStatus()`

### 객체 지향 음악 플레이어

```java
package oop1;

public class MusicPlayer {
    int volume;
    boolean isOn;

    void on() {
        System.out.println("음악 플레이어를 시작합니다.");
        isOn = true;
    }

    void off() {
        System.out.println("음악 플레이어를 종료합니다.");
        isOn = false;
    }

    void volumeUp() {
        volume++;
        System.out.println("음악 플레이어 볼륨: " + volume);
    }

    void volumeDown() {
        volume--;
        System.out.println("음악 플레이어 볼륨: " + volume);
    }

    void showStatus() {
        if (isOn == true) {
            System.out.println("음악 플레이어: ON, 볼륨: " + volume);
        } else {
            System.out.println("음악 플레이어: OFF");
        }
    }
}
```

MusicPlayer 클래스에 음악 플레이어에 필요한 속상과 기능 모두 정의.

이제 음악 플레이어가 필요한 곳에서 이 클래스만 있으면 온전한 음악 플레이어를 생성해 사용 가능.

음악 플레이어를 사용하는데 필요한 모든 속상과 기능이 하나의 클래스에 포함됨.

**MusicPlayerMain4()**

```java
package oop1;

public class MusicPlayerMain4 {
    public static void main(String[] args) {
        MusicPlayer player = new MusicPlayer();
        // 음악 플레이어 켜기
        player.on();

        // 볼륨증가
        player.volumeUp();

        // 볼륨증가
        player.volumeUp();

        // 볼륨감소
        player.volumeDown();

        // 시스템 확인
        player.showStatus();

        // 종료
        player.off();
    }
}
```

**실행결과**

```java
음악 플레이어를 시작합니다.
음악 플레이어 볼륨: 1
음악 플레이어 볼륨: 2
음악 플레이어 볼륨: 1
음악 플레이어: ON, 볼륨: 1
음악 플레이어를 종료합니다.
```

`MusicPlayer`를 사용하는 입장에서는 객체를 생성하고 기능(메서드)를 호출하기만 하면 된다.

필요한 모든 것은 `MusicPlayer`안에 들어있음.

**캡슐화**

`MusicPlayer` 를 보면 속성과 기능이 마치 하나의 캡슐에 쌓여있는 것 같다.

이렇게 속성과 기능을 하나로 묶어서 필요한 기능을 메서드를 통해 외부에 제공하는 것을 캡슐화라 함.

객체 지향 프로그래밍 덕분에 음악 플레이어 객체를 사용하는 입장에서 진짜 음악 플레이어를 만들고 사용하는 것처럼 친숙하게 느껴진다. 그래서 코드가 더 읽기 쉽고, 속성과 기능이 한 곳에 있기 때문에 변경도 쉽다.

예를 들어 `MusicPlayer` 내부 코드가 변하는 경우에 다른 코드는 변경하지 않아도 된다. `MusicPlayer`의 `volume`이라는 필드 이름이 다른 이름으로 변한다고 할 때 MusicPlayer 내부만 변경하면 된다.

또 음악 플레이어가 내부에서 출력하는 메세지를 변경할 때에도 `MusicPlayer` 내부만 변경하면 된다.

이 경우 `MusicPlayer`를 사용하는 개발자는 코드를 전혀 변경하지 않아도 됨.

물론. 메서드의 이름을 변경한다면 `MusicPlayer`를 사용하는 곳의 코드도 변경.

## 문제와 풀이

### 문제1 - 절차 지향 직사각형 프로그램을 객체 지향으로 변경하기

다음은 직사각형 넓이(Area), 둘레 길이(Perimeter), 정사각형 여부(square)를 구하는 프로그램

- 절차 지향 프로그래밍 방식으로 되어 있는 코드를 객체 지향 프로그래밍 방식으로 변경
- `Rectangle` 클래스 만들기
- `RectangleOopMain`에 해당 클래스를 사용하는 `main()` 코드 만들기

**Rectangle**

```java
package oop1.ex;

public class Rectangle {
    int width;
    int height;
    int area;
    int perimeter;
    boolean square;

    void calculateArea() {
        area = width * height;
        System.out.println("넓이: " + area);
    }

    void calculatePerimeter() {
        perimeter = 2 * (width + height);
        System.out.println("둘레 길이: " + perimeter);
    }

    void isSquare() {
        if (width == height) {
            square = true;
            System.out.println("정사각형 여부: " + square);
        } else {
            square = false;
            System.out.println("정사각형 여부: " + square);
        }
    }
}
```

**RectangleOopMain**

```java
package oop1.ex;

public class RectangleOopMain {
    public static void main(String[] args) {
        Rectangle rectangle = new Rectangle();
        rectangle.width = 5;
        rectangle.height = 8;

        rectangle.calculateArea();
        rectangle.calculatePerimeter();
        rectangle.isSquare();
    }
}
```

**실행결과**

```java
넓이: 40
둘레 길이: 26
정사각형 여부: false
```

### 문제2 - 객체 지향 계좌

은행 계좌를 객체지향으로 설계

- `Account` 클래스
    - `int balance`: 잔액
    - `deposit(int amount)`: 입금 메서드
        - 입금시 잔액 증가
    - `withdraw(int amount)`: 출금 메서드
        - 출금시 잔액 감소
        - 만약 잔액이 부족하다면 잔액 부족 출력
    - `AccountMain` 클래스를 만들고 `main()`메서드를 통해 프로그램 시작.
        - 계좌에 10000원 입금
        - 계좌에서 9000원 출금
        - 계좌에서 2000원 출금 시도 → `잔액 부족` 출력 확인
        - 잔고 출력 → `잔고: 1000`

**실행 결과**

```java
10000원을 입금하였습니다. 현재 잔액: 10000
9000원을 출금하였습니다. 현재 잔액: 1000
잔액 부족, 현재 잔액: 1000
현재 잔액: 1000
```

**Account**

```java
package oop1.ex;

public class Account {
    int balance;

    void deposit(int amount) {
        balance += amount;
        System.out.println(amount + "원을 입금하였습니다. 현재 잔액: " + balance);
    }

    void withdraw(int amount) {
        if ((balance - amount) > 0) {
            balance -= amount;
            System.out.println(amount + "원을 출금하였습니다. 현재 잔액: " + balance);
        } else {
            System.out.println("잔액 부족, 현재 잔액: " + balance);
        }
    }

    void showBalance() {
        System.out.println("현재 잔액: " + balance);
    }
}
```

**AccountMain**

```java
package oop1.ex;

public class AccountMain {
    public static void main(String[] args) {
        Account myAccount = new Account();
        myAccount.deposit(10000);
        myAccount.withdraw(9000);
        myAccount.withdraw(2000);
        myAccount.showBalance();
    }
}
```

**실행 결과**

```java
10000원을 입금하였습니다. 현재 잔액: 10000
        9000원을 출금하였습니다. 현재 잔액: 1000
        잔액 부족, 현재 잔액: 1000
현재 잔액: 1000
```