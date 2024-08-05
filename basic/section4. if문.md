## if 문

`if`문은 특정 조건이 참인지 확인하고, 그 조건이 참(true)일 경우 특정 코드 블록 실행

```java
if (condition) {
    // 조건이 참일 때 실행되는 코드
}
```

코드블록 : `{}`(중괄호) 사이에 있는 코드

### if1

```java
package cond;

public class If1 {
    public static void main(String[] args) {
        int age = 20; //사용자의 나이

        if (age >= 18) {
            System.out.println("성인입니다.");
        }
        if (age < 18) {
            System.out.println("미성년자 입니다.");
        }
    }
}
```

**실행결과**

성인입니다.

---

## else문

else문은 if문에서 만족하는 조건이 없을 때 실행하는 코드

```java
if (condition) {
    // 조건이 참일 때 실행되는 코드
} else {
    // 만족하는 조건이 없을 떄 실행되는 코드
}
```

### if2

```java
package cond;

public class If2 {

    public static void main(String[] args) {
        int age = 20;

        if (age >= 18) {
            System.out.println("성인입니다.");
        } else {
            System.out.println("미성년자입니다.");
        }
    }
}
```

**실행결과**

성인입니다.

---

## else if

**문제**

당신은 연령에 따른 다른 메세지를 출력하는 프로그램을 작성해야한다.

이 프로그램은 `int age`라는 변수를 사용해야 하며, 연령에 따라 다음의 출력을 해야한다.

- 7세 이하일 경우: “미취학”
- 8세 이상 13세 이하일 경우: “초등학생”
- 14세 이상 16세 이하일 경우: “중학생”
- 17세 이상 19세 이하일 경우: “고등학생”
- 20세 이상일 경우: “성인”

```java
package cond;

public class If3 {

    public static void main(String[] args) {
        int age = 14;

        if (age <= 7) {
            System.out.println("미취학");
        } else if (age <= 13) {
            System.out.println("초등학생");
        } else if (age <= 16) {
            System.out.println("중학생");
        } else if (age <= 19) {
            System.out.println("고등학생");
        } else {
            System.out.println("성인");
        }
    }
}
```

**실행결과**

중학생

---

## if문과 else if문

if문에 else if를 함께 사용하는 것은 서로 연관된 조건일 때 사용한다. 그런데 서로 관련이 없는 독립 조건이면 else if를 사용하지 않고 if문을 각각 따로 사용한다.

**예시**

```java
// 예시1. if-esle 사용 : 서로 연관된 조건이어서, 하나로 묶을 때
if (condition) {
    // 작업1 수행
} else if {
    // 작업2 수행
}

// 예시2. if 각각 사용: 독립된 조건일 때
if (condition1) {
    // 작업1 수행
}
if (condition2) {
    // 작업2 수행
}
```

예시 1은 작업1, 작업2 둘 중 하나만 수행된다. 그러나 예시2는 조건만 맞다면 둘다 수행될 수 있다.

**문제**

온라인 쇼핑몰의 할인 시스템을 개발해야한다. 한 사용자가 어떤 상품을 구매할 때, 다양한 할인 조건에 따라 총 할인 금액이 달라질 수 있다.

각각의 할인 조건은 다음과 같다.

- 아이템 가격이 10000원 일 때, 1000원 할인
- 나이가 10살 이하일 때 1000원 할인

**If5**

```java
package cond;

public class If5 {

    public static void main(String[] args) {

        int age = 10;
        int price = 10000;
        int discount = 0;

        if (price >= 10000) {
            discount += 1000;
            System.out.println("10,000원 이상일 때 1000원 할인");
        }
        if (age <= 10) {
            discount += 1000;
            System.out.println("어린이 1000원 할인");
        }
        System.out.println("총 할인 금액: " + discount);
        System.out.println("결제금액: " + (price - discount));
    }
}
```

**실행결과**

10,000원 이상일 때 1000원 할인
어린이 1000원 할인
총 할인 금액: 2000
결제금액: 8000

**If6**

