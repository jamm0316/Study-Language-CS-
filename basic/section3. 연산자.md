## 산술연산자

- 주로 숫자 계산
    - 나머지 계산 연산자 %
    - int형 끼리 계산하면 정수형이기 때문에 소수점 이하는 포함할 수 없다 (ex. 2.5 → 2)
- 주의! 0으로 나누기
    - `10 / 0` 과 같이 숫자는 0으로 나눌 수 없다
    - `Exception in thread "main" java.lang.ArithmeticException: / by zero` 오류 출력

```java
package Operator;

public class Operator1 {
    public static void main(String[] args) {
        //변수 초기화
        int a = 5;
        int b = 2;

        //덧셈
        int sum = a + b;
        System.out.println("a + b = " + sum); //출력 a + b = 7

        //뺄셈
        int diff = a - b;
        System.out.println("a - b = " + diff);

        //곱셈
        int multi = a * b;
        System.out.println("a * b = " + multi);

        //나눗셈
        int div = a / b;
        System.out.println("a / b = " + div);

        //나머지
        int mod = a % b;
        System.out.println("a % b = " + mod);

        int z = 10 / 0;
    }
}

```

---

## 문자열 더하기

- 문자열에 `+`를 이용해 더하면 문자열을 이어준다
- 문자열에 숫자를 더하면 숫자를 문자로 바꾸어 더한다

```java
package Operator;

import java.sql.SQLOutput;

public class Operator2 {

    public static void main(String[] args) {

        //문자열과 문자열 더하기 1
        String result1 = "hello " + "world";
        System.out.println(result1);

        //문자열과 문자열 더하기2
        String s1 = "String1";
        String s2 = "String2";
        String result2 = s1 + s2;
        System.out.println(result2);

        //문자열과 숫자 더하기1
        String result3 = "a + b = " + 10;
        System.out.println(result3);  //문자열에 숫자를 문자열로 바꾸어서 더해버림

        //문자열과 숫자 더하기2
        int num = 20;
        String str = "a + b = ";
        String result4 = str + num;  //문자열에 숫자를 문자열로 바꾸어서 더해버림
        System.out.println(result4);
    }
}

```

---

## 연산자 우선순위

<aside>
📄 연산자 우선순위를 외우는 것보다 직관적으로 **이해되는 코드가 잘 짠 코드다**

1. **상식선에서 우선순위를 사용하자**
2. **애매하면 괄호()를 사용하자**

---

- 연산자 우선순위를 상식선에서 생각하고, 애매하면 괄호를 사용하자.
- 누구나 코드를 보고 쉽고 명확하게 이해할 수 있어야 한다.
    - 복잡하면 괄호를 넣자.
- 개발에서 가장 중요한 것은 단순함과 명확함이다. 애매하거나 복잡하면 안된다.
</aside>

```java
package Operator;

public class Operator4 {

    public static void main(String[] args) {
        int sum3 = 2 * 3 + 3 * 3;
        int sum4 = (2 * 3) + (3 * 3);
        System.out.println("sum3 = " + sum3);
        System.out.println("sum4 = " + sum4);  //better code
        //같은 값이라도 이처럼 괄호를 써서 직관적으로 이해되게 짠 코드가 잘 짠 코드다.
    }
}
```

---

## 증감 연산자

- `++` `--`로 표기
- 해당 변수의 숫자 값을 하나 증가 또는 감소 시키는 것
    
    ```java
    package Operator;
    
    public class OperatorAdd1 {
    
        public static void main(String[] args) {
            int a = 0;
    
            a = a + 1;
            System.out.println("a = " + a);
    
            a = a + 1;
            System.out.println("a = " + a);
    
            //증감 연산자
            ++a; //a = a + 1
            System.out.println("a = " + a);
            ++a; //a = a + 1
            System.out.println("a = " + a);
        }
    }
    ```
    
- 전위, 후위 증감연산자
    - ++a(전위 증가연산자): 변수의 값을 먼저 증가시키기고, 그 결과를 대입
        
        ```
        a = 1, b = 0
        b = ++a  //전위 증감 연산자
        
        a = a + 1 //a의 값을 1증가 시킴
        b = a  //a의 값을 b에 대입
        
        결과 : a = 2, b = 2
        ```
        
    - a++(후위 증가연산자): 변수를 먼저 대입시키고, 값을 증가시킴
        
        ```
        a = 1, b = 0
        b = a++  //후위 증감 연산자
        
        b = a  //a의 값을 b에 먼저 대입
        a = a + 1  //a의 값을 1증가 시킴
        
        결과 : a = 1, b = 2
        ```
        
    - 증감 연산자를 단독으로 쓰면 전위를 쓰든 후위를 쓰든 동일하다.
        
        ```java
        int a = 0;
        
        ++a;
        System.out.println("a = " + a);  //출력 a = 1
        
        a++;
        System.out.println("a = " + a);  //출력 a = 1
        ```
        

---

## 비교 연산자

두값을 비교한는데 사용

**비교연산자**

- `==` : 동등성
- `!=` : 불일치
- `>` : 크다
- `<` : 작다
- `>=` : 크거나 같다
- `<=` : 작거나 같다

비교 연산자를 사용하면 참(`true`) 또는 거짓(`false`)이라는 결과가 나온다. 참 거짓은 `boolean`형을 사용

여기서 주의할 점은 `=`와 `==`이 다르다는 점

- `=` : 대입 연산자, 변수에 값 대입
- `==` : 동등한지 확인하는 비교 연산자

불일치 연산자는 `!=` 를 사용한다. `!` 는 반대라는 뜻

**Comp1**

