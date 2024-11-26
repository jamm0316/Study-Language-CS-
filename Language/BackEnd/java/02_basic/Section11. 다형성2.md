[<back](https://www.notion.so/basic-_-3ee7f95ac24d43f881b7ee2e8f21c210?pvs=21)

---

<aside>
📃 목차

</aside>

---

## 다형성 활용1

### 예제1

다형성 사용하지 않고 프로그램 개발.

동물 소리로 문제로 접근

개, 고양이, 소의 울음 소리를 테스트하는 프로그램 작성.

**Dog**

```java
package poly.ex1;

public class Dog {

    public void sound() {
        System.out.println("bow bow!");
    }
}
```

**Cat**

```java
package poly.ex1;

public class Cat {

    public void sound() {
        System.out.println("mew mew~");
    }
}
```

**Caw**

```java
package poly.ex1;

public class Cat {

    public void sound() {
        System.out.println("mew mew~");
    }
}
```

**AnimalSoundMain**

```java
package poly.ex1;

public class AnimalSoundMain {
    public static void main(String[] args) {
        Dog dog = new Dog();
        Cat cat = new Cat();
        Caw caw = new Caw();

        System.out.println("동물 소리 테스트 시작");
        dog.sound();
        System.out.println("동물 소리 종료");

        System.out.println("동물 소리 테스트 시작");
        cat.sound();
        System.out.println("동물 소리 종료");

        System.out.println("동물 소리 테스트 시작");
        caw.sound();
        System.out.println("동물 소리 종료");
    }
}
```

**실행 결과**

```java
동물 소리 테스트 시작
bow bow!
동물 소리 종료
동물 소리 테스트 시작
mew mew~
동물 소리 종료
동물 소리 테스트 시작
baa baa~
동물 소리 종료
```

단순히 개, 고양이, 소 동물들의 울음 소리를 출력하는 프로그램.

만약 영기서 새로운 동물이 추가된다면?

만약 기존 코드에 소가 없었다면 `Caw` 클래스를 만들고 다음 코드도 추가해야함

```java
//Caw를 생성하는 코드
Caw caw = new Caw();

//Caw를 사용하는 코드
System.out.println("동물 소리 테스트 시작");
caw.sound();
System.out.println("동물 소리 테스트 종료");
```

`Caw`를 생성하는 부분은 당연히 필요하니 크게 상관 없지만, `Dog`, `Cat`, `Caw`를 사용해서 출력하는 부분은 계쏙 중복이 증가.

**중복코드**

```java
System.out.println("동물 소리 테스트 시작");
dog.sound();
System.out.println("동물 소리 종료");

System.out.println("동물 소리 테스트 시작");
cat.sound();
System.out.println("동물 소리 종료");

System.out.println("동물 소리 테스트 시작");
caw.sound();
System.out.println("동물 소리 종료");
...
```

이 부분의 중복을 어떻게 제거 가능할까?

### 중복 제거 시도

**메서드로 중복 제거 시도**

메서드를 사용하면 매개변수를 클래스 `Caw`, `Dog`, `Cat` 중 하나로 정해야한다.

```java
private static void soundCaw(Caw caw) {
    System.out.println("동물 소리 테스트 시작");
    caw.sound();
    System.out.println("동물 소리 테스트 종료");
}
```

따라서 이 메서드는 `Caw`전용 메서드가 되고, `Dog`, `Cat`은 인수로 사용 불가.

**배열과 for문을 통한 중복 제거 시도**

```java
Caw[] cawArr = {dog, cat, caw};  //컴파일 오류 발생
System.out.println("동물 소리 테스트 시작");
    for (Caw caw : cawArr) {
        cawArr.sound();
}
System.out.println("동물 소리 테스트 종료")
```

배열과 `for`문 사용해서 중복을 제거하려고 해도 배열의 타입을 `Dog`, `Cat`, `Caw` 중 하나로 지정해야함.

`Caw`들을 배열에 담아서 처리하는 것은 가능하지만 `Dog`, `Cat`, `Caw`를 하나의 배열에 담을 수 없음.

새로운 동물이 추가될 때 마다 더 많은 중복 코드 작성해야함.

**문제의 핵심은 바로 타입이 다르다는 점.** 반대로 `Dog`, `Cat`, `Caw`가 모두 같은 타입이라면 메서드와 배열을 활용해서 코드의 중복 제거 가능.

다형성의 핵심은 다형적 참조와 메서드 오버라이딩이다. 이 둘을 활용하면 `Dog`, `Cat`, `Caw`가 모두 같은 타입을 사용하고, 각자 자신의 메서드도 호출할 수 있다.

## 다형성의 활용2

### 예제2

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/102441ce-93eb-455e-b587-aac2d96cb44b/56c091dd-21a4-4cf1-9d16-0637c8d7843b.png)

다형성을 사용하기 위해 상속 관계 사용.

`Animal`(동물)이라는 부모 클래스를 만들고 `sound()`메서드를 정의한다. 이 메서드는 자식 클래스에서 오버라이딩 할 목적으로 만들어짐.

