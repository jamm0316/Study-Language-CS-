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

만약 `students[5]`와 같이 접근 가능한 배열의 인덱스 범위를 넘어가면 다음과 같은 오류가 발생한다.

**인덱스 허용 범위를 넘어설 때 발생하는 오류**

```java
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 5
	at array.Array1Ref1.main(Array1Ref1.java:14)
```

### 배열에 값 대입

배열에 값을 대입하든 배열의 값을 사용하든 간에 일반적인 변수와 사용법은 같다. 추가로 `[]`를 통해 인덱스만 넣어주면 된다.

```java
students[0] = 90;  //1. 배열의 값을 대입
x001[0] = 90;  //2. 변수에 있는 참조값을 통해 실제 배열에 접근, 인덱스를 사용해서 해당 위치의 요소에 접근, 값 대입
```

```java
students[1] = 80;  //1. 배열에 값을 대입
x001[1] = 80;  //2. 변수에 있는 참조값을 통해 실제 배열에 접근, 인덱스를 사용해서 해당 위치의 요소에 접근, 값 대입
```

### 배열 값 읽기

```java
//1. 변수값 읽기
System.out.println("학생 점수: " + students[0]);

//2. 변수에 있는 참조값을 통해 실제 배열에 접근, 인덱스를 사용해서 해당 위치의 요소에 접근
System.out.println("학생1 점수: " + x001[0]);

//3. 배열의 값을 읽어옴
System.out.println("학생1 점수: " + 90);
```

배열을 사용하면 이렇게 참조값을 토해서 실제 배열에 접근하고 인덱스를 통해서 원하는 요소를 찾음.

### 기본형 vs 참조형

자바의 변수 데이터 타입을 가장 크게 보면 기본형과 참조형으로 분류.

사용하는 값을 직접 넣을 수 있는 기본형, 방금 본 배열 변수와 같이 메모리 참조값을 넣을 수 있는 참조형

- 기본형(Primitive Type): `int`, `long`, `double`, `boolean` 처럼 변수에 사용할 값을 직접 넣을 수 있는 데이터 타입
- 참조형(Reference Type): `int[] students` 와 같이 데이터에 접근하기 위한 참조(주소)를 저장하는 데이터 타입. 객체나 클래스를 담을 수 있는 변수들 모두 참조형.

**참고**

배열은 왜 참조형을 사용할까?

- 기본형은 모두 사이즈가 명확하게 정해져있다.

```java
int i;  //4byte
long l;  //8byte
double d;  //8byte
```

- 그러나 배열은 다음과 같이 동적으로 사이즈를 변경할 수 있다.

```java
int size = 10000;  //사용자가 입력한 값을 넣었다고 가정해보자.
new int[size];   //이 코드가 실힝되는 시점에서 배열의 크기가 정해진다.
```

- 기본형은 선언과 동시에 크기가 정해지지만 배열은 크기를 동적으로 할당.
- 기본형은 사용할 값을 직접 저장하지만 참조형은 메모리에 저장된 배열이나 객체의 참조를 저장한다. 이로 인해 참조형은 더 복잡한 데이터 구조를 만들고 관리할 수 있다. 반면 기본형은 더 빠르고 메모리를 효율적으로 처리함.

## 배열 리펙토링

### 배열 리펙토링 - 변수 값 사용

```java
System.out.println("학생1 점수: " + students[0]);
System.out.println("학생1 점수: " + students[1]);
System.out.println("학생1 점수: " + students[2]);
System.out.println("학생1 점수: " + students[3]);
System.out.println("학생1 점수: " + students[4]);
```

변수명이 `students` 로 같기 때문에 숫자가 반복되는 부분만 해결하면 반복문을 도입할 수 있을 것 같다.

> **참고: 리펙토링**
리펙토링(Refactoring)은 기존의 코드 기능은 유지하면서, 내부 구조를 개선하여 가독성을 높이고, 유지보수를 용이하게 하는 과정
이는 중복을 제거하고 , 복잡성을 줄이며, 이해가 쉬운 코드로 만들기 위해 수행.
리펙토링은 버그를 줄이고, 프로그램 성능을 향상시킬 수 도 있으며, 코드의 설계를 개선하는데 도움이 된다.
즉, 기능을 같고, 코드를 개선하는 것
> 

### 배열 리펙토링 - 초기화

**ArrayRef2**

```java
package array;

public class Array1Ref2 {
    public static void main(String[] args) {
        int[] students;  //배열 변수 선언
        students = new int[5];  //배열 생성

        //변수 값 대입
        students[0] = 90;
        students[1] = 80;
        students[2] = 70;
        students[3] = 60;
        students[4] = 50;

        System.out.println(students.length);
        //변수 값 사용
        for (int i = 0; i < students.length; i++) {
            System.out.println("학생" + (i + 1) + " 점수: " + students[i]);
        }
    }
}
```

**실행결과**

```java
5
학생1 점수: 90
학생2 점수: 80
학생3 점수: 70
학생4 점수: 60
학생5 점수: 50
```

