[<back](https://www.notion.so/basic-_-3ee7f95ac24d43f881b7ee2e8f21c210?pvs=21)

---

<aside>
📃 목차

</aside>

---

### 자바 메모리 구조

자바 메모리 구조 - 비유

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/a4ba4b8c-c6fb-4bbe-8bf6-26514ee7f6d3/ffcc392e-6610-4f60-b03c-a35708f4ce52.png)

자바의 메모리 구조

- 메서드 영역: 클래스 정보 보관.(= 붕어빵 틀)
- 스택 영역: 실제 프로그램이 실행되는 영역. 메서드를 실행할 때마다 하나씩 쌓임
- 힙 영역: 객체(인스턴스)가 생성되는 영역. new명령어를 사용하면 이 영역 사용.(= 붕어빵과 배열이 존재하는 공간)

자바 메모리 구조 - 실제

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f932162f-8369-4fc2-9b9d-20d90159bb54/485f6c6a-50d7-4ccc-8ebe-ba4d00a0f909.png)

- 메서드 영역(Method Area): 프로그램을 실행하는데 필요한 공통의 데이터 관리. 프로그램 모든 영역에서 공유
    - 클래스 정보: 클래스의 실행코드(바이트 코드), 필드, 메서드와 생성자 코드 등 모든 실행 코드 존재
    - static 영역: `static` 변수들을 보관.
    - 런타임 상수 풀: 프로그램을 실행하는데 필요한 공통 리터럴 상수 보관.
        - `“hello”`라는 리터럴 문자가 있으면 이런 문자들을 공통으로 묶어서 관리.
        - 그 밖에 상수 관리
- 스택(Stack Area): 자바 실행 시, 하나의 실행 스택이 생성됨. 각 스택 프레임은 지역 변수, 중간 연산 결과, 메서드 호출 정보 포함.
    - 스택 프레임: 스택 영역에 쌓이는 네모 박스가 하나의 스택 프레임.
      메서드를 호출할 때 마다 하나의 스택 프레임이 쌓이고, 메서드가 종료되면 해당 스택프레임 제거
- 힙 영역(Heap Area): 객체(인스턴스)와 배열이 생성되는 영역. 가비지 컬렉션(GC)이 이루어지는 주요 영역이며, 더이상 참조되지 않는 객체는 GC에 의해 제거됨.

> **참고**: 스택 영역은 더 정확히는 각 쓰레드별로 하나의 실행 스택이 생성된다. 따라서 쓰레드 수 만큼 스택 영역이 생성되고 지금은 쓰레드 1개만 사용하므로 스택 영역도 1개이다. 쓰레드에 대한 부분은 멀티 쓰레드를 학습해야 이해가능
>

**메서드 코드는 메서드 영역에**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/b6c8aa81-bbe0-48c0-8868-3a09bc8e6f15/e2c6c2b0-d50a-4d72-a18c-e8303c763da6.png)

인스턴스는 100개 생성하면 100개 모두 다른 값이 할당되어도 메서드는 같은 값이다.

메서드는 공통된 코드이므로 이는 공유한다. 인스턴스 변수에는 메모리가 할당되지만, 메서드에 대한 새로운 메모리 할당은 없다. 메서드는 메서드 영역에서 공통 관리 되고 실행된다.

**정리하면 인스턴스의 메서드를 호출하면 실제로는 메서드 영역에서 코드를 불러 수행함.**

## 스택과 큐 자료구조

### 스택구조

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/335ccf4f-73a7-474e-9018-87058ecac3ec/2b38a1ba-7af2-47c7-b22f-3892487f83eb.png)

블럭을 1→2→3 순서대로 넣을 수 있다.

블럭을 빼려면 위에서 부터 순서대로 빼야한다.

3 → 2 → 1 순으로 뺄 수 있다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/7df98500-c4d3-4a71-a465-dbcac26a12df/d7d09632-98e1-427b-b887-a4a2cd22cad5.png)

**후입선출(LIFO, Last In First Out)**

여기서 가장 마지막에 넣은 3번이 먼저 나온다. 이렇게 나중에 넣은 것이 가장 먼저 나오는 것을 후입 선출이라하고, 이러한 자료 구조를 스택이라 한다.

**선입선출(FIFO, First In Last Out)**

후입 선출과 반대로 가장 먼저 넣은 것이 가장 먼저 나온느 것을 선입 선출이라한다. 이런 자료 구조를 큐(Queue)라한다.

**큐(Queue) 자료구조**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/ac5c0d99-a2b0-458b-8168-6b37a4388e5a/32b9d967-6c6f-40f5-8f32-3823aff6db8f.png)

이런 자료 구조는 각자 필요한 영역이 있다.

