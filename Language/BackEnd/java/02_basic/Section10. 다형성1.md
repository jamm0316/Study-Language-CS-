[<back](https://www.notion.so/basic-_-3ee7f95ac24d43f881b7ee2e8f21c210?pvs=21)

---

<aside>
📃 목차

</aside>

---

## 다형성 시작

객체 지향 프로그래밍의 특징으로 캡슐화, 상속, 다형성이 있다. 그 중 다형성은 객체 지향 프로그래밍의 꽃이라 불린다. 캡슐화나 상속는 직관적 이해가 가능하지만 다형성은 제대로 이해하고 활용하기 어렵다. 그러나, 좋은 개발자가 되기위한 필수 조건이다.

다형성(Polymorphism) 이름 그대로 “다양한 형태”, “여러 형태”

다형성은 한 객체가 여러 타입의 객체로 취급될 수 있는 능력을 뜻함.

보통 하나의 객체는 하나의 타입으로 고정되어있다(int, String, boolean 등) 그러나, 다형성을 사용하면, 하나의 객체가 다른 타입으로 사용될 수 있다.

다형성의 핵심이론

- **다형적 참조**
- **메서드 오버라이딩**

### 다형적 참조

**Parent**

```java
package poly;

public class Parent {

    public void parentMethod() {
        System.out.println("Parent.parentMethod");
    }
}
```

**Child**

```java
package poly;

public class Child extends Parent {

    public void childMethod() {
        System.out.println("Child.MethodChild");
    }

}
```

**PolyMain**

```java
package poly;

public class PolyMain {

    public static void main(String[] args) {
        //부모 변수가 부모 인스턴스 참조
        System.out.println("Parent -> Parent");
        Parent parent = new Parent();
        parent.parentMethod();
    }
}
```

**실행 결과**

```java
Parent -> Parent
Parent.parentMethod
```

**부모 타입의 변수가 부모 인스턴스 참조**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/b61b2406-4b9a-4254-a827-a7819d6d0dbd/df743f60-63ef-4ae1-8e09-c632ba8de44e.png)

- `Parent` 인스턴스 만들면 메모리 상에 `Parent`만 생성됨.
- `parent.printParent()` 를 호출하면 인스턴스의 `Parent`클래스에 있는 `printParent()` 호출됨.

**PolyMain**

```java
package poly;

public class PolyMain {

    public static void main(String[] args) {
        //부모 변수가 부모 인스턴스 참조
        System.out.println("Parent -> Parent");
        Parent parent = new Parent();
        parent.parentMethod();

        //자식 변수가 자식 인스턴스 참조
        System.out.println("Child -> Child");
        Child child = new Child();
        child.parentMethod();
        child.childMethod();
    }
}
```

**실행 결과**

```java
Parent -> Parent
Parent.parentMethod
Child -> Child
Parent.parentMethod
Child.MethodChild
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/4c895fa8-aab3-4375-980b-b9b495efadc9/ab70706d-708a-401c-bce3-1c97c06164f1.png)

- `Child` 인스턴스를 만들면 메모리 상에 `Parent` 인스턴스도 생성된다.
- 이에 따라 `child.parentMethod()` 와 `child.childMethod()` 둘다 호출 가능

PolyMain

```java
package poly;

public class PolyMain {