- 배열의 인덱스는 0부터 시작하므로 반복문에서 i = 0을 초기값을 사용함.
- `students.length`
    - 배열의 길이를 제공하는 특별한 기능.
    - 참고로 이 값은 조회만 할 수 있다. 대입은 불가
    - 현재 배열의 크기가 5이기 때문에 여기서는 5가 출력된다.
- for문의 조건이 `i < students.length` 이기 때문에 `i`는 `0, 1, 2, 3, 4`까지만 반복.
    - `i`가 `5`가 되면 `5 < 5`가 되면서 조건이 `거짓`이 되고, 반복문을 종료

**ArrayRef3**

```java
package array;

public class Array1Ref3 {
    public static void main(String[] args) {
        int[] students;  //배열 변수 선언
        students = new int[]{90, 80, 70, 60, 50};  //배열 생성과 초기화

        System.out.println(students.length);
        //변수 값 사용
        for (int i = 0; i < students.length; i++) {
            System.out.println("학생" + (i + 1) + " 점수: " + students[i]);
        }
    }
}
```

### 배열 리펙토링 -  간단한 배열 생성

배열의 값을 `{}`안에 넣어 선언할 수도 있다. 

**배열의 편리한 초기화**

```java
int[] students = {90, 80, 70, 60, 50);
```

단, 이때는 예제와 같이 배열 변수의 선언을 한 줄에 함께 사용할 때만 가능.

자바가 내부에서 배열 요소의 크기를 보고 `new int[5]`을 사용하여 배열을 생성

**오류**

```java
int[] students;
students = {90, 80, 70, 60, 50};
```

**ArrayRef4**

```java
package array;

public class Array1Ref4 {
    public static void main(String[] args) {
        int[] students = {90, 80, 70, 60, 50};  //배열 생성과 초기화

        System.out.println(students.length);
        //변수 값 사용
        for (int i = 0; i < students.length; i++) {
            System.out.println("학생" + (i + 1) + " 점수: " + students[i]);
        }
    }
}
```

이제 학생의 점수를 추가해도 `{90, 80, 70, 60, 50}` 의 내용만 변경하면 된다. 그러면 나머지 코드는 변경하지 않아도 된다. 배열을 사용한 덕분에 프로그램을 전체적으로 잘 구조화 할 수 있다.

## 2차원 배열 - 시작

지금까지 학습한 배열은 단순히 순서대로 나열. → 1차원 배열

이번에 학습할 2차원 배열은 이름 그대로 하나의 차원이 추가 → 2차원 배열은 행과 열로 구성

2차원 배열은 `int[][] arr = new int[2][3]`와 같이 선언하고 생성.

`arr[1][2]`와 같이 사용하는데, 먼저 행(row) 번호를 찾고 그 다음 열(column)번호를 찾는다.

2차원 배열의 사용법은 `[]` 가 하나 추가되는 것을 제외하고는 앞서본 1차원 배열과 같다.

`arr[행][열]`, `arr[row][column]`

| index | column [0] | column [1] | column [2] |
| --- | --- | --- | --- |
| row [0] | 1 | 2 | 3 |
| row [1] | 4 | 5 | 6 |

2차원 배열 데이터

- `arr[0][0]: 1`
- `arr[0][1]: 2`
- `arr[0][2]: 3`
- `arr[1][0]: 4`
- `arr[1][1]: 5`
- `arr[1][2]: 6`

`arr = [[1, 2, 3], [4, 5, 6]]` 이므로

`arr[0] = [1,2,3]`

`arr[0][0] = 1` 이 출력되는 것임

**ArrayDi0**

```java
package array;

public class ArrayDi0 {
    public static void main(String[] args) {

        int[][] arr = new int[2][3];  //행2, 열3

        arr[0][0] = 1;  //0행, 0열
        arr[0][1] = 2;  //0행, 1열
        arr[0][2] = 3;  //0행, 2열
        arr[1][0] = 4;  //1행, 0열
        arr[1][1] = 5;  //1행, 1열
        arr[1][2] = 6;  //1행, 2열

        // 0행 출력
        System.out.print(arr[0][0] + " ");  //1열 출력
        System.out.print(arr[0][1] + " ");  //2열 출력
        System.out.print(arr[0][2] + " ");  //3열 출력
        System.out.println();  //한 행이 끝나면 빈 칸을 출력한다.

        // 1행 출력
        System.out.print(arr[1][0] + " ");  //1열 출력
        System.out.print(arr[1][1] + " ");  //2열 출력
        System.out.print(arr[1][2] + " ");  //3열 출력
        System.out.println();  //한 행이 끝나면 빈 칸을 출력한다.
    }
}
```

- 이 코드는 2차원 배열을 만들고, 배열에 값을 1부터 6까지 순서대로 직접 입력
- 다음과 같은 결과를 만들기 위해 0행에 있는 0, 1, 2열을 출력, 그리고 다음으로 1행에 있는 0, 1, 2열 출력

**실행결과**

```java
1 2 3 
4 5 6 
```

## 2차원 배열 - 리펙토링1

### 구조 개선 - 행 출력 반복

**구조변경**

코드 구조를 보면 비슷한 부분이 반복 된다.

