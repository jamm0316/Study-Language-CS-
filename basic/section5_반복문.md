## while문 1

while문은 조건에 따라 코드를 반복해서 실행할 때 사용.

```java
while (condition) {
    // code
}
```

- 조건식을 확인한다. 참이면 코드 블럭을 실행, 거짓이면 while문을 벗어난다.
- 조건식이 참이면 코드블럭 실행. 이후 코드 블럭이 끝나면 다시 조건식 검사로 돌아가서 조건식 검사
(무한반복)

while문을 사용해서 1을 한번 씩 더해서 총 3번 더하는 코드를 만들어 보자.

**while1_2**

```java
package loop;

public class While1_2 {

    public static void main(String[] args) {

        int count = 0;

        while (count < 3) {
            count += 1;
            System.out.println("현재 숫자는: " + count);
        }
    }
}
```

**실행결과**

현재 숫자는: 1
현재 숫자는: 2
현재 숫자는: 3

## while문 2

**문제: 1부터 하나씩 증가하는 수를 3번 더해라(1 ~3 더하기)**

이 문제는 1부터 하나씩 증가하는 수이기 때문에 1 + 2 + 3을 더해야 한다.

**While2_1**

```java
package loop;

public class While2_1 {

    public static void main(String[] args) {

        int sum = 0;

        sum += 1;
        System.out.println("i=" + 1 + " sum=" + sum);
        sum += 2;
        System.out.println("i=" + 2 + " sum=" + sum);
        sum += 3;
        System.out.println("i=" + 3 + " sum=" + sum);
    }
}
```

실행결과

i=1 sum=1
i=2 sum=3
i=3 sum=6

정답은 맞으나 개선할 점이 있다. 무엇보다 변경에 유연하지 않다.

다음과 같이 요규사항이 변경되었다.

문제: 10부터 하나씩 증가하는 수를 3번 더해라(10 ~ 12더하기)

이렇게 되면 10 + 11 + 12를 계산해야 한다. 문제는 코드를 너무 많이 변경해야한다는 점

변수를 사용해서 더 변경하기 쉬운 코드로 작성. 변경되는 부분을 변수 i를 사용해서 바꿔보자.

```java
package loop;

public class While2_2 {

    public static void main(String[] args) {

        int sum = 0;
        int i = 10;

        sum += i;
        System.out.println("i=" + i + " sum=" + sum);
        i++;

        sum += i;
        System.out.println("i=" + i + " sum=" + sum);
        i++;

        sum += i;
        System.out.println("i=" + i + " sum=" + sum);
        i++;
    }
}
```

**실행결과**

i=10 sum=10
i=11 sum=21
i=12 sum=33

새로운 문제: i부터 하나씩 증가하는 수를 endNum(마지막 수)까지 더해라 (i ~ endNum 더하기)

예)

- `i=1, endNum=3` ⇒ 1~3 까지 3번 더함
- `i=1, endNum=10` ⇒ 1~10 까지 10번 더함
- `i=10, endNum=12` ⇒ 10~12 까지 3번 더함

**While2_3**

```java
package loop;

public class While2_3 {

    public static void main(String[] args) {

        int sum = 0;
        int i = 1;
        int endNum = 3;

        while (i <= endNum) {
            sum += i;
            System.out.println("i=" + i + " sum=" + sum);
            i++;
        }
    }
}
```

**실행결과**

i=1 sum=1
i=2 sum=3
i=3 sum=6

## do-while문

`do-while`문은 `while`문과 비슷하지만, 조건에 상관없이 무조건 한 번은 코드를 실행.

**do-while문 구조**

```java
do{
    //코드
} while (조건식);
```

예를 들어 조건에 만족하지 않아도 한 번은 현재값을 출력하고 싶을 때 사용

**DoWhile2**

```java
package loop;

public class DoWhile_2 {

    public static void main(String[] args) {
        int i = 10;

        do {
            System.out.println("현재 숫자는: " + i);
            i++;
        } while (i < 3);
    }
}
```

**실행 결과**

현재 숫자는: 10

while이 조건에 만족하는 경우

```java
package loop;

public class DoWhile_2 {

    public static void main(String[] args) {
        int i = 10;

        do {
            System.out.println("현재 숫자는: " + i);
            i++;
        } while (i < 13);
    }
}
```

**실행 결과**