`Dog`, `Cat`, `Caw`는 `Animal` 클래스를 상속 받고, 부모의 `sound()` 메서드 오버라이딩

**Animal**

```java
package poly.ex2;

public class Animal {

    public void sound() {
        System.out.println("동물 울음 소리");
    }
}
```

**Dog**

```java
package poly.ex2;

public class Dog extends Animal{

    @Override
    public void sound() {
        System.out.println("bow bow!");
    }
}
```

**Cat**

```java
package poly.ex2;

public class Cat extends Animal {

    @Override
    public void sound() {
        System.out.println("mew mew~");
    }
}
```

**Caw**

```java
package poly.ex2;

public class Caw extends Animal{

    @Override
    public void sound() {
        System.out.println("baa baa~");
    }
}
```

**AnimalPolyMain1**

```java
package poly.ex2;

public class AnimalPolyMain_2 {
    public static void main(String[] args) {
        Dog dog = new Dog();
        Cat cat = new Cat();
        Caw caw = new Caw();

        animalSound(dog);
        animalSound(cat);
        animalSound(caw);
        }

    private static void animalSound(Animal animal) {
        System.out.println("동물 소리 테스트 시작");
        animal.sound();
        System.out.println("동물 소리 테스트 종료");
    }
}
```

**실행 결과**

```java
동물 소리 테스트 시작
bow bow!
동물 소리 테스트 종료
동물 소리 테스트 시작
mew mew~
동물 소리 테스트 종료
동물 소리 테스트 시작
baa baa~
동물 소리 테스트 종료
```

**코드 분석**

- `soundAnimal(dog)`를 호출하면
  - `soundAnimal(Animal animal)`에 `Dog` 인스턴스가 전달된다.
  - `Animal animal = (Animal) dog` → `Animal`은 `Dog`의 부모타입이므로 업캐스팅이 필요 없다.
  - 메서드 안에서 `animal.sound()` 메서드 호출

  ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/be7c6262-7077-4214-b1b8-0c8360db758f/3300c1a7-f086-4c34-a3f2-19aba8d5ba43.png)

- `animal` 변수 타입은 `Animal`이므로 `Dog` 인스턴스의 `Animal` 클래스의 `sound()`메서드를 찾아 실행.
- 그런데 하위 클래스인 `Dog` 클래스에서 `sound()` 메서드가 오버라이딩 되어있음. 따라서 오버라이딩한 메서드가 우선권을 갖음.
- `Dog` 클래스에 있는 `sound()`가 호출됨.

이 코드의 핵심은 `Animal animal` 부분.

- **다형적 참조** 덕분에 `animal` 변수는 자식인 `Dog`, `Cat`, `Caw`의 인스턴스 참조 가능(부모는 자식 담을 수 있음)
- **메서드 오버라이딩** 덕분에 `animal.sound()`를 호출해도 `Dog.sound()`, `Cat.sound()`, `Caw.sound()`와 같이 인스턴스의 메서드를 호출할 수 있다.

다형성 덕분에 새로운 동물이 추가되어도. 다음 코드 재사용 가능.

```java
private static void aniamlSound(Animal animal) {
    System.out.println("동물 소리 테스트 시작")
    animal.sound()
    System.out.println("동물 소리 테스트 시작")
}
```

## 다형성 활용3

**AnimalPolyMain2**

```java
package poly.ex2;

public class AnimalPolyMain {
    public static void main(String[] args) {
        Animal dog = new Dog();
        Animal cat = new Cat();
        Animal caw = new Caw();

        Animal[] animals = {dog, cat, caw};
        for (Animal animal : animals) {
            System.out.println("동물 소리 테스트 시작");
            animal.sound();
            System.out.println("동물 소리 테스트 종료");
        }
    }
}
```

**실행결과**

```java
동물 소리 테스트 시작
bow bow!
동물 소리 테스트 종료
동물 소리 테스트 시작
mew mew~
동물 소리 테스트 종료
동물 소리 테스트 시작
baa baa~
동물 소리 테스트 종료
```

배열은 같은 타입의 데이터를 나열할 수 있다.

`Dog`, `Cat`, `Caw`는 모두 `Animal`의 자식이므로 `Animal` 타입.

`Animal` 타입의 배열을 만들고 다형적 참조 사용.

```java
//둘은 같은 코드
Animal[] animals = new Animal[]{dog, cat, caw};
Animal[] animals = {dog, cat, caw};
```

다형적 참조 덕분에 `Dog`, `Cat`, `Caw`의 부모 타입인 `Animal` 타입으로 배열을 만들고 , 각각 배열에 포함.

이제 배열을 for문을 사용해 반복

```java
//변하지 않는 부분
for (Animal animal : animals) {
    System.out.println("동물 소리 테스트 시작");
    animal.sound();
    System.out.println("동물 소리 테스트 종료");
```

`animal.sound()`를 호출하지만 `Dog`, `Cat`, `Caw` 인스턴스에 오버라이딩 된 `sound()` 메서드가 호출됨.

### 조금 더 개선

**AnimaPolyMain3**

