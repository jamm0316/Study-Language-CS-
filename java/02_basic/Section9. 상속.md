[<back](https://www.notion.so/basic-_-3ee7f95ac24d43f881b7ee2e8f21c210?pvs=21)

---

<aside>
📃 목차

</aside>

---

## 상속 - 시작

### 예제 코드

패키지 위치에 주의

**ElectricCar**

```java
package extends1.ex;

public class ElectricCar {

    public void move() {
        System.out.println("차를 이동합니다.");
    }

    public void charge() {
        System.out.println("차를 충전합니다.");
    }
}
```

**GasCar**

```java
package extends1.ex;

public class GasCar {

    public void move() {
        System.out.println("차를 이동합니다.");
    }

    public void fillUp() {
        System.out.println("기름을 주유하합니다.");
    }
}
```

**CarMain**

```java
package extends1.ex;

public class CarMain {
    public static void main(String[] args) {
        ElectricCar electricCar = new ElectricCar();
        electricCar.move();
        electricCar.charge();

        GasCar gasCar = new GasCar();
        gasCar.move();
        gasCar.fillUp();
    }
}
```

**실행결과**

```java
차를 이동합니다.
차를 충전합니다.
차를 이동합니다.
기름을 주유하합니다.
```

전기차(`ElectricCar`)와 가솔린차(`GasCar`)를 만들었다. 전기차는 이동(`move()`), 충전(`charge()`)기능이 있고, 가솔린 차는 이동(`move()`), 주유(`fillup()`)기능이 있다.

전기차와 가솔린차는 자동차(`Car`)의 더 구체적인 개념이다.

이 둘의 공통 기능은 이동(`move()` )이고, 주유하는 방식만 다르다.

이런 경우 상속관계를 사용하는 것이 효과적이다.

## 상속 관계

상속은 객체 지향 프로그래밍의 핵심 요소.

기존 클래스의 필드와 메서드를 새로운 클래스에서 재사용하게 해준다.

이름 그대로 기존 클래스의 속성과 기능을 그대로 물려받는 것.

상속을 사용하려면 `extends` 키워드 사용. 그리고 `extends`대상은 하나만 선택 가능

**용어정리**

- **부모 클래스 (슈퍼 클래스)**: 상속을 통해 자신의 필드와 메서드를 다른 클래스에 제공하는 클래스
- **자식 클래스 (서브 클래스)**: 부모 클래스로부터 필드와 메서드를 상속받는 클래스

**주의!**

**기존 코드 유지를 위해 새로운 패키지에 기존 코드를 옮겨가며 코드작성할 것임.**

**클래스의 이름이 같기 때문에 패키지 명과 import 사용에 주의**

**Car**

```java
package extends1.ex2;

public class Car {

    public void move() {
        System.out.println("차를 이동합니다.");
    }
}
```

**ElectricCar**

```java
package extends1.ex2;

public class ElectricCar extends Car {
    public void charge() {
        System.out.println("차를 충전합니다.");
    }
}
```

**GasCar**

```java
package extends1.ex2;

public class GasCar extends Car {

    public void fillUp() {
        System.out.println("기름을 주유합니다.");
    }
}
```

**CarMain**

```java
package extends1.ex2;

public class CarMain {

    public static void main(String[] args) {
        ElectricCar electricCar = new ElectricCar();
        electricCar.move();
        electricCar.charge();

        GasCar gasCar = new GasCar();
        gasCar.move();
        gasCar.fillUp();
    }
}
```

**실행결과**

```java
차를 이동합니다.
차를 충전합니다.
차를 이동합니다.
기름을 주유합니다.
```

**상속 구조도**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/04a3c359-35b8-4d1f-b5a3-40a490293f4b/98202ad8-67ed-4f40-8b61-707ff60807df.png)

전기차와 가솔린차가 `Car`를 상속 받은 덕분에 `electicCar.move()`, `gasCar.move()`를 사용할 수 있다.

참고, 자식 클래스는 부모기능을 물려받아 사용할 수 있지만 반대는 불가하다.

### 단일 상속

참고로 자바는 다중상속을 지원하지 않는다. 그래서 extend 대상은 하나만 선택할 수 있다. 부모를 하나만 선택할 수 있다는 뜻이다. 물론 부모 부모가 또다른 부모를 하나 가지는 것은 괜찮다