예를 들어 선착순 이벤트를 하는데 고객이 대기해야 한다면 큐 자료구조를 사용해야함.

프로그램 실행과 메서드 호출에는 스택 구조가 적합하다. 스택 구조를 학습했으니, 자바에서 스택영역이 어떤방식으로 작동하는지 알아보자

## 스택영역

**JavaMemoryMain**

```java
package Memory;

public class JavaMemoryMain1 {
    public static void main(String[] args) {
        System.out.println("main start");
        method1(10);
        System.out.println("main end");
    }

    static void method1(int m1) {
        System.out.println("method1 start");
        int cal = m1 * 2;
        method2(cal);
        System.out.println("method1 end");
    }

    static void method2(int m2) {
        System.out.println("method2 start");
        System.out.println("method2 end");
    }
}
```

**실행결과**

```java
main start
method1 start
method2 start
method2 end
method1 end
main end
```

**호출 그림**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f1c1e435-6be1-4479-86bb-bae3aedf972f/9878b8f0-9730-4a7a-a50d-6a9215728f55.png)

- 자바 프로그램 실행 시 `main()`을 실행 `main()`을 위한 스택 프레임 하나 생성
    - `main()` 스택 프레임은 내부에 args라는 매개변수를 가진다.
- `main()`은 `method1()`을 호출. `method1()` 스택프레임 생성
    - `method1()`은 `m1`, `cal` 지역변수 (매개변수 포함)를 가지므로 해당 지역 변수들이 스택프레임에 포함
- `method1()`은 `method2()`를 호출. `method2()` 스택 프레임 생성. `methode2()`는 `m2` 지역변수(매개변수)를 가지므로 해당 지역변수가 스택 프레임에 포함

**종료 그림**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f674eae2-d7a0-4f88-b537-6569c16238f1/c5c0b6ff-14c8-4a8c-9293-c94120829c75.png)

- `method2()`가 종료되면 `method2()` 스택 프레임이 제거되고, 매개변수 `m2`도 제거됨.
- `method1()`에서 `method2()`를 호출한 지점으로 돌아가 `method1()`이 스택 프레임에서 제거 됨. 지역변수(매개변수) `m1`, `cal`도 제거됨.
- `main()`으로 돌아가 종료됨. 이 때는 스택 프레임도 완전히 비워지고 프로그램 정리 후 종료.

**정리**

- 자바는 스택영역을 사용해서 메서드 호출과 지역변수(매개변수 포함)를 관리한다.
- 메서드를 계속 호출하면 스택 프레임이 계속 쌓인다.
- 지역 변수(매개변수 포함)는 스택영역에서 관리.
- 스택 프레임이 종료되면 지역 변수도 함께 제거.
- 스택 프레임이 모두 제거되면 프로그램 종료.

## 스택 영역과 힙 영역

**Data**

```java
package Memory;

public class Data {
    int value;

    public Data(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}
```

**JavaMemoryMain2**

```java
package Memory;

public class JavaMemoryMain2 {
    public static void main(String[] args) {
        System.out.println("main start");
        method1();
    }

    static void method1() {
        System.out.println("method1 start");
        Data data1 = new Data(10);
        method2(data1);
        System.out.println("method1 end");
    }

    private static void method2(Data data2) {
        System.out.println("method2 start");
        System.out.println("value= " + data2.getValue());
        System.out.println("method2 end");
    }
}
```

**실행결과**

```java
main start
method1 start
method2 start
value= 10
method2 end
method1 end
```

- `main()` → `method1()` → `method2()` 순서로 호출하는 코드
- `method1()`에서 `Data` 클래스의 인스턴스 생성.
- `method1()`에서 `method2()`를 호출할 때 매개변수에서 `Data` 인스턴스 값 참조하여 전달.

**그림**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/3c4bad16-6ebf-4e04-aeae-7db9255d6dc5/826a0e4b-1a7a-402f-8ddd-3fd3250c137a.png)

- `main()` 메서드 실행시 `main()` 스택 프레임 생성

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/58ecfcb9-84d6-48ca-8bcb-7d28adb4d4a6/425fdc89-35c7-4208-8e37-e169c9cb2a39.png)

- `main()` 에서 `method1()` 실행. `method1()` 스택프레임 생성
- `method1()`은 지역변수로 `Data data1`을 가지고 있음. 지 지역변수도 스택 프레임에 포함
- `method1()`은 `new Data(10)`을 사용해 힙 영역에 `Data` 인스턴스 생성. 그리고 참조값을 `data1`에 보관
-

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/edfcf2ce-b4c6-454d-bc9b-4911eadcaff8/b7601fd0-e4cc-408e-8434-080f439cfe0b.png)

