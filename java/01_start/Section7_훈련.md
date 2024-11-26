## Scanner 학습

변수, 연산자, 조건문, 반복문은 프로그래밍의 가장 기본이 되는 기능. 

대부분의 프로그램 언어는 이 기능을 필수로 가지며, 프로그래머가 하는 일의 대부분은 지금까지 설명한 변수, 연산자, 조건문, 반복문을 다루는 일.

그래서 이 기능을 다루는 것은 무엇보다 중요.

> **백문이 불여일타**
변수, 연산자, 조건문, 반복문을 머리로 이해하는 것은 전혀 어렵지 않다. 하지만 중요한 것은 코딩은 몸으로 익히는 것이다. 그러기 위해서는 직접 코딩하는 것이 무엇보다 중요하다. 예제 코드는 모두 따라해보고, 문제도 직접 다 풀어보자. 문제가 안풀리면 답을 보고 코드를 따라친 다음 기존 코드를 모두 지우고 처음부터 본인이 스스로 다시 풀어보아도 좋다. 백문이 불여일타!
> 

### Scanner

`System.out`을 통해서 출력 했듯이, `System.in`을 통해 사용자의 입력을 받을 수 있다.

그런데 자바가 제공하는 `System.in`을 통해서 사용자 입력을 받으려면 여러 과정을 거쳐야해서 복잡하고 어렵다. 자바는 이런문제를 해결하기 위해 `Scanner`라는 클래스를 제공한다. 이 클래스를 사용하면 사용자 입력을 매우 편리하게 받을 수 있다.

### Scanner예제1

**scanner1**

```java
package scanner;

import java.util.Scanner;

public class Scanner1 {
    public static void main(String[] agrs) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("문자열을 입력하세요: ");
        String str = scanner.nextLine();  // 입력 String을 가져옵니다.
        System.out.println("입력한 문자열: " + str);

        System.out.print("정수를 입력하세요: ");
        int intValue = scanner.nextInt();  // 입력 int를 가져옵니다.
        System.out.println("입력한 정수: " + intValue);

        System.out.print("실수를 입력하세요: ");
        double doubleValue = scanner.nextDouble();  // 입력 doubel을 가져옵니다.
        System.out.println("입력한 실수: " + doubleValue);
    }
}
```

**실행결과**

문자열을 입력하세요: hello java
입력한 문자열: hello java
정수를 입력하세요: 100
입력한 정수: 100
실수를 입력하세요: 10.5
입력한 실수: 10.5

- `Scanner scanner = new Scanner(System.in);`
    - 객체와 클래스를 배워야 정확히 이해할 수 있음. 지금은 `Scanner`의 기능을 사용하기 위해 `new` 를 사용해서 `Scanner`를 만든다 정도로 이해. `Scanner`는 `System.in`을 사용해서 사용자의 입력을 편리하게 받도록 도와준다.
    - `Scanner scanner` 코드는 `scanner` 변수를 선언한는 것임. 이제부터 `scanner`변수를 통해서 `scanner`를 사용할 수 있다.
- `scanner.nextLine()`
    - 엔터( `\n` )를 입력할 때 까지 문자를 가져온다.
- `scanner.nextInt()`
    - 입력을 `int`형으로 가져온다. 정수 입력에 사용
- `scanner.nextDouble()`
    - 입력을 `double`형으로 가져온다. 실수 입력에 사용.

**주의! - 다른 타입 입력시 오류**

타입이 다르면 오류가 발생. 예제와 같이 숫자에 문자를 입력하면 오류가 발생.

```java
문자열을 입력하세요: 즐
입력한 문자열: 즐
정수를 입력하세요: hello
Exception in thread "main" java.util.InputMismatchException
	at java.base/java.util.Scanner.throwFor(Scanner.java:964)
	at java.base/java.util.Scanner.next(Scanner.java:1619)
	at java.base/java.util.Scanner.nextInt(Scanner.java:2284)
	at java.base/java.util.Scanner.nextInt(Scanner.java:2238)
	at scanner.Scanner1.main(Scanner1.java:14)
```

**print() vs println()**

다음 코드를 보면 `println()`이 아니라 `print()`를 사용한다.

`System.out.print(”문자열을 입력하세요:”)`

그 이유는 다음과 같다.

**print()** 출력하고 다음 라인으로 넘기지 않는다.

