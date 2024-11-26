## 스코프1 - 지역변수와 스코프

변수는 선언한 위치에 따라 지역 변수, 멤버 변수(클래스 변수, 인스턴스 변수)와 같이 분류

지금까지 학습한 변수는 모두 로컬 변수(Local Variable)로 한국말로 지역 변수

지역변수는 이름 그대로 특정 지역에서만 사용가능

특정 지역을 벗어나면 사용 불가.

지역변수는 코드 블록`({})` 이다. 지역변수는 자신이 선언된 코드블록 `({})` 안에서만 생존하고, 자신이 선언된 코드 블록을 벗어나면 제거된다.

**Scope1**

```java
package Scope;

public class Scope1 {

    public static void main(String[] args) {
        int m = 10;  // m 생존시작
        if (true) {
            int x = 20;  // x 생존시작
            System.out.println("if m = " + m);
            System.out.println("if x = " + x);
        }  //x 생존 종료

        //System.out.println("mian x = " + x);  // 오류, 변수 x 접근 불가
        System.out.println("mian m = " + m);
    }  // m 생존 종료
}
```

**실행결과**

if m = 10
if x = 20
mian m = 10

- `int m`
    - `int m`은 `main{}`의 코드 블록안에서 선언됨. 따라서, 변수를 선언한 시점부터 `main{}`의 코드 블록이 종료될 때까지 생존
    - `if{}`블록 내부에서도 외부 블록에서 선언된 `m`에 접근 가능
- `int x`
    - `int x`는 `if{}`블록안에서 선언됨. 따라서 `if{}`의 코드 블록이 종료될 때까지 생존
    - `if{}` 내부에서는 자신의 범위에서 선언한 `x`에 당연히 접근 가능
    - `if{}` 코드 블록이 끝나면 `x`는 제거됨. 따라서 더는 `x`에 접근 불가
    - 따라서, 이후 접근하면 `cannot find symbol`이라는 변수 이름을 찾을 수 없다는 컴파일 오류 발생

정리하면 지역 변수는 본인의 코드 블록 안에서만 생존.

자신의 코드 블록을 벗어나면 제거.

이렇게 변수의 접근이 능한 범위를 스코프(Scope)라 한다. 참고로 Scope를 번역하면 범위라는 뜻.

`int m`은 `main{}` 전체에서 접근할 수 있기 때문에 스코프가 넓고 `int x`는 `if{}` 코드 블록 안에서만 접근할 수 있기 때문에 스코프가 짧다.

**Scope2**

```java
package Scope;

public class Scope2 {

    public static void main(String[] arge) {
        int m = 10;
        for (int i = 0; i < 2; i++) {  //블록 내부, for문 내
            System.out.println("for m = " + m);  //블록 내부에서 외부는 접근 가능
            System.out.println("for i = " + i);
        }  // i 생존 종료
        //System.out.println("main i = " + i);  // 오류, i에 접근 불가
        System.out.println("main m = " + m);
    }
}
```

**실행결과**

for m = 10
for i = 0
for m = 10
for i = 1
main m = 10

for문으로 바뀐 것을 제외하면 앞의 예제와 비슷.

for문의 경우 for(int i = 0; …)과 같이 for문 안에서 초기식에 직접 변수를 선언 가능.

그리고 이렇게 선언한 변수는 for문 코드 블록안에서만 사용 가능.

## 스코프2 - 스코프 존재 이유

**Scope3_1**

```java
package Scope;

public class Scope3_1 {
    public static void main(String[] args) {
        int m = 10;
        int temp = 0;
        if (m > 0) {
            temp = m * 2;
            System.out.println("temp = " + temp);
        }
        System.out.println("m = " + m);
    }
}
```

위 코드는 좋은 코드라고 볼 수 없다.

이유는 `temp`는 `if`조건이 만족할 때 임시로 잠깐 사용하는 변수다. 그러나 `temp`는 `main()`코드 블록에 선언되어있다. 이렇게 되면 다음과 같은 문제가 발생한다.