- `method1()`은 `method2()`를 호출하면서 `Data data2` 매개변수에 `x001` 참조 값을 넘김
- 따라서, `method1()`, `method2()` 의 `data1`, `data2` 모두 `x001` 인스턴스 참조

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/9bc94f51-4d47-4e26-a94f-9f757893bbe0/c77aaa1b-7188-4239-92bc-6735276c8690.png)

- `method2()` 스택프레임이 제거되면서 매개변수 `data2`도 함께 제거됨.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/29ca2c83-6351-4372-8dd1-d945b51ed350/916a1073-1792-4891-90d7-b03b16a7c7dc.png)

- `method1()` 스택프레임에 제거 되면서 지역변수 `data1`도 제거됨

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/5916eb70-a67c-4a2e-a821-947bdbd1aeca/3c6e6289-518d-45f7-9d5d-5a87d1e5ceac.png)

- `method1()`이 종료된 직후, `x001` 참조값을 가진 `Data`인스턴스를 참조하는 곳이 더이상 없음.
- 결과적으로 GC(가비지 컬렉션)은 이렇게 참조가 모두 사라진 인스턴스를 찾아서 메모리에서 제거함

참고: 힙 영역 외부가 아닌, 힙 영역 안에서만 인스턴스끼리 서로 참조하는 경우에도 GC의 대상이 된다.

**정리**

- 지역 변수는 스택영역에, 객체(인스턴스)는 힙 영역에 관리되는 것을 확인.

## static 변수1

`static` 키워드는 주로 멤버 변수와 메서드에 사용.

### 인스턴스 내부 변수에 카운트 저장

**Data1**

```java
package static1;

public class Data1 {
    public String name;
    public int count;

    public Data1(String name) {
        this.name = name;
        count++;
    }
}
```

생성된 객체가 생성될 때 마다 생성자를 통해 인스턴스 멤버 변수인 `count`값을증가 시킴.

참고로 예제를 단순화 하기 위해 `public` 사용

**DataCountMain1**

```java
package static1;

public class DataCountMain1 {
    public static void main(String[] args) {

        Data1 data1 = new Data1("A");
        System.out.println("A count= " + data1.count);

        Data1 data2 = new Data1("B");
        System.out.println("B count= " + data2.count);

        Data1 data3 = new Data1("C");
        System.out.println("C count= " + data3.count);
    }
}
```

**실행결과**

```java
A count= 1
B count= 1
C count= 1
```

객체를 생성할 때마다 `Data1` 인스턴스는 새로 만들어진다. 그리고 인스턴스에 포함된 `count`변수도 새로 만들어진다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/ec2baea2-c065-453b-a9ee-9f353a00cdb9/c626feb5-ea44-4621-8b11-bd406eb8127b.png)

처음 `Data(”A”)` 인스턴스를 생성하면 `count` 값은 `0`으로 초기화 된다. 생성자에서 `count++`을 호출했으므로 `count` 값은 `1`이 된다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/bacd7d6e-0ed8-4126-bbff-01f24990e798/9026a942-9ab6-46b0-8266-dccf4f6b4ed8.png)

다음으로 `Data1(”B”)` 인스턴스를 생성하면 완전 새로운 인스턴스를 생성한다. 이 새로운 인스턴스의 `count` 값은 `0`으로 초기화 된다. 생성자에서 `count++`를 했으므로 `count` 값은 `1`이 된다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/506b0f44-3806-45bd-9dee-1d2c23e69bb5/85bab1a6-136f-4838-a284-19134e3e0fb1.png)

다음으로 `Data1(”C”)` 인스턴스를 생성하면 완전 새로운 인스턴스를 생성한다. 이 새로운 인스턴스의 `count` 값은 `0`으로 초기화 된다. 생성자에서 `count++`를 했으므로 `count` 값은 `1`이 된다.

따라서, 인스턴스에서 사용되는 멤버 변수 `count` 값은 인스턴스 끼리 서로 공유되지 않는다. 따라서 원하는 답을 구할 수 없다. 이 문제를 해결하려면 변수를 공유해야 한다.

### 외부 인스턴스에 카운트 저장

**Counter**

```java
package static1;

public class Counter {
    int count;
}
```

**Data2**

```java
package static1;

public class Data2 {
    String name;

    public Data2(String name, Counter counter) {
        this.name = name;
        counter.count++;
    }
}
```

**DataCountMain2**

```java
package static1;

public class DataCountMain2 {
    public static void main(String[] args) {
        Counter counter = new Counter();

        Data2 data1 = new Data2("A", counter);
        System.out.println("A count= " + counter.count);

        Data2 data2 = new Data2("B", counter);
        System.out.println("B count= " + counter.count);

        Data2 data3 = new Data2("C", counter);
        System.out.println("C count= " + counter.count);
    }
}
```

**실행결과**

