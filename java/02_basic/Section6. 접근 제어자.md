[<back](https://www.notion.so/basic-_-3ee7f95ac24d43f881b7ee2e8f21c210?pvs=21)

---

<aside>
📃 목차

</aside>

---

## 접근 제어자 이해1

java는 `public`, `private` 같은 접근 제어자(access modifier) 제공.

접근 제어자를 사용해 해당 클래스 외부에서 특정 필드나 메서드에 접근하는 것을 허용하거나 제한 가능.

**접근 제어자의 필요성**

스피커에 들어가는 소프트웨어 개발자라 가정.

스피커 음량은 절대 100을 넘으면 안된다는 요구사항 있음**(100을 넘어가면 스피커의 부품들이 고장남)**

스피커 객체를 만들어보자.

스피커는 음량을 높이고, 내리고, 현재 음량을 확인할 수 있는 단순한 기능 제공

요구사항 대로 스피커 음량은 100까지만 증가할 수 있다.

**Speaker**

```java
package access;

public class Speaker {
    int volume;

    Speaker(int volume) {
        this.volume = volume;
    }

    void volumeUp() {
        if (volume >= 100) {
            System.out.println("더 이상 볼률을 올릴 수 없습니다. 최대 음량입니다.");
        } else {
            volume += 10;
            System.out.println("음략을 10 증가합니다. 현재음량: " + volume);
        }
    }

    void volumeDown() {
        if (volume <= 0) {
            System.out.println("더 이상 볼륨을 줄일 수 없습니다. 최소 음량입니다.");
        } else {
            volume -= 10;
            System.out.println("음략을 10 감소합니다. 현재음량: " + volume);
        }
    }

    void showVolume() {
        System.out.println("현재음량: " + volume);
    }
}
```

**SpeakerMain**

```java
package access;

public class SpeakerMain {
    public static void main(String[] args) {
        Speaker speaker = new Speaker(90);
        speaker.showVolume();

        speaker.volumeUp();
        speaker.showVolume();

        speaker.volumeUp();
        speaker.showVolume();

        speaker.volumeUp();
        speaker.volumeUp();
    }
}
```

**실행 결과**

```java
현재음량: 90
음략을 10 증가합니다. 현재음량: 100
현재음량: 100
더 이상 볼률을 올릴 수 없습니다. 최대 음량입니다.
현재음량: 100
더 이상 볼률을 올릴 수 없습니다. 최대 음량입니다.
더 이상 볼률을 올릴 수 없습니다. 최대 음량입니다.
```

기대한대로 음량은 100을 넘지 않았다. 프로젝트는 성공적으로 끝남.

시간이 흘러 업그레이드 된 다음 버전의 스피커 출시.
이 떄 새로운 개발자가 급하게 기존코드를 이어받아 개발하게 됨.
새로운 개발자는 기존 요구사항을 잘 모르고 코드를 실행해보니 이상하게 음량이 100이상 올라가지 않았다.
소리를 더 올리면 좋겠다고 생각한 개발자는 `Speaker`클래스의 `volume` 필드를 직접 사용할 수 있었으므로 필드의 값을 `200`으로 설정하고 실행하는 순간 스피커가 폭발해버림.

**SpeakerMain**

```java
package access;

public class SpeakerMain {
    public static void main(String[] args) {
        Speaker speaker = new Speaker(90);
        speaker.showVolume();

        speaker.volumeUp();
        speaker.showVolume();

        speaker.volumeUp();
        speaker.showVolume();

        speaker.volumeUp();
        speaker.volumeUp();

        //필드에 직접 접근
        System.out.println("volume필드에 직접 접근");
        speaker.volume = 200;
        System.out.println(speaker.volume);
    }
}
```

**실행 결과**

```java
현재음량: 90
음략을 10 증가합니다. 현재음량: 100
현재음량: 100
더 이상 볼률을 올릴 수 없습니다. 최대 음량입니다.
현재음량: 100
더 이상 볼률을 올릴 수 없습니다. 최대 음량입니다.
더 이상 볼률을 올릴 수 없습니다. 최대 음량입니다.
volume필드에 직접 접근
200
```

**volume필드**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f7e3585d-80e0-49ab-911f-79d24b960d50/572b35fd-b4ed-4c40-aa71-cace973fc334.png)

`Speaker` 객체를 사용하는 사용자는 `Speaker`의 `volume` 필드와 메서드에 모두 접근 가능.