1. **비효율적인 메모리 사용**
    1. `temp`는 `if`코드 블록에서만 필요하지만, `main()`코드 블록이 종료될 때 까지 메모리에 유지됨.
    2. 만약 `if` 코드 블록 안에 `temp`를 선언했다면, java를 구현하는 곳에서 `if`코드 블록의 종료 시점에서 이 변수를 메모리에서 제거해서 더 효율적으로 메모리를 사용할 수 있다.
2. **코드 복잡성 증가**
    1. 좋은 코드는 군더더기 없는 단순한 코드
    2. 만약 `if`코드 안에 `temp`코드를 선언했다면 `if` 가 끝나고  나면 `temp`를 전혀 생각하지 않아도 됨.
    3. 그러나 위의 경우 `if`코드 블록이 끝나도 `main()` 어디서나 `temp`를 여전히 접근할 수 있다.
    4. 누군가 이 코드를 유지보수 할 때 `m`은 물론이고 `temp`까지 계속 신경써야한다.
    5. 스코프가 불필요하게 넓은 것은 코드가 복잡한 실무에서 비효율적이다.

`temp`를 꼭 필요한 곳에 한정해보자

**Scope3_2**

```java
package Scope;

public class Scope3_2 {
    public static void main(String[] args) {
        int m = 10;
        if (m > 0) {
            int temp = 0;
            temp = m * 2;
            System.out.println("temp = " + temp);
        }
        System.out.println("m = " + m);
    }
}
```

`temp`를 `if`코드 블록 안에서 선언함.

이제 `temp`는 `if`코드 블록 안으로 스코프가 줄어든다. 덕분에 `temp` 메모리를 빨리 제거해서 메모리를 효율적으로 사용하고, `temp`변수를 생각해야하는 범위를줄여서 더 유지보수 하기 좋은 코드를 만듦.

## while문 vs for문 - 스코프 관점

**while**

```java
package loop;

public class While2_3 {

    public static void main(String[] args) {

        int sum = 0;
        int i = 1;
        int endNum = 100;

        while (i <= endNum) {
            sum += i;
            System.out.println("i=" + i + " sum=" + sum);
            i++;
        }
    }
}
```

**for**

```java
package loop;

public class For2 {

    public static void main(String[] args) {
        int sum = 0;
        int endNum = 3;

        for (int i = 1; i <= endNum; i++) {
            sum += i;
            System.out.println("i = " + i + " sum = " + sum);
        }
    }
}
```

변수의 스코프 관점에서 카운터 변수 `i`를 비교해보자.

- `while`문의 경우 변수 `i`의 스코프가 `main()` 메서드 전체가 된다. 반면에 for문의 경우 변수 `i`의 스코프가 for문 안으로 한정됨.
- 따라서, 변수 `i`와 같이 for문 안에서만 사용되는 카운터 변수가 있다면 `while`문 보다는 for문을 사용해서 스코프의 범위를 제한하는 것이 메모리 사용과 유지보수 관점에서 더 좋다.

**정리**

- 변수는 꼭 필요한 범위로 한정해서 사용하는 것이 좋다. 변수의 스코프는 꼭 필요한 곳으로 한정해서 사용하자. 메모리를 효율적으로 사용하고 더 유지보수하기 좋은 코드를 만들 수 있다.
- **좋은 프로그램은 무한한 자유가 있는 프로그램이 아닌 적절한 제약이 있는 프로그램이다.**

## 형변환1 - 자동 형변환

**형변환**

- 작은 범위에서 큰 범위로는 당연히 값을 넣을 수 있다.
    - 예) `int` → `long` → `double`
- 큰 범위에서 작은 범위는 다음과 같은 문제가 발생할 수 있다.
    - 소수점 버림
    - 오버플로우

**작은 범위에서 큰 범위로 대입은 허용된다.**

자바에서 숫자를 표현할 수 있는 범위는 다음과 같다.