```java
System.out.print("hello");
System.out.print("world");
//결과: helloworld
```

**println()** 출력하고 다음 라인으로 넘긴다.

```java
System.out.println("hello");
System.out.println("world");
//결과:
hello
wolrd
```

우리가 엔터 키를 치면(`\n`)이라는 문자를 남기는 것.

이 문자는 영어로 new line character, 한글로 줄바꿈 문자 또는 개행문자라고 하는데, 이름 그대로 새로운 라인으로 넘기라는 뜻이다. 콘솔에서는 이 문자를보고 다음라인으로 넘김.

`println()`은 `print()`의 마지막에 `\n`을 추가한다. 따라서 다음 코드는 `println()`과 같다.

```java
System.out.print("hello\n");
System.out.print("world\n");
//결과:
hello
world
```

## Scanner - 기본 예제

### Scanner 예제2

두 수를 입력받고 그 합을 출력하는 예제

**Scanner2**

```java
package scanner;

import java.util.Scanner;

public class Scanner2 {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("첫 번째 정수를 입력하세요: ");
        int intValue1 = scanner.nextInt();

        System.out.print("두 번째 정수를 입력하세요: ");
        int intValue2 = scanner.nextInt();

        int sum = intValue1 + intValue2;
        System.out.println(sum + sum);
        System.out.println("두 숫자의 합: " + sum + sum);
        System.out.println("두 숫자의 합: " + intValue1 + intValue2);
    }
}
```

**실행결과**

```java
첫 번째 정수를 입력하세요: 10
두 번째 정수를 입력하세요: 15
50
두 숫자의 합: 2525
두 숫자의 합: 1015
```

- `System.out.println(sum + sum)` ⇒ int 반환
- `System.out.println("두 숫자의 합: " + sum + sum)` ⇒ String 반환
- `System.out.println("두 숫자의 합: " + intValue1 + intValue2)` ⇒ String 반환

### Scanner 예제3

사용자로부터 두 개의 정수를 입력 받고, 더 큰 수를 출력하는 프로그램을 작성해보자. 두 숫자가 같은 경우 두 숫자는 같다고 출력.

**Scanner3**

```java
package scanner;

import java.util.Scanner;

public class Scanner3 {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("첫 번째 수를 입력하세요: ");
        int num1 = scanner.nextInt();

        System.out.print("두 번째 수를 입력하세요: ");
        int num2 = scanner.nextInt();

        if (num1 > num2) {
            System.out.println(num1);
        } else if (num1 < num2) {
            System.out.println(num2);
        } else {
            System.out.println("두 수는 같습니다.");
        }
    }
}
```

**실행결과**

```java
첫 번째 수를 입력하세요: 15
두 번째 수를 입력하세요: 15
두 수는 같습니다.
```

```java
첫 번째 수를 입력하세요: 20
두 번째 수를 입력하세요: 2
더 큰 숫자는: 20
```

## Scanner - 반복 예제

위에는 일회성 프로그램. 실제 프로그램은 사용자가 프로그램을 종료할 때 까지 반복해서 실행됨. 이제부터 사용자의 입력을 지속적으로 받아들이고, 반복해서 동작하는 프로그램 개발.

`Scanner`와 반복문을 함께 사용

### Scanner 반복 예제1

- 사용자가 입력한 문자열을 그대로 출력하는 예제
- `exit`라는 문자가 입력되면 프로그램 종료.
- 프로그램은 반복해서 실행

**ScannerWhile1**

```java
package scanner;

import java.util.Scanner;

public class ScannerWhile1 {

    public static void main(String[] agrs) {
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.print("문자열을 입력하세요(종료:exit): ");
            String str = scanner.nextLine();
            if (str.equals("exit")) {
                System.out.println("프로그램을 종료합니다.");
                break;
            }
            System.out.println("입력한 문자열: " + str);
        }
    }
}
```

**실행 결과**

```java
문자열을 입력하세요(종료:exit): 안녕?
입력한 문자열: 안녕?
문자열을 입력하세요(종료:exit): hello
입력한 문자열: hello
문자열을 입력하세요(종료:exit): exit
프로그램을 종료합니다.
```

`while (true)`: 중간에 `break`문을 만나기 전까지 무한 반복.

```java
if (str.equals("exit")) {
    System.out.println("프로그램을 종료합니다.");
    break;
}
```