```java
package cond;

public class If6 {

    public static void main(String[] args) {
        int age = 10;
        int price = 10000;
        int discount = 0;

        if (price >= 10000) {
            discount += 1000;
            System.out.println("10,000원 이상일 때 1000원 할인");
        } else if (age <= 10) {
            discount += 1000;
            System.out.println("어린이 1000원 할인");
        } else {
            System.out.println("할인없음");
        }
        System.out.println("총 할인 금액: " + discount);
        System.out.println("결제금액: " + (price - discount));
    }
}
```

**실행결과**

10,000원 이상일 때 1000원 할인
총 할인 금액: 1000
결제금액: 9000

---

## switch문

**문제**

당신은 회원 등급에 따라 다른 쿠폰을 발급하는 프로그램을 작성해야 한다.

이 프로그램은 `int grade`라는 변수를 사용하여, 회원 등급(`grade`)에 따라 다음의 쿠폰을 발급해야 한다.

- 1등급: 쿠폰 1000
- 2등급: 쿠폰 2000
- 3등급: 쿠폰 3000
- 위의 등급이 아닐 경우: 쿠폰 500

각 쿠폰이 할당된 후에는 “발급받은 쿠폰” + 쿠폰값을 출력해야 한다.

2등급 사용자 출력 예)

발급받은 쿠폰 2000

**Switch1**

```java
package cond;

public class Switch1 {

    public static void main(String[] args) {

        int grade = 2;
        int coupon;

        if (grade == 1) {
            coupon = 1000;
        } else if (grade == 2) {
            coupon = 2000;
        } else if (grade == 3) {
            coupon = 3000;
        } else {
            coupon = 500;
        }
        System.out.println("발급받은 쿠폰: " + coupon);
    }
}
```

**실행결과**

발급받은 쿠폰: 2000

`switch`문은 앞서 배운 `if`문을 조금 더 편리하게 사용할 수 있는 기능

참고로 `if`문은 비교 연산자를 사용할 수 있지만, `switch`문은 단순히 값이 같은지만 비교할 수 있다.

```java
switch (조건식) {
    case value1:
        // 조건식의 결과 값이 value1일 때 실행되는 코드
        break;
    case value2:
        // 조건식의 결과 값이 value2일 때 실행되는 코드
        break;
    default:
        // 조건식의 결과 값이 위의 어떤 값에도 해당하지 않을 때 실행되는 코드
```

**Switch2**

```java
package cond;

public class Switch2 {

    public static void main(String[] args) {
        int grade = 2;
        int coupon;

        switch (grade) {
            case 1:
                coupon = 1000;
                break;
            case 2:
                coupon = 2000;
                break;
            case 3:
                coupon = 3000;
                break;
            default:
                coupon = 500;
        }
        System.out.println("발급받은 쿠폰: " + coupon);
    }
}
```

**실행결과**

발급받은 쿠폰: 2000

**break문이 없다면?**

**Switch3**

```java
package cond;

public class Switch2 {

    public static void main(String[] args) {
        int grade = 2;
        int coupon;

        switch (grade) {
            case 1:
                coupon = 1000;
                break;
            case 2:
                coupon = 2000;
            case 3:
                coupon = 3000;
                break;
            default:
                coupon = 500;
        }
        System.out.println("발급받은 쿠폰: " + coupon);
    }
}
```

**실행결과**

발급받은 쿠폰: 3000

break문을 만날 때까지 실행한다.

### 자바 14 새로운 switch문

`switch`문은 `if`문 보다 덜 복잡하지만 자바 14부터는 새로운 `switch`문이 도입됨

**Switch 4**

```java
package cond;

public class Switch4 {

    public static void main(String[] args) {
        int grade = 2;

        int coupon = switch (grade) {
            case 1 -> 1000;
            case 2 -> 2000;
            case 3 -> 3000;
            default -> 500;
        };
        System.out.println("발급받은 쿠폰 = " + coupon);
    }
}
```

**실행결과**

발급받은 쿠폰: 2000

---

## 삼항연산자

if문을 사용할 때 단순히 참과 거짓을 값을 사용하여 구하는 경우가 있다.