`int < long < double`

`int`보다는 `long`이, `long`보다는 `double`이 더 큰 범위를 표현할 수 있다.

작은 범위에서 큰 범위에 값을 대입하는 다음 코드를 실행하면 특별한 문제없이 잘 수행됨

**Casting 1**

```java
package casting;

public class Casting1 {

    public static void main(String[] args) {
        int intValue = 10;
        long longValue;
        double doubleValue;

        longValue = intValue;  // int -> long
        System.out.println("longValue = " + longValue);  // longValue = 10

        doubleValue = intValue;  // int -> double
        System.out.println("doubleValue = " + doubleValue);  // doubleValue = 10.0

        doubleValue = 20L;  // long -> double
        System.out.println("doubleValue2 = " + doubleValue);  // doubleValue = 20.0
    }
}
```

**실행결과**

longValue = 10
doubleValue = 10.0
doubleValue2 = 20.0

- 자바는 기본적으로 같은 타입의 값을 대입할 수 있다.
- 다른 타입의 값을 대입할 때에는 작은 범위에서 큰 범위로의 대입은 자바 언어에서 허용한다.
    - `int` → `long`을 비교해보면 `long`이 `int`보다 더 큰 숫자 범위를 표현
    - `long` → `double`의 경우에도 `double`은 부동 소수점을 사용하기 때문에 더 큰 숫자 범위를 표현

**자동 형변환**

하지만 결국 대입하는 형(타입)을 맞추어야 하기 때문에 개념적으로는 다음과 같이 동작함

```java
//intValue = 10
doubleValue = intValue
doubleValue = (double) intValue  //형 맞추기
doubelValue = (double) 10  //변수 값 읽기
doubleValue = 10.0  //형 변환
```

이렇게 앞에 (double)과 같이 적어주면 int형이 double형으로 형이 변한다.

작은 범위 숫자 타입에서 큰 범위 숫자 타입으로 대입은 개발자가 이렇게 직접 형변환하지 않아도 된다.

이런 과정이 자동으로 일어나기 때문에 **자동 형변환**, 또는 **묵시적 형변환**이라고 한다.q

## 형변환 2 - 명시적 형변환

이이번에는 반대로 큰 범위에서 작은 범위로 대입.

**큰 범위에서 작은 범위 대입은 명시적 형변환이 필요**

`double`은 실수를 표현할 수 있다. 따라서 `1.5`가 가능하다. 그런데 `int`는 실수를 표현할 수 없다. 이 경우 `double` → `int`로 대입하면?

**Casting2**

```java
package casting;

public class Casting2 {

    public static void main(String[] args) {
        double doubleValue = 1.5;
        int intValue = 0;

        //intValue = doubleValue;  //컴파일 오류 발생
        intValue = (int) doubleValue;  //형변환
        System.out.println(intValue);
    }
}
```

**실행 결과**

1

다음 코드의 앞부분에 있는 주석을 풀면(주석을 제거하면) 컴파일 오류가 발생한다.

`intValue = doubleValue  //컴파일 오류 발생`

`java : incompatible types: possible lossy conversion from double to int
//java: 호환되지 않는 유형: double에서 int로의 가능한 손실 변환`

int형은 double형보다 숫자의 표현 범위가 작다.

그리고 실수를 표현할 수도 없다. 따라서 이 경우 숫자가 손실되는 문제가 발생할 수 있다.

예를 들어 은행 프로그램이 고객에게 은행 이자를 계산해서 입금해야 하는데 만약 이런 코드가 아무런 오류 업이 수행된다면 끔찍한 문제를 만들 수 있다. 그래서 자바는 이런 경우 컴파일 오류를 발생시킨다. 항상 강조하지만 컴파일 오류는 문제를 가장 빨리 발견할 수 있는 좋은 오류이다.

**형변환**

하지만 만약 이런 위험을 개발자가 직접 감수하고도 값을 대입하고 싶다면 데이터 타입을 강제로 변경할 수  있다.