    public static void main(String[] args) {
        //부모 변수가 부모 인스턴스 참조
        System.out.println("Parent -> Parent");
        Parent parent = new Parent();
        parent.parentMethod();

        //자식 변수가 자식 인스턴스 참조
        System.out.println("Child -> Child");
        Child child = new Child();
        child.parentMethod();
        child.childMethod();

        //부모 변수가 자식 인스턴스 참조
        System.out.println("Parent -> Child");
        Parent poly = new Child();
        poly.parentMethod();
    }
}
```

**실행 결과**

```java
Parent -> Parent
Parent.parentMethod
Child -> Child
Parent.parentMethod
Child.MethodChild
Parnet -> Child
Parent.parentMethod
```

**다형적 참조: 부모 타입의 변수가 자식 인스턴스 참조**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9246fe1d-482d-4ebd-898e-e0ce722e3d72/34f0b7b8-647d-4d1b-81ce-eec86ea3fc65.png)

- 부모타입의 변수가 자식 인스턴스 참조
- `Parent poly = new child()`
- `Child` 인스턴스를 만들었기 때문에 메모리상에 `Child`와 `Parent`가 모두 생성딤.
- 생성된 참조값을 `Parent` 타입 변수인 `poly`에 담음

**부모는 자식을 담을 수 있다.**

- 부모 타입은 자식 타입을 담을 수 있다.
- `Parent poly`는 부모 타입. `new Child()`를 통해 생성된 결과는 `Child` 타입니다. 자바에서는 부모 타입은 자식 타입을 담을 수 있다.
  - `Parent poly = new Child()`: 성공
- 반대로 자식 타입은 부모 타입을 담을 수 없다.
  - `Child child1 = new Parent()`: 컴파일 오류 발생

**다형적 참조**

지금까지 학습한 내용으로는 항상 같은 타입만 참조함.

- `Parent parent = new Parent()`
- `Child child = new Child()`

그런데 Parent 타입의 변수는 다음과 같이 자기 자신인 Parent는 물론이고, 자식, 손자 타입, 그 하위 타입도 참조 가능

- `Parent poly = new Parent()`
- `Parent poly = new Child()`
- `Parent poly = new Grandson()`

자바에서 부모 타입은 자신은 물론이고, 자신의 기준으로 모든 자식타입을 참조할 수 있다.

**다형적 참조와 인스턴스 실행**

앞의 그림을 참고하여. `poly.parentMethod()`를 호출하면 먼저 참조값을 사용해서 인스턴스 찾음.

다음으로 인스턴스 안에서 실행할 타입도 찾음. `poly`는 `Parent`타입이다.

따라서, `Parent`클래스 부터 시작해서 필요한 기능을 찾음. 인스턴스 `Parent`에 `parentMethod()`가 있으므로 해당 메서드 호출.

**다형적 참조의 한계**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f12ec729-9a2a-4dc2-8266-3ba5ea4d9e92/d63acc2c-1988-4c72-a57f-f55805593cab.png)

`Parent poly = new Child()` 이렇게 자식을 참조한 상황에서 `poly` 는 `Parent` 타입이다. 자식 타입인 `Child`에 있는 `childMethod()`를 호출한다면 상속 관계는 부모 방향으로 찾아 올라갈 수 있지만 자식 방향으로는 찾아 내려갈 수 없다. 따라서 `childMethod()`를 불러올 수 없어 컴파일 오류가 발생한다.

이런 경우 `childMethod()`를 호출하고 싶으면 **캐스팅** 필요하다.

**다형적 참조의 핵심은 부모는 자식을 품을 수 있다는 것이다.**

일단 문법과 이론을 익히고 응용은 차차 알아보자. 왜냐하면 다른 이론도 알아야 이해가능하기 때문

## 다형성과 캐스팅

`Parent poly = new Child();` 와 같이 부모 타입의 변수를 자식 인스턴스를 참조하면, `Child`의 메서드는 호출할 수 없다.

**CastingMain1**

```java
package poly;

public class CastingMain1 {

    public static void main(String[] args) {
        //부모 변수가 자식 인스턴스 참조(다형적 참조)
        Parent poly = new Child();  //x001

        //단 자식의 기능ㅇ은 호출할 수 없다. 컴파일 오류 발생
        //poly.childMethod();

        //다운캐스팅(부모 타입 -> 자식타입)
        Child child = (Child) poly;  //x001
        child.childMethod();
    }
}
```

**실행결과**

```java
Child.MethodChild
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/6dd178a4-1847-4a5c-bf10-aeb2d493afd7/47b73fe1-45d5-4567-9599-0ffb6a8f5e09.png)