```java
A count= 1
B count= 2
C count= 3
```

`Counter` 인스턴스를 공용으로 사용한 덕분에 객체를 생성할 때 마다 값을 정확하게 증가시킬 수 있다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/0a898686-a116-4503-907b-a28c0ad76a4b/fd4a25b3-fa7e-464b-a9f1-79ac44028815.png)

`Data2(”A”)` 인스턴스를 생성하면 생성자를 통해 `Counter` 인스턴스에 있는 `count` 값을 하나 증가시킴. `count` 값은 `1`이 된다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/32388c14-beab-4fa5-af18-056f2343f4de/37795547-288a-4fe4-a425-235c3ab5b30d.png)

`Data2(”B”)` 인스턴스를 생성하면 생성자를 통해 `Counter` 인스턴스에 있는 `count` 값을 하나 증가시킴.
`count` = `2`

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/44cc3c47-67f9-4347-8f09-e18be7801f10/8898b961-e429-4697-affb-a59563ea2c15.png)

`Data2(”C”)` 인스턴스를 생성하면 생성자를 통해 `Counter` 인스턴스에 있는 `count` 값을 하나 증가시킴.
`count` = `3`

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/4523f0d3-2123-47c7-a06e-076809cc7bbe/8745853d-0578-4aa1-b94d-449447266d41.png)

결과적으로 `Data2` 인스턴스가 3개 생겼고, `count` 값도 인스턴스 숫자와 같은 3으로 정확하게 측정됨

불편한 점

- `Data2` 클래스와 관련된 일인데, `Counter`라는 별도의 클래스 추가로 사용.
- 생성자의 매개변수도 추가되고, 생성자가 복잡해짐. 생성자를 호출하는 부분도 복잡해짐.

## static 변수2

### static 변수 사용

특정 클래스에서 공용으로 함께 사용할 수 있는 변수

`static` 키워드를 사용하여 공용 변수를 만들 수 있다.

**Data3**

```java
package static1;

public class Data3 {
    public String name;
    public static int count;

    public Data3(String name) {
        this.name = name;
        count++;
    }
}
```

- `static in count` 변수 타입(`int`) 앞에 `static` 키워드 붙음
- 이렇게 멤버 변수에 `static`을 붙이게 되면 `static` 변수, 정적 변수 또는 클래스 변수라 한다.
- 객체가 생성되면 생성자에서 정적 변수 count 값을 하나 증가시킨다.

**DataCountMain3**

```java
package static1;

public class DataCountMain3 {
    public static void main(String[] args) {

        Data3 data1 = new Data3("A");
        System.out.println("A count= " + Data3.count);

        Data3 data2 = new Data3("B");
        System.out.println("B count= " + Data3.count);

        Data3 data3 = new Data3("C");
        System.out.println("C count= " + Data3.count);
    }
}
```

**실행 결과**

```java
A count= 1
B count= 2
C count= 3
```

코드를 보면 `count` 정적 변수에 접근하는 방법이 조금 특이함.

`Data3.count`와 같이 클래스명에 `.`(dot)을 사용. 마치 클래스에 직접 접근하는 것 처럼 느껴진다.

**그림으로 설명**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/61824708-e145-4df0-a5df-d287a3bf4214/1670163e-8ddd-4b70-9302-b62a19676d54.png)

- `static` 이 붙은 멤버 변수는 메서드 영역에서 관리
    - `static`이 붙은 멤버 변수 `count`는 인스턴스 영역에 생성되지 않는다. 대신에 메서드 영역에서 이 변수를 관리.
- `Data3(”A”)` 인스턴스를 생성하면 생성자가 호출된다.
- 생성자에는 `count++`코드가 있다. `count`는 `static`이 붙은 정적 변수이므로 인스턴스 영역이 아니라 메서드 영역에서 관리한다. 따라서 메서드 영역에 있는 `count`의 값이 하나 증가한다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/0c9ffbd2-8679-4d75-ac62-1da3935a8d2b/49889642-6bcf-4360-bc67-7c9db35309f8.png)

- `Data(”B”)` 인스턴스를 생성하면 생성자 호출
- `count++` 코드 실행. `count`는 `static`이 붙은 정적변수. 메서드 영역의 `count` 변수 값 하나 증가

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f718af7e-a761-41ef-a836-9adad16a1beb/9a4cbae0-7f5d-492e-a02f-39a8473a7443.png)

- `Data(”C”)` 인스턴스를 생성하면 생성자 호출
- `count++` 코드 실행. `count`는 `static`이 붙은 정적변수. 메서드 영역의 `count` 변수 값 하나 증가

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/88a96219-999c-4829-88fa-52fcb99b205f/be97912b-f93b-45b6-82b8-4e662780f846.png)