```java
// 0행 출력
System.out.print(arr[0][0] + " ");  //1열 출력
System.out.print(arr[0][1] + " ");  //2열 출력
System.out.print(arr[0][2] + " ");  //3열 출력
System.out.println();  //한 행이 끝나면 빈 칸을 출력한다.

// 1행 출력
System.out.print(arr[1][0] + " ");  //1열 출력
System.out.print(arr[1][1] + " ");  //2열 출력
System.out.print(arr[1][2] + " ");  //3열 출력
System.out.println();  //한 행이 끝나면 빈 칸을 출력한다.
```

행을 출력하는 부분이 거의 같다. 차이가 있는 부분은 행에서 `arr[0]`으로 시작할지 `arr[1]`로 시작할지 차이.

다음과 같이 행(row)에 들어가는 숫자만 하나씩 증가하면서 반복하면 됨

```java
// 0행 출력
System.out.print(arr[row][0] + " ");  //1열 출력
System.out.print(arr[row][1] + " ");  //2열 출력
System.out.print(arr[row][2] + " ");  //3열 출력
System.out.println();  //한 행이 끝나면 빈 칸을 출력한다.
```

**ArrayDi1**

```java
package array;

public class ArrayDi1 {
    public static void main(String[] args) {

        int[][] arr = new int[2][3];  //행2, 열3

        arr[0][0] = 1;  //0행, 0열
        arr[0][1] = 2;  //0행, 1열
        arr[0][2] = 3;  //0행, 2열
        arr[1][0] = 4;  //1행, 0열
        arr[1][1] = 5;  //1행, 1열
        arr[1][2] = 6;  //1행, 2열

        for (int row = 0; row < 2; row++) {
            System.out.print(arr[row][0] + " ");
            System.out.print(arr[row][1] + " ");
            System.out.print(arr[row][2] + " ");
            System.out.println();  //한 행이 끝나면 빈 칸을 출력한다.
        }
    }
}
```

- for문을 통해서 행(row)을 반복해서 접근.
- 각 행 안에서 열(column)이 3개이므로 `arr[row][0]`, `arr[row][1]`, `arr[row][2]` 3번 출력한다.
- 이렇게 for문을 한번 도는동안 3개의 열 출력가능.

**실행결과**

```java
1 2 3 
4 5 6 
```

### 구조개선 - 열 출력 반복

**ArrayDi2**

```java
package array;

public class ArrayDi2 {
    public static void main(String[] agrs) {
        int[][] arr = new int[2][3];

        arr[0][0] = 1;  //0행, 0열
        arr[0][1] = 2;  //0행, 1열
        arr[0][2] = 3;  //0행, 2열
        arr[1][0] = 4;  //1행, 0열
        arr[1][1] = 5;  //1행, 1열
        arr[1][2] = 6;  //1행, 2열

        for (int row = 0; row < 2; row++) {
            for (int column = 0; column < 3; column++) {
                System.out.print(arr[row][column] + " ");
            }
            System.out.println();  //행이 끝나면 라인을 변경한다.
        }
    }
}
```

- for문을 2번 중첩해서 사용하는데, 첫번째 for문은 행을 탐색하고, 내부에 있는 두번째 for문은 열을 탐색
- 내부에 있는 for문은 앞서 작성한 다음 코드와 같다. for문을 사용해서 열을 효과적으로 출력

**실행결과**

```java
1 2 3 
4 5 6 
```

## 2차원 배열 - 리펙토링2

### 구조 개선 - 초기화, 배열의 길이

1. 초기화: 기존 배열처럼 2차원 배열도 편리하게 초기화 가능
2. for문에서 배열의 길이 활용: 배열의 길이가 달라지면 for문에서 row < 2, column < 3 같은 부분을 같이 변경해야함. 이 부분을 배열의 길이를 사용하도록 변경해보자. 배열이 커지거나 줄어들어도 for문의 코드를 변경하지 않고 그대로 유지할 수 있다.

**ArrayDi3**

```java
package array;

public class ArrayDi3 {
    public static void main(String[] args) {

        int[][] arr = {
                {1, 2, 3},
                {4, 5, 6}
        };  //행2, 열3

        for (int row = 0; row < arr.length; row++) {
            for (int column = 0; column < arr[row].length; column++) {
                System.out.print(arr[row][column] + " ");
            }
            System.out.println();  //행이 끝나면 라인을 변경한다.
        }
    }
}
```

**초기화**

1차원 배열은 다음과 같이 초기화 한다.

```java
{1, 2, 3}
```

2차원 배열은 다음과 같이 초기화 한다. 밖이 행이되고, 안이 열이된다.

```java
{{1, 2, 3}, {4, 5, 6}}
```

아래와 같이 행행에 따라 라인을 구분해주면, 코드를 더 쉽게 이해할 수 있다.

```java
{
{1, 2, 3},
{4, 5, 6}
}
```

**배열의 길이**

`for`문에서 2차원 배열의 길이

- `arr.length`는 행의 길이
    - `{{}, {}}`를 생각해보면, `arr`배열은 `{}`, `{}` 2개의 배열 요소를 가진다.
- `arr[row].length`는 열의 길이
    - `arr[0]`은 `{1, 2, 3}` 배열을 뜻하고
    - `arr[1]`은 `{4, 5, 6}` 배열을 뜻함.