입력 받은 문자가 `“exit”`이면 “프로그램을 종료합니다” 출력하고 `break`문을 통해서 while문을 빠져나간다.

### Scanner 반복 예제2

- 첫 번째 숫자와 두 번째 숫자를 더해서 출력하는 프로그램 개발
- 첫 번째 숫자와 두번째 숫자 모두 0을 입력하면 프로그램을 종료한다.
- 프로그램은 반복해서 실행된다.

**ScannerWhile2**

```java
package scanner;

import java.util.Scanner;

public class ScannerWhile2 {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("첫 번째 숫자와 두 번째 숫자 모두 0을 입력하면 프로그램을 종료합니다.");
        while (true) {
            System.out.print("정수를 입력하세요: ");
            int num1 = scanner.nextInt();

            System.out.print("정수를 입력하세요: ");
            int num2 = scanner.nextInt();

            if (num1 == 0 && num2 == 0) {
                System.out.println("프로그램을 종료합니다.");
                break;
            }
            int sum = num1 + num2;
            System.out.println("두 숫자의 합: " + sum);
        }
    }
}
```

실행결과

```java
첫 번째 숫자와 두 번째 숫자 모두 0을 입력하면 프로그램을 종료합니다.
정수를 입력하세요: 10
정수를 입력하세요: 20
두 숫자의 합: 30
정수를 입력하세요: 0
정수를 입력하세요: 0
프로그램을 종료합니다.
```

`num1 == 0 && num2 == 0`

- 입력 받은 `num1`, `num2` 둘다 함께 0 일 때 `“프로그램을 종료합니다.”`를 출력하고 `break`를 통해 while문을 빠져나간다.
- 비교연산자를 사용했다. `true && true` → `true` 이다. 따라서 두 조건이 모두 참이어야 if문의 코드 블럭이 실행된다.

### Scanner 반복 예제3

사용자 입력을 받아 그 합계를 계산하는 프로그램 작성

사용자는 한 번에 한 개의 정수를 입력할 수 있으며, 사용자가 0을 입력하면 프로그램은 종료됨.

이 때, 프로그램이 종료될 때까지 사용자가 입력한 모든 정수의 합을 출력해야함.

**ScannerWhile3**

```java
package scanner;

import java.util.Scanner;

public class ScannerWhile3 {

    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        int sum = 0;
        System.out.println("0을 입력하면 프로그램이 종료됩니다. ");

        while (true) {
            System.out.print("정수를 입력하세요: ");
            int number = input.nextInt();

            if (number == 0) {
                System.out.println("프로그램을 종료합니다.");
                break;
            }
            sum += number;
        }

        System.out.println("입력한 모든 정수의 합: " + sum);
    }
}
```

**실행결과**

```java
0을 입력하면 프로그램이 종료됩니다. 
정수를 입력하세요: 1
정수를 입력하세요: 2
정수를 입력하세요: 3
정수를 입력하세요: 4
정수를 입력하세요: 5
정수를 입력하세요: 0
프로그램을 종료합니다.
입력한 모든 정수의 합: 15
```

- `int sum`: 이곳에 사용자가 입력한 값을 누적
- `sum += number`: 사용자가 입력한 `number`값을 `sum`에 누적
    - `+=`를 사용했으므로 다음 코드와 같다: `sum = sum + number`

## 문제와 풀이1

### 문제 - 이름 나이 입력받고 출력하기

사용자로부터 입력받은 이름과 나이를 출력하세요. 출력형태는 “당신의 이름은 [이름]이고, 나이는[나이]살 입니다” 이어야 합니다.

실행결과 예시

```
당신의 이름을 입력하세요: 자바
당신의 나이를 입력하세요: 30
당신의 이름은 자바이고, 나이는 30살입니다.
```

**ScannerEx1**

```java
package ScannerEx;

import java.util.Scanner;

public class ScannerEx1 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.print("당신의 이름을 입력하세요: ");
        String name = input.nextLine();

        System.out.print("당신의 나이를 입력하세요: ");
        int age = input.nextInt();

        System.out.println("당신의 이름은 " + name +"이고, 나이는" + age + "살 입니다.");
    }
}
```

**실행결과**

```java
당신의 이름을 입력하세요: evan
당신의 나이를 입력하세요: 30
당신의 이름은 evan이고, 나이는30살 입니다.
```