**다중 상속 그림**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/3f539415-be1a-438c-8c51-ed244980a5a0/cbb2842b-d361-4085-b19d-5baf4a6882de.png)

만약 비행기와 자동차를 상속받아 하늘을 나는 자동차를 만든다고 가정해보자. 만약 그림과 같이 다중 상속을 사용하게 되면 `AirplaneCar` 입장에서 `move()`를 호출할 때 어떤 부모의 `move()`를 사용해야할지 문제가 생긴다.

이것을 다이아몬드 문제라고 한다. 그리고 다중 상속을 사용하면 클래스 계층 구조가 매우 복잡해질 수 있으므로, 자바는 다중 상속을 허용하지 않는다. 대신 이후에 인터페이스의 다중 구현을 허용해서 이런 문제를 피할 수 있다.

## 상속과 메모리 구조

**이 부분을 제대로 이해하는 것이 정말 중요**

상속 관계를 객체로 생성할 때 메모리 구조를 확인해보자.

```java
ElectriCar electircCar = new ElectricCar();
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/60637a54-0a3c-430e-ad7e-93435881ed54/27d52826-10d5-431b-a0a0-7be726fbe84a.png)

new ElectricCar()를 호출하면 ElectricCar 뿐만 아니라 상속 관계에 있는 Car 까지 함꼐 포함해서 인스턴스 생성. 참조값은 x001로 하나지만 실제로 그 안에서는 Car, ElectricCar라는 두가지 클래스 정보가 공존.

상속이라고 해서 단순하게 부모의 필드와 메서드만 물려받는게 아니다. 상속 관계를 사용하면 부모 클래스도 함께 포함해서 생성됨.

외부에서 볼 때는 하나의 인스턴스를 생성하는 것 같지만 내부에서는 부모와 자식이 모두 생성되고 공간도 구분된다.

`electricCar.charge()` 호출

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9441cdbb-bbf0-43b7-9d62-7e56ff359659/ba08a3bf-62ae-4f22-90e9-9e4ddd222a84.png)

`electricCar.charge()`를 호출하면 참조값을 확인해서 `x001.charge()`를 호출한다. 따라서 `x001`을 찾아서 `charge()`를 호출하면 되는 것.

그런데 상속 관계의 경우 내부에서 부모와 자식 모두 존재. 이 때 부모인 `Car`를 통해서 `charge()`를 호출해야할지 `EelctricCar`를 통해서 `charge()`를 찾을지 선택해야한다.

이때는 **호출하는 변수의 타입(클래스)을 기준으로 선택** 한다. `electricCar` 변수 탑입이 `ElectricCar`이므로 인스턴스 내부에 같은 타입인 `ElectricCar`를 통해서 `charge()`를 호출한다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/49534a76-8778-4873-9693-4877e5ac3ff4/dd7db9f0-051b-42a4-91bd-ffce1e9cb76a.png)

`electricCar.move()` 호출

`electricCar.move()`를 호출하면 먼저 `x001` 참조로 이동한다. 내부에는 `Car`, `ElectricCar` 두가지 타입이 있다. 이때 호출하는 변수인 `electricCar`의 타입이 `ElectircCar`이므로 이 타입을 선택.

`ElectricCar`에 `move()` 메서드가 없다. 따라서 상속관계에서는 자식 타입에 해당 기능이 없으면 부모타입으로 올라가서 찾는다. `Car` 타입에는 `move()`가 있으므로 `Car`타입의 `move()`를 호출한다.

**지금까지 설명한 상속과 메모리 구조는 반드시 이해해야한다.**

- 상속 관계의 객체를 생성하면, 그 내부에는 부모와 자식 모두 생성됨
- 상속 관계의 객체를 호출할 때, 대상 타입을 정해야한다. 이 때 호출자의 타입을 통해 대상 타입을 찾는다.
- 현재 타입에서 기능을 찾지 못하면 상위 부모 타입으로 기능을 찾아서 실행한다. 기능을 찾지 못하면 컴파일 오류가 발생

## 상속과 기능 추가

상속 관계의 장점을 알아보기 위해, 상속 관계에 다음 기능 추가

- 모든 차량에 문열기(openDoor()) 기능을 추가해야 한다.
- 새로운 수소차(HydrogenCar)를 추가해야한다.
  - 수소차는 fillHydrogen() 기능을 통해 수소를 충전할 수 있다.

**Car**

```java
package extends1.ex3;