메서드에서는 제약을 뒀지만 필드에 직접 접근하게 되면 `volume`에 원하는 값 설정가능

이런 문제를 근본적으로 해결하기 위해 `volume` 필드의 외부 접근을 막을 방법 필요

## 접근 제어자 이해2

이 문제의 근본적 해결 방법은 `volume` 필드를 `Speaker` 클래스 외부에서 접근하지 못하게 막는 것임.

**Speaker - volume 접근 제어자를 private로 수정**

```java
package access;

public class Speaker {
    private int volume;
    ...
}
```

`private` 접근 제어자는 모든 외부 호출을 막는다.
따라서 `private`이 붙은 경우 해당 클래스 내부에서만 호출 가능.

**SpeakerMain**

```java
package access;

public class SpeakerMain {
    public static void main(String[] args) {
        Speaker speaker = new Speaker(90);
        ...
        //필드에 직접 접근
        System.out.println("volume필드에 직접 접근");
        speaker.volume = 200;
        System.out.println(speaker.volume);
    }
}
```

**실행 결과**

```java
java: volume has private access in access.Speaker
```

volume필드 - private 변경 후

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/1e1d464c-08b6-41d2-b713-44c250b565a3/b2b981f6-19ca-4abf-a55c-c862460eeee5.png)

`volume` 필드를 `private`을 사용해서 `Speaker` 내부에 숨김.

외부에서 `volume` 필드에 직접 접근할 수 없게 막음. `volume`필드는 이제 `Speaker` 내부에서만 접근 가능.

> 참고: 좋은 프로그램은 무한한 자유도가 주어지는 프로램이 아니라 적절한 제약을 제공하는 프로그램이다.
>

## 접근 제어자의 종류

java는 4가지 종류의 접근 제어자 제공.

**접근 제어자의 종류**

- `private` : 모든 외부 호출을 막는다.
- `default` (package-private): 가은 패키지안에서 호출은 허용.
- `protected` : 같은 패키지안에서 호출은 허용. 패키지가 달라도 상속 관계 호출 허용
- `public` : 모든 외부 호출 허용

순서대로 `private`이 가장 많이 차단 `public`이 가장 많이 허용.

`private` → `default` → `protected` → `public`

**package-private(default)**

접근 제어자르 명시하지 않으면 package-private(default) 접근 제어자 적용.

해당 접근자를 사용하는 멤버는 동일한 패키지 내의 다른 클래스에서만 접근 가능.

접근 제어자 예시

```java
package access;

public class Speaker {
    private int volume;

    Speaker(int volume) {}  //생성자

    void volumeUp() {}  //메서드
    void volumeDown() {}
    void showVolume() {}
```

**접근 제어자의 핵심은 속성과 기능을 외부로부터 숨기는 것.**

- `private`은 나의 클래스 안으로 속성과 기능을 숨길 때 사용. 외부 클래스에서 해당 기능 호출 불가
- `default`는 나의 패키지 안으로 속성과 기능을 숨길 때 사용. 외부 패키지에서 해당 기능 호출 불가
- `protected`는 상속 관계로 속성과 기능을 숨길 때 사용. 상속 관계가 아닌 곳에서 해당기능 호출 불가
- `public`은 기능을 숨기지 않고 어디서든 호출 가능

## 접근 제어자 사용 - 필드, 메서드

**주의! 지금부터는 패키지 위치가 매우 중요.**

### 필드, 메서드 레벨의 접근 제어자

**AccessData**

```java
package access.a;

public class AccessData {
    public int publicField;
    int defaultField;
    private int privateField;

    public void publicMethod() {
        System.out.println("publicMethod 호출 " + publicField);
    }

    void defaultMethod() {
        System.out.println("defaultMethod 호출 " + defaultField);
    }

    private void privateMethod() {
        System.out.println("privateMethod 호출 " + privateField);
    }

    public void innerAccess() {
        System.out.println("내부 호출");
        publicField = 100;
        defaultField = 200;
        privateField = 300;
        publicMethod();
        defaultMethod();
        privateMethod();
    }
}
```

- 패키지 위치는 `package access.a`이다.
- 순서대로 `public`, `default`, `private`를 필드와 메서드에 사용
- `innerAccess()` 메서드는 내부 호출을 보여주며, `private`를 포함한 모든 필드, 메서드에 접근 가능