### 문제 - 홀수 짝수

사용자로부터 하나의 정수를 입력받고, 이 정수가 홀수인지 짝수인지 판별하는 프로그램을 작성하세요.

**실행 결과 예시**

```
하나의 정수를 입력하세요: 1
입력한 숫자 1는 홀수입니다.
```

```
하나의 정수를 입력하세요: 4
입력한 숫자 4는 짝수입니다.
```

**ScannerEx2**

```java
package ScannerEx;

import java.util.Scanner;

public class ScannerEx2 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.print("하나의 정수를 입력하세요: ");
        int number = input.nextInt();

        if (number % 2 == 0) {
            System.out.println("입력한 숫자 " + number + "는 짝수입니다.");
        } else {
            System.out.println("입력한 숫자 " + number + "는 홀수입니다.");
        }
    }
}
```

**실행결과**

```java
하나의 정수를 입력하세요: 15
입력한 숫자 15는 홀수입니다.
```

### 문제 - 음식점 주문

- 사용자로부터 음식 이름(`foodName`), 가격(`foodPrice`), 그리고 수량(`foodQuantity`)을 입력받아, 주문한 음식의 총 가격을 계산하고 출력하는 프로그램을 계산하시오.
- 음식의 총 가격을 계산하세요. 총 가격은 각 음식의 가격(`foodPrice`)과 수량(`foodQuantity`)을 곱한 값이며, 이를 `totalPrice`라는 이름의 변수에 저장
- 주문 정보와 총 가격을 출력하세요. 출력 형태는 `“[음식 이름][수량]개를 주문하셨습니다. 총 가격은 [총 가격]원 입니다.”` 이어야합니다.

**실행결과 예시**

```
음식 이름을 입력해주세요: 피자
음식의 가격을 입력해주세요: 20000
음식의 수ㅑㄹㅇ을 입력해주세요: 2
피자 2개를 주문하셨습니다. 총 가격은 40000원입니다.
```

**ScannerEx3**

```java
package ScannerEx;

import java.util.Scanner;

public class ScannerEx3 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.print("음식 이름을 입력하세요: ");
        String foodName = input.nextLine();

        System.out.print("음식의 가격을 입력해주세요: ");
        int foodPrice = input.nextInt();

        System.out.print("음식의 수량을 입력해주세요: ");
        int foodQuantity = input.nextInt();

        int totalPrice = foodPrice * foodQuantity;

        System.out.println(foodName + " " + foodQuantity +"개를 주문하셨습니다. 총 가격은 " + totalPrice + "원 입니다.");
    }
}
```

**실행결과**

```java
음식 이름을 입력하세요: pizza
음식의 가격을 입력해주세요: 20000
음식의 수량을 입력해주세요: 2
pizza 2개를 주문하셨습니다. 총 가격은 40000원 입니다.
```

### 문제 - 구구단 출력

사용자로부터 하나의 정수 `n`을 입력받고, 입력받은 정수 `n`의 구구단을 출력하는 프로그램을 작성하세요.

**실행결과 예시**

```
구구단의 단 수를 입력해주세요: 8
8단의 구구단:
8 x 1 = 8
8 x 2 = 16
8 x 3 = 24
8 x 4 = 32
8 x 5 = 40
8 x 6 = 48
8 x 7 = 56
8 x 8 = 64
8 x 9 = 72
```

**ScannerEx4**

```java
package ScannerEx;

import java.util.Scanner;

public class ScannerEx4 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.print("구구단의 단 수를 입력해주세요: ");
        int number = input.nextInt();

        System.out.println(number + "단의 구구단:");
        for (int i = 1; i <= 9; i++) {
            int result = number * i;
            System.out.println(number + " x " + i + " = " +  result);
        }
    }
}
```

**실행결과**

```java
구구단의 단 수를 입력해주세요: 9
9단의 구구단:
9 x 1 = 9
9 x 2 = 18
9 x 3 = 27
9 x 4 = 36
9 x 5 = 45
9 x 6 = 54
9 x 7 = 63
9 x 8 = 72
9 x 9 = 81
```

## 문제와 풀이2

### 문제 - 변수 값 교환

변수 `a=10`이 들어있고, `b=20`이 들어있다.

변수 `a`의 값과 변수 `b`의 값을 서로 바꾸어라.