```java
package poly.ex2;

public class AnimalPolyMain3 {
    public static void main(String[] args) {

        Animal[] animals = {new Dog(), new Cat(), new Caw()};
        for (Animal animal : animals) {
            soundAnimal(animal);
        }
    }
    //변하지 않는 부분
    private static void soundAnimal(Animal animal) {
        System.out.println("동물 소리 테스트 시작");
        animal.sound();
        System.out.println("동물 소리 테스트 종료");
    }
}
```

- `soundAnimal(Animal animal)`
  - 하나의 동물을 받아서 로직 처리

새로운 동물이 추가되어도 `soundAnimal(…)` 메서드는 코드 변경 없이 유지 가능.

이유는 이 메서드는 `Dog`, `Cat`, `Caw`같은 구체적 클래스가 아니라, `Animal`이라는 추상적 부모를 참조하기 때문.

**Duck**

```java
package poly.ex2;

public class Duck extends Animal{
    
    @Override
    public void sound() {
        System.out.println("kwak! kwak!");
    }
}
```

**AnimalPolyMain3**

```java
package poly.ex2;

public class AnimalPolyMain3 {
    public static void main(String[] args) {

        Animal[] animals = {new Dog(), new Cat(), new Caw(), new Duck()};
        for (Animal animal : animals) {
            soundAnimal(animal);
        }
    }
    //변하지 않는 부분
    private static void soundAnimal(Animal animal) {
        System.out.println("동물 소리 테스트 시작");
        animal.sound();
        System.out.println("동물 소리 테스트 종료");
    }
}
```

**실행결과**

```java
동물 소리 테스트 시작
bow bow!
동물 소리 테스트 종료
동물 소리 테스트 시작
mew mew~
동물 소리 테스트 종료
동물 소리 테스트 시작
baa baa~
동물 소리 테스트 종료
동물 소리 테스트 시작
kwak! kwak!
동물 소리 테스트 종료
```

새로운 기능이 추가되었을 때 변화는 부분을 최소화하는 것이 잘 작성된 코드이다.

**남은 문제**

**2가지 문제가 있다.**

- `Animal` 클래스를 생성할 수 있는 문제.
- `Animal` 을 상속 받는 생성한 클래스에 `sound()`를 override 하지 않을 수 있는 문제.

**문제1: `Animal` 클래스를 생성할 수 있는 문제.**

```java
Animal aniaml = new Animal();
```

실제 `Animal`은 추상적 개념이고 사용할 일은 없다. 그러나 `Animal`도 클래스이므로 인스턴스를 생성하고 사용하는데 아무런 제약이 없다. 하지만 누군가 실수로 `new Animal()` 을 생성하고 사용하면 인스턴스는 제대로 작동하지 않을 것임.

**문제2: `Animal` 클래스를 상속 받는 곳에서 `sound()`를 override 하지 않을 가능성**

```java
package poly.ex2;

public class Pig extends Animal {
    
    public void sound() {
        System.out.println("GGol GGol");
    }
}
```

위 처럼 오버라이드 하지 않으면 `sound()`호출 시, 컴파일이나 런타임 오류가 발생하지는 않지만 `Pig.sound()`가 아닌 `Animal.sound()`가 호출될 것이다.

좋은 프로그램이란 제약 있는 프로그램이다. 추상 클래스와 추상 메서드를 사용하면 이를 해결할 수 있다.

## 추상 클래스1

**추상 클래스**

동물(`Animal`)과 같이 부모 클래스는 제공하지만, 실제 생성되면 안되는 클래스를 추상 클래스라함.

추상 클래스는 이름 그대로 추상적인 개념을 제공하는 클래스. 따라서 실체인 인스턴스 존재하지 않고 상속을 목적으로 사용되는 부모 클래스

```java
abstract clss AbstracAnimal {...}
```

- 추상 클래스는 클래스를 선언할 때 앞에 추상이라는 의미의 `abstract` 키워드 붙힘
- 기존 클래스와 같은 기능을 하며, `new AbstractAimal()`과 같은 인스턴스를 생성하지 못하는 제약 추가.

**추상 메서드**

부모 메서드에 상속받는 자식 클래스들이 반드시 상속받아야하는 메서드.

이름 그대로 추상적인 개념을 제공하는 메서드. 따라서 실체 존재하지 않고, 메서드 바디가 없다.

```java
public abstract void sound();
```

- 추상 메소드는 선언할때 메소드 앞에 추상 이라는 의미의 `astract`  키워드를 붙여주면 된다.
- **추상 메소드가 하나라도 있는 클래스는 추상 클래스로 선언 해야 한다.**
  - 그렇지 않으면 컴파일 오류가 발생 한다.
  - 추상 메소드는 메서드 바디가 없다. 따라서 작동하지 않는 메소드를 가진 불안정한 클래스로 볼 수 있다. 따라서 직접 생성 하지 못하도록 추상 클래스로 선언 해야 한다.
- 추석 메소드는 상속받는 자식 클래스가 반드시 오버 라이딩 해서 사용해야 한다.
  - 그렇지 않으면 컴파일 오류가 발생 한다.
  - 추상 메소드는 자식 클래스가 반드시 오버 라이딩 해야 하기 때문에 메서드 바디 부분이 없다. 바디 부분을 만들면 컴파일 오류가 발생 한다.
  - 오버 라이딩 하지 않으면 자식도 추상 클래스가 되어야 한다.