이제 외부에서 이 클래스에 접근해보자

**AccessInnerMain**

```java
package access.a;

public class AccessInnerMain {
    public static void main(String[] args) {
        AccessData data = new AccessData();

        //public 호출 가능
        data.publicField = 1;
        data.publicMethod();

        //같은 패키지 default 호출 가능
        data.defaultField = 2;
        data.defaultMethod();

        //private 호출 불가
        //data.privateField = 3;
        //data.privateMethod();
        
        data.innerAccess();
    }
}
```

**실행결과**

```java
publicMethod 호출 1
defaultMethod 호출 2
내부 호출
publicMethod 호출 100
defaultMethod 호출 200
privateMethod 호출 300
```

**AccessOuter**

```java
package access.b;

import access.a.AccessData;

public class AccessOuterMain {

    public static void main(String[] args) {
        AccessData data = new AccessData();

        //public 사용 가능
        data.publicField = 1;
        data.publicMethod();

        //같은 패키지가 아니므로 default 사용 불가
        //data.defaultField = 2;
        //data.defaultMethod();

        //private 사용 불가
        //data.privateField = 3;
        //data.privateMethod();

        //InnerAccess 사용 가능
        data.innerAccess();
    }
}
```

실행 결과

```java
publicMethod 호출 1
내부 호출
publicMethod 호출 100
defaultMethod 호출 200
privateMethod 호출 300
```

## 접근 제어자 사용 - 클래스 레벨

**클래스 레벨의 접근 제어자 규칙**

- 클래스 레벨의 접근 제어자는 `public`, `default`만 사용할 수 있다.
    - `private`, `protected` 는 사용할 수 없다.
- `public` 클래스는 파일명과 클래스명이 같아야한다.
    - 하나의 자바 파일에 `public` 클래스는 하나만 등장할 수 있다.
    - 하나의 자바 파일에 `default` 접근 제어자를 사용하는 클래스는 무한정 만들 수 있다.

**PublicClass.java 파일**

```java
package access.a;

public class PublicClass {
    public static void main(String[] args) {
        PublicClass publicclass = new PublicClass();
        DefaultClass1 defaultClass1 = new DefaultClass1();
        DefaultClass2 defaultClass2 = new DefaultClass2();
    }
}
    class DefaultClass1 {
    }

    class DefaultClass2 {
    }
```

- DefaultClass1, DefaultClass2는 default 접근 제어자로 패키지 내부에서만 접근 가능.

**PublicClassInnerMain**

```java
package access.a;

public class PublicClassInnerMain {
    public static void main(String[] args) {
        //public 클래스 호출
        PublicClass publicClass = new PublicClass();
        DefaultClass1 defaultClass1 = new DefaultClass1();
        DefaultClass2 defaultClass2 = new DefaultClass2();
    }
}
```

**PublicClassOuterMain**

```java
package access.b;

import access.a.PublicClass;

public class AccessClassOuterMain {
    public static void main(String[] args) {

        //public 호출
        PublicClass publicClass = new PublicClass();

        //default 클래스 호출 -> 같은 패키지가 아니므로 호출 안됨
        //DefaultClass1 defaultClass = new DefaultClass1;
        //DefaultClass2 defaultClass = new DefaultClass2;
    }
}
```

## 캡슐화

캡슐화(Encapsulation)는 객체지향 프로그래밍 중 중요한 개념 중 하나. 캡슐화는 데이터와 해당데이터에 접근하는 메서드를 하나로 묶어서 외부에서 접근을 제한하는 것을 말함.

캡슐화를 통해 데이터의 직접적인 변경을 방지할 수 있다.
쉽게 얘기해서 속성과 기능을 하나로 묶고 외부에 꼭 필요한 기능만 노출하고 나머지는 내부로 숨기는 것.

이전에는 객체지향 프로그래밍을 설명하면서 캡슐화에 대해 알아 보았다.
이 때는 데이터와 데이터를 처리하는 메서드들을 하나로 모으는데 초점을 맞추었다면 이제는 접근 제어자를 통해 캡슐화를 좀 더 업그레이드 시켜보자.

1. **데이터(속성)를 숨겨라**

   객체에는 속성(데이터), 기능(메서드)이 있다. 캡슐화에서 가장 필수로 숨겨야 하는 것은 속성(데이터)이다.

   **객체의 데이터는 객체가 제공하는 기능인 메서드를 통해 접근해야한다.**