현재 숫자는: 10
현재 숫자는: 11
현재 숫자는: 12

## break, continue

`break`와 `continue`는 반복문에서 사용할 수 있는 키워드

`break`는 반복문을 즉시 종료하고 나간다.

`continue`는 반복문의 나머지 부분을 건너 뛰고 다음 반복문으로 진행

`while`, `do-while`, `for`와 같은 모든 반복문에서 사용가능

**break**

```java
while(조건식) {
    코드 1;
    break; //즉시 while문 종료로 이동.
    코드 2;
}
//while문 종료
```

`break`를 만나면 `코드2`가 실행되지 않고 `while`문이 종료됨.

**continue**

```java
while(조건식) {
    코드1;
    continue;  //즉시 조건식으로 이동
    코드2;
}
```

`continue`를 만나면 `코드2`가 실행되지 않고 다시 조건식으로 이동한다. 조건식이 참이면 `while`문 실행.

**문제: 1부터 시작해서 숫자를 계속 누적해서 더하다가 합계가 10보다 처음으로 큰 값은 얼마인가?**

**Break1**

```java
package loop;

public class Break {

    public static void main(String[] args) {

        int sum = 0;
        int i = 1;

        while (true) {
            sum += i;
            if (sum > 10) {
                System.out.println("합이 10보다 크면 종료: i = " + i + " sum = " + sum);
                break;
            }
            i++;
        }
    }
}
```

- 조건식에 `while(true)`라고 되어있다. 조건이 항상 참이기 때문에 `break`없이 `while`문은 무한히 반복

**실행결과**

합이 10보다 크면 종료: i = 5 sum = 15

**문제: 1부터 5까지 숫자를 출력하는데, 숫자가 3일 때는 출력을 건너뛰어야 한다.**

**Continue1**

```java
package loop;

public class Continue1 {

    public static void main(String[] args) {

        int sum = 1;

        while (sum <= 5) {
            if (sum == 3) {
                sum++;
                continue;
            }
            System.out.println(sum);
            sum++;
        }
    }
}
```

실행 결과

1
2
4
5

---

## for문 1

for문은 주로 반복 횟수가 정해져 있을 때 사용.

```java
for (1.초기식, 2.조건식, 4.증감식) {
    //3. 코드
}
```

for문은 다음 순서대로 실행된다.

1. 초기식이 실행된다. 주로 반복 횟수와 관련된 변수를 선언하고, 초기화 할 때 사용. 초기식은 딱 1번 사용됨.
2. 조건식을 검증한다. 참이면 코드를 실행, 거짓이면 for문 빠져나감
3. 코드 실행
4. 코드가 종료되면 증감식을 실행. 주로 초기식에 넣은 반복 횟수와 관련된 변수 값을 증가할 때 사용
5. 다시 2. 조건식 부터 시작(반복)

for문은 복잡해 보이지만 while문을 조금 편하게 다룰 수 있도록 구조화 한것

예를 들어 1~10까지 출력하는 for문은 다음와 같다.

```java
for (int i = 1; i <= 10; i++) {
    System.out.println(i);
}
```

1. 초기식이 실행 `int i = 1`
2. 조건식을 검증 `i ≤ 10`
3. 조건식이 참이면 코드를 실행 `System.out.println(i);`
4. 코드가 종료되면 증감식을 실행한다. `i++` 
5. 다시 2. 조건식을 검증(반복) `i ≤ 10` 의 조건이 거짓이 되면 for문 빠져나감

**For1**

```java
package loop;

public class For1 {

    public static void main(String[] agrs) {

        for (int i = 1; i <= 10; i++) {
            System.out.println(i);
        }
    }
}
```

**실행결과**

1
2
3
4
5
6
7
8
9
10

**문제: i부터 하나씩 증가하는 수를 endNum(마지막 수)까지 더해라**

**For2**

```java
package loop;

public class For2 {

    public static void main(String[] args) {
        int sum = 0;
        int endNum = 3;

        for (int i = 1; i <= endNum; i++) {
            sum = sum + i;
            System.out.println("i = " + i + " sum = " + sum);
        }
    }
}
```

**실행결과**

i = 1 sum = 1
i = 2 sum = 3
i = 3 sum = 6

### for vs while

**while2_3**