- 추석 메서드는 기존 스터드와 완전히 갔다. 다만 메서드 바디가 없고 자식 클래스가 해당 메서드를 반드시 오버 라이딩 해야 한다는 제약이 추가 된 것이다.

### 예제3

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/98e5e335-a6c9-4a28-bf63-afcb393b258d/f4d0283a-21c7-4ac6-a476-4f201d7e9cc7.png)

**AstractAnimal**

```java
package poly.ex3;

public abstract class AbstractAnimal {

    public abstract void sound();

    public void move() {
        System.out.println("동물이 움직입니다.");
    }
}
```

- `AbstractAnimal`은 `abstract`가 붙은 추상 클래스. 이 클래스는 직접 인스턴스 생성 불가.
- `sound()`는 `abstract`가 붙은 추상메서드, 이 메서드는 반드시 자식 클래스에서 오버라이딩 해야함.

`move()` 메서드는 추상 메서드가 아님. 따라서 자식 클래스에서 오버라이딩 하지 않아도 됨.

**Dog**

```java
package poly.ex3;

public class Dog extends AbstractAnimal {

    @Override
    public void sound() {
        System.out.println("bow! bow!");
    }
}
```

**Cat**

```java
package poly.ex3;

public class Cat extends AbstractAnimal {

    @Override
    public void sound() {
        System.out.println("mew~ mew~");
    }
}
```

**Caw**

```java
package poly.ex3;

public class Caw extends AbstractAnimal {

    @Override
    public void sound() {
        System.out.println("baa~ baa~");
    }
}
```

**AbstractMain**

```java
package poly.ex3;

public class AbstractMain {

    public static void main(String[] args) {
        //추상클래스 생성 불가
        //AbstractAnimal animal = new AbstractAnimal();

        Dog dog = new Dog();
        Cat cat = new Cat();
        Caw caw = new Caw();

        cat.sound();
        cat.move();

        soundAnimal(dog);
        soundAnimal(cat);
        soundAnimal(caw);
    }

    public static void soundAnimal(AbstractAnimal animal) {
        System.out.println("동물 소리 테스트 시작");
        animal.sound();
        System.out.println("동물 소리 테스트 종료");
    }
}
```

**실행결과**

```java
mew~ mew~
동물이 움직입니다.
동물 소리 테스트 시작
bow! bow!
동물 소리 테스트 종료
동물 소리 테스트 시작
mew~ mew~
동물 소리 테스트 종료
동물 소리 테스트 시작
baa~ baa~
동물 소리 테스트 종료
```

추상 클래스는 생성이 불가함. 컴파일 오류 발생

```java
//추상클래스 생성 불가
AbstractAnimal animal = new AbstractAnimal();
```

**컴파일 오류 - 인스턴스 생성**

```java
java: poly.ex3.AbstractAnimal is abstract; cannot be instantiated
```

추상 메서드는 반드시 오버라이딩 해야 하낟. 만약 자식에서 오버라이딩 메서드를 만들지 않으면 다음과 같이 컴파일 오류 발생.

**Dog**

```java
package poly.ex3;

public class Dog extends AbstractAnimal {

//    @Override
//    public void sound() {
//        System.out.println("bow! bow!");
    }
}
```

**컴파일 오류 - 오버라이딩 x**

```java
Class 'Dog' must either be declared abstract or implement abstract method 'sound()' in 'AbstractAnimal'
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/88e8fb50-e506-4333-9df0-8695dd530db6/965859cf-d0fe-46d0-a584-70882069685f.png)

추상 클래스는 제약이 추가된 클래스일 뿐이며 메모리, 구조, 실행 결과 모두 동일.

**정리**

- 추상 클래스 덕분에 실수로 `Animal` 인스턴스를 생성할 문제를 근본적으로 해결
- 추상 메서드 덕분에 새로운 동물의 자식 클래스를 만들 때 실수로 `sound()`를 Override 하지 않은 문제 해결

## 추상 클래스 2

**순수 추상 클래스: 모든 메서드가 추상 메서드인 추상 클래스**

`move()` 도 추상클래스로 만들어야한다면, `AbstractAnimal`의 자식들은 모든 기능은 오버라이딩 해야하며, 이를 순수 추상 클래스라 한다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/c0e96b0a-5acc-47a7-92e5-6e62eb977f39/965139fd-f5ea-4cc1-add1-73ac7e493bcc.png)

### 예제4

**AbstactAnimal**

```java
package poly.ex4;

public abstract class AbstractAnimal {

    public abstract void sound();

    public abstract void move();
}
```

**Dog**

```java
package poly.ex4;

public class Dog extends AbstractAnimal {
    @Override
    public void sound() {
        System.out.println("bow! bow!");
    }

    @Override
    public void move() {
        System.out.println("개가 이동합니다.");
    }
}
```

**Cat**

```java
package poly.ex4;