최종적으로 메서드 영역에 있는 `count` 변수의 값은 `3`

`static`이 붙은 정적 변수에 접근하려면 `Data3.count`와 같이 `클래스 명` + `.`(dot) + `변수명`으로 접근.

참고로 `Data3` 의 생성자와 같이 자신의 클래스에 있는 정적 변수라면 클래스 명을 생략할 수 있다.

```java
package static1;

public class Data3 {
    public String name;
    public static int count;

    public Data3(String name) {
        this.name = name;
        Data3.count++;  // -> count++ 로 클래스 명을 생략해도 됨.
    }
}
```

정적변수인 `static` 변수를 사용한 덕분에 공용 변수로 편리하게 문제 해결 가능

**정리**

- `static` 변수는 클래스인 붕어빵 틀이 특별히 관리하는 변수. 붕어빵 틀은 1개이므로 클래스 변수도 하나만 존재.
- 반면 인스턴스인 붕어빵은 인스턴스 수 만큼 변수가 존재(`Data3.name`)

## static 변수3

### 용어정리

```java
package static1;

public class Data3 {
    public String name;
    public static int count;
```

예제 코드에서 `name`, `count`는 둘다 멤버 변수

멤버 변수(필드)는 `static`이 붙은 것 과 아닌 것에 따라 다음과 같이 분류

**멤버 변수(필드)의 종류**

- **인스턴스 변수**: `static` 이 붙지 않은 멤버 변수. 예) `name`
    - static이 붙지 않은 멤버 변수는 인스턴스를 생성해야 사용할 수 있고, 인스턴스에 소속되어 있다. 따라서 인스턴스 변수라고 한다.
    - 인스턴스 변수는 인스턴스를 만들 때 마다 새로 만들어진다.
- **클래스 변수**: `static`이 붙은 멤버 변수. 예) `count`
    - `static`이 붙은 멤버 변수는 인스턴스와 무관하게 클래스에 바로 접근해서 사용할 수 있고, 클래스 자체 소속이다. 따라서 클래스 변수라 한다.
    - 클래스 변수는 자바 프로그램을 시작할 때 딱 1개가 만들어진다. 인스턴스와 다르게 보통 여러곳에서 공유하는 목적으로 사용.

### **변수와 생명주기**

- **지역 변수(매개변수 포함)**: 지역 변수는 스택 영역에 있는 스택 프레임 안에 보관. 메서드가 종료되면 스택 프레임도 제거 되는데 이 때 해당 스택 프레임에 포함된 지역 변수들도 함께 제거됨.
- **인스턴스 변수**: 인스턴스 멤버 변수를 인스턴스 변수라 한다. 힙 영역을 사용하며 GC(가비지 컬렉션)가 발생하기 전까지 생존함.
- **클래스 변수**: 메서드 영역의 `static` 영역에 보관되는 변수. 메서드 영역은 프로그램 전체에서 사용하는 공용 공간으로 클래스 변수는 해당 클래스가 JVM에 로딩 되는 순간 생성됨. 그리고 JVM이 종료될 때 까지 생명주기가 이어짐.

힙 영역에서 생성되는 인스턴스 변수는 동적으로 생성되고, 제거된다. 반면, `static`인 정적 변수는 거의 프로그램 실행 시점에 만들어지고, 프로그램 종료 시점에 제거됨.

### 정적 변수 접근 법

**DataCountMain3 - 추가**

```java
//추가
//인스턴스를 통한 접근
Data3 data4 = new Data3("D");
System.out.println("D count= " + data4.count);

//클래스를 통한 접근
System.out.println("D count= " + Data3.count);
```

**실행 결과 - 추가된 부분**

```java
D count= 4
D count= 4
```

둘다 결과적으로 정적 변수에 접근.

**인스턴스를 통한 접근 `data4.count` → 추천하지 않음**

정적 변수의 경우 인스턴스를 통한 접근은 마치 count가 인스턴스 변수에 접근하는 것처럼 오해할 수 있다. 따라서 추천하지 않음.

**클래스를 통한 접근 `Data3.count`  → 추천**

정적 변수는 클래스에서 공용으로 관리하기 때문에 클래스를 통해 접근하는 것이 더 명확하다. 따라서 정적 변수에 접근할때는 클래스를 통해 접근.

## static 메서드1

예제

특정 문자열을 꾸미는 간단한 기능을 만들어보자.

예를 들어 `“hello”`라는 문자를 꾸미면 앞 뒤에 `*`을 붙여서 `“*hello**”*`와 같이 꾸며주는 기능.

### 인스턴스 메서드

**DecoUtil1**

```java
package static2;

public class DecoUtil1 {

    public String deco(String str) {
        return "*" + str + "*";
    }
}
```

**DecoMain1**