```java
package loop;

public class While2_3 {

    public static void main(String[] args) {

        int sum = 0;
        int i = 1;
        int endNum = 3;

        while (i <= endNum) {
            sum += i;
            System.out.println("i=" + i + " sum=" + sum);
            i++;
        }
    }
}
```

둘을 비교했을 때 for문이 더 깔끔하다.

1. 초기화
2. 조건검사
3. 반복 후 작업

등이 규칙적으로 한 줄에 모두 들어있어 코드를 이해하기 쉽다.

`for (int i = 1; i ≤ endNum; i++)`

여기서 바로 변수 `i`가 카운터 변수, 증가하면서 반복 횟수가 올라가고, 또 변수 `i`를 사용해서 계속 반복할지 아니면 빠져나갈지 판단.

이렇게 반복 횟수에 직접적인 영향을 주는 변수 선언부터 값 증가, 조건식 활용까지
`for (초기식; 조건식; 증감식)` 구조를 활용해서 처리.

반면 `while`문은 `i`를 선언하는 부분 그리고 `i++`로 증가하는 부분이 기존 코드에 분산.

## for문2

**for문 구조**

```java
for (초기식; 조건식; 증감식) {
    //코드
}
```

for문에서 초기식, 조건식, 증감식은 선택이다. 따라서 모두 생략해도 무방하며 단, 각 영역을 구분하는 세미콜론(;;)은 유지해야함.

```java
for (;;) {
    //code
}
```

위 경우 조건이 없기 때문에 무한히 반복하는 코드가 된다.

따라서 아래와 같은 코드가 된다.

```java
while (true) {
    //code
}
```

**문제: 1부터 시작하여 숫자를 계속 누적해서 더하다가 합계가 10보다 큰 처음 값은 얼마인가?**

```java
package loop;

public class Break2 {

    public static void main(String[] args) {
        int sum = 0;
        int i = 1;

        for (;;) {
            sum += i;
            if (sum > 10) {
                System.out.println("합이 10보다 크면 종료 i = " + i + " sum = " + sum);
                break;
            }
            i++;
        }
    }
}
```

**실행 결과**

합이 10보다 크면 종료 i = 5 sum = 15

**Break3**

```java
package loop;

public class Break3 {

    public static void main(String[] args) {
        int sum = 0;

        for (int i = 1;;i++) {
            sum += i;
            if (sum > 10) {
                System.out.println("합이 10보다 크면 종료 i = " + i + " sum = " + sum);
                break;
            }
        }
    }
}
```

**실행결과**

합이 10보다 크면 종료 i = 5 sum = 15

> 참고
for문을 좀 더 편리하게 사용하도록 도와주는 향상된 for문 또는 for-each문으로 불리는 반복문도 있다.
> 

## 중첩 반복문 1

반복문 내부에 또 반복문을 만들 수 있다. `for`, `while` 모두 가능

**Nested1**

```java
package loop;

public class Nested1 {

    public static void main(String[] args) {
        for (int i = 0; i < 2; i++) {
            System.out.println("외부 for 시작 i:" + i);
            for (int j = 0; j < 3; j++) {
                System.out.println("-> 내부 for " + i + "-" + j);
            }
            System.out.println("외부 for 종료 i " + i);
            System.out.println();
        }
    }
}
```

**실행결과**

외부 for 시작 i:0
-> 내부 for 0-0
-> 내부 for 0-1
-> 내부 for 0-2
외부 for 종료 i 0

외부 for 시작 i:1
-> 내부 for 1-0
-> 내부 for 1-1
-> 내부 for 1-2
외부 for 종료 i 1

외부 for는 2번, 내부 for는 3번 실행된다. 그런데 외부 for 1번 당 내부 for 가 3번 실행되기 때무에 외부(2)*내부(3)에 의해 총 6번의 내부 for 코드 수행됨

## 문제와 풀이1

### **문제: 자연수 출력**

처음 10개의 자연수를 출력하는 프로그램을 작성해 보세요. 이 떄 count라는 변수를 사용해야합니다.

while문, for문 2가지 버전의 정답을 만들어야 합니다.

출력예시

1
2
3
4
5
6
7
8
9
10

해답: 자연수 출력 - while

**WhileEx1**

```java
package loop.Ex;

public class WhileEx1 {

    public static void main(String[] args) {

        int i = 1;
        while (i <= 10) {
            System.out.println(i);
            i++;
        }
    }
}
```