public class Cat extends AbstractAnimal {
    @Override
    public void sound() {
        System.out.println("mew~ mew~");
    }

    @Override
    public void move() {
        System.out.println("고양이가 이동합니다.");
    }
}
```

**Caw**

```java
package poly.ex4;

public class Caw extends AbstractAnimal {
    @Override
    public void sound() {
        System.out.println("baa~ baa~");
    }

    @Override
    public void move() {
        System.out.println("소가 이동합니다.");
    }
}
```

**AbstractMain**

```java
package poly.ex4;

public class AbstractMain {
    public static void main(String[] args) {

        Dog dog = new Dog();
        Cat cat = new Cat();
        Caw caw = new Caw();

        soundAnimal(dog);
        soundAnimal(cat);
        soundAnimal(caw);

        moveAnimal(dog);
        moveAnimal(cat);
        moveAnimal(caw);
    }

    public static void soundAnimal(AbstractAnimal animal) {
        System.out.println("동물 소리 테스트 시작");
        animal.sound();
        System.out.println("동물 소리 테스트 종료");
    }

    public static void moveAnimal(AbstractAnimal animal) {
        System.out.println("동물 이동 테스트 시작");
        animal.move();
        System.out.println("동물 이동 테스트 종료");
    }
}
```

**실행 결과**

```java
동물 소리 테스트 시작
bow! bow!
동물 소리 테스트 종료
동물 소리 테스트 시작
mew~ mew~
동물 소리 테스트 종료
동물 소리 테스트 시작
baa~ baa~
동물 소리 테스트 종료
동물 이동 테스트 시작
개가 이동합니다.
동물 이동 테스트 종료
동물 이동 테스트 시작
고양이가 이동합니다.
동물 이동 테스트 종료
동물 이동 테스트 시작
소가 이동합니다.
동물 이동 테스트 종료
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/787b813f-5289-4d38-ad7e-6e680fc3d6f0/db386c12-0a9e-4c68-bd5b-509727856227.png)

**순수 추상 클래스**

모든 메서드가 추상 메서드인 추상 클래스는 바디부분이 전혀 없다.

**AbstractAnimal**

```java
package poly.ex4;

public abstract class AbstractAnimal {

    public abstract void sound();

    public abstract void move();
}
```

이러한 순수 추상 클래스는 실행 로직을 전혀 가지고 있지 않고 다형성을 위한 부모 타입으로 껍데기만 제공.

순수 추상 클래스는 다음과같은 특징

- 인스턴스를 생성할 수 없다.
- 상속 시 자식은 모든 메서드를 오버라이딩 해야 한다.
- 주로 다형성을 위해 사용한다.

**상속하는 클래스는 모든 메서드를 구현해야함**

이런 특징을 잘 생각해보면 순수 추상 클래스는 마치 어떤 규격을 구현해야하는 것처럼 느껴진다.

`AbstractAnimal`의 경우 `sound()`, `move()`라는 규격에 맞추어 구현해야한다.

이것은 우리가 일반적으로 말하는 인터페이스같이 느껴진다.

이런 순수 추상 클래스의 개념은 프로그래밍에서 매우 자주 사용된다. 자바는 순수 클래스를 더 편리하게 사용할 수 있도록 인터페이스라는 개념을 제공.

## 인터페이스

자바는 순수 추상 클래스를 더 편리하게 사용할 수 있는 인터페이스라는 기능을 제공

**순수 추상 클래스**

```java
public abstract class AbstractAnimal {
    public abstract void sound();
    public abstract void move();
}
```

인터페이스는 `class`가 아니라 `interface` 키워드 사용

**인터페이스**

```java
public interface InterfaceAnimal {
    public abstract void sound();
    public abstract void move();
}
```

인터페이스 - public abstract 키워드 생략 가능

```java
public interface InterfaceAnimal {
    void sound();
    void move();
}
```

순수 추상 클래스는 다음과 같은 특징을 가진다.

- 인스턴스를 생성 할 수 없다.
- 상속 시 모든 메소드를 오버 라이딩 해야 한다.
- 주로 다양성을 위해 사용된다.

인터페이스는 앞서 설명한 수 추상 클래스와 같다. 여기에 약간의 편의 기능이 추가 된다.

- 인터페이스 매스터 드는 모두 `public abstract` 다.
- 메소드의 `public abstract` 를 생략 할 수 있다. 참고로 생략이 권장 된다.
- 인터페이스는 다중 구현(다중 상속)을 지원한다.

**인터페이스와 멤버 변수**

```java
public interface InterfaceAnimal {
    public static final int MY_PI = 3.14;
}
```

인터페이스에서 멤버 변수는 `public static final` 이 모두 포함되어 있다고 간주.

즉, 정적이면서 고칠 수 없는 변수 ⇒ 상수라 하고 관례상 대문자에 언더스코어(`_`)로 구분

해당 키워드는 다음과 같이 생략 가능(생략이 권장됨)

```java
public interface InterfaceAnimal {
    int MY_PI = 3.14;
}
```

### 예제5

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/0e0afa69-6b85-4863-8d95-2bbded0c5e86/c779da42-04d6-4fa6-a459-029db3b6a008.png)