- `poly.childMethod()`를 호출하면 먼저 참조값을 사용해서 인스턴스를 찾는다.
- 인스턴스 안에서 사용할 타입 `poly`는 `Parnet`타입이다.
- `Parnet`는 최상위 부모이다. 상속 관계는 부모로만 찾아서 올라갈 수 있다.
  - `childMethod()`는 자식 타입에 있으므로 호출 할 수 없다. → 컴파일 오류 발생

**다운캐스팅**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/8dc007a5-e20e-462b-af0f-465ceb77f999/6173dd89-aef5-47de-940b-55ab70e7510e.png)

호출하는 타입을 `child` 타입으로 변경하면 인스턴스의 `Child`에 있는 `childMethod()`를 호출할 수 있다. 하지만 다음과 같은 문제에 봉착

**부모는 자식을 담을 수 있지만 자식은 부모를 담을 수 없다.**

- `Parent parent = new Child()` : 부모는 자식을 담을 수 있다.
- `Parent parent = child  //Child child 변수` : 부모는 자식을 담을 수 있다.

반면 다음과 같이 자식은 부모를 담을 수 없다.

```java
Child child = poly  //Parent poly 변수 
```

부모타임을 사용하는 변수를 자식 타입에 대입 하려고 하면 컴파일 오류가 발생한다. 자식은 부모를 담을 수 없다. 이때 다운 캐스팅 이라는 기능을 사용해서 부모 타입을 잠깐 자식 타입으로 변경 하면 된다.

```java
Child child = (Child) poly  //parent poly
```

(타입)처럼 괄호와 그 사이에 타입을 지정하면 참조 대상을 특정 타입으로 변경할 수 있다. 이렇게 특정 타입으로 변경하는 것을 캐스팅이라한다.

오른쪽에 있는 `(Child) poly` 코드를 먼저 보자. `poly`는 `Parent` 타입이다. 이 타입을 `(Child)`를 사용해서 일시적으로 자식 타입인 `Child` 타입으로 변경한다. 그리고 나서 왼쪽에 있는 `Child child`에 대입한다.

**실행 순서**

```java
Child child = (Child) poly  //다운캐스팅을 통해 부모타입을 자식 타입으로 변환 -> 대입
Child child = (Child) x001  //참조값을 읽은 다음 자식 타입으로 지정
Child child = x001  //최종 결과
```

캐스팅 한다고해서 `Parent poly`의 타입이 변경되는 것이 아님. 해당 참조값을 꺼내어 복사한 후 `Child`타입으로 지정해주는 것이고, 따라서 `poly`의 타입은 `Parent`로 기존과 같이 유지된다.

**캐스팅**

- 업캐스팅(upcasting): 부모 타입으로 변경
- 다운캐스팅(downcasting): 자식 타입으로 변경

**캐스팅 용어**

“캐스팅”은 영어 단어 “cast”에서 유래됨. “cast”는 금속이나 다른 물질을 녹여서 특정한 형태나 모양으로 만드는 과정.

`Child child = (Child) poly`의 경우 `Parent poly`라는 부모 타입을 `Child`라는 자식 타입으로 변경

부모타입을 자식 타입으로 변경하는 것을 다운 캐스팅이라 한다. 반대로 부모타입으로 변경하는 것을 업캐스팅.

**다운캐스팅과 실행**

```java
//다운캐스팅(부모 타입 -> 자식 타입)
Child child = (Child) poly;
child.childMethod();
```

다운 캐스팅 덕분에 `child.childMethod();` 호출 가능. `childMethod()`를 호출하기 위해 해당 인스턴스를 찾아 간다음 `Child` 타입을 찾는다. `Child` 타입에는 `childMethod()`가 있으므로 해당 기능 호출 가능.

## 캐스팅의 종류

```java
//다운캐스팅(부모 타입 -> 자식 타입)
Child child = (Child) poly;
child.childMethod();
```