2. **기능(메서드)을 숨겨라**

   객체의 기능 중에서 외부에서 사용하지 않고 내부에서만 사용하는 기능들이 있다. 이런 기능도 모두 감추는 것이 좋다.

   사용자 입장에서 꼭 필요한 기능만 외부에 노출하자. 나머지 기능은 모두 내부로 숨기자.


**정리하자면 데이터는 모두 숨기고, 기능은 꼭 필요한 것만 노출하는 것이 잘만든 캡슐화**

**BankAccount**

```java
package access;

public class BankAccount {
    private int balance;

    public BankAccount() {
        balance = 0;
    }

    public void deposit(int amount) {
        if (isAmountValid(amount)) {
            balance += amount;
        } else {
            System.out.println("유효하지 않은 금액입니다.");
        }
    }

    public void withDraw(int amount) {
        if (isAmountValid(amount) && balance - amount > 0) {
            balance -= amount;
        } else {
            System.out.println("유효하지 않은 금액이거나 잔액이 부족합니다.");
        }
    }

    public int getBalance() {
        return balance;
    }

    private boolean isAmountValid(int amount) {
        // 금액이 0보다 커야함
        return amount > 0;
    }
}
```

**BankAccountMain**

```java
package access;

public class BankAcountMain {

    public static void main(String[] args) {
        BankAccount account = new BankAccount();
        account.deposit(10000);
        System.out.println(account.getBalance());
        account.withDraw(3000);
        System.out.println(account.getBalance());
        account.deposit(-1000);
        System.out.println(account.getBalance());
        account.withDraw(20000);
        System.out.println(account.getBalance());
    }
}
```

**실행결과**

```java
10000
7000
유효하지 않은 금액입니다.
7000
유효하지 않은 금액이거나 잔액이 부족합니다.
7000
```

은행 계좌는 다음과 같은 기능을 가지고 있다.

**private**

- `balance`: 데이터 필드는 외부에 직접 노출하지 않는다. `BankAccount`가 제공하는 메서드를 통해서만 접근 가능
- `isAmountValid()` : 입력 금액을 검증하는 기능은 내부에서만 필요한 기능. 따라서 `private` 사용

**public**

- `deposit()`: 입금
- `withdraw()`: 출금
- `getBalance()`: 잔고

`BankAccount`를 사용하는 입장에서 단 3가지 메서드만 알면 된다. 나머지 복잡한 내용은 모두 `BankAccount`안에 숨어있다.

`isAmountValid()`를 외부에 노출하면 사용하는 개발자 입장에서는 입금, 출금 전에 해당 메서드를 통해 검증을 하고 호출해야하는지 고민하게 됨.

`BankAccount` field를 노출하게 되면 캡슐화가 깨지고 잔고를 무한정 늘리고 출금하는 심각한 문제가 발생할 수 있다.

따라서, 외부에 노출하지 않는 `private` 속성과 기능은 사용하는 개발자가 신경쓰지 않아도 되는 것이고 그 것으로 고민하지 않아도 된다는 뜻도 하므로, 캡슐화가 잘된 프로그래밍은 적절한 제약을 준다.

## 문제와 풀이

### 문제 - 최대 카운터와 캡슐화

`MaxCounter` 클래스만들기

최대값을 지정하고 최대값까지 증가하는 기능 제공

- `int count`: 내부에서 사용하는 숫자. 초기값 0
- `int max`: 최대값. 생성자 통해 입력
- `increment`: 숫자 하나 증가
- `getCount`: 지금까지 증가한 값 반환

**요구사항**

- **접근 제어자를 사용해 데이터 캡슐화**
- **해당 클래스는 다른 패키지에서도 사용 가능**

**MaxCounter**

```java
package access.ex;

public class MaxCounter {
    private int count;
    private int max;

    public MaxCounter(int max) {
        this.max = max;
    }

    public void increment() {
        //검증 로직
        if (count >= max) {
            System.out.println("최대값을 초과하였습니다. 현재 count: " + getCount());
            return;
        }
        //실행 로직
        count++;
    }

    public int getCount() {
        return count;
    }
}
```

**CounterMain**

```java
package access.ex;

public class CounterMain {
    public static void main(String[] args) {

        MaxCounter counter = new MaxCounter(3);
        counter.increment();  //1
        counter.increment();  //2
        counter.increment();  //3
        counter.increment();  //error
        int count = counter.getCount();
        System.out.println(count);
    }
}
```