예를 들어서 대략적인 결과를 보고 싶은데, 이 때 소수점을 버리고 정수로만 볼 수 있다.

형변환은 다음과 같이 변경하고 싶은 데이터 타입을 (int)와 같이 괄호를 사용해서 명시적으로 입력하면 도니다.

`intValue = (int) doubleValue;  //형변환` 

이것을 형(타입)으르 바꾼다고 해서 형변환이라 한다. 영어로는 캐스팅.

개발자가 직접 형변환 코드를 입력한다고 해서 명시적 형변환이라 한다.

**캐스팅 용어**

“캐스팅”은 영어 단어 “cast”에서 유래되었다. “cast”는 금속이나 다른 물질을 녹여서 특정한 형태나 모양으로 만드는 과정을 의마한다.

**명시적 형변환 과정**

//doubleValue = 1.5
intValue = (int) doubleValue;
intValue = (int) 1.5;  //doubleValue에 있는 값을 읽는다.
intValue = 1;  //(int)로 형변환 한다. intValue에 int형인 숫자 1을 대입한다

형변환 후 출력해보면 숫자 `1` 이 출력되는 것을 확인할 수 있다.

참고로 형변환을 한다고 해서 `doubleValue` 자체의 타입이 변경되거나 그 안에 있는 값이 변경되는 것은 아니다. `doubleValue` 에서 읽은 값을 형변환 하는 것.

`doubleValue` 안에 들어있는 값은 `1.5` 로 그대로 유지된다. 참고로 변수의 값은 대입연산자(`=`)를 사용해서 직접 대입할 때만 변경됨.

**형변환과 오버플로우**

형변환을 할 때 만약 작은 숫자가 표현할 수 있는 범위가 넘어서면

**Casting3**

```java
package casting;

public class Casting3 {
    public static void main(String[] args) {
        long maxIntValue = 2147483647;  //int 최고값
        long maxIntOver = 2147483648L;  //int 최고값 + 1(초과)
        int intValue = 0;

        intValue = (int) maxIntValue;  //형변환
        System.out.println("maxIntValue casting= " + intValue);  //출력:2147483647

        intValue = (int) maxIntOver;  //형변환
        System.out.println("maxIntOver casting= " + intValue);  //출력:-2147483648
    }
}
```

**출력 결과**

maxIntValue casting= 2147483647
maxIntOver casting= -2147483648

**정상 범위**

`long maxIntValue = 2147483647` 를 보면 `int`로 표현할 수 있는 가장 큰 숫자인 `2147483647`를 입력했다. 이 경우 `int`로 표현할 수 있는 범위에 포함되기 때문에 다음과 같이 `long` → `int`로 형변환을 해도 아무런 문제가 없다.

`intValue = (int) maxIntValue;  //형변환`

수행과정

```java
maxIntValue = 2147483647;  //int 최고값
intValue = (int) maxIntValue;  //변수 값 읽기
intValue = (int) 2147483647;  //형변환
intValue = 2147483647;
```

**초과 범위**

다음으로 `long maxIntOver = 2147483648L`를 보면 `int`로 표현할 수 있는 가장 큰 숫자인 `2147483647`보다 1 큰 숫자를 입력.

이 숫자는 리터럴은 `int` 범위를 넘어가기 때문에 마지막에 `L`을 붙여서 `long`형을 사용해야한다.

이경우 `int`로 표현할 수 있는 범위를 넘기 때문에 다음과 같이 `long` → `int`로 형변환 하면 문제가 발생.

`intValue = (int) maxInOver;  //형변환` 

수행과정

```java
maxIntOver = 2147483648L;  //int 최고값 + 1
intValue = (int) maxInOver;  //변수 값 읽기
intValue = (int) 2147483648L;  //형변환 시도
intValue = -2147483648;
```