다운 캐스팅을 하고 변수에 담아두는 과정이 번거롭다. 이런 과정 없이 일시적으로 다운캐스팅을 해서 인스턴스에 있는 하위 클래스 기능을 바로 호출 가능.

### 일시적 다운 캐스팅

```java
package poly;

public class CastingMain2 {

    public static void main(String[] args) {
        //부모 변수가 자식 인스턴스 참조(다형적 참조)
        Parent poly = new Child();  //x001

        //단 자식의 기능ㅇ은 호출할 수 없다. 컴파일 오류 발생
        //poly.childMethod();

        //다운캐스팅(부모 타입 -> 자식타입)
        Child child = (Child) poly;  //x001
        child.childMethod();
        
        //일시적 다운캐스팅 - 해당 메서드를 호출하는 순간만 다운캐스팅
        ((Child) poly).childMethod();
    }
}
```

**실행결과**

```java
Child.MethodChild
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/d46ffb52-d1ff-46fe-9fef-fc92f0a09cb4/abceb5c9-5618-4c08-ac21-4a694b675c38.png)

정확히는 `poly`가 `Child` 타입으로 바뀌는 것이 아니다.

```java
((Child) poly).childMethod()  //다운캐스팅을 통해 부모타입을 자식 타입으로 변환 후 기능 호출
((Child) x001).childMethod()  //참조값을 읽은 다음 자식 타입으로 다운캐스팅
```

참고로 캐스팅 한다고 해서 `Parent poly`의 타입이 변하는 것이 아니다. 해당 참조값을 꺼내고 꺼낸 참조값이 `Child` 타입이 되는 것이다. 따라서 `poly`의 타입은 `Parent`로 그대로 유지된다.

일시적으로 다운캐스팅을 사용하면 별도의 변수 없이 인스턴스의 자식 타입을 기능을 사용할 수 있다.

### 업캐스팅

다운캐스팅과 반대로 현재 타입을 부모 타입으로 변경하는 것을 업캐스팅이라 한다.

**CastingMain3**

```java
package poly.basic;

//upacsting vs downcasting
public class CastingMain3 {

    public static void main(String[] args) {
        Child child = new Child();
        Parent parent1 = (Parent) child;  //업캐스팅은 생략 가능, 생략 권장
        Parent parent2 = child;  //업캐스팅 생략

        parent1.parentMethod();
        parent2.parentMethod();
    }
}
```

**실행 결과**

```java
Parent.parentMethod
Parent.parentMethod
```

```java
Parent parent1 = (Parent) child;
```

`Child` 타입을 `Parent` 타입에 대입해야 한다. 따라서 타입을 변환하는 캐스팅이 필요.

그런데 부모 타입으로 변환하는 경우 다음과 같이 캐스팅 코드인 `(타입)`을 생략할 수 있다.

```java
Parent parent2 = child
Parent aprent2 = new Child()
```

업 캐스팅은 생략 할 수 있다. 다운 캐스팅은 생략 할 수 없다. 참고로 업 캐스팅은 매우 자주 사용하기 때문에 생략을 권장 한다. 자바에서 부모는 자식을 담을 수 있다. 하지만 그 반대는 안 된다. (꼭 필요하다면 다운 캐스팅을 해야 한다.)

캐스팅은 생략 해도 되고 다운 캐스팅은 왜 개발자가 직접 명시적으로 캐스팅을 해야 할까?

## 다운캐스팅과 주의점

다운캐스팅은 잘못하면 심각한 런타임 오류 발생가능

**CastingMain4**

```java
package poly.basic;

//다운캐스팅을 자동으로 하지 않는 이유
public class CastingMain4 {

    public static void main(String[] args) {
        Parent parent1 = new Child();
        Child child1 = (Child) parent1;
        child1.childMethod();  //문제 없음

        Parent parent2 = new Parent();
        Child child2 = (Child) parent2;  //런타임 오류 - ClassCastException
        child2.childMethod();  //실행 불가
    }
}
```

**실행결과**

```java
Child.MethodChild
Exception in thread "main" java.lang.ClassCastException: class poly.basic.Parent cannot be cast to class poly.basic.Child (poly.basic.Parent and poly.basic.Child are in unnamed module of loader 'app')
	at poly.basic.CastingMain4.main(CastingMain4.java:12)
