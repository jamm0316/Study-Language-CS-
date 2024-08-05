## 배열 시작

### 배열이 필요한 이유

학생의 점수를 출력하는 간단한 프로그램을 작성해보자.

**Array1**

```java
package array;

public class Array1 {
    public static void main(String[] args) {
        int student1 = 90;
        int student2 = 80;
        int student3 = 70;
        int student4 = 60;
        int student5 = 50;

        System.out.println("학생1 점수: " + student1);
        System.out.println("학생2 점수: " + student2);
        System.out.println("학생3 점수: " + student3);
        System.out.println("학생4 점수: " + student4);
        System.out.println("학생5 점수: " + student5);
    }
}
```

**실행결과**

```java
학생1 점수: 90
학생2 점수: 80
학생3 점수: 70
학생4 점수: 60
학생5 점수: 50
```

- 학생을 몇 명 더 추가해야한 다면 변수를 선언하는 부분과 점수를 출력하는 부분의 코드를 추가해야한다.
    - 학생을 수백 명 이상 추가해야한다면 코드가 상당히 많이 늘어날 것이다.
- 변수를 선언하는 부분을 보면 학생 수 가 증가함에 따라 int형 변수를 계속해서 추가해야한다.
- 반복문으로 해결할 수 있을 것 같지만, 점수를 출력하는 부분을 보면 변수의 이름이 다르기 때문에 반복문도 적용할 수 없다.

이렇게 같은 타입의 변수를 반복해서 선언하고 반복해서 사용하는 문제를 해결하는 것이 바로 배열이다.

## 배열의 생성과 선언

배열은 같은 타입의 변수를 사용하거나 편하게 하나로 묶어둔 것이다. 이전 예제를 배열을 사용하도록 변경해보자.
참고로 단계적으로 구조를 변경해 나갈 것이다.

**Array1Ref1**

```java
package array;

public class Array1Ref1 {
    public static void main(String[] args) {
        int[] students;  //배열 변수 선언
        students = new int[5];  //배열 생성

        //변수 값 대입
        students[0] = 90;
        students[1] = 80;
        students[2] = 70;
        students[3] = 60;
        students[4] = 50;

        System.out.println("학생1 점수: " + students[0]);
        System.out.println("학생1 점수: " + students[1]);
        System.out.println("학생1 점수: " + students[2]);
        System.out.println("학생1 점수: " + students[3]);
        System.out.println("학생1 점수: " + students[4]);
    }
}
```

다음 두줄에 대한 설명

```java
int[] students;  //배열 변수 선언
students = new int[5];  //배열 생성
```

1. **배열 변수 선언 (`int[] students;`)**
    - 배열을 사용하려면 `int[] students;` 와 같이 배열 변수를 선언해야 한다.
    - 일반적인 변수와 차이점은 `int[]`처럼 타입 다음에 대괄호(`[]`)가 들어간다는 점.
    - 배열 변수를 선언한다고 해서 아직 사용할 수 있는 배열이 만들어진 것은 아니다.
        - `int a` 에 는 정수를, `double b`에는 실수를 담을 수 있다.
        - `int[] students`와 같은 배열 변수에는 배열을 담을 수 있다. (배열 변수에는 10, 20 같은 값이 아니라 배열이라는 것을 담을 수 있다.)
2. **배열 생성 (`students = new int[5]`)**
    - 배열을 사용하려면 배열을 생성해야한다.
    - `new int[5]`라고 입력하면 오른쪽 그림과 같이 총 5개의 `int`형 변수가 만들어진다.
    - `new`는 새로 생성한다는 뜻이고, `int[5]`는 `int`형 변수 5개라는 뜻이다. 
    따라서, `int`형 변수 5개를 다룰 수 있는 배열을 새로 만든다는 뜻.
    - 앞서 `int students1`, `int students2` … `int students5`까지 총 5개의 변수를 직접 선언함.
        - 배열을 사용하면 이런 과정을 깔끔하게 처리할 수 있다.

**배열과 초기화**

- `new int[5]` 라고 하면 총 5개의 `int`형 변수가 만들어짐.
    - 자바는 배열을 생성할 때 그 내부값을 자동으로 초기화
- `숫자`는 `0`, `boolean`은 `false`, `String`은 `null`(없다는 뜻)로 초기화 된다.

1. **배열 참조값 보관**
- `new int[5]`로 배열을 생성하면 배열의 크기만큼 메모리를 확보한다.
    - `int`형 5개를 사용하면 `4byte * 5` → `20byte`를 확보
- 배열을 생성하고 나면 자바는 메모리 어딘가에 있는 이 배열에 접근할 수 있는 참조값(주소)(`x001`)을 반환
    - 여기서 `x001`은 참조값이다.
- 앞서 선언한 배열 변수인 `int[] students`에 생성된 배열의 참조값(`x001`)을 보관함
- `int[] students` 변수는 `new int[5]`로 생성한 배열의 참조값을 가지고 있다.
    - 이 변수는 참조값을 가지고 이를 통해 배열을 참조할 수 있다.
    - 즉, 참조값을 통해 실제 배열에 접근한다.
    - 참고로 배열을 생성하는 `new int[5]` 차체에는 아무런 이름이 없다.
        - 그냥 `int`형 변수를 5개 연속으로 만드는 것
        - 따라서, 생성한 배열에 접근하는 방법이 필요하고 배열을 생성할 때 반환되는 참조값을 어딘가에 보관해두어야 한다. 앞서 `int[] students`변수에 참조값(`x001`)을 보관해두었다. 이 변수를 통해 배열에 접근가능

이 부분을 풀어서 설명하면 다음과 같다.

```java
int[] students = new int[5];  //1. 배열 생성
int[] students = x001;  //2. new int[5]의 결과로 x001 참조값 반환
students = x001  //3. 최종 결과
```

참조값을 확인하고 싶다면 다음과 같이 배열의 변수를 출력해보면 된다.

```java
System.out.println(students);  //[I@7b23ec81 (I는 int형 배열, @뒤에 16진수는 참조값을 뜻함)
```

## 배열 사용

### 인덱스

배열은 변수와 사용법이 비슷한데, 차이점이 있다면 []사이에 숫자 번호를 넣어주면 됨.

배열의 위치를 나타내는 숫자를 인덱스(index)라고 한다.

```java
public class Array1Ref1 {
    public static void main(String[] args) {
        int[] students;  //배열 변수 선언
        students = new int[5];  //배열 생성

        //변수 값 대입
        students[0] = 90;
        students[1] = 80;

        System.out.println(students);
        System.out.println("학생1 점수: " + students[0]);
        System.out.println("학생1 점수: " + students[1]);
```

**배열은 0부터 시작**

`new int[5]`와 같이 5개 요소를 가지는 `int`형 배열을 만들었다면 인덱스는 `0, 1, 2, 3, 4` 가 존재

따라서, 사용가능한 인덱스의 범위는 `0 ~ (n - 1)`이다. 즉, `students[4]`가 배열의 마지막 요소

만약 students[5]와 같이 접근 가능한 배열의 인덱스 범위를 넘어가면 다음과 같은 오류가 발생한다.

**인덱스 허용 범위를 넘어설 때 발생하는 오류**

```java
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 5
	at array.Array1Ref1.main(Array1Ref1.java:14)
```