public class Car {

    public void move() {
        System.out.println("차를 이동합니다.");
    }
    public void openDoor() {
        System.out.println("문을 엽니다.");
    }
}
```

**HydrogenCar**

```java
package extends1.ex3;

public class HydogenCar extends Car {

    public void fillHydrogen() {
        System.out.println("수소를 충전합니다.");
    }
}
```

**CarMain**

```java
package extends1.ex3;

public class CarMain {

    public static void main(String[] args) {
        ElectricCar electricCar = new ElectricCar();
        electricCar.move();
        electricCar.charge();
        electricCar.openDoor();

        GasCar gasCar = new GasCar();
        gasCar.move();
        gasCar.fillUp();
        gasCar.openDoor();

        HydogenCar hydogenCar = new HydogenCar();
        hydogenCar.move();
        hydogenCar.fillHydrogen();
        hydogenCar.openDoor();
    }
}
```

**실행결과**

```java
차를 이동합니다.
차를 충전합니다.
문을 엽니다.
차를 이동합니다.
기름을 주유합니다.
문을 엽니다.
차를 이동합니다.
수소를 충전합니다.
문을 엽니다.
```

**기능 추가와 클래스 확장**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/851169dc-6177-4aa7-ae3b-6c0ee0e8547e/78a24772-81bc-4a5c-88dd-c92465a95d32.png)

상속 관계 덕분에 중복은 줄어들고, 새로운 수소차를 편리하게 확장(extend)함.

## 상속과 메서드 오버라이딩

부모 타입의 기능을 자식에서느 다르게 재정의 하고 싶을 수 있다.

예를 들어 자동차의 경우 `Car.move()`라는 기능이 있다. 이 기능을 사용하면 단순히 “차를 이동합니다.”라고 출력한다. 그러나 전기차의 경우 보통 더 빠르기 때문에 `move()`를 호출하면 “전기차를 빠르게 이동합니다.”라고 출력을 변경하고 싶다.

이렇게 부모에게 상속받은 기능을 자식이 재정의 하는 것을 **메서드 오버라이딩(Overriding)**이라고 한다.

**Car**

```java
package extends1.oevrriding;

public class Car {

    public void move() {
        System.out.println("차를 이동합니다.");
    }
    public void openDoor() {
        System.out.println("문을 엽니다.");
    }
}
```

**ElectricCar**

```java
package extends1.oevrriding;

public class ElectricCar extends Car {

    @Override  //옵션
    public void move() {
        System.out.println("전기차를 빠르게 이동합니다.");
    }

    public void charge() {
        System.out.println("차를 충전합니다.");
    }
}
```

**CarMain**

```java
package extends1.oevrriding;

public class CarMain {

    public static void main(String[] args) {
        ElectricCar electricCar = new ElectricCar();
        electricCar.move();

        GasCar gasCar = new GasCar();
        gasCar.move();

        HydogenCar hydogenCar = new HydogenCar();
        hydogenCar.move();
    }
}
```

**실행결과**

```java
전기차를 빠르게 이동합니다.
차를 이동합니다.
차를 이동합니다.
```

`ElectricCar`는 부모인 `Car`의 `move()` 기능을 그대로 사용하고 싶지 않아 메서드 이름은 같지만 새로운 기능으로 만들었다. 이렇게 부모의 기능을 자식이 새로 재정의 하는것을 메서드 오버라이딩이라고 한다.

이제 `ElectricCar`의 `move()`를 호출하면 `Car`의 `move()`가 아닌 `ElectricCar`의 `move()`가 호출됨.

**@Override**

`@` 가 붙은 부분을 애노테이션이라한다. 이는 주석과 비슷한데, 프로그램이 읽을 수 있는 특별한 주석.

`@Override` 애노테이션은 상위 클래스의 메서드를 오버라이드 하는 것을 나타낸다.

컴파일러는 이 애노테이션을 보고 메서드가 정확히 오버라이드 되었는지 확인한다. 오버라이딩 조건을 만족시키지 않으면 컴파일 에러를 발생시킴. 따라서 실수로 오버라이딩 못하는 경우를 방지해준다.

필수는 아니지만 코드의 명확성을 위해 붙혀주자.

**ElectircCar**

```java
package extends1.oevrriding;