```java
package Operator;

public class Comp1 {
    public static void main(String[] args) {
        int a = 2;
        int b = 3;

        System.out.println(a == b);  //false, a와 b는 같지 않다
        System.out.println(a != b);  //true, a와 b는 다르다
        System.out.println(a > b);  //false, a와 b보다 크다
        System.out.println(a < b);  //true, a와 b보다 작다
        System.out.println(a <= b);  //true, a와 b보다 작거나 크다
        System.out.println(a >= b);  //false, a와 b보다 작거나 크다

        boolean result = false;
        System.out.println(result);
    }
}
```

---

## 문자열 비교

문자열이 같은지 비교할 때는 `==`이 아니라, `.equals()` 메서드를 사용

`==`를 사용하면 성공할 때도 있지만 실패할 때도 있다.

`.equals()` 메서드를 사용해야 한다 정도로 알고 있자.

**Comp2**

```java
package Operator;

public class Comp2 {
    public static void main(String[] args) {
        String str1 = "문자열1";
        String str2 = "문자열2";

        boolean result1 = "hello".equals("hello");  //리터럴 비교
        boolean result2 = str1.equals("문자열1");  //문자열 변수, 리터럴 비교
        boolean result3 = str2.equals(str2);  //문자열 변수 비교

        System.out.println("result1 = " + result1);
        System.out.println("result2 = " + result2);
        System.out.println("result3 = " + result3);
    }
}
```

---

## 논리 연산자

논리 연산자는 boolean형인 true, false를 비교하는데 사용

**논리 연산자**

- `&&`(그리고): 모두 참이면 참, 나머지 모두 거짓
- `||`(또는): 모두 거짓이면 거짓, 나머지는 모두 참
- `!`(부정): 서로 반대 되는 값 출력(참 → 거짓, 거짓 → 참)

**Logical1**

```java
package Operator;

public class Logical1 {
    public static void main(String[] args) {
        System.out.println("&&: AND 연산");
        System.out.println(true && true);  //true
        System.out.println(true && false);  //false
        System.out.println(false && false);  //false

        System.out.println("||: OR 연산");
        System.out.println(true || true);  //true
        System.out.println(true || false);  //true
        System.out.println(false || false);  //false

        System.out.println("!연산");
        System.out.println(!true);  //false
        System.out.println(!false);  //true

        System.out.println("변수활용");
        boolean a = true;
        boolean b = false;
        System.out.println(a && b);  //false
        System.out.println(a || b);  //true
        System.out.println(!a);  //false
        System.out.println(!b);  //true
    }
}
```

**Logical2**

```java
package Operator;

public class Logical2 {
    public static void main(String[] agrs) {
        int a = 15;
        //a는 10보다 크고 20보다 작다

        // boolean result = (a > 10) && (a < 20);
        boolean result = (10 < a) && (a < 20);  //이렇게 사용하면 좀 더 직관적으로 이해할 수 있다.
        System.out.println("result = " + result);
    }
}
```

---

## 대입 연산자

**대입연산자**

변수 할당 연산자

**축약(복합) 대입 연산자**

`+=`, `-=`, `*=`, `/=`, `%=`

변수에 값을 연산하고 할당하라는 뜻

**Assign1**

```java
package Operator;

public class Assign1 {

    public static void main(String[] args) {
        int a = 5;
        a += 3; // 8 (5 + 3) : a = a + 3
        a -= 2; // 6 (8 - 2) : a = a - 2
        a *= 4; // 24 (6 * 4) : a = a * 4
        a /= 3; // 8 (24 / 3) : a = a / 3
        a %= 5; // 3 (8 % 3) : a = a % 5
        System.out.println(a);
    }
}
```

**실행결과**

3

## 문제와 풀이

문제1 - int와 평균

다음과 같은 작업을 수행하는 프로그램을 작성하세요.

클래스 이름은 `OperationEx1` 라고 적어주세요

1. `num1`, `num2`, `num3` 라는 이름의 세 개의 `int` 변수를 선언 하고, 각각 10, 20, 30으로 초기화 하세요.
2. 세 변수의 합을 계산 하고, 그 결과 `sum` 이라는 이름에 `int` 변수의 저장 하세요.
3. 세 변수의 평균을 계산하고 그 결과를 `average` 라는 이름의 `int` 변수의 저장하세요 평균 계산시 소숫점 이하의 결과는 버림 하세요.
4. `sum`과 `average` 변수의 값을 출력하세요.

**OperationEx1**

```java
package Operator;

public class OperationEx1 {

    public static void main(String[] args) {
        int num1 = 10;
        int num2 = 20;
        int num3 = 30;

        int sum = num1 + num2 + num3;
        int average = sum / 3;

        System.out.println(sum);
        System.out.println(average);
    }
}
```

**실행결과**

60
20

## 문제2 - double과 평균

클래스 이름: OperationEx2

```java
// 다음 double 변수들을 선언하고 그 합과 평균을 출력하는 프로그램을 작성하세요.
double val1 = 1.5;
double val2 = 2.5;
double val3 = 3.5;
```

**OperationEx2**

```jsx
package Operator;

public class OperationEx1 {

    public static void main(String[] args) {
        int num1 = 10;
        int num2 = 20;
        int num3 = 30;

        int sum = num1 + num2 + num3;
        int average = sum / 3;

        System.out.println(sum);
        System.out.println(average);
    }
}
```

실행결과

7.5
2.5

## 문제3 - 합격 범위

클래스 이름 : OperationEx3

- `int`형 변수 `score`를 선언
- `score` 가 80점 이상이고, 100점 이하이면 `true`를 출력하고, 아니면 `false`를 출력

**OperationEx3**

```java
package Operator;

public class OperationEx3 {

    public static void main(String[] args) {
        int score = 80;
        boolean result = (80 <= score) && (score <= 100);
        System.out.println(result);
    }
}
```

실행결과

True