다음 코드에서 시작과 종료 부분 사이에 변수의 값을 교환하는 코드를 작성하면 된다.

힌트: `temp`변수를 활용.

출력결과

```java
a = 20
b = 10
```

**ChangVarEx**

```java
package ScannerEx;

public class ChangeVarEx {
    public static void main(String[] args) {
        System.out.println("코드 실행 전");
        int a = 10;
        int b = 20;
        int temp;
        System.out.println("a: " + a);
        System.out.println("b: " + b);

        //시작
        temp = a;
        a = b;
        b = temp;
        //종료
        
        System.out.println();
        System.out.println("코드 실행 후");
        System.out.println("a: " + a);
        System.out.println("b: " + b);
    }
}

```

**실행결과**

```java
코드 실행 전
a: 10
b: 20

코드 실행 후
a: 20
b: 10
```

### 문제 - 사이 숫자

사용자로부터 두 개의 정수를 입력받고, 이 두 정수 사이의 모든 정수를 출력하는 프로그램을 작성하세요.

- 사용자에게 첫 번째 숫자를 입력받으세요. 변수의 이름은 `num1`이어야합니다.
- 사용자에게 두 번째 숫자를 입력받으세요. 변수의 이름은 `num2`이어야합니다.
- 만약 첫 번째 숫자 `num1`이 두번째 숫자 `num2`보다 크다면, 두 숫자를 교환하세요.
    - 참고: 교환을 위해 임시 변수 사용을 고려하세요.
- `num1`부터 `num2`까지의 각 숫자를 출력하세요.
- 출력 결과에 유의하세요. 다음 예시와 같이 `2, 3, 4, 5`처럼 `,`로 구분해서 출력해야합니다.

**실행결과 예시**

```java
첫 번째 숫자를 입력하세요: 2
두 번째 숫자르 입력하세요: 5
두 숫자 사이의 모든 정수: 2, 3, 4, 5
```

```java
첫 번째 숫자를 입력하세요: 7
두 번째 숫자르 입력하세요: 5
두 숫자 사이의 모든 정수: 5, 6, 7
```

**ScannerEx5**

```java
package ScannerEx;

import java.util.Scanner;

public class ScannerEx5 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.print("첫 번째 숫자를 입력하세요: ");
        int num1 = input.nextInt();

        System.out.print("두 번째 숫자를 입력하세요: ");
        int num2 = input.nextInt();

        if (num1 > num2) {
            int temp = num1;
            num1 = num2;
            num2 = temp;
        }
        for (int i = num1; i <= num2; i++) {
            System.out.print(i);
            if (i != num2) {
                System.out.print(", ");
            }
        }  
    }
}
```

**실행결과**

```java
첫 번째 숫자를 입력하세요: 1
두 번째 숫자를 입력하세요: 6
1, 2, 3, 4, 5, 6
```

```java
첫 번째 숫자를 입력하세요: 10
두 번째 숫자를 입력하세요: 4
4, 5, 6, 7, 8, 9, 10
```

## 문제와 풀이3

### 문제 - 이름과 나이 반복

- 사용자로부터 이름과 나이를 반복해서 입력받고, 입력받은 이름과 나이를 출력하는 프로그램을 작성하세요. 사용자가 “종료”를 입력하면 프로그램이 종료되어야 합니다.
- 다음 실행 결과 예시를 참고해주세요.

**실행 결과 예시**

```java
이름을 입력하세요 (종료를 입력하면 종료): 자바
나이를 입력하세요: 30
입력한 이름: 자바, 나이: 30
```

**ScannerWhileEx1**

```java
package ScannerEx;

import java.util.Scanner;

public class ScannerWhileEx1 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        while (true) {
            System.out.print("이름을 입력하세요 (exit을 입력하면 종료): ");
            String name = input.nextLine();

            if (name.equals("exit")) {
                System.out.println("프로그램을 종료합니다.");
                break;
            }

            System.out.print("나이를 입력하세요: ");
            int age = input.nextInt(); //10\n 이 생성됨
            input.nextLine();  //숫자 입력후 줄바꿈 처리 : 10을 age가 가져가고, nextLine이 \n을 버리게 됨

            System.out.println("입력한 이름: " + name + ", " + "나이: " + age);
        }
    }
}
```

**실행결과**