public class ElectricCar extends Car {

    //@Override
    public void move() {
        System.out.println("전기차를 빠르게 이동합니다.");
    }

    public void charge() {
        System.out.println("차를 충전합니다.");
    }
}
```

**CarMain**

```java
package extends1.oevrriding;

public class CarMain {

    public static void main(String[] args) {
        ElectricCar electricCar = new ElectricCar();
        electricCar.move();

        GasCar gasCar = new GasCar();
        gasCar.move();

        HydogenCar hydogenCar = new HydogenCar();
        hydogenCar.move();
    }
}
```

**실행결과**

```java
전기차를 빠르게 이동합니다.
차를 이동합니다.
차를 이동합니다.
```

**ElectircCar**

```java
package extends1.oevrriding;

public class ElectricCar extends Car {

    //@Override
    public void moveee() {  //move -> moveee 오타
        System.out.println("전기차를 빠르게 이동합니다.");
    }

    public void charge() {
        System.out.println("차를 충전합니다.");
    }
}
```

**실행결과**

```java
java: method does not override or implement a method from a supertype
```

오버라이드가 제대로 되지 않았다는 컴파일 오류가 뜸

실행결과를 보면 `electricCar.move()`를 호출했을 때, 오버라이딩한 `ElectricCar.move()` 메서드가 실행된 것임을 알 수 있다.

**오버라이딩과 클래스**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/c31cca1f-9744-41a0-b22e-d2a46fc222fb/c472e724-a04b-4f9e-a633-bcf44923dcc9.png)

Car의 move() 메서드를 ElectricCar에서 오버라이딩 함

**오버라이딩과 메모리 구조**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9ffe77cd-eabd-4461-a5a1-d8fd3efc0127/5ac50838-1b06-4003-aafd-565ae8b8ef9c.png)

1. `electricCar.move()`를 호출
2. 호출한 `electircCar`의 타입은 `ElectricCar`이다. 따라서 인스턴스 내부 `ElectricCar` 타입에서 시작.
3. `ElectricCar` 타입에 `move()`메서드가 있으므로, 해당 메서드를 실행하며, 부모 타입은 찾지 않는다.

**오버로딩(Overloading)과 오버라이딩(Overriding)**

- **메서드 오버로딩**: 메서드 이름은 같고 파라미터(매개변수)가 다른 메서드를 여러개 정의하는 것.
  번역하면 과적인데, 과하게 물건을 담았다는 뜻. 따라서 같은 이름의 메서드를 여러개 정의 했다고 이해.
- **메서드 오버라이딩**: 하위 클래스에서 사우이 클래스의 메서드를 재정의하는 과정. 따라서 상속 관계에서 사용하며, 부모의 기능을 자식이 다시 정의하는 것.
  번역하면 재정의. 상속 관계에서 기존의 기능을 다시 정의한다고 이해.

### 메서드 오버라이딩 조건

**부모 메서드와 같은 메서드를 오버라이딩 할 수 있다.**

**메서드 오버라이딩 조건**

- **메서드 이름**: 메서드 이름이 같아야한다.
- **메서드 매개변수(파라미터)**: 매개변수(파라미터) 타입, 순서, 개수가 같아야한다.
- **반환 타입**: 반환 타입이 같아야한다. 단, 반환 타입이 하위 클래스 타입일 수 있다.
- **접근 제어자**: 오버라이딩 메서드의 접근 제어자는 상위 클래스의 메서드보다 더 제한적이어서는 안된다.
  예를 들어, 상위 클래스의 메서드가 `protected` 로 선언되어있으면 하위 클래스에서 이름 `public` 또는 `protected`로 오버라이드 할 수 있지만, `private`, `default`로 오버라이드 할 수 없다.
- **예외**: 오버라이딩 메서드는 상위 클래스의 메서드보다 더 많은 체크 예외를 `throw`로 선언할 수 없다.
  하지만, 더 적거나 같은 수의 예외, 또는 하위 타입의 예외는 선언가능.
- `static`, `final`, `private`: 키워드가 붙은 메서드는 오버라이딩 불가
  - `static`은 클래스 레벨에서 작동하므로 인스턴스 레벨에서 사용하는 오버라이딩이 의미가 없다.
    그냥 클래스 이름을 통해 필요한 곳에 직접 접근하면 됨.
  - `final` 메서드는 재정의를 금지함.
  - `private` 메서드는 해당 클래스에서만 접근 가능하기 떄문에 하위 클래스에서 보이지 않는다. 따라서 오버라이딩 불가
- **생성자 오버라이딩**: 생성자는 오버라이딩 할 수 없다.

## 상속과 접근 제어

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/a9caeee5-1a53-4573-9363-d597aa7f2f69/db1cdd7a-2dcc-492b-b69e-0c7428f15afb.png)

접근 제어자를 표현하기 위해 UML 표기법 사용

- `+`: public
- `#`: protected
- `~`: default
- `-`: private