```java
package static2;

public class DecoMain1 {
    public static void main(String[] args) {
        String s = "hello java";
        DecoUtil1 utils = new DecoUtil1();
        String deco = utils.deco(s);

        System.out.println("before: " + s);
        System.out.println("after: " + deco);
    }
}
```

**실행결과**

```java
before: hello java
after: *hello java*
```

앞서 개발한 `deco()`메서드를 호출하기 위해서 `DecoUtil1`의 인스턴스를 먼저 생성해야함.

그러나 `deco()`라는 기능은 멤버 변수도 없고, 단순히 기능만 제공.

인스턴스가 필요한 이유는 멤버 변수(인스턴스 변수)등을 사용하는 목적이 큰데, 이 메서드는 사용하는 인스턴스 변수도 없고 단순히 기능만 제공함.

### static 메서드

**DecoUtil2**

```java
package static2;

public class DecoUtil2 {

    public static String deco(String str) {
        return "*" + str + "*";
    }
}
```

**DecoMain2**

```java
package static2;

public class DecoMain2 {
    public static void main(String[] args) {
        String s = "hello java";
        String deco = DecoUtil2.deco(s);

        System.out.println("before: " + s);
        System.out.println("after: " + deco);
    }
}
```

**실행결과**

```java
before: hello java
after: *hello java*
```

`DecoUtil2.deco(s)`

static이 붙은 정적 메서드는 객체 생성 없이 `클래스 명` + `.`(dot) + `메서드 명`으로 바로 호출 가능.

정적 메서드 덕분에 불피룡한 객체 생성 없이 편리하게 메서드 사용.

**클래스 메서드**

메서드 앞에 `static` 붙일 수 있음.

이것을 **정적 메서드, 클래스 메서드라 한다.**

이는 클래스에서 바로 메서드를 호출하여 사용할 수 있다.

**인스턴스 메서드**

`static`이 붙지 않은 메서드로 인스턴스를 호출해야 사용가능.

## static 메서드2

정적 메서드는 객체 생성없이 클래스에 있는 메서드를 바로 호출할 수 있다는 장점

그러나 어디서나 사용할 수 있는 것은 아님.

- static 메서드는 static만 사용할 수 있다.
    - 클래스 내부의 기능을 사용할 때, 정적 메서드는 `static`이 붙은 정적 메서드나 **정적 변수만 사용할 수 있다.**
    - 클래스 내부의 기능을 사용할 때, 정적 메서드는 인스턴스 변수나, 인스턴스 메서드 사용 불가.
- 반대로 모든 곳에서 `static` 호출 가능.
    - 정적 메서드는 공용 기능. 따라서 접근 제어자만 허락한다면 클래스를 통해 모든 곳에서 `static`을 호출 할 수 있다.

**DecoData**

```java
package static2;

public class DecoData {
    private int instanceValue;
    private static int staticValue;

    public static void staticCall() {  //static 메서드, static 변수 접근
        // instanceValue++;  //인스턴스 변수 접근, compile error
        // instanceMethod();  //인ㅅ턴스 메서드 접근, compile error

        staticValue++;  //정적 변수 접근
        staticMethod();  //정적 메서드 접근
    }

    public void instanceCall() {  //instance 메서드, instancne 변수 접근
        instanceValue++;  //인스턴스 변수 접근, compile error
        instanceMethod();  //인ㅅ턴스 메서드 접근, compile error

        staticValue++;  //정적 변수 접근, DecoData.staticValue++;
        staticMethod();  //정적 메서드 접근, DecoData.staticMethod();
    }

    private void instanceMethod() {
        System.out.println("instanceMethod= " + instanceValue);
    }

    private static void staticMethod() {
        System.out.println("staticMethod= " + staticValue);
    }
}
```

- `instanceValue`: 인스턴스 변수
- `staticValue`: 정적 변수(클래스 변수)
- `instanceMethod()`: 인스턴스 메서드
- `staticMethod()`: 정적 메서드(클래스 메서드)

`staticCall()`메서드

정적 메서드로 `static`만 사용할 수 있음. 정적 변수, 정적 메서드 접근 가능

그러나, `static`이 없는 인스턴스 변수나 인스턴스 메서드는 접근 불가. → 컴파일 오류 발생.

**왜냐하면, 인스턴스 변수 또는 인스턴스 메서드는 새로운 인스턴스가 생성될 때 정의 되므로, 정적메서드 입장에서는 인스턴스 메서드가 가리키는 변수가 무엇인지 알 수 없다.**

따라서 `staticCall()` → `staticMethod()`로 `static`만 호출 가능.

`instanceCall()` 메서드

모든 곳에서 공용인 `static` 호출 가능. 따라서 정적 메서드, 정적 변수, 인스턴스 메서드, 인스턴스 변수 모두 접근 가능.