```java
이름을 입력하세요 (exit을 입력하면 종료): java
나이를 입력하세요: 30
입력한 이름: java, 나이: 30
이름을 입력하세요 (exit을 입력하면 종료): hanee
나이를 입력하세요: 20
입력한 이름: hanee, 나이: 20
이름을 입력하세요 (exit을 입력하면 종료): exit
프로그램을 종료합니다.
```

### 문제 - 상품 가격 계산

- 사용자로 부터 상품의 가격(price)과 수량(quantity)을 입력받고, 총 비용을 출력하는 프로그램을 작성.
- 가격과 수량을 입력받은 후에는 이들의 곱을 출력하세요. 출력 형태는 “총 비용: [곱한 결과]”
- -1을 입력하여 가격 입력을 종료

**실행결과 예시**

```java
상품의 가격을 입력하세요 (-1을 입력하면 종료): 1000
구매하려는 수량을 입력하세요: 3
총 비용: 3000
```

**ScannerWhileEx2**

```java
package ScannerEx;

import java.util.Scanner;

public class ScannerWhileEx2 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        while (true) {
            System.out.print("상품의 가격을 입력하세요 (-1을 입력하면 종료): ");
            int price = input.nextInt();

            if (price == -1) {
                System.out.println("프로그램을 종료합니다.");
                break;
            }

            System.out.print("구매하려는 수량을 입력하세요: ");
            int quantity = input.nextInt();

            int totalCost = price * quantity;
            System.out.println("총 비용: " + totalPrice);
        }
    }
}
```

**실행 결과**

```java
상품의 가격을 입력하세요 (-1을 입력하면 종료): 2000
구매하려는 수량을 입력하세요: 4
총 비용: 8000
상품의 가격을 입력하세요 (-1을 입력하면 종료): -1
프로그램을 종료합니다.
```

## 문제와 풀이4

### 문제 - 입력한 숫자의 합계와 평균

- 사용자로부터 여러 개의 숫자를 입력 받고, 그 숫자들의 합계와 평균을 계산하는 프로그램을 작성하세요. 사용하는 숫자를 입력하고, 마지막에는 -1을 입력하여 종료한다고 가정합니다.
- 모든 숫자의 입력이 끝난 후, 숫자들의 합계 sum과 평균 average를 출력하세요. 평균은 소수점 아래까지 계산해야합니다.
- 다음 실행 결과 예시를 참고해주세요

**실행결과 예시**

```java
숫자를 입력하세요. 입력을 중단하려면 -1을 입력하세요:
1
2
3
4
-1
입력한 숫자들의 합계: 10
입력한 숫자들의 평균: 2.5
```

**ScannerWhileEx3 - 1**

```java
package ScannerEx;

import java.util.Scanner;

public class ScannerWhileEx3 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        int sum = 0;
        int count = 0;
        int input = 0;

        System.out.print("숫자를 입력하세요. (입력을 중단하려면 -1을 입력하세요): ");
        while (true) {
            input = scanner.nextInt();
            if (input == -1) {
                double average = (double) sum / count;
                System.out.println("입력한 숫자들의 합계: " + sum);
                System.out.println("입력한 숫자들의 평균: " + average);
                break;
            }
             sum += input;
             count++;
        }
    }
}
```

**실행결과**

```java
숫자를 입력하세요. (입력을 중단하려면 -1을 입력하세요): 1
2
3
4
5
6
-1
입력한 숫자들의 합계: 21
입력한 숫자들의 평균: 3.5
```

**ScannerWhileEx3 - 2**

```java
package ScannerEx;

import java.util.Scanner;

public class ScannerWhileEx3 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        int sum = 0;
        int count = 0;
        int input = 0;

        System.out.print("숫자를 입력하세요. (입력을 중단하려면 -1을 입력하세요): ");
/*          while (true) {
            input = scanner.nextInt();
            if (input == -1) {
                break;
            }
             sum += input;
             count++;
*/
        while ((input = scanner.nextInt()) != -1) {
            sum += input;
            count++;
        }
        double average = (double) sum / count;
        System.out.println("입력한 숫자들의 합계: " + sum);
        System.out.println("입력한 숫자들의 평균: " + average);
    }
}
```

- 이 문제에서 while을 주석으로 처리한 것 처럼 축약할 수 있다.

### 문제 - 상품구매