CondOp1

```java
package cond;

public class CondOp1 {

    public static void main(String[] args) {

        int age = 18;
        String status;

        if (age >= 18) {
            status = "성인";
        } else {
            status = "미성년자";
        }
        System.out.println("age = " + age + " status = " + status);
    }
}
```

**실행 결과, age = 18**

age = 18 status = 성인

**실행 결과, age = 17**

age = 17 status = 미성년자

**삼항연산자 기본형태**

(조건) ? 참 표현식 : 거짓 표현식

단순히 참과 거짓에 따라 특정 값을 구하는 경우 **삼항 연산자** 또는 **조건 연산자**라고 불리는

`? :` 연산자를 사용할 수 있다.

**CondOp2**

```java
package cond;

public class CondOp2 {

    public static void main(String[] args) {

        int age = 18;
        String status = (age >= 18) ? "성인" : "미성년자";
        System.out.println("age = " + age + " status = " + status);
    }
}
```

**실행 결과, age = 18**

age = 18 status = 성인

**실행 결과, age = 17**

age = 17 status = 미성년자

---

## 문제와 풀이 1

### 문제: ”학점 계산하기”

학생의 점수를 기반으로 학점을 출력하는 자바 프로그램을 작성하자. 다음과 같은 기준을 따른다.

- 90점 이상: ”A”
- 80점 이상 90점 미만: ”B”
- 70점 이상 80점 미만: ”C”
- 60점 이상 70점 미만: ”D”
- 60점 미만: ”F”

점수는 변수(`int score`)에 저장하고, 해당 변수를 기반으로 학점 출력

**출력예시**

score : 95
출력 : 학점은 A입니다.

**ScoreEx**

```java
package cond.Ex;

public class ScoreEx {

    public static void main(String[] args) {
        int score = 85;
        String status;

        if (score >= 90) {
            status = "A";
        } else if (score >= 80) {
            status = "B";
        } else if (score >= 70) {
            status = "C";
        } else if (score >= 60) {
            status = "D";
        } else {
            status = "F";
        }
        System.out.println("학점은 " + status + "입니다.");
    }
}
```

**실행 결과**

학점은 B입니다.

### 문제 : “거리에 따른 운송 수단 선택하기”

주어진 거리에 따라 가장 적합한 운송 수단을 선택하는 프로그램을 작성하자. 다음과 같은 기준을 따른다.

- 거리가 1km 이하이면: “도보”
- 거리가 10km 이하이면: “자전거”
- 거리가 100km 이하이면: “자동차”
- 거리가 100km 초과이면: “비행기”

거리는 변수(int distance)로 지정하고, 해당 변수를 기반으로 운송 수단 출력

**출력 예시**

distacne : 5
출력 : 자전거를 이용하세요.

**DistanceEx**

```java
package cond.Ex;

public class DistanceEx {

    public static void main(String[] args) {
        int distance = 25;
        String transportation;

        if (distance <= 5) {
            transportation = "도보";
        } else if (distance <= 10) {
            transportation = "자전거";
        } else if (distance <= 100) {
            transportation = "자동차";
        } else {
            transportation = "비행기";
        }
        System.out.println(transportation + "를 이용하세요.");
    }
}
```

**실행결과**

자동차를 이용하세요.

### 문제: “환율 계산하기”

특정 금액을 미국 달러에서 한국 원으로 변호나하는 프로그램을 작성하자. 환율은 1달러 당 1300원이라고 가정.
다음과 같은 기준을 따른다.

- 달러가 0미만이면: “잘못된 금액입니다.”
- 달러가 0일 때: “환전할 금액이 없습니다.”
- 달러가 0초과일 때: “환전 금액은 (계산된 원화 금액)원 입니다.”

금액은 변수(`int dollar`)로 지정하고, 해당 변수를 기반으로 한국 원으로의 환전 금액을 출력하자.

**출력 예시**

dollar: 10
출력: 환전 금액은 13000원 입니다.

**ExchangeRateEx**