**이 또한 위와 같은 원리로 static을 통해 정의된 변수나 메서드는 프로그램이 시작되면서 정의 되기 때문에 인스턴스 생성 시 해당 값과 주소를 알 수 있고 접근이 가능하다.**

**DecoDataMain**

```java
package static2;

public class DecoDataMain {
    public static void main(String[] args) {

        //1. 정적 호출
        DecoData.staticCall();

        //2. 인스턴스 호출
        DecoData data1 = new DecoData();
        data1.instanceCall();

        //3. 인스턴스 호출
        DecoData data2 = new DecoData();
        data2.instanceCall();
    }
}
```

**실행결과**

```java
staticMethod= 1
instanceMethod= 1
staticMethod= 2
instanceMethod= 1
staticMethod= 3
```

**그림 설명**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/fee9a801-0dfd-4277-9b55-4a5383d50b4b/e048c741-e467-430a-9d97-144c8190e804.png)

**정적 메서드가 인스턴스의 기능을 사용할 수 없는 이유**

정적 메서드는 클래스 이름을 통해 바로 호출. 그래서 인스턴스처럼 참조값 없음.

특정 인스턴스의 기능을 사용하려면 참조값을 알아야 하는데, 정적 메서드는 참조값 없이 호출한다. 따라서 정적 메서드 내부에서 인스턴스 변수나 인스턴스 메서드를 사용할 수 없다.

만일 정적 메서드라도 매개변수로 인스턴스를 받으면 참조값을 알게 되므로, 인스턴스 변수, 인스턴스 메서드에 접근 가능하다.

```java
public static void staticCall(DecoData data) {
    data.instanceValue++;
    data.instanceCall();
}
```

## static 메서드3

### 용어 정리

**멤버 메서드의 종류**

- **인스턴스 메서드**: `static`이 붙지 않은 멤버 메서드
- **클래스 메서드**: `static`이 붙은 멤버 메서드
    - 클래스 메서드, 정적 메서드, `static` 메서드 로 불림

`static`이 붙지 않은 메서드는 인스턴스를 생성해야 사용할 수 있고, 인스턴스 소속이다. 따라서 인스턴스 메서드라 한다. 그러나 `static`이 붙은 메서드는 클래스에 직접 접근해 사용할 수 있고 클래스 자체 소속이다. 따라서 클래스 메서드라 한다.

**정적 메서드 활용**

정적 메서드는 객체 생성이 필요 없이 메서드의 호출만으로 필요한 기능을 수행할 때 주로 사용.

예를 들어 간단한 메서드만으로 구성된 클래스를 만들 주로 사용한다.

수학 계산 같은 경우 여러가지 기능을 담은 클래스를 만들 수 있는데, 이 경우 인스턴스 변수 없이 값을 계산하고 반환하는 것이 대부분이다. 이럴떄 정적 메서드를 사용해서 유틸리티성 메서드를 만들면 유용하다.

**DecoDataMain - 추가**

```java
//추가
//인스턴스를 통한 접근
DecoData data3 = new DecoData();
data3.staticCall();

//클래스를 통한 접근
DecoData.staticCall();
```

**인스턴스를 통한 접근 `data3.staticCall()` → 추천하지 않음**

정적 메서드의 경우 인스턴스를 통한 접근은 마치 `staticCall()`가 인스턴스 메서드에 접근하는 것처럼 오해할 수 있다. 따라서 추천하지 않음.

**클래스를 통한 접근 `DecoData.staticCall()`  → 추천**

정적 메서드는 클래스에서 공용으로 관리하기 때문에 클래스를 통해 접근하는 것이 더 명확하다. 따라서 정적 메서드에 접근할때는 클래스를 통해 접근.

**static import**

정적 메서드르 사용할 때 해당 메서드를 자주 호출해야한다면 `static import` 기능을 고려

```java
DecoData.staticCall();
DecoData.staticCall();
DecoData.staticCall();
```

`import static` 기능을 사용하면 `staticCall();` 만으로 해당 메서드를 호출할 수 있다.

```java
import static static2.DecoData.staticCall;

public class DecoDataMain {
    public static void main(String[] args) {

        //1. 정적 호출
        staticCall();
        staticCall();
        ...
}
```

**DecoDataMain - static import 적용**

```java
package static2;

import static static2.DecoData.staticCall;
//import static static2.DecoData.*;

public class DecoDataMain {
    public static void main(String[] args) {

        //1. 정적 호출
        staticCall();
        staticCall();
        staticCall();

        //2. 인스턴스 호출
        DecoData data1 = new DecoData();
        data1.instanceCall();

        //3. 인스턴스 호출
        DecoData data2 = new DecoData();
        data2.instanceCall();

        //4. 정적 호출
        staticCall(data1);

        //추가
        //인스턴스를 통한 접근
        DecoData data3 = new DecoData();
        staticCall();

        //클래스를 통한 접근
        staticCall();
    }
}

```