```java
package array;

public class ArrayDi3 {
    public static void main(String[] args) {

        int[][] arr = {
                {1, 2, 3},
                {4, 5, 6},
                {7, 8, 9}
        };  //행2, 열3

        for (int row = 0; row < arr.length; row++) {
            for (int column = 0; column < arr[row].length; column++) {
                System.out.print(arr[row][column] + " ");
            }
            System.out.println();  //행이 끝나면 라인을 변경한다.
        }
    }
}

```

**실행결과**

```java
1 2 3 
4 5 6 
7 8 9 
```

- 배열 요소만 바꿔도 출력값이 바뀌는것을 확인할 수 있다.

### 구조 개선 - 값 입력

**ArrayDi4**

```java
package array;

public class ArrayDi4 {
    public static void main(String[] args) {

        int[][] arr = new int[3][3];

        int i = 1;
        for (int row = 0; row < arr.length; row++) {
            for (int column = 0; column < arr[row].length; column++) {
                arr[row][column] = i++;
            }
        }

        for (int row = 0; row < arr.length; row++) {
            for (int column = 0; column < arr[row].length; column++) {
                System.out.print(arr[row][column] + " ");
            }
            System.out.println();  //한행이 끝날때마다 라인을 변경
        }
    }
}
```

- 중첩된 for문을 사용해서 값을 순서대로 입력
- `arr[row][column] = i++` 후위 증감 연산자(`++`)를 사용해서 값을 먼저 대입한 다음에 증가.
    - 코드에서 `int i = 1`으로 `i`가 `1`부터 시작한다.

**실행결과**

```java
1 2 3 
4 5 6 
7 8 9 
```

2차 배열 선언 부분이 new int[2][3]을 new int[4][5]처럼 다른 숫자로 변경해도 잘 동작하는 것을 확인가능

**new int[4][5]로 변경시 출력**

```java
1 2 3 4 5 
6 7 8 9 10 
11 12 13 14 15 
16 17 18 19 20 
```

## 향상된 for문

각각의 요소를 탐색한다는 의미로 for-each문이라고 부름. 향상된 for문(Enhanced For loop)

향상된 for문의 정의

```java
for(변수 : 배열 또는 컬렉션) {
  //배열 또는 컬렉션의 요소를 순회하며 수행할 작업
}
```

**EnhancedFor1**

```java
package array;

public class EnhacedFor1 {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4, 5};

        //일반 for문
        for (int i = 0; i < numbers.length; i++) {
            int number = numbers[i];
            System.out.print(number + " ");
        }

        System.out.println();

        //향상된 for문, for-each문
        for (int number : numbers) {
            System.out.print(number + " ");
        }
    }
}
```

**실행결과**

```java
1 2 3 4 5 
1 2 3 4 5 
```

**일반 for문**

```java
//일반 for문
for (int i = 0; i < numbers.length; i++) {
    int number = numbers[i];
    System.out.print(number + " ");
```

배열에 있는 값을 순서대로 읽어서 `number`변수에 넣고, 출력

배열은 처음부터 끝까지 순서대로 읽어서 사용하는 경우가 많다. 그런데 배열의 값을 읽으려면 `int i`와 같은 인덱스를 탐색할 수 있는 변수를 선언해야한다.

그리고 `i < numbers.length`와 같이 배열의 끝 조건을 지정해주어야한다. 마지막으로 배열의 값을 하나 읽을 때마다 인덱스를 하나씩 증가해야한다.

개발자 입장에서는 그냥 배열을 순서대로 처음부터 끝까지 탐색하고 싶은데, 너무 번잡하다.
따라서 향상된 for문이 등장.

**향상된 for문**

```java
//향상된 for문, for-each문
for (int number : numbers) {
    System.out.print(number + " ");
```

- 앞서 일반 for문과 동일하게 작동.
- 배열의 인덱스를 사용하지 않고, 종료 조건을 주지 않아도 된다. 단순히 해당 배열을 처음 부터 끝까지 탐색
- `:`의 오른쪽에 `numbers`와 같이 탐색할 배열을 선택하고, `:` 왼쪽에는 `int number`와 같이 반복할 때 마다 찾은 값을 저장할 변수를 선언한다.
그러면 배열의 값을 하나씩 꺼내 왼쪽의 `number`에 담고 for문을 수행.

**향상된 for문을 사용하지 못하는 경우**

향상된 for문에는 증가하는 인덱스 값이 감추어져 있기 때문에 `int i` 와 같은 증가하는 인덱스 값을 직접 사용해야 하는 경우에는 향상된 for문을 사용할 수 없다.

```java
//for-each문을 사용할 수 없는 경우, 증가하는 index 값 필요
for (int i = 0; i < numbers.length; i++) {
    System.out.println("number" + i + "번의 결과는: " + numbers[i]);
}
```

위의 경우 `i`값을 출력해야하므로 일반적인 for문을 사용.

아래 처럼 향상된 for문을 사용할 수 있지만, 이런 경우 일반적인 for문을 사용하는 것이 더욱 좋다.

```java
int i = 0
for (number : numbers) {
    System.out.println("number" + i + "번의 결과는: " + number);
    i++;
}
```