```

**다운캐스팅이 가능한 경우**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/23cb15b6-9c14-4836-bdde-2de9e1f6702f/fe3e70e6-e06f-42d2-b7c7-c18565401a36.png)

**다운캐스팅이 불가한 경우**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/eb499946-7f1f-48ed-97f6-de7fec831db0/560037ae-4b95-4e32-911a-345893e959b1.png)

예제의 `parent2`를 다운캐스팅 하면 `ClassCastException`이라는 심각한 런타임 오류가 발생.

```java
Parent parent2 = new Parent()
```

`new Parent()`로 부모타입으로 객체 생성. 따라서 메모리 상에 자식 타입은 존재하지 않음.

생성 결과를 `parent2`에 담아두는 것이기때문에 여기서는 문제 없음.

```java
Child child2 = (Child) parent2
```

`parent2`를 `Child`타입으로 다운 캐스팅. 그러데 `parent2` 는 `Parent`로 생성되었기 때문에 메모리상에 `Child`가 존재하지 않는다. 따라서 `Child`를 사용할 수 없음.

자바는 이렇게 사용할 수 없는 타입으로 다운캐스팅 하는 경우에 `ClassCastException`이라는 예외를 발생시키고, 프로그램이 종료된다.

### 업캐스팅이 안전하고 다운캐스팅이 위험한 이유

업캐스팅의 경우 이런 경우가 절대 발생하지 않음.

객체를 생성하면 해당 타입 상위 부모 타입은 모두 함께 생성되기 때문.

따라서 위로만 타입을 변경하는 업캐스팅은 메모리 상에 인스턴스가 모두 존재하기 때문에 항상 안전.

따라서 캐스팅 생략가능.

반면 다운 캐스팅의 경우 인스턴스에 존재하지 않는 하위 타입으로 캐스팅하는 문제가 발생할 수 있다.

왜냐하면 객체를 생성하면 부모 타입은 모두 함께 생성되지만 자식 타입은 생성되지 않는다.

따라서 개발자가 이런 문제를 인지하고 사용해야 한다는 의미로 명시적으로 캐스팅.

**업케스팅**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/d5738232-769b-49aa-a5ac-2b91865b7766/d4a937d2-021d-4bf8-8dfc-4c8f1fdedefa.png)

클래스 `A`, `B`, `C`는 상속관계

`new C`로 인스턴스 생성하면 인스턴스 내부에 자신의 부모인 `A`, `B`, `C` 모두 생성. 따라서 `A`, `B`, `C` 모두 `C`인스턴스를 참조할 수 있다.

- `A a = new C()` : A로 업캐스팅
- `B b = new C()` : B로 업캐스팅
- `C c = new C()` : 자신과 같은 타입

**다운캐스팅**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/421d6ecd-41b9-4577-998e-47f110a85b46/dcb654f1-561c-484a-b458-0f25ed0dae14.png)

`new B()` 로 인스턴스를 생성하면 인스턴스 내부에 자신과 부모인 A, B 생성.

따라서 B의 부모 타입인 A, B 모두 B인스턴스 참조 가능. 상위로 올라가는 업캐스팅은 인스턴스 내부에 부모가 모두 생성되기 때문에 문제가 발생하지 않지만, 객체를 생성할 때 자식은 생성되지 않으므로, 하위로 내려가는 다운 캐스팅은 인스턴스 내부에 없는 부분을 선택하는 문제가 발생할 수 있다.

- `A a = new B()` : A로 업캐스팅
- `B b = new B()` : 자신과 같은 타입
- `C c = new B()` : 하위 타입은 대입할 수 없음. **컴파일 오류**
- `C c = (C) new B()`: 하위 타입으로 강제 다운캐스팅. 하지만 B인스턴에스 C와 관련된 부분이 없으므로 잘못 캐스팅
  `ClassCastException` **런타임 오류 발생**

**컴파일 오류 vs 런타임 오류**

- **컴파일 오류**는 변수명 오타, 잘못된 클래스 이름 사용 등 자바 프로그램을 실행하기 전에 발생하는 오류.
  - 이런 오류는 IDE에서 즉시 확인할 수 있기 때문에 안전하고 좋은 오류.
- **런타임 오류**는 이름 그대로 프로그램이 실행되고 있는 시점에 발생하는 오류.
  - 보통 고객이 해당 프로그램을 실행하는 도중에 발생하기 때문에 매우 안좋은 오류.

## instanceof

다형성에서 참조형 변수는 이름 그대로 다양한 자식 대상 참조 가능.

여기서 `Parent`는 자신과 같은 `Parent`의 인스턴스도 참조할 수 있고, 자식 타입인 `Child`의 인스턴스도 참조할 수 있다. 이때 `parent1`, `parent2` 변수가 참조하는 **인스턴스 타입을 확인**하고 싶다면 `instanceof` 키워드 사용

**CastingMain5**

```java
package poly.basic;