클래스 상속 관계에서 UML에서 실선을 사용하지만, 인터페이스 구현(상속) 관계는 UML에서 점선을 사용.

**InterfaceAnimal**

```java
package poly.ex5;

public interface IntefaceAnimal {
    void sound();  //public abstract
    void move();  //public abstract
}
```

**Dog**

```java
package poly.ex5;

public class Dog implements IntefaceAnimal {
    @Override
    public void move() {
        System.out.println("bow! bow!");
    }

    @Override
    public void sound() {
        System.out.println("개가 이동합니다.");
    }
}
```

**Cat**

```java
package poly.ex5;

public class Cat implements IntefaceAnimal {

    @Override
    public void sound() {
        System.out.println("mew~ mew~");
    }

    @Override
    public void move() {
        System.out.println("고양이가 이동합니다.");
    }
}
```

**Caw**

```java
package poly.ex5;

public class Caw implements IntefaceAnimal {
    @Override
    public void sound() {
        System.out.println("baa~ baa~");
    }

    @Override
    public void move() {
        System.out.println("소가 이동합니다.");
    }
}
```

**InterfaceMain**

```java
package poly.ex5;

public class InterfaceMain {
    public static void main(String[] args) {
        //인터페이스 생성 불가
        //IntefaceAnimal intefaceAnimal = new IntefaceAnimal()

        Dog dog = new Dog();
        Cat cat = new Cat();
        Caw caw = new Caw();

        soundAnimal(dog);
        soundAnimal(cat);
        soundAnimal(caw);

        moveAnimal(dog);
        moveAnimal(cat);
        moveAnimal(caw);
    }

    public static void soundAnimal(IntefaceAnimal animal) {
        System.out.println("동물 소리 테스트 시작");
        animal.sound();
        System.out.println("동물 소리 테스트 종료");
    }

    public static void moveAnimal(IntefaceAnimal animal) {
        System.out.println("동물 이동 테스트 시작");
        animal.move();
        System.out.println("동물 이동 테스트 종료");
    }
}
```

**실행결과**