- 변수 `int i` 가 사용되는 구문이 for문밖에 없는데 너무 넓은 범위에서 사용되기 때문에 비효율 적이다.

## 문제와 풀이1

### 문제 - 배열을 사용하도록 변경

다음 문제를 배열을 사용해서 개선하자.

**ArrayEx1**

```java
package array.ex;

public class ArrayEx1 {
    public static void main(String[] args) {
        int student1 = 90;
        int student2 = 80;
        int student3 = 70;
        int student4 = 60;
        int student5 = 50;

        int total = student1 + student2 + student3 + student4 + student5;
        double average = (double) total / 5;

        System.out.println("점수 총합: " + total);
        System.out.println("점수 평균: " + average);
    }
}
```

**실행 결과 예시**

```java
점수 총합: 350
점수 평균: 70.0
```

**ArrayEx1Ref**

```java
package array.ex;

public class ArrayEx1Ref {
    public static void main(String[] args) {
        int[] students = {90, 80, 70, 60, 50};

        int total = 0;
        for (int student : students) {
            total += student;
        }
        double average = (double) total / 5;

        System.out.println("점수 총합: " + total);
        System.out.println("점수 평균: " + average);
    }
}
```

**실행 결과**

```java
점수 총합: 350
점수 평균: 70.0
```

### 문제 - 배열의 입력과 출력

사용자에게 5개의 정수를 입력받아서 배열에 저장하고, 입역 순서대로 출력하자.

출력시 포맷은 1, 2, 3, 4, 5와 같이 `,` 쉼표로 구분하고, 마지막에는 쉼표를 넣지 않아야한다.

실행결과와 예시를 참고하자.

**실행 결과 예시**

```java
5개의 정수를 입력하세요:
1
2
3
4
5
출력
1, 2, 3, 4, 5
```

**ArrayEx2**

```java
package array.ex;

import java.util.Scanner;

public class ArrayEx2 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        int[] arr = new int[5];

        System.out.println("5개의 정수를 입력하세요:");
        for (int i = 0; i < arr.length; i++) {
            int number = input.nextInt();
            arr[i] = number;
            }

        System.out.println("출력");
        for (int i : arr) {
            System.out.print(i);
            if (i < 5) {
                System.out.print(", ");
            }
        }
    }
}
```

**실행결과**

```java
package array.ex;

import java.util.Scanner;

public class ArrayEx2 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        int[] numbeers = new int[5];

        System.out.println("5개의 정수를 입력하세요:");
        for (int i = 0; i < numbeers.length; i++) {
            numbeers[i] = input.nextInt();
            }

        System.out.println("출력");
        for (int number : numbeers) {
            System.out.print(number);
            if (number < numbeers.length) {
                System.out.print(", ");
            }
        }
    }
}
```

### 문제 - 배열과 역순 출력

사용자에게 5개의 정수를 입력받아 배열에 저장하고, 입력받은 순서의 반대인 역순으로 출력.

출력시 출력 포맷은 5, 4, 3, 2, 1과 같이 `,` 쉼표로 구분하고, 마지막에는 쉼표를 넣지 않아야한다.

**실행 결과 예시**

```java
5개의 정수를 입력하세요;
1
2
3
4
5
입력한 정수를 역순으로 출력:
5, 4, 3, 2, 1
```

**ArrayEx3**

```java
package array.ex;

import java.util.Scanner;

public class ArrayEx3 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        int[] numbers = new int[5];

        System.out.println("5개의 정수를 입력하세요:");
        for (int i = 0; i < numbers.length; i++) {
            numbers[i] = input.nextInt();
        }

        System.out.println("출력");
        for (int i = numbers.length - 1; i >= 0; i--) {
            System.out.print(numbers[i]);
            if (i > 0) {
                System.out.print(", ");
            }
        }
    }
}

```

**실행 결과**

```java
5개의 정수를 입력하세요:
1
2
3
4
5
출력
5, 4, 3, 2, 1
```

### 문제 - 합계와 평균

사용자에게 5개의 정수를 입력받아서 이들의 정수의 합계와 평균을 계산하는 프로그램 작성

**실행 결과 예시**

```java
5개의 정수를 입력하세요:
1
2
3
4
5
입력한 정수의 합계: 1.5
입력한 정수의 평균: 3.0
```

**ArrayEx4**

```java
package array.ex;

import java.util.Scanner;

public class ArrayEx4 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        int[] nubmers = new int[5];
        int sum = 0;
        double average;

        System.out.println("5개의 정수를 입력하세요:");
        for (int i = 0; i < nubmers.length; i++) {
            nubmers[i] = input.nextInt();
            sum += nubmers[i]
        }

        average = (double) sum / nubmers.length;
        System.out.println("입력한 정수의 합계: " + sum);
        System.out.println("입력한 정수의 평균: " + average);
    }
}

```

**실행 결과**

```java
5개의 정수를 입력하세요:
1
2
3
4
5
입력한 정수의 합계: 15
입력한 정수의 평균: 3.0
```

### 문제 - 합계와 평균2

이전 문제에서 입력받을 숫자의 개수를 입력받도록 개선하자.

**실행 결과 예시1**