public class CastingMain5 {
    public static void main(String[] args) {
        Parent parent1 = new Parent();
        System.out.println("parent1 호출");
        call(parent1);

        Parent parent2 = new Child();
        System.out.println("parent2 호출");
        call(parent2);

    }
    private static void call(Parent parent) {
        parent.parentMethod();
        if (parent instanceof Child) {
            System.out.println("Child 인스턴스 맞음");
            Child child = (Child) parent;
            child.childMethod();
        }
    }
}
```

**실행결과**

```java
parent1 호출
Parent.parentMethod
parent2 호출
Parent.parentMethod
Child 인스턴스 맞음
Child.MethodChild
```

`parent1`은 `Parent`의 인스턴스를 참조

```java
parent1 instanceof Child  //parent1은 Parnet 인스턴스
new Parent() instanceof Child  //false
```

따라서 `false` 반환

`parent2`는 `Child`의 인스턴스 참조

```java
parent2 instanceof Child  //parent2는 Child 인스턴스
new Child() instanceof Child  //ture
```

따라서 `true` 반환

참고로 `instanceof` 키워드는 오른쪽 대상의 자식 타입을 왼쪽에서 참조하는 경우에도 `true` 반환

```java
parent instanceof Parent  //parnet는 Child의 인스턴스

new Parent() instanceof Parent  //parent가 Parent의 인스턴스를 참조하는 경우: true
new Child() instanceof Parent  //parent가 Child의 인스턴스를 참조하는 경우: true
```

### 자바 16 - Pattern Matching for instanceof

자바16

**CastingMain6**

```java
package poly.basic;

public class CastingMain6 {
    public static void main(String[] args) {
        Parent parent1 = new Parent();
        System.out.println("parent1 호출");
        call(parent1);

        Parent parent2 = new Child();
        System.out.println("parent2 호출");
        call(parent2);

    }
    private static void call(Parent parent) {
        parent.parentMethod();
        if (parent instanceof Child child) {
            System.out.println("Child 인스턴스 맞음");
            child.childMethod();
        }
    }
}
```

**실행결과**

```java
parent1 호출
Parent.parentMethod
parent2 호출
Parent.parentMethod
Child 인스턴스 맞음
Child.MethodChild
```

`instanceof`와 동시에 `Child` 다운캐스팅 을 선언할 수 있다.

## 다형성과 메서드 오버라이딩

**오버라이딩 된 메서드가 항상 우선권을 가진다.**

메서드 오버라이딩의 진짜 힘은 다형성과 함께 할때 나타난다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/b9d721f3-3943-4545-81cf-0cd89d23664c/8a2b1896-bb75-4195-9aa2-fcdf01cef06f.png)

- `Parent`, `Child` 모두 `value`라는 같은 멤버 변수 가지고 있음
  - 멤버 변수는 오버라이딩 불가
- `Parent`, `Child` 모두 `method()`라는 메서드를 가지고 있음. `Child`에서 메서드 오버라이딩.
  - 메서드는 오버라이딩 가능

**Parent**

```java
package poly.overriding;