**접근 제어자 종류**

- `private`: 모든 외부 호출 막는다
- `default`: 같은 클래스 호출
- `protected`: 패키지가 달라도 상속관계는 호출 가능
- `public`: 모든 외부 호출 허용

**외부 호출에 공개된 정도**

`public` → `protected` → `default` → `private`

**Parent**

```java
package extends1.access.parent;

public class Parent {

    public int publicValue;
    protected int protectedValue;
    int defaultValue;
    private int privateValue;

    public void publicMethod() {
        System.out.println("Parent.publicMethod");
    }

    protected void protectedMethod() {
        System.out.println("Parent.ProtectedValue");
    }

    void defaultMethod() {
        System.out.println("Parent.defaultValue");
    }

    private void privateMethod() {
        System.out.println("Parent.privateValue");
    }

    public void printParent() {
        System.out.println("==Parent 메서드 안==");
        System.out.println("publicValue = " + publicValue);
        System.out.println("protectedValue = " + protectedValue);
        System.out.println("defaultValue = " + defaultValue);
        System.out.println("privateValue = " + privateValue);

        //부모 메서드 안에서 모두 접근 가능
        defaultMethod();
        privateMethod();
    }
}
```

**Child**

```java
package extends1.access.child;

import extends1.access.parent.Parent;

public class Child extends Parent {

    public void call() {
        publicValue = 1;
        protectedValue = 2;  //상속 관계 or 같은 패키지
        //defaultValue = 3;  //다른 패키지 접근 불가, 컴파일 오류
        //privateValue = 4;

        publicMethod();
        protectedMethod();
        //defaultMethod();  //상속 관계 or 같은 패키지
        //privateMethod();  //다른 패키지 접근 불가, 컴파일 오류

        printParent();
    }

}
```

**ExtendsAccessMain**

```java
package extends1.access;

import extends1.access.child.Child;

public class ExtendsAccessMain {
    public static void main(String[] args) {

        Child child = new Child();
        child.call();
    }
}
```

**실행결과**

```java
Parent.publicMethod
Parent.ProtectedValue
==Parent 메서드 안==
publicValue = 1
protectedValue = 2
defaultValue = 0
privateValue = 0
Parent.defaultValue
Parent.privateValue
```

**둘다 패키지가 다르다는 부분에 유의**

자식 클래스인 `Child` 에서 부모 클래스인 `Parent`에 얼마나 접근할 수 있는지 확인.

- `publicValue = 1`: 부모의 `public` 필드에 접근. `public`이므로 접근 가능.
- `protectedValue = 2`: 부모의 `protected` 필드에 접근. 서로 다른 패키지이지만, 상속관계이므로 접근 가능.
- `defaultValue = 1`: 부모의 `default` 필드에 접근. 자식과 부모가 다른 패키지이므로 접근 불가.
- `privateValue = 1`: 부모의 `private` 필드에 접근, 자식이라도 호출 불가.

**메서드 경우도 앞서 설명한 필드와 동일**

코드를 실행해 보면 `Child.call()` → `Parent.printParent()` 순서로 호출 한다.

`Child`는 부모의 `public`, `protected` 필드나 메서드만 접근 할 수 있다. Parent.printParent() 의 경우 `Parent` 안에 있는 메서드 이기 때문에 `Parent` 자신의 모든 필드와 메서드의 얼마든지 접근할 수 있다.