```java
입력받을 숫자의 개수를 입력하세요: 3
3개의 정수를 입력하세요:
1
2
3
입력한 정수의 합계: 6
입려한 정수의 평균: 2.0
```

**실행 결과 예시2**

```java
입력받을 숫자의 개수를 입력하세요: 5
5개의 정수를 입력하세요:
1
2
3
4
5
입력한 정수의 합계: 15
입력한 정수의 평균: 3.0
```

**ArrayEx4Ref**

```java
package array.ex;

import java.util.Scanner;

public class ArrayEx4Ref {
    public static void main(String[] agrs) {
        Scanner input = new Scanner(System.in);
        int sum = 0;
        double average;

        System.out.print("입력받을 숫자의 개수를 입력하세요: ");
        int arr_length = input.nextInt();
        int[] numbers = new int[arr_length];

        System.out.println(numbers.length + "개의 정수를 입력하세요:");
        for (int i = 0; i < arr_length; i++) {
            int number = input.nextInt();
            numbers[i] = number;
            sum += numbers[i];
        }
        average = (double) sum / numbers.length;

        System.out.println("입력한 정수의 합계: " + sum);
        System.out.println("입력한 정수의 평균: " + average);
    }
}
```

**실행 결과**

```java
입력받을 숫자의 개수를 입력하세요: 6
6개의 정수를 입력하세요:
1
2
3
4
5
6
입력한 정수의 합계: 21
입력한 정수의 평균: 3.5
```

## 문제와 풀이2

### 문제 - 가장 작은수, 큰 수 찾기

사용자로부터 n개의 정수를 입력받아 배열에 저장한 후, 배열 내에서 가장 작은 수와 가장 큰 수를 찾아 출력하는 프로그램을 작성하자

**실행 결과 예시**

```java
입력받을 숫자의 개수를 입력하세요: 3
3개의 정수를 입력하세요:
1
2
5
가장 작은 정수: 1
가장 큰 정수: 5
```

**ArrayEx6**

```java
package array.ex;

import java.util.Scanner;

public class ArrayEx6 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.print("입력받은 숫자의 개수를 입력하세요: ");
        int arrLength = input.nextInt();
        
        int[] numbers = new int[arrLength];
        int minNumber, maxNumber;

        System.out.println(numbers.length + "개의 정수를 입력하세요:");
        for (int i = 0; i < numbers.length; i++) {
            numbers[i] = input.nextInt();
        }

        minNumber = numbers[0];
        maxNumber = numbers[0];
        for (int number : numbers) {
            if (minNumber > number) {
                minNumber = number;
            }
            if (maxNumber < number) {
                maxNumber = number;
            }
        }

        System.out.println("가장 작은 정수: " + minNumber);
        System.out.println("가장 큰 정수: " + maxNumber);
    }
}
```

**실행 결과**

```java
입력받은 숫자의 개수를 입력하세요: 5
5개의 정수를 입력하세요:
6
4
1
2
4
가장 작은 정수: 1
가장 큰 정수: 6
```

### 문제 - 2차원 배열1

사용자로부터 4명의 학생의 국어, 수학, 영어 점수를 입력받아 각 학생의 총점과 평균을 계산하는 프로그램을 작성.

2차원 배열을 사용하고, 실행 결과 예시를 참고하자.

**실행 결과 예시**

```java
1번 학생의 성적을 입력하세요: 
국어 점수: 100
영어 점수: 80
수학 점수: 70
2번 학생의 성적을 입력하세요: 
국어 점수: 30
영어 점수: 40
수학 점수: 50
3번 학생의 성적을 입력하세요: 
국어 점수: 60
영어 점수: 70
수학 점수: 50
4번 학생의 성적을 입력하세요: 
국어 점수: 90
영어 점수: 100
수학 점수: 80
1번 학생의 총점: 250, 평균: 83.33333333333333
2번 학생의 총점: 120, 평균: 40.0
3번 학생의 총점: 180, 평균: 60.0
4번 학생의 총점: 270, 평균: 90.0
```

**ArrayEx7 - 내 풀이**

```java
package array.ex;

import java.util.Scanner;

public class ArrayEx7 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        int[][] students = new int[4][3];

        for (int student = 0; student < students.length; student++) {
            System.out.println((student + 1) + "번 학생의 성적을 입력하세요: ");
            for (int score = 0; score < students[student].length; score++) {
                if (score == 0) {
                    System.out.print("국어 점수: ");
                    int korean = input.nextInt();
                    students[student][score] = korean;
                }
                if (score == 1) {
                    System.out.print("영어 점수: ");
                    int english = input.nextInt();
                    students[student][score] = english;
                }
                if (score == 2) {
                    System.out.print("수학 점수: ");
                    int math = input.nextInt();
                    students[student][score] = math;
                }
            }
        }
        for (int student = 0; student < students.length; student++) {
            int sum = 0;
            for (int score = 0; score < students[student].length; score++) {
                sum += students[student][score];
            }
            double average = (double) sum / students[student].length;
            System.out.println((student + 1) + "번 학생의 총점: " + sum + ", " + "평균: " + average);
        }
    }
}
```

**ArrayEx7_2 김영한 풀이**