**실행결과**

```java
최대값을 초과하였습니다. 현재 count: 3
3
```

### 문제 - 쇼핑카트

ShoppingCartMain 코드가 작동하도록 Item, ShoppingCart 클래스 완성

요구사항

- 접근 제어자 사용. 데이터 캡슐화
- 다른 패키지에서 사용가능
- 장바구니에 상품 최대 10개 담기 가능
    - 10개 초과 등록 시: “장바구니가 가득 찼습니다.” 출력

**Item_1(나의 코드)**

```java
package access.ex;

public class Item_1 {
    private String name;
    private int price;
    private int quantity;

    public Item_1(String name, int price, int quantity) {
        this.name = name;
        this.price = price;
        this.quantity = quantity;
    }

    public int printItem(Item_1 item) {
        int total_price = item.price * item.quantity;
        System.out.println("상품명: " + item.name + ", 합계: " + total_price);
        return total_price;
    }
}
```

**ShoppingCart_1(나의 코드)**

```java
package access.ex;

public class ShoppingCart_1 {
    private Item_1[] items = new Item_1[10];
    private int itemCount;

    public ShoppingCart_1() {
    }

    public void addItem(Item_1 item) {
        //로직 검증
        if (itemCount >= items.length) {
            System.out.println("장바구니가 가득 찼습니다.");
            return;
        }
        items[itemCount] = item;
        itemCount++;
    }

    public void displayItem() {
        System.out.println("장바구니 상품 출력");
        int total_price = 0;
        for (int i = 0; i < itemCount; i++) {
            Item_1 item = items[i];
            total_price += item.printItem(item);
        }
        System.out.println("총 상품 가격: " + total_price);
    }
}
```

**ShoppingCartMain_1(나의 코드)**

```java
package access.ex;

public class ShoppingCartMain_1 {

    public static void main(String[] args) {
        ShoppingCart_1 cart = new ShoppingCart_1();

        Item_1 item1 = new Item_1("마늘", 2000, 2);
        Item_1 item2 = new Item_1("상추", 3000, 4);

        cart.addItem(item1);
        cart.addItem(item2);

        cart.displayItem();
    }
}
```

**Item_2(제시된 코드)**

```java
package access.ex;

public class Item_2 {
    private String name;
    private int price;
    private int quantity;

    public Item_2(String name, int price, int quantity) {
        this.name = name;
        this.price = price;
        this.quantity = quantity;
    }

    public String getName() {
        return name;
    }

    public int getTotalPrice() {
        return price * quantity;
    }
}
```

**ShoppingCart_2(제시된 코드)**

```java
package access.ex;

public class ShoppingCart_2 {
    private Item_2[] items = new Item_2[10];
    private int itemCount;

    public ShoppingCart_2() {
    }

    public void addItem(Item_2 item) {
        //로직 검증
        if (itemCount >= items.length) {
            System.out.println("장바구니가 가득 찼습니다.");
            return;
        }
        items[itemCount] = item;
        itemCount++;
    }

    public void displayItem() {
        System.out.println("장바구니 상품 출력");
        for (int i = 0; i < itemCount; i++) {
            Item_2 item = items[i];
            System.out.println("상품명: " + item.getName() +", 상품가격: " + item.getTotalPrice());
        }
        System.out.println("총 상품 가격: " + calculateTotalPrice());
    }

    private int calculateTotalPrice() {
        int total_price = 0;
        for (int i = 0; i < itemCount; i++) {
            Item_2 item = items[i];
            total_price += item.getTotalPrice();
        }
        return total_price;
    }
}
```

**ShoppingCartMain_2(제시된 코드)**

```java
package access.ex;

public class ShoppingCartMain_2 {

    public static void main(String[] args) {
        ShoppingCart_2 cart = new ShoppingCart_2();

        Item_2 item1 = new Item_2("마늘", 2000, 2);
        Item_2 item2 = new Item_2("상추", 3000, 4);

        cart.addItem(item1);
        cart.addItem(item2);

        cart.displayItem();
    }
}
```

**실행 결과**

```java
장바구니 상품 출력
상품명: 마늘, 상품가격: 4000
상품명: 상추, 상품가격: 12000
총 상품 가격: 16000
```