**접근 제어와 메모리구조**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/b0839bda-4b59-4b43-a17e-63d2b8e5514c/2be648ad-2969-4656-a851-ebbc193e3135.png)

본인 타입에 없으면 부모 타입에서 기능을 찾는데, 이때 접근 제어자가 영향을 준다.

왜나하면 객체 내부에서는 자식과 부모가 구분되어있기 때문이다. 결국 자식 타입에서 부모 타입의 기능을 호출할 때, 부모 입장에서 보면 외부에서 호출한 것과 같다.

## super - 부모 참조

부모와 자식의 필드 명이 갔거나 메소드가 오버 라이딩 되어 있으면, 자식에서 부모의 필드나 메서드를 호출 할 수 없다. 이 때 `super`  키워드를 사용하면 부모를 참조 할 수 있다. `super`  는 이름 그대로 부모 클래스에에 대한 참조를 나타낸다.

부모의 필드명과 자식의 필드명 둘 다 `value`로 똑같다. 메서드도 `hello()`로 자식에서 오버라이딩 되어있다. 이 때, 자식 클래스에서 부모 클래스의 `value`와 `hello()`를 호출 하고 싶다면 `super` 키워드를 사용하면 됨.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/c73b5d33-707d-4fed-b9c6-a3e8fb1f86e5/1e09c3bc-b17c-4bfd-8bf1-071f5641a200.png)

**Parent**

```java
package extends1.super1;

public class Parent {

    public String value = "parent";

    public void hello() {
        System.out.println("Parent.hello");
    }
}
```

**Child**

```java
package extends1.super1;

public class Child extends Parent{

    public String value = "child";

    @Override
    public void hello() {
        System.out.println("Child.hello");
    }

    public void call() {
        System.out.println("this.value = " + this.value);  //this 생략 가능
        System.out.println("super.value = " + super.value);

        this.hello();
        super.hello();
    }
}
```

**SuperMain**

```java
package extends1.super1;

public class SueprMain {
    public static void main(String[] args) {
        Child child = new Child();
        child.call();
    }
}
```

**실행결과**

```java
this.value = child
super.value = parent
Child.hello
Parent.hello
```

- `this`는 자기 자신의 참조를 뜻함. this 생략 가능.
- `super`는 부모 클래스 참조 뜻함.
- 필드 이름과 메서드 이름이 같지만 `suepr`를 사용해서 부모 클래스에 있는 기능 사용 가능.

**super 메모리 그림**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/c3bfcc8f-1322-45ed-9f38-6db4dfd6da02/3dfa98f1-19d1-433c-bf56-83ebd4fb4079.png)

## suepr - 생성자

상속 관계 인스턴트를 생성 하면 결국 메모리 내부에는 자식과 부모 클래스가 각각 만들어진다. `Child`를 만들면 부모인 `Parent`까지 함께 만들어지는 것. 따라서 각각의 생성자도 모두 호출 되어야한다.

**상속 관계를 사용하면 자식 클래스의 생성자에서 부모 클래스의 생성자를 반드시 호출해야한다. (규칙)**

상속 관계에서는 부모의 생성자를 호출할 때는 `super(…)`를 사용한다.

**ClassA**

```java
package extends1.suepr2;

public class ClassA {
    public ClassA() {
        System.out.println("ClassA 생성자");
    }
}
```

- 최상위 부모 클래스

**ClassB**

```java
package extends1.suepr2;

public class ClassB extends ClassA {
    public ClassB(int a) {
        super();  //기본 생성자 생략 가능
        System.out.println("ClassB 생성자 a = " + a);
    }

    public ClassB(int a, int b) {
        super();  //기본 생성자 생략 가능
        System.out.println("ClassB 생성자 a = " + a + ", b = " + b);
    }
}
```

- `ClassB`는 `ClassA`를 상속 받음
- 생성자의 첫줄에 `super(…)`를 사용해 부모 클래스 생성자를 호출해야한다.
  - 예외로 생성자 첫줄에 `this(…)`를 사용할 수 있다. 하지만 `super(…)`는 자식의 생성자 안에서 언젠가 반드시 호출해야함.