public class Parent {

    public String value = "parent";

    public void method() {
        System.out.println("Parent.method");
    }
}
```

**Child**

```java
package poly.overriding;

public class Child extends Parent {

    public String value = "child";

    @Override
    public void method() {
        System.out.println("Child.method");
    }
}
```

**OverridingMain**

```java
package poly.overriding;

public class OverridingMain {
    public static void main(String[] args) {
        //자식변수가 자식 인스턴스 참조
        Child child = new Child();
        System.out.println("Child -> Child");
        System.out.println("value = " + child.value);
        child.method();

        //부모변수가 부모 인스턴스 참조
        Parent parent = new Parent();
        System.out.println("Parent -> Parent");
        System.out.println("value = " + parent.value);
        parent.method();

        //부모 변수가 자식 인스턴스 참조(다형적 참조)
        Parent poly = new Child();
        System.out.println("Parent -> Child");
        System.out.println("value = " + poly.value);  //value = parent -> 변수는 오버라이딩 X
        poly.method();  //Child.method -> 메서드는 오버라이딩 O
    }
}
```

**실행 결과**

```java
Child -> Child
value = child
Child.method
Parent -> Parent
value = parent
Parent.method
Parent -> Child
value = parent
Child.method
```

**Child → Child**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/1fdadff3-d679-48c1-ac06-512a96dea10d/475b9114-98c3-4b75-898c-07f287f97c78.png)

- `child` 변수는 `Child` 타입이다. 따라서 `child.value`, `child.method()`를 호출하면 `Child`타입에서 기능을 찾아 실행

**Parent → Parent**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/4cb6f8fa-7440-4c94-9668-e071898376c1/41130bf4-fa6c-414b-9659-c93e4f1c44ad.png)

- `parent` 변수는 `Parent` 타입이다. 따라서 `parent.value`, `parent.method()`를 호출하면 `Parent`타입에서 기능을 찾아서 실행

**Parent → Child**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/24569d6e-6c8d-4ad8-8194-9ce074a8eeb2/c451973e-7d42-44d6-9ec3-24c2e882e717.png)

<aside>
❗ **중요**

- **오버라이딩 된 메서드는 항상 우선권을 갖는다!**
</aside>

- `poly` 변수는 `Parent` 타입이다. 따라서 `poly.value`, `poly.method()`를 호출하면 인스턴스 `Parent`타입에서 기능을 찾아 실행한다.
  - `poly.value`: `Parent`타입에 있는 `value`값을 읽는다.
  - `poly.method()`: `Parent`타입에 있는 `method()`를 실행시키려고 한다. 그런데 하위 타입인 `Child.method()`가 오버라이딩 되어있다. 오버라이딩 된 메서드는 항상 우선권을 갖는다. 따라서 `Parnet.method()`가 아니라 `Child.method()`가 실행된다.

**오버라이딩된 메서드는 항상 우선권을 가진다.** 오버라이딩은 부모 타입에서 정의한 기능을 자식 타입에서 재정의하는 것이다. 만약 자식에서도 오버라이딩 하고 손자에서도 같은 메서드를 오버라이딩을 하면 손자의 오버라이딩 메서드가 우선권을 가진다. 더 하위 자식의 오버라이딩 된 메서드가 우선권을 가진다.

지금까지 다형성을 이루는 핵심 이론인 다형적 참조와 메서드 오버라이딩에 대해 학습함.

- **다형적 참조**: 하나의 변수 타입으로 다양한 자식 인스턴스를 참조할 수 있는 기능.
- **메서드 오버라이딩**: 기존 기능을 하위 타입에서 새로운 기능으로 재정의

이 둘을 이해하고 나면 진정한 다형성의 위력을 맛볼 수 있다.

##