- 결과를 보면 `-2147483648` 이라는 전혀 다른 숫자가 보인다.
    - int형은 `2147483648L` 를 표현할 수 있는 방법이 없다. 이렇게 기존 범위를 초과해서 표현하게 되면 전혀 다른 숫자가 표현되는데 이 현상을 오버플로우라 한다.
- 보통 오버플로우가 발생하면 마치 시계가 한바퀴 돈 것처럼 다시 처음부터 시작한다.
    - 참고로 `-2147483648` 숫자는 `int` 의 가장 작은 숫자이다.
- 중요한 것은 오버플로우가 발생하는 것 자체가 문제라는 점!
    - 오버플로우가 발생했을 때 결과가 어떻게 되는지 계산하는데 시간 낭비하면 안된다.
    - 오버플로우 자체가 발생하지 않도록 막아야한다. 이 경우 단순히 대입하는 변수(`intValue`)의 타입을 `int` → `long`으로 변경해서 사이즈를 늘리면 오버플로우 문제가 해결된다

## 계산과 형변환

형변환은 대입 뿐만 아니라, 계산을 할 때도 발생한다.

**Casting4**

```java
package casting;

public class Casting4 {

    public static void main(String[] args) {
        int div1 = 3 / 2;
        System.out.println("div1 = " + div1);  //1

        double div2 = 3 / 2;
        System.out.println("div2 = " + div2);  //1.0

        double div3 = 3.0 / 2;
        System.out.println("div3 = " + div3);  //1.5

        double div4 = (double) 3 / 2;
        System.out.println("div4 = " + div4);  //1.5

        int a = 3;
        int b = 2;
        double result = (double) a / b;
        System.out.println("result = " + result);  //1.5
    }
}
```

**실행 결과**

div1 = 1
div2 = 1.0
div3 = 1.5
div4 = 1.5
result = 1.5

자바에서 계산은 다음 2가지를 기억하자.

1. **같은 타입끼리의 계산은 같은 타입의 결과를 낸다.**
    1. `int` + `int`는 `int`를 `double` + `double`의 결과가 나온다.
2. **서로 다른 타입의 계산은 큰 범위로 자동 형변환이 일어난다.**
    1. `int` + `double`은 `double` + `double`로 자동 형변환이 일어난다.

다양한 타입별로 더 자세히 들어가면 약간 차이가 있지만 이 기준으로 이해하면 충분

예시를 통해 자세히 이해해보기

```java
int div1 = 3 / 2;  //int / int
int div1 = 1;  //int / int이므로 int타입으로 결과가나온다.
```

```java
double div2 = 3 / 2;  //int / int
double div2 = 1;  //int / int이므로 int타입결과가 나온다.
double div2 = (double) 1;  //int -> double에 대입해야 한다. 자동 형변환 발생
double div2 = 1.0;  //1(int) -> 1.0(double)로 형변환 되었다.
```

```java
double div3 = 3.0 / 2;  //double / int
double div3 = 3.0 / (double) 2;  //double / int이므로, double / double로 형변환 발생
double div3 = 3.0 / 2.0;  //double / double -> double이 된다.
double div3 = 1.5
```

```java
double div4 = (double) 3 / 2;  //명시적 형변환을 사용했다. (double) int / int
double div4 = (double) 3 / (double) 2;  //double + int이므로, double + double로 형변환이 발생
double div4 = 3.0 / 2.0;  //double / double -> double이 된다.
double div4 = 1.5;
```

3 / 2와 같이 int형끼리 나눗셈을 해서 소수까지 구하고 싶다면 div4의 예제처럼 명시적 형변환 사용

변수를 사용하는 경우에도 다음과같이 형변환 가능

```java
int a = 3;
int b = 2;
double result = (double) a / b;
```

**처리과정**

```java
double result = (double) a / b;  //(double) int / int
double result = (double) 3 / 2;  //변수 값 읽기
double result = (double) 3 / (double) 2;  //double + int 이므로 더 큰 범위로 형변환
double result = 3.0 / 2.0;  //(double / double) -> double이 된다.
double result = 1.5;
```