- 부모 클래스의 생성자가 기본 생성자(매개변수(파라미터)가 없는 생성자)인 경우 `super()`생략 가능
  - 상속 관계 첫줄에 `super(…)`를 생략함녀 자바는 부모의 기본 생성자를 호출하는 `super()`를 자동으로 만듦.
  - 첨고로 기본 생성자를 많이 사용하기 때문에 편의상 이런 기능 제공

**ClassC**

```java
package extends1.suepr2;

public class ClassC extends ClassB {

    public ClassC() {
        //super();  //부모 클래스가 기본 생성자의 경우에만 생략가능
        super(10, 20);
        System.out.println("ClassC 생성자");
    }
}
```

- ClassC는 ClassB를 상속받음.
- ClassB의 2개 생성자
  - `ClassB(int a)`
  - `ClassB(int a, int b)`
- 생성자는 하나만 호출 가능. 두 생성자 중 하나 선택.
- 참고로 `ClassC` 의 부모인 `ClassB` 에는 기본 생성자가 없다. 따라서 부모의 기본 생성자를 호출하는 `super()`를 사용하거나 생략 불가

**Super2Main**

```java
package extends1.suepr2;

public class ClassC extends ClassB {

    public ClassC() {
        //super();  //부모 클래스가 기본 생성자의 경우에만 생략가능
        super(10, 20);
        System.out.println("ClassC 생성자");
    }
}
```

**실행결과**

```java
ClassA 생성자
ClassB 생성자 a = 10, b = 20
ClassC 생성자
```

실행해보면

`ClassA` → `ClassB` → `ClassC` 순서로 실행됨.

자식 생성자의 첫줄에서 부모의 생성자를 호출해야하기 때문.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/713e075c-1110-4ffa-a1b9-d2d3c73888db/baf40f87-849b-456b-bee9-e7b37fd07b43.png)

## 문제와 풀이

### 문제: 상속 관계 상품

쇼핑몰 판매 상품을 만들기

- `Book`, `Album`, `Movie` 3가지 클래스
- 코드 중복 없이 상속 관계 사용. 부모 클래스는 `Item`
- 공통 속성: `name`, `price`
  - `Book`: 저자(`author`), isbn(`isbn`)
  - `Album`: 아티스트(`artist`)
  - `Movie`: 감독(`director`), 배우(`actor`)

**ShopMain**

```java
package extends1.ex;

public class ShopMain {

    public static void main(String[] args) {
        Book book = new Book("JAVA", 10000, "han", "12345");
        Album album = new Album("앨범1", 15000, "seo");
        Movie movie = new Movie("영화1", 18000, "감독1", "배우1");

        book.print();
        album.print();
        movie.print();

        int sum = book.getPrice() + album.getPrice() + movie.getPrice();
        System.out.println("상품 가격의 합: " + sum);
    }
}
```

**Item**

```java
package extends1.ex;

public class Item {

    private String name;
    private int price;

    public Item(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public void print() {
        System.out.println("이름: " + name + ", 가격: " + price);
    }

    public int getPrice() {
        return price;
    }
}

```

**Book**

```java
package extends1.ex;

public class Book extends Item {

    private String author;
    private String isbn;

    public Book(String name, int price, String author, String isbn) {
        super(name, price);
        this.author = author;
        this.isbn = isbn;
    }

    @Override
    public void print() {
        super.print();
        System.out.println("- 저자: " + author + ", isbn: " + isbn);
    }
}
```

**Album**

```java
package extends1.ex;

public class Album extends Item{

    private String artist;

    public Album(String name, int price, String artist) {
        super(name, price);
        this.artist = artist;
    }

    @Override
    public void print() {
        super.print();
        System.out.println("- 아티스트: " + artist);
    }
}
```

**Movie**

```java
package extends1.ex;

public class Movie extends Item{

    private String director;
    private String actor;

    public Movie(String name, int price, String director, String actor) {
        super(name, price);
        this.director = director;
        this.actor = actor;
    }

    @Override
    public void print() {
        super.print();
        System.out.println("- 감독: " + director + ", 배우: " + actor);
    }
}
```

**실행결과**

```java
이름: JAVA, 가격: 10000
- 저자: han, isbn: 12345
이름: 앨범1, 가격: 15000
- 아티스트: seo
이름: 영화1, 가격: 18000
- 감독: 감독1, 배우: 배우1
상품 가격의 합: 43000
```