```java
동물 소리 테스트 시작
개가 이동합니다.
동물 소리 테스트 종료
동물 소리 테스트 시작
mew~ mew~
동물 소리 테스트 종료
동물 소리 테스트 시작
baa~ baa~
동물 소리 테스트 종료
동물 이동 테스트 시작
bow! bow!
동물 이동 테스트 종료
동물 이동 테스트 시작
고양이가 이동합니다.
동물 이동 테스트 종료
동물 이동 테스트 시작
소가 이동합니다.
동물 이동 테스트 종료
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/35a14bb3-981d-4a1f-8f0c-3ec31859e678/6a1089e5-a6fb-4930-8dd6-a7e378fe3b99.png)

앞서 설명한 순수 추상 클래스 예제와 거의 유사. 순수 추상클래스가 인터페이스가 되었을 뿐.

**클래스, 추상 클래스, 인터페이스는 모두 똑같다.**

- 클래스, 추상 클래스, 인터페이스는 프로그램 코드, 메모리 구조상 모두 똑같다. 모두 자바에서 `.class` 로 다루어진다. 인터페이스를 작성할 때도 `.java` 에 인터페이스를 정의한다.
- 인터페이스 순수 추상 클래스와 비슷하다.

**상속 vs 구현**

부모 클래스의 기능을 자식 클래스가 상속 받을 때, 클래스는 상속 받는다고 표현하지만, 부모 인터페이스의 기능을 자식이 상속 받을 때는 인터페이스를 구현한다고 표현. 이렇게 서로 다르게 표현하는 이유는 상속은 이름 그대로 부모의 기능을 물려 받는 것이 목적이다. 하지만 인터페이스는 모든 메서드가 추상 메서드이다. 따라서 물려받을 수 있는 기능이 없고, 오히려 인터페이스에 정의한 모든 메서드를 자식이 오버라이딩 해서 기능을 구현해야 한다. 따라서 구현한다고 표현.

인터페이스는 메서드 이름만 있는 설계도, 이 설계도가 실제 어떻게 작동하는지는 하위 클래스에서 모두 구현. 따라서 인터페이스의 경우 상속이 아니라 해당 인터페이스를 구현한다고 표현.

**인터페이스를 사용해야 하는 이유**

모든 메서드가 추상 메서드인 경우 순수 추상 클래스를 만들어도 되고, 인터페이스를 만들어도 된다.

- **제약**: 인터페이스를 만드는 이유는 인터페이스를 구현하는 곳에서 인터페이스의 메서드를 반드시 구현해라는 규약(제약)을 주는 것. 그런데 순수 추상 클래스의 경우 미래에 누군가 그곳에 실행 가능한 메서드를 끼워 넣을 수 있다. 이렇게 되면 추가된 기능을 자식 클래스에서 구현하지 않을 수도 있고, 또 더 순수 추상 클래스가 아니게 된다. 인터페이스는 모든 메서드가 추상 메서드이다. 따라서 이런 문제를 원천 차단할 수 있다.
- **다중 구현**: 자바에서 클래스 상속은 부모를 하나만 지정할 수 있다. 반면에 인터페이스는 부모를 여러명 두는 다중 구현(다중 상속)이 가능하다.

좋은 프로그램은 제약이 있는 프로그램.

> **참고**
자바8에 등장한 `default` 메서드를 사용하면 인터페이스도 메서드를 구현 할 수 있다. 하지만 이것은 예외적으로 아주 특별한 경우에만 사용. 자바9에서 등장한 인터페이스의 `private` 메서드도 마찬가지다.
>

## 인터페이스 - 다중 구현

**자바가 다중 상속을 지원하지 않는 이유 - 복습**

자바는 다중 상속을 지원하지 않는다. 그래서 `extend` 대상은 하나만 선택할 수 있다. 부모를 하나만 선택할 수 있다는 뜻. 물론 부모가 또 부모를 가지는 것 괜찮다.

**다중 상속 그림**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/4c67b0c7-2423-488a-b6d7-d9f6f20e453c/77a63659-11d8-4c34-99c3-aad2f62cd200.png)

만약 비행기와 자동차를 상속 받아서 하늘을 나는 자동차를 만든다고 가정해보자. 만약 그림과 같이 다중 상속을 사용하게 되면 `AirplaneCar` 입장에서 `move()` 를 호출할 때 어떤 부모의 `move()`를 사용해야 할지 애매한 문제가 발생한다. 그리고 다중 상속을 사용하면 클래스 계층 구조가 매우 복잡해지 수 있다. 이런 문제점 때문에 자바는 클래스의 다중 상속을 허용하지 않는다. 대신에 인터페이스의 다중 구현을 허용하여 이러한 문제를 피한다.

클래스는 앞서 설명한 이유로 다중 상속이 안되는데, 인터페이스의 다중 구현은 허용한 이유는?

인터페이스는 모두 추상 메서드로 이루어져 있음.

**인터페이스 다중 구현 그림**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/2e675194-8334-41b7-87e0-2d3dc11833d4/4af6968c-ad70-4c07-a7aa-5eb58644af91.png)

`InterfaceA`, `InterfaceB` 는 둘다 같은 `methodCommon()` 을 가지고 있다. 그리고 `Child` 는 두 인터페이스를 구현했다. 상속 관계의 경우 두 부모 중에 어떤 한 부모의 `methodCommon()` 을 사용해야할지 결정해야하는 다이아몬드 문제 발생.

하지만 인터페이스 자신은 구현을 가지지 않는다. 대신에 인터페이스를 구현하는 곳에서 해당 기능을 모두 구현해야 한다. 여기서 `InterfaceA`, `InterfaceB`는 같은 이름의 `methodCommon()`를 제공하지만 이것의 기능은 `Child`가 구현한다. 그리고 오버라이딩에 의해 어차피 `Child`에 있는 `methodCommon()`이 호출된다.

결과적으로 두 부모 중 어떤 한 부모의 `methodCommon()`을 선택하는 것이 아니라 그냥 인터페이스들을 구현한 `Child`에 있는 `methodCommon()`이 사용된다. 이런 이유로 인터페이스는 다이아몬드 문제가 발생하지 않는다. 인터페이스의 경우 다중 구현을 허용한다.

예제를 코드로 작성.

**InterfaceA**

```java
package poly.diamond;

public interface InterfaceA {
    void methodA();
    void methodCommon();
}
```

**InterfaceB**

```java
package poly.diamond;

public interface InterfaceB {
    void methodB();
    void methodCommon();
}
```

**Child**

```java
package poly.diamond;

public class Child implements InterfaceA, InterfaceB{
    @Override
    public void methodA() {
        System.out.println("Child.methodA");
    }

    @Override
    public void methodCommon() {
        System.out.println("Child.methodCommon");
    }

    @Override
    public void methodB() {
        System.out.println("Child.methodB");
    }
}
```

- `implement InterfaceA, InterfaceB`와 같이 다중 구현 가능.
- `implements` 키워드 위에 `,` 로 여러 인터페이스를 구분하면 됨.
- `methodCommon()`의 경우 양쪽 인터페이스에 다 있지만 같은 메서드이므로 구현은 하나만 하면 됨.

**DiamondMain**

```java
package poly.diamond;

public class DiamondMain {

    public static void main(String[] args) {
        InterfaceA a = new Child();
        a.methodA();
        a.methodCommon();

        InterfaceB b = new Child();
        b.methodB();
        b.methodCommon();
    }
}
```

**실행 결과**

```java
Child.methodA
Child.methodCommon
Child.methodB
Child.methodCommon
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f50338ee-b918-4639-bbda-bb270c6813a9/ef0a42fd-0638-447d-add7-2b5208e2f913.png)

1. `a.methodCommon()`을 호출하면 먼저 `x001` `Child` 인스턴스 찾음
2. 변수 `a`가 `InterfaceA` 타입이므로 해당 타입에서 `methodCommon()`을 찾음
3. `methodCommon()`은 하위 타입인 `Child`에 오버라이딩 되어 있으므로 `Child` `methodCommon()`호출

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f805ed0f-e0e0-495e-88cd-51eee1d9ff7d/1a3b490f-4b5f-4301-af40-7c6bdea5e3f6.png)