특정 클래스의 정적 메서드 하나만 적용하려면 다음과 같이 생략할 메서드 명을 적어주면 된다.

`import static static2.DecoData.staticCall;`

특정 클래스의 모든 정적 메서드를 적용하려면 다음과 같이 `*` 을 사용한다.

`import static static2.DecoData.*;`

참고로, `import static`은 정적 메서드 뿐만아니라 정적 변수에도 사용할 수 있다.

### main() 메서드는 정적 메서드

인스턴스 없이 실행하는 가장 대표적인 메서드가 바로 `main()` 메서드.

`main()`메서드는 프로그램을 시작하는 시작점이 되는데, 객체를 생성하지 않아도 `main()`메서드가 작동했다. 이는 `main()`메서드가 `static`이기 때문.

정적 메서드는 정적 메서드만 호출할 수 있다. 따라서 정적 메서드는 `main()`이 호출하는 메서드에는 정적 메서드를 사용했다.

정확히 말하면 정적 메서드는 같은 클래스 내부에서 정적 메서드만 호출할 수 있다. 따라서 정적 메서드인 `main()` 메서드가 같은 클래스에서 호출하는 메서드도 정적 메서드로 선언해서 사용.

**main() 메서드와 static 메서드 호출 예**

```java
public class ValueDataMain {

    public static void main(String[] args) {
        ValueData valuData = new ValueData();
        add(valueData);
    }
    
    static void add(ValueData valueData) {
        valueData.value++;
        System.out.println("숫자 증가 value=" + valueData.value);
    }
}
```

## 문제와 풀이

### 문제1: 구매한 자동차 수

생성한 차량 수를 출력하는 프로그램 작성.

`Car` 클래스 작성!

**Car**

```java
package static2.ex;

public class Car {
    private String name;
    private static int totalCars;

    public Car(String name) {
        this.name = name;
        System.out.println("차량 구입, 이름: " + name);
        totalCars++;
    }

    public static void showTotalCars() {
        System.out.println("구매한 차량 수: " + totalCars);
    }
}
```

**CarMain**

```java
package static2.ex;

public class CarMain {

    public static void main(String[] args) {
        Car car1 = new Car("K3");
        Car car2 = new Car("G80");
        Car car3 = new Car("Model Y");

        Car.showTotalCars();  //구매한 차량 수를 출력하는 static 메서드
    }
}
```

**실행결과**

```java
차량 구입, 이름: K3
차량 구입, 이름: G80
차량 구입, 이름: Model Y
구매한 차량 수: 3
```

### 문제2: 수학 유틸리티 클래스

다음 기능을 제공하는 배열용 수학 유틸리티 클래스(`MathArrayUtils`)를 만드세요.

- `sum(int[] array)`: 배열의 모든 용소를 더하여 합계 반환.
- `average(int[] array)`: 배열의 모든 요소의 평균값 계산.
- `min(int[] array)`: 배열에서 최소값 찾기.
- `max(int[] array)`: 배열에서 최대값 찾기.

**요구사항**

- `MathArrayUtils`는 객체를 생성하지 않고 사용. 누군가 실수로 `MathArrayUtils`의 인스턴스를 생성하지 못하도 록 막으세요
- 실행코드에 `static import`를 사용해도 됩니다.

**실행코드**

**MathArrayUtilsMain**

```java
package static2.ex;

import static static2.ex.MathArrayUtils.*;

public class MathArrayUtilsMain {

    public static void main(String[] args) {
        int[] values = {1, 2, 3, 4, 5};
        System.out.println("sum= " + sum(values));
        System.out.println("average= " + average(values));
        System.out.println("min= " + min(values));
        System.out.println("max= " + max(values));
    }
}
```

**실행 결과**

```java
sum= 15
average= 3.0
min= 1
max= 5
```

**MathArrayUtils**

```java
package static2.ex;

public class MathArrayUtils {

    private MathArrayUtils() {
        //private로 인스턴스 생성을 막는다.
    }
    
    public static int sum(int[] values) {
        int total = 0;
        for (int i : values) {
            total += i;
        }
        return total;
    }

    public static double average(int[] values) {
        return (double) sum(values) / values.length;
    }

    public static int min(int[] values) {
        int minValue = values[0];
        for (int i : values) {
            if (minValue > i) {
                minValue = i;
            }
        }
        return minValue;
    }

    public static int max(int[] value) {
        int maxValue = value[0];
        for (int i : value) {
            if (maxValue < i) {
                maxValue = i;
            }
        }
        return maxValue;
    }
}
```