**실행 결과**

1
2
3
4
5
6
7
8
9
10

**ForEx1**

```java
package loop.ex;

public class ForEx1 {

    public static void main(String[] args) {

        for (int i = 0; i <= 10; i++) {
            System.out.println(i);
        }
    }
}
```

**실행결과**

1
2
3
4
5
6
7
8
9
10

### **문제: 짝수 출력**

반복문을 사용하여 처음10개의 짝수를 출력하는 프로그램을 작성해보세요. 이때 `num`이라는 변수를 사용하여 수를 표현

**while2**

```java
package loop.ex;

public class WhileEx2 {

    public static void main(String[] args) {

        int num = 1;
        int count = 1;
        while (count <= 10) {
            if (num % 2 == 0) {
                System.out.println(num);
                num++;
                count++;
            } else {
                num++;
            }
        }
    }
}
```

**실행결과**

2
4
6
8
10
12
14
16
18
20

**ForEx2**

```java
package loop.ex;

public class ForEx2 {

    public static void main(String[] args) {

        for (int num = 1, count = 1; count <= 10; num++, count++) {
            if (num % 2 == 0) {
                System.out.println(num);
            } else {
                count -= 1;
            }
        }
    }
}
```

결과 값은 같지만 아래 코드가 더 가독성이 좋은 코드다

```java
package loop.ex;

public class ForEx2 {

    public static void main(String[] args) {
        int num = 1;
        for (int count = 1; count <= 10; count++) {
            if (num % 2 == 0) {
                System.out.println(num);
                num++;
            } else {
                num++;
                count -= 1;
            }
        }
    }
}
```

**실행결과**

2
4
6
8
10
12
14
16
18
20

### 문제: 누적 합 계산

반복문을 사용하여 1부터 max까지의 합을 계산하고 출력하는 프로그램 작성.

이때 sum이라는 변수를 사용하여 누적합을 표현하고, i라는 변수를 사용하여 카운트 (1부터 max까지 증가하는 변수)를 수행

while문, for문 2가지 버전의 정답만들기

**출력예시:**

//max = 1
1

//max=2
3

//max=3
6

WhileEx3

```java
package loop.ex;

public class WhileEx3 {

    public static void main(String[] args) {

        int max = 100;
        int i = 1;
        int sum = 0;

        while (i <= max) {
            sum = sum + i;
            i++;
        }
        System.out.println(sum);
    }
}
```

**실행 결과**

5050

```java
package loop.ex;

public class ForEx3 {

    public static void main(String[] args) {

        int max = 100;
        int sum = 0;
        for (int i = 1; i <= max; i++) {
            sum += i;
        }
        System.out.println(sum);
    }
}
```

**실행 결과**

5050

## 문제와 풀이 2

### 문제: 구구단 출력

중첩 for문을 사용해서 구구단 완성

**출력형태**

1 * 1 = 1
1 * 2 = 2
…
9 * 9 = 81

**NestedEx1**

```java
package loop.ex;

public class Nested1 {

    public static void main(String[] args) {

        for(int i = 1; i <= 9; i++) {
            for (int j = 1; j <= 9; j++) {
                System.out.println(i + " * " + j + " = "+ i * j);
            }
        }
    }
}
```

### 문제: 피라미드 출력

`int rows` 를 선언

이 수만큼 다음과 같은 피라미드 출력

참고: `println()`은 출력 후 다음 라인으로 넘어간다. 라인을 넘기지 않고 출력하려면 `print()`를 사용하면 된

예)`System.out.print(”*”)`

**출력형태**

```java
//rows = 2
*
**

//rows = 5
*
**
***
****
*****
```

**Nested2(나의 풀이)**

```java
package loop.ex;

public class Nested2 {

    public static void main(String[] args) {

        int rows = 5;

        int i = 1;
        while (i <= rows) {
            String star = "*";
            String stars = star.repeat(i);
            System.out.println(stars);
            i++;
        }
    }
}
```

**Nested2(김영한 풀이)**

```java
package loop.ex;

public class Nested2 {

    public static void main(String[] args) {
        int row = 5;

        for (int i = 1; i <= row; i++) {
            for (int j = 1; j <= i; j++) {
                System.out.print("*");
            }
            System.out.println();
        }
    }
}
```

**실행 결과**

*
**
***
****
*****