```java
package array.ex;

import java.util.Scanner;

public class ArrayEx7 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        int[][] students = new int[4][3];
        String[] subject = {"국어", "영어", "수학"};

        for (int i = 0; i < students.length; i++) {
            System.out.println((i + 1) + "번 학생의 성적을 입력하세요: ");
            for (int j = 0; j < students[i].length; j++) {
                System.out.print(subject[j] + " 점수: ");
                students[i][j] = input.nextInt();
            }
        }
        for (int i = 0; i < students.length; i++) {
            int total = 0;
            for (int j = 0; j < students[i].length; j++) {
                total += students[i][j];
            }
            double average = (double) total / students[i].length;
            System.out.println((i + 1) + "번 학생의 총점: " + total + ", " + "평균: " + average);
        }
    }
}
```

**실행 결과**

```java
1번 학생의 성적을 입력하세요: 
국어 점수: 80
영어 점수: 90
수학 점수: 70
2번 학생의 성적을 입력하세요: 
국어 점수: 100
영어 점수: 60
수학 점수: 90
3번 학생의 성적을 입력하세요: 
국어 점수: 90
영어 점수: 90
수학 점수: 80
4번 학생의 성적을 입력하세요: 
국어 점수: 95
영어 점수: 75
수학 점수: 100
1번 학생의 총점: 240, 평균: 80.0
2번 학생의 총점: 250, 평균: 83.33333333333333
3번 학생의 총점: 260, 평균: 86.66666666666667
4번 학생의 총점: 270, 평균: 90.0
```

### 문제 - 2차원 배열2

**실행 결과 예시**

```java
학생 수를 입력하세요: 3
1번 학생의 성적을 입력하세요: 
국어 점수: 80
영어 점수: 90
수학 점수: 70
2번 학생의 성적을 입력하세요: 
국어 점수: 100
영어 점수: 60
수학 점수: 90
3번 학생의 성적을 입력하세요: 
국어 점수: 90
영어 점수: 90
수학 점수: 80
1번 학생의 총점: 240, 평균: 80.0
2번 학생의 총점: 250, 평균: 83.33333333333333
3번 학생의 총점: 260, 평균: 86.66666666666667
```

**ArrayEx8**

```java
package array.ex;

import java.util.Scanner;

public class ArrayEx8 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.print("학생 수를 입력하세요: ");
        int n = input.nextInt();
        int[][] students = new int[n][3];
        String[] subject = {"국어", "영어", "수학"};

        for (int i = 0; i < students.length; i++) {
            System.out.println((i + 1) + "번 학생의 성적을 입력하세요: ");
            for (int j = 0; j < students[i].length; j++) {
                System.out.print(subject[j] + " 점수: ");
                students[i][j] = input.nextInt();
            }
        }
        for (int i = 0; i < students.length; i++) {
            int total = 0;
            for (int j = 0; j < students[i].length; j++) {
                total += students[i][j];
            }
            double average = (double) total / students[i].length;
            System.out.println((i + 1) + "번 학생의 총점: " + total + ", " + "평균: " + average);
        }
    }
}
```

**실행결과**

```java
학생 수를 입력하세요: 4
1번 학생의 성적을 입력하세요: 
국어 점수: 10
영어 점수: 10
수학 점수: 10
2번 학생의 성적을 입력하세요: 
국어 점수: 20
영어 점수: 20
수학 점수: 20
3번 학생의 성적을 입력하세요: 
국어 점수: 30
영어 점수: 30
수학 점수: 30
4번 학생의 성적을 입력하세요: 
국어 점수: 40
영어 점수: 40
수학 점수: 40
1번 학생의 총점: 30, 평균: 10.0
2번 학생의 총점: 60, 평균: 20.0
3번 학생의 총점: 90, 평균: 30.0
4번 학생의 총점: 120, 평균: 40.0
```

## 문제와 풀이3

### 상품 관리 프로그램 만들기

자를 이용하여 상품 관리 프로그램을 만들어 보자. 이 프로그램은 다음의 기능이 필요하다.

- 상품 등록: 상품 이름과 가격을 입력받아 저장.
- 상품 목록: 지금까지 등록한 모든 상품의 목록을 출력.

다음과 같이 동작한다.

- 첫 화면에서 사용자에게 세 가지 선택을 제시 “1. 상품등록”, ”2. 상품 목록”, ”3. 종료”
- “1. 상품 등록”을 선택하면, 사용자로부터 상품 이름과 가격을 입력받아 배열에 저장
- “2. 상품 목록”을 선택하면, 배열에 저장된 모든 상품 출력
- “3. 종료”를 선택하면 프로그램 종료

**제약조건**

상품은 최대 10개 까지 등록 가능.

사용해야하는 변수 및 구조

- `Scanner scanner`: 사용자 입력을 받기 위한 객체
- `String[] productName`: 상품의 이름을 받기 위한 String 배열
- `int[] productPrices`: 상품 가격을 저장할 int 배열
- `int productCount`: 현재 등록된 상품의 개수를 저장할 int변수

**실행 결과 예시**

**ProductAdminEx_1 (내가 작성한 코드)**