```java
package cond.Ex;

public class ExchangeRateEx {

    public static void main(String[] args) {
        
        int dollar = 10;
        int rate = 1300;
        
        if (dollar < 0) {
            System.out.println("잘못된 금액입니다.");
        } else if (dollar == 0) {
            System.out.println("환전할 금액이 없습니다.");
        } else {
            System.out.println("환전할 금액은 " + dollar * rate + "입니다.");
        }
    }
}
```

**실행결과**

환전할 금액은 13000입니다.

---

## 문제와 풀이2

### 문제: “평점에 따른 영화 추천하기”

요청한 평점 이상의 영화를 찾아서 추천하는 프로그램 작성

- 어바웃타임 - 평점 9
- 토이스토리 - 평점 8
- 고질라 - 평점 7

평점 변수는 `double rating`을 사용하세요. `if`문을 활용하여 예제 풀이

**출력 예시**

- rating = 7.1
- 출력:
‘어바웃타임’을 추천합니다.
’토이스토리’를 추천합니다.

MovieRateEx

```java
package cond.Ex;

public class MovieRateEx {

    public static void main(String[] args) {

        double rating = 7.1;

        if (rating <= 9) {
            System.out.println("어바웃타임을 추천합니다.");
        }
        if (rating <= 8) {
            System.out.println("토이스토리를 추천합니다.");
        }
        if (rating <= 7) {
            System.out.println("고질라를 추천합니다.");
        }
    }
}
```

**실행 결과**

어바웃타임을 추천합니다.
토이스토리를 추천합니다.

### 문제: “학점에 따른 성취도 출력하기”

`String grade`라는 문자열을 만들고 학점에 따라 성취도를 출력하는 프로그램을 작성하자.
각 학점은 다음과 같은 성취도를 나타낸다.

- “A”: “탁월한 성과입니다!”
- “B”: “좋은 성과입니다.”
- “C”: “준수한 성과입니다.”
- “D”: “향상이 필요합니다.”
- “F”: “불합격 입니다.”
- 나머지: “잘못된 학점입니다.”

`switch`문을 사용해 문제를 해결하자

**출력 예시**

grade: “B”
출력: “좋은 성과입니다!”

**GradeSwitchEx**

```java
package cond.Ex;

public class GradeSwitchEx {

    public static void main(String[] args) {

        String grade = "B";
        String achievement = switch (grade) {
            case "A" -> "탁월한 성과입니다.";
            case "B" -> "좋은 성과입니다.";
            case "C" -> "준수한 성과입니다.";
            case "D" -> "향상이 필요합니다.";
            case "F" -> "불합격 입니다.";
            default -> "잘못된 점수입니다.";
        };
        System.out.println(achievement);
    }
}
```

**실행 결과**

좋은 성과입니다.

### 문제: 더 큰 숫자 찾기

여러분은 두 개의 정수 변수 `a`와 `b`를 가지고 있다. `a`의 값은 `10`이고, `b`의 값은 `20`이다. 삼항 연산자를 사용하여 두 숫자 중 더 큰 숫자를 출력하는 코드를 작성하자.

**출력 예시**

더 큰 숫자는 20입니다.

**CondOpEx**

```java
package cond.Ex;

public class CondOpEx {

    public static void main(String[] args) {

        int a = 10;
        int b = 20;

        int max = a > b ? a : b;
        System.out.println("더 큰 숫자는 " + max + "입니다.");
    }
}
```

**실행 결과**

더 큰 숫자는 20입니다.

### 문제: 홀수 짝수 찾기

정수 x가 주어지면 x가 짝수이면 “짝수”를, x가 홀수이면 “홀수”를 출력하는 프로그램을 작성하자.
삼항 연산자를 사용.

**출력 예시**

x: 2
출력: x = 2, 짝수

**EvenOddEx**

```java
package cond.Ex;

public class EvenOddEx {

    public static void main(String[] args) {

        int x = 2;

        String result = x % 2 == 0 ? "짝수" : "홀수";
        System.out.println("x = " + x + ", " + result);
    }
}
```

**실행 결과**

x = 2, 짝수