- 사용자로부터 상품 정보(상품명, 가격, 수량)를 입력받고, 이들의 총 가격을 출력하는 프로그램을 작성하세요. 사용자는 여러 상품을 추가하고 결제할 수 있으며, 프로그램을 언제든지 종료할 수 있습니다.
- 사용자에게 다음 세가지 옵션을 제공해야합니다. 1: 상품 입력, 2: 결제, 3: 프로그램 종료, 옵션은 정수로 입력받으며, 옵션을 저장하는 변수의 이름은 `option`이어야 합니다.
- 상품 입력 옵션을 선택하려면, 사용자에게 상품명과 가격, 수량을 입력받으세요.
- 결제 옵션을 선택하면, 총 비용을 출력하고, 총 비용을 0으로 초기화하세요. (사용자가 총 비용을 확인하고, 결제를 완료했다고 가정합니다. 따라서, 다음 사용자를 위해 총 비용을 다시 0으로 초기화 해야합니다.)
- 프로그램 종료 옵션을 선택하면, “프로그램을 종료합니다.”라는 메시지를 출력하고 프로그램을 종료하세요.
- 위 세가지 옵션 외 다른 값을 입력하면, “올바른 옵션을 선택해 주세요.”라는 메시지를 출력하세요.

**실행결과 예시**

```java
1: 상품 입력, 2: 결제, 3: 프로그램 종료
1
상품명을 입력하세요: 스프링
상품의 가격을 입력하세요: 30000
구매 수량을을 입력하세요: 1
상품명: 스프링 가격:30000 수량:1 합계:30000
1: 상품 입력, 2: 결제, 3: 프로그램 종료
1
상품명을 입력하세요: JPA
상품의 가격을 입력하세요: 40000
구매 수량을 입력하세요: 2
상품명: JPA 가격:80000 수량:2 합계: 80000
2
총 비용: 110000
1: 상품 입력, 2: 결제, 3: 프로그램 종료
3
프로그램을 종료합니다.
```

**ScannerWhileEx4 - 내가 풀어낸 코드**

```java
package ScannerEx;

import java.util.Scanner;

public class ScannerWhileEx4 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        int option;
        int purchase = 0;
        while (true) {
            System.out.println("1: 상품 입력, 2: 결제, 3: 프로그램 종료");
            option = scanner.nextInt();
            if (option == 3) {
                System.out.println("프로그램을 종료합니다.");
                break;
            }
            switch (option) {
                case 1:
                    scanner.nextLine();
                    System.out.print("상품명을 입력하세요: ");
                    String name = scanner.nextLine();
                    
                    System.out.print("상품의 가격을 입력하세요: ");
                    int price = scanner.nextInt();
                    
                    System.out.print("구매 수량을 입력하세요: ");
                    int quantity = scanner.nextInt();
                    
                    int totalprice = (price * quantity);
                    purchase += totalprice;
                    
                    
                    System.out.println("상품명:" + name + " 가격:" + price + " 수량:" + quantity + " 합계:" + totalprice);
                    break;
                case 2:
                    System.out.println("총 비용: " + purchase);
                    purchase = 0;
                    break;
                default:
                    System.out.println("올바른 옵션을 선택해 주세요.");
                    break;
            }
        }
    }
}
```

**ScannerWhileEx4 - 제시된 코드**

```java
package ScannerEx;

import java.util.Scanner;

public class ScannerWhileEx4_1 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        int totalCost = 0;
        int option;
        while (true) {
            System.out.println("1. 상품 입력, 2: 결제, 3: 프로그램 종료");
            option = scanner.nextInt();
            scanner.nextLine();

            if (option == 1) {
                System.out.print("상품명: ");
                String name = scanner.nextLine();

                System.out.print("상품 가격: ");
                int price = scanner.nextInt();

                System.out.print("수량: ");
                int quantity = scanner.nextInt();

                totalCost += price * quantity;
                System.out.println("상품명:" + name + " 가격:" + price + " 수량:" + quantity + " 합계:" + price * quantity);
            } else if (option == 2) {
                System.out.println(totalCost);
                totalCost = 0;
            } else if (option == 3) {
                System.out.println("프로그램을 종료합니다.");
                break;
            } else {
                System.out.println("올바른 옵션을 입력해 주세요.");
            }
        }
    }
}
```