```java
package array.ex;

import java.util.Scanner;

public class ProductAdminEx_1 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String[] productName = new String[10];  // 상품 이름 최대 등록 갯수 10개
        int[] productPrice = new int[10];  // 상품 가격 최대 등록 갯수 10개
        int productCount = 0;  // 현재 상품 등록 개수

        while (true) {
            System.out.println("1. 상품 등록 | 2. 상품 목록 | 3. 종료");
            System.out.print("메뉴를 선택하세요: ");
            int select = scanner.nextInt();
            scanner.nextLine();  // \n 버림
            if (select == 1) {
                if (productCount == 10) {
                    System.out.println("더 이상 상품을 등록할 수 없습니다.");
                } else {
                    System.out.print("상품 이름을 입력하세요: ");
                    String name = scanner.nextLine();
                    productName[productCount] = name;

                    System.out.print("상품 가격을 입력하세요: ");
                    int price = scanner.nextInt();
                    productPrice[productCount] = price;
                    productCount++;
                }

            } else if (select == 2) {
                if (productCount == 0) {
                    System.out.println("등록된 상품이 없습니다.");
                } else {
                    for (int i = 0; i < productCount; i++) {
                        System.out.println(productName[i] + ": " + productPrice[i]);
                    }
                }

            } else if (select == 3) {
                System.out.println("프로그램을 종료합니다.");
                break;
            } else {
                System.out.println("올바른 명령어를 입력하세요.");
            }
        }
    }
}
```

**실행 결과**

```java
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: MacBook Air
상품 가격을 입력하세요: 1390000
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: MacBook Pro
상품 가격을 입력하세요: 1690000
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: Ipad Pro
상품 가격을 입력하세요: 1499000
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: Ipad Air
상품 가격을 입력하세요: 899000
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: Ipad
상품 가격을 입력하세요: 529000
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: Iphone
상품 가격을 입력하세요: 1550000
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: Iphone 15
상품 가격을 입력하세요: 1250000
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: Iphone 14
상품 가격을 입력하세요: 1090000
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: Iphone 13
상품 가격을 입력하세요: 950000
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: Iphone SE
상품 가격을 입력하세요: 650000
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
더 이상 상품을 등록할 수 없습니다.
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 2
MacBook Air: 1390000
MacBook Pro: 1690000
Ipad Pro: 1499000
Ipad Air: 899000
Ipad: 529000
Iphone: 1550000
Iphone 15: 1250000
Iphone 14: 1090000
Iphone 13: 950000
Iphone SE: 650000
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 3
프로그램을 종료합니다.
```

**ProductAdminEx_2 (김영한 제안 코드)**

```java
package array.ex;

import java.util.Scanner;

public class ProductAdminEx_2 {
    public static void main(String[] args) {
        int maxProduct = 10;
        String[] productName = new String[maxProduct];  // 상품 이름 최대 등록 갯수 10개
        int[] productPrice = new int[maxProduct];  // 상품 가격 최대 등록 갯수 10개
        int productCount = 0;  // 현재 상품 등록 개수

        Scanner scanner = new Scanner(System.in);
        while (true) {
            System.out.print("1. 상품 등록 | 2. 상품 목록 | 3. 종료\n메뉴를 선택하세요: ");
            int menu = scanner.nextInt();
            scanner.nextLine();  // \n 버림
            // menu_1
            if (menu == 1) {

                if (productCount >= maxProduct){
                    System.out.println("더 이상 상품을 등록할 수 없습니다.");
                    continue;
                }

                System.out.print("상품 이름을 입력하세요: ");
                productName[productCount] = scanner.nextLine();

                System.out.print("상품 가격을 입력하세요: ");
                productPrice[productCount] = scanner.nextInt();

                productCount++;
            
            // menu_2
            } else if (menu == 2) {
                if (productCount == 0) {
                    System.out.println("등록된 상품이 없습니다.");
                    continue;
                }
                for (int i = 0; i < productCount; i++) {
                    System.out.println(productName[i] + ": " + productPrice[i]);
                }

            // menu_3
            } else if (menu == 3) {
                System.out.println("프로그램을 종료합니다.");
                break;
            } else {
                System.out.println("올바른 명령어를 입력하세요.");
            }
        }
    }
}
```

**실행결과**

```java
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 2
등록된 상품이 없습니다.
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: 1
상품 가격을 입력하세요: 10
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: 2
상품 가격을 입력하세요: 20
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: 3
상품 가격을 입력하세요: 30
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: 4
상품 가격을 입력하세요: 40
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: 5
상품 가격을 입력하세요: 50
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: 6
상품 가격을 입력하세요: 60
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: 7
상품 가격을 입력하세요: 70
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: 8
상품 가격을 입력하세요: 80
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: 9
상품 가격을 입력하세요: 90
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
상품 이름을 입력하세요: 10
상품 가격을 입력하세요: 100
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 1
더 이상 상품을 등록할 수 없습니다.
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 2
1: 10
2: 20
3: 30
4: 40
5: 50
6: 60
7: 70
8: 80
9: 90
10: 100
1. 상품 등록 | 2. 상품 목록 | 3. 종료
메뉴를 선택하세요: 3
프로그램을 종료합니다.
```