1. `b.methodCommon()`을 호출하면 먼저 `x001` `Child` 인스턴스 찾음
2. 변수 `b`가 `InterfaceB` 타입으로 해당 타입에서 `methodCommon()`을 찾음.
3. `methodCommon()`은 하위 타입인 `Child`에 오버라이딩 되어있으므로, `Child` `methodCommon()` 호출

## 클래스와 인터페이스 활용

- `AbsractAnimal`은 추상클래스
  - `sound()`: 동물 소리를 내기 위한 `sound()` 추상 메서드 제공
  - `move()`: 동물의 이동 표현을 위한 메서드, 이 메서드는 추상 메서드가 아니다. 상속을 목적으로 함.
- `Fly`는 인터페이스. 나는 동물은 이 인터페이스 구현 가능
  - `Bird`, `Chicken`은 날 수 있는 동물. `fly()` 메서드 구현

**AbstractAnimal**

```java
package poly.ex6;

public abstract class AbstractAnimal {

    public abstract void sound();

    public void move() {
        System.out.println("동물이 이동합니다.");
    }
}
```

**Fly**

```java
package poly.ex6;

public interface Fly {
    void fly();
}
```

**Dog**

```java
package poly.ex6;

public class Dog extends AbstractAnimal {
    @Override
    public void sound() {
        System.out.println("baw! baw!");
    }
}
```

**Chicken**

```java
package poly.ex6;

public class Chicken extends AbstractAnimal implements Fly {
    @Override
    public void sound() {
        System.out.println("kackadoldo");
    }

    @Override
    public void fly() {
        System.out.println("닭 날기");
    }
}
```

**Bird**

```java
package poly.ex6;

public class Bird extends AbstractAnimal implements Fly {

    @Override
    public void sound() {
        System.out.println("Jack! Jack!");
    }

    @Override
    public void fly() {
        System.out.println("새 날기");
    }
}
```

**SoundFlyMain**

```java
package poly.ex6;

public class SoundFlyMain {
    public static void main(String[] args) {
        Dog dog = new Dog();
        Chicken chicken = new Chicken();
        Bird bird = new Bird();

        soundAnimal(dog);  // AbstractAnimal animal = dog -> 업캐스팅
        soundAnimal(chicken);  // AbstractAnimal animal = dog -> 업캐스팅
        soundAnimal(bird);  // AbstractAnimal animal = dog -> 업캐스팅

        flyAnimal(chicken);  //Fly animal = chicken -> 업캐스팅
        flyAnimal(bird);  //Fly animal = bird -> 업캐스팅
    }
    //AbstractAnimal 사용 가능
    public static void soundAnimal(AbstractAnimal animal) {
        System.out.println("동물 소리 테스트 시작");
        animal.sound();
        System.out.println("동물 소리 테스트 종료 \n");
    }

    //Fly 인터페이스 있는 곳 곳 사용 가능
    public static void flyAnimal(Fly animal) {
        System.out.println("동물 날기 테스트 시작");
        animal.fly();
        System.out.println("동물 날기 테스트 종료 \n");
    }
}
```

**실행 결과**

```java
동물 소리 테스트 시작
baw! baw!
동물 소리 테스트 종료 

동물 소리 테스트 시작
kackadoldo
동물 소리 테스트 종료 

동물 소리 테스트 시작
Jack! Jack!
동물 소리 테스트 종료 

동물 날기 테스트 시작
닭 날기
동물 날기 테스트 종료 

동물 날기 테스트 시작
새 날기
동물 날기 테스트 종료 

```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/3e3daf31-50db-4e4b-a354-ce57b1e372b6/6ac5ba39-d4eb-4e0c-8b00-2545d152bc88.png)

`soundAnimal(AbstractAnimal animal)`

`AbstractAnimal`를 상속한 `Dog`, `Bird`, `Chicken`을 전달해 실행 가능

**실행과정**

- `soundAnimal(bird)`를 호출
- 메서드 안에서 `animal.sound()` 호출 → 참조 대상인 `x001` `Bird` 인스턴스 찾기
- 호출한 `animal` 변수는 `AbstractAnimal` 타입이다. 따라서 `AbstractAnimal.sound()`찾음
  해당 메서드는 `Bird.sound()`에 오버라이딩 되어 있음.
- `Bird.sound()` 호출

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/75fef501-e491-4f58-bc9a-343f0f9be934/ecbe5f2b-f4b7-4aef-9569-631e3c4f7b45.png)

`flyAnimal(Fly fly)`

`Fly` 인터페이스를 구현한 `Bird`, `Chicken`을 전달해서 실행.

**실행 과정**

- `fly(bird)` 호출
- 메서드 안에서 `fly.fly()`를 호출하면 참조 대상인 `x001` `Bird` 인스턴스 찾음
- 호출한 `fly` 변수는 `Fly` 타입. → `Fly.fly()`찾음 → 해당 메서드는 `Bird.fly()`에 오버라이딩
- `Bird.fly()` 호출