[<back](https://www.notion.so/basic-_-3ee7f95ac24d43f881b7ee2e8f21c210?pvs=21)

---

<aside>
📄 목차

</aside>

## 기본형 vs 참조형1 - 시작

**자바에서는 참조형을 제대로 이해하는 것은 정말 중요하다.**

변수의 데이터 타입을 가장 크게 보면 기본형과 참조형으로 분류.

사용하는 값을 변수에 직접 넣는 기본형.

`Student student1`과 같이 객체가 저장된 메모리 위치를 가르키는 참조값을 넣을 수 있는 참조형으로 분류

- 기본형(Primitive Type): `int`, `long`, `double`, `boolean` 처럼 변수에 사용할 값을 직접 넣을 수 있는 데이터 타입.
- 참조형(Reference Type): `Student student1`, `int[] students` 와 같이 데이터에 접근하기 위한 참조(주소)를 저장하는 데이터 타입. 참조형은 객체 또는 배열에 사용됨.

**기본형 vs 참조형 - 기본**

- 기본형은 숫자 `10`, `20`과 같이 실제 사용하는 값을 변수에 담을 수 있다. 그래서 해당 값을 바로 사용가능
- 참조형은 실제 사용하는 값을 변수에 담는 것이 아닌, 객체의 위치(참조, 주소)를 저장. 참조형에는 객체와 배열이 있다.
    - 객체는 `.`(dot)을 통해서 메모리 상에 생성된 객체를 찾아가야 사용가능.
    - 배열은 `[]`를 통해서 메모리 상에 생성된 배열을 찾아가야 사용가능.

**기본형 vs 참조형 - 계산**

- 기본형은 들어있는 값을 그대로 계산에 사용할 수 있다.
- 참조형은 들어있는 참조값을 그대로 사용할 수 없다. → 주소지만 가지고 있기 때문

**쉽게 이해하는 팁**

기본형을 제외한 나머지는 모두 참조형

- 기본형은 소문자로 시작 `int`, `long`, `double`, `boolean`
    - 자바가 기본으로 제공하는 데이터 타입. 개발자가 새로 정의 할 수 없다.
- 클래스는 대문자로 시작 `Student`
    - 클래스는 모두 참조형이다.

**참고 - String**

자바에서 String은 클래스(참조형)이지만 자주 사용되므로 특별히 기본형처럼 문자 값을 바로 대입가능.

## 기본형 vs 참조형2 - 변수 대입

**대원칙: 자바는 항상 변수의 값을 복사해서 대입한다.**

자바에서 변수에 값을 대입하는 것은 변수에 들어있는 값을 복사해서 대입하는 것.

기본형, 참조형 모두 항상 변수에 있는 값을 복사해서 대입. 기본형이면 변수에 들어있는 실제 값을 복사해서 대입

참조형이면 변수에 들어이쓴 값을 복사해서 대입

**기본형 대입**

```java
int a = 10;
int b = a;
```

**참조형 대입**

```java
Student s1 = new Student()
Student s2 = s1;
```

기본형의 경우 값을 대입하더라도 실제 사용하는 값이 변수에 바로 들어있기 때문에 쉽게 이해가능

**참조형의 경우 실제 사용하는 객체가 아니라 객체 위치를 가르키는 참조값만 복사**

### 기본형과 변수 대입

다음 코드를 보고 어떤 결과가 나올것인가 생각해보자.

**VarChange1**

```java
package ref;

public class VarChange1 {

    public static void main(String[] args) {

        int a = 10;
        int b = a;
        System.out.println("a = " + a);
        System.out.println("b = " + b);

        //a 변경
        a = 20;
        System.out.println("변경 a = 20");
        System.out.println("a = " + a);  //a = 20
        System.out.println("b = " + b);  //b = 10

        b = 30;
        System.out.println("변경 b = 30");
        System.out.println("a = " + a);  //a = 20
        System.out.println("b = " + b);  //b = 30
    }
}
```

**실행결과**

```java
a = 10
b = 10
변경 a = 20
a = 20
b = 10
변경 b = 30
a = 20
b = 30
```

그림 통해 자세히 알아보기

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/28274783-e31f-453b-8e60-f7a7e157864b/fb5997ce-6642-4d45-ad0e-3aade10608fd.png)

**실행결과**

```java
a = 10
b = 10
```

변수의 대입은 변수에 들어있는 값을 복사해서 대입.

여기서 변수 `a`에 들어있는 값 `10`을 복사해서 변수 `b`에 대입. 변수 `a`자체를 `b`에 대입하는 것이 아님.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/74dbd0e4-c85e-4e3c-ab22-d09b84f76e89/f590c88e-9775-4b1b-9e59-1b5dac7af662.png)

**실행결과**

```java
a = 20
b = 10
```

변수 `a`에 값 `20`을 대입했다. 따라서 변수 `a`의 값이 `10`에서 `20`으로 변경되었다. `b`에는 아무런 영향을 주지 않는다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/e3b25f4d-eca6-4bec-9119-ed49c5f249af/15ce1199-7890-4e7e-877c-e9b563e02f1f.png)

**실행결과**

```java
a = 20
b = 30
```

`b`의 값이 `30`으로 변경되고 `a`에는 영향 없음

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/fc502490-f688-4b17-ae48-b4031c18a796/cf33a063-88c9-4e95-9376-d8634790859b.png)

여기서 핵심은 `int a = b`라고 했을 때 변수에 들어있는 값을 복사해 전달하는다는 점.

<aside>
📄 더 쉽게 얘기하면

**변수가 담고 있는 값을 바인딩 하는 것이다.**

```java
int a = 10;
System.out.println(a)  // 10
int b = a;

a = 20;
System.out.println(a)  // 20
System.out.println(b)  // 10
```

즉, `int b = a;` 로 바인딩 할 시점 a를 출력하면 a변수가 담고있는 값(10)이 b로 바인딩 되는 것.

참조형의 경우

```java
String[] students = new String[3];
students[0] = "학생1";
System.out.println(students);  // x001
System.out.println(students[0]);  // "학생1"

String[] a = students;
System.out.println(a);  // x001
a[0] = "학생76";
System.out.println(students[0]);  // "학생76"
```

즉, `Students a = students` 로 바인딩 되는 값은 참조값이므로, a의 경우 새로운 값이 바인딩된 것이아니라 주소만 바인딩된 것이므로 수정된 값이 동일하게 나타난다.

</aside>

### 참조형과 변수 대입

**Data**

```java
package ref;

public class Data {
    int value;
}
```

**VarChange2**

```java
package ref;

public class VarChange2 {

    public static void main(String[] args) {
        Data dataA = new Data();
        dataA.value = 10;
        Data dataB = dataA;  // dataB.value = 10

        System.out.println("dataA 참조값= " + dataA);  //x001
        System.out.println("dataB 참조값= " + dataB);  //x001
        System.out.println("dataA.value= " + dataA.value);  //dataA.value = 10
        System.out.println("dataB.value= " + dataB.value);  //dataB.value = 10

        //dataA 변경
        dataA.value = 20;
        System.out.println("변경 dataA.value = 20");
        System.out.println("dataA.value= " + dataA.value);  // dataA.value = 20
        System.out.println("dataB.value= " + dataB.value);  // dataB.value = 20

        //dataB 변경
        dataB.value = 30;
        System.out.println("변경 dataB.value = 30");
        System.out.println("dataA.value= " + dataA.value);  // dataA.value = 30
        System.out.println("dataB.value= " + dataB.value);  // dataB.value = 30
    }
}
```

**실행결과**

```java
dataA 참조값= ref.Data@6acbcfc0
dataB 참조값= ref.Data@6acbcfc0
dataA.value= 10
dataB.value= 10
변경 dataA.value = 20
dataA.value= 20
dataB.value= 20
변경 dataB.value = 30
dataA.value= 30
dataB.value= 30
```

그림을 통한 설명

**Data dataA = new Data()**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f12862eb-44b0-47b9-8649-1ef8222c0d13/9a8934c9-66e1-4cb4-80c6-a3c6b1253503.png)

`dataA` 변수는 `Data`클래스를 통해서 만들었기에 참조형.

이 변수는 `Data`형 객체 참조값을 저장. `Data`객체를 생성하고, 참조값을 `dataA`에 저장, 그리고 객체의 `value` 변수에 값 10을 저장

**Data dataB = dataA**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/841a02dc-730d-426f-b328-10c5444f4293/20d997e4-5a04-469a-8bc6-7582fc75a69f.png)

**실행 코드**

```java
Data dataA = new Data();
dataA.value = 10;
Data dataB = dataA;  // dataB.value = 10

System.out.println("dataA 참조값= " + dataA);  //x001
System.out.println("dataB 참조값= " + dataB);  //x001
System.out.println("dataA.value= " + dataA.value);  //dataA.value = 10
System.out.println("dataB.value= " + dataB.value);  //dataB.value = 10
```

**출력 결과**

```java
dataA 참조값= ref.Data@6acbcfc0
dataB 참조값= ref.Data@6acbcfc0
dataA.value= 10
dataB.value= 10
```

변수 `dataA`가 가르키느 인스턴스를 복사하는 것이 아니라 참조값만 복사해서 전달.

**dataA.value = 20**

1. dataA.value = 20

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/c1c4fb8c-14c9-430a-aafd-427c9118eb24/da30dc59-cddb-4c69-9f51-8a4bce817fcb.png)

**실행코드**

```java
//dataA 변경
dataA.value = 20;
System.out.println("변경 dataA.value = 20");
System.out.println("dataA.value= " + dataA.value);  // dataA.value = 20
System.out.println("dataB.value= " + dataB.value);  // dataB.value = 20
```

**출력결과**

```java
변경 dataA.value = 20
dataA.value= 20
dataB.value= 20
```

**dataB.value = 30 도 위와 같이 실행됨.**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/0928ba9d-619e-4028-9ff9-d9f3b13aaf52/66a82d5a-c6c5-4ed2-a112-4404ea27aa6c.png)

여기서 핵심은 `Data dataB = dataA`라고 했을 때 변수에 들어있는 값을 복사해서 사용한다는 점.

두 변수는 같은 객체 인스턴스를 참조하게 된다.

## 기본형 vs 참조형3 - 메서드 호출

**대원칙: 자바는 항상 변수의 값을 복사해서 대입한다.**

- 기본형 → 실제 값 복사해서 대입
- 참조형 → 참조값(주소값) 복사해서 대입

메서드 호출 시 매개변수(파라미터)도 결국 변수일 뿐.

### 기본형과 메서드 호출

**MethodChange1**

```java
package ref;

public class MethodChange1 {

    public static void main(String[] args) {
        int a = 10;
        System.out.println("메서드 호출 전: a = " + a);  //10
        changePrimitive(a);
        System.out.println("메서드 호출 후: a = " + a);  //10
    }

    static void changePrimitive(int x) {
        x = 20;
    }
}
```

**실행 결과**

```java
메서드 호출 전: a = 10
메서드 호출 후: a = 10
```

1. **메서드 호출**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/7c769045-d7c7-4942-87e6-1d65adfc1ca3/f1a7939a-f19f-4cbd-a85a-8c987ce0333b.png)

메서드를 호출할 때 매개변수 x에 변수 a의 값을 전달한다.

```java
int x = a
```

자바에서 변수에 값을 대입하는 것은 항상 값을 복사해서 대입한다. 따라서 변수 `a`, `x` 각각 숫자 `10`을 가지고있다.

1. **메서드 안에서 값을 변경**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/1839d542-4e70-4f94-b845-c18621944f06/4a96d173-4c4b-4159-9911-3fd08c321bcf.png)

메서드 안에서 `x = 20`으로 새로운 값을 대입

결과적으로 `x`의 값만 `20`으로 변경되고, `a`의 값은 `10`으로 유지됨.

1. **메서드 종료**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/a24d9ccf-61f9-4609-835b-9c6ad3de831f/0941a682-228f-48b7-9b15-245524b3f9a5.png)

메서드 종료 후 값을 확인해보면 `a`는 `10`이 출력, 매개변수 `x`는 제거됨.

### 참조형과 메서드 호출

**MethodChange2**

```java
package ref;

public class MethodChange2 {
    public static void main(String[] args) {
        Data dataA = new Data();
        dataA.value = 10;
        System.out.println("메서드 호출 전: dataA.value = " + dataA.value);  // 10
        changeReference(dataA);  // dataA.value가 바뀌는 것이기 때문에 다시 참조할 때는 바뀐 값 적용
        System.out.println("메서드 호출 후: dataA.value = " + dataA.value);  // 20
    }

    public static void changeReference(Data dataX) {
        dataX.value = 20;
    }
}
```

**실행결과**

```java
메서드 호출 전: dataA.value = 10
메서드 호출 후: dataA.value = 20
```

**그림으로 설명**

`Data` 인스턴스를 생성하고, 참조값을 `dataA` 변수에 담고 `value`에 숫자 `10`을 할당한 상태는 다음과 같다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/f67c7799-b7d2-4131-ae16-b05f81e98b17/4e42de90-5c3b-47bc-94f4-e170f2eb1d39.png)

1. **메서드 호출**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/31a2feb5-7ace-4f37-95ed-333082e7bae2/f611eeae-be57-40d8-a2f6-f549d0284fc1.png)

메서드를 호출할 때 매개변수 dataX에 변수 dataA의 값이 전달.

```java
Data dataX = dataA
```

자바에서 변수에 값을 대입하는 것은 항상 값을 복사해서 대입한다.

변수 `dataA`는 참조값 `x001`을 가지고 있으므로 참조값을 복사해서 전달.

따라서, 변수 `dataA`, `dataX` 둘다 같은 참조값인 `x001`을 가지게 된다.

이제 `dataX`를 통해서도 `x001`에 있는 Data인스턴스에 접근 가능.

1. **메서드 안에서 값을 변경**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/e76a6b60-473d-4403-a44f-8a10224e9ce6/b8e9a7cc-6e3b-4a3c-a254-ee05c9887d53.png)

메서드 안에서 `dataX.value = 20`으로 새로운 값을 대입.

참조값을 통해 `x001` 인스턴스에 접근하고 그 안에 있는 `value` 값을 `20`으로 변경.

`dataA`, `dataX` 모두 같은 `x001` 인스턴스를 참조하기 때문에 `dataA.value`와 `dataX.value`는 둘다 `20`이라는 값을 가진다.

1. **메서드 종료**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/cf50104c-b289-48f2-8d3e-044c7cb8f4e2/ab9202a0-cbb3-4d4a-ab79-f234dcb53b23.png)

메서드 종료 후 `dataA.value`의 값을 확인해보면 다음과 같이 20으로 변경된 것을 확인할 수 있다.

```java
메서드 호출 전: dataA.value = 10
메서드 호출 후: dataA.value = 20
```

**기본형과 참조형의 메서드 호출**

자바에서 메서드의 매개변수(파라미터)는 항상 값에 의해 전달된다. 그러나 이 값이 실제값이냐 참조값(주소값)이냐 에따라 동작이 달라진다.

- 기본형: 메서드 내부에서 매개변수(파라미터)값을 변경해도, 호출자의 값에는 영향이 없다.
    - 서로 다른 박스에 있는 것.
- 참조형: 메서드 내부에서 매개변수(파라미터)값을 변경하면, 호출자의 객체도 변경된다.
    - 주소를 가져다 주기때문에 서로 같은 박스에 있는 것.

## 참조형과 메서드 호출 - 활용

이전에 보았던 `class1. ClassStudent3` 코드에는 중복되는 부분이 2가지 있다.

- `name`, `age`, `grade` 에 값을 할당
- 학생정보 출력

**ClassStudent3**

```java
package class1;

public class ClassStudent3 {

    public static void main(String[] args) {
        Student student1;  //String student; 와 같은 표
        student1 = new Student();
        student1.name = "학생1";
        student1.age = 15;
        student1.grade = 90;

        Student student2 = new Student();
        student2.name = "학생2";
        student2.age = 16;
        student2.grade = 80;

        System.out.println(student1);
        System.out.println(student2);

        System.out.println("이름: " + student1.name + " 나이: " + student1.age + " 성적: " + student1.grade);
        System.out.println("이름: " + student2.name + " 나이: " + student2.age + " 성적: " + student2.grade);
    }
}
```

이런 중복은 메서드를 통해 쉽게 제거할 수 있다.

### 메서드에 객체 전달

**Method1**

```java
package ref;

public class Method1 {

    public static void main(String[] args) {
        Student student1 = new Student();
        initStudent(student1, "학생1", 15, 90);

        Student student2 = new Student();
        initStudent(student2, "학생2", 16, 80);

        printStudent(student1);
        printStudent(student2);

    }

    public static void initStudent(Student student, String name, int age, int grade) {
        student.name = name;
        student.age = age;
        student.grade = grade;
    }

    public static void printStudent(Student student) {
        System.out.println("이름: " + student.name + " 나이: " + student.age + " 성적: " + student.grade);
    }
}
```

**실행결과**

```java
이름: 학생1 나이: 15 성적: 90
이름: 학생2 나이: 16 성적: 80
```

참조형 메서드를 호출할 때 참조값을 전달. 따라서 메서드 내부에서 전달된 참조값을 통해 객체의 값을 변경하거나 사용가능.

- `initStudent(Student student, …)`: 전달한 학생 객체(인스턴스)의 필드(멤버 변수)에 값을 설정
- `printStudent(Student student, …)`: 전달한 학생 객체(인스턴스)의 필드(멤버 변수)값을 읽고 출력

**initStudent() 메서드 호출 분석**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/c78e3304-6802-4a5e-8e1a-730068a62c1b/977c4360-fb3d-4574-bb43-ecfa00b1c6ba.png)

- `student1` 이 참조하는 `Student` 인스턴스에 값을 편리하게 할당하고 싶어서 `initStudent()` 메서드 만듦.
- 이 메서드를 호출하면서 `student1`을 전달, 그러면 `student1`의 참조값이 매개변수 `student`에 전달.
- 이 참조값을 통해 `initStudent()`메서드 안에서 `student1`이 참조하는 것과 동일한 `x001` `Student` 인스턴스에 접근하고 값을 변경

**주의!**

```java
package ref;

import class.Student;

public class Method {
    ...
}
```

- `import calss1.Student;` 이 선언되어 있으면 안된다.
- 이렇게 되면 `class1` 패키지에서 선언한 `Student`를 사용하게 된다. 이 경우 접근 제어자 때문에 컴파일 오류가 발생.
- 만약 선언되어있다면 삭제하자. 삭제하면 같은 패키지에 있는 `ref.Student`를 사용

### 메서드에서 객체 변환

**Method3**

```java
package ref;

public class Method3 {

    public static void main(String[] args) {
        Student student1 = createStudent("학생1", 15, 90);
        Student student2 = createStudent("학생2", 16, 80);
        printStudent(student1);
        printStudent(student2);
    }

    public static Student createStudent(String name, int age, int grade) {
        Student student = new Student();  //x001
        student.name = name;
        student.age = age;
        student.grade = grade;
        return student;  //x001
    }

    public static void printStudent(Student student) {
        System.out.println("이름: " + student.name + " 나이: " + student.age + " 성적: " + student.grade);
    }
}
```

- `createStudent()`라는 메서드를 만들고 객체를 생성하는 부분도 메서드 안에 포함.
    - 따라서 이 메서드로 객체의 생성과 초기값 설정까지 모두 처리함.
- 그런데 메서드 안에서 객체를 생성했기 때문에 밖에서도 사용할 수 있게 주소값을 반환해야한다.
    - 따라서 `return student`를 통해 객체의 참조값을 메서드 밖으로 반환.

**createStudent() 메서드 분석**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/39f60a08-834e-4696-917b-1c8767a60ac1/339d287c-2e8f-4ae5-bf8c-cf0120eb4b45.png)

메서드 내부에서 인스턴스 생성한 후 참조값을 메서드 외부로 반환

이 참조값만 있으면 해당 인스턴스에 접근할 수 있음.

**참조값 확인**

```java
package ref;

public class Method3 {

    public static void main(String[] args) {
        Student student1 = createStudent("학생1", 15, 90);
        System.out.println(student1);
        Student student2 = createStudent("학생2", 16, 80);
        System.out.println(student2);
        printStudent(student1);
        printStudent(student2);
    }

    public static Student createStudent(String name, int age, int grade) {
        Student student = new Student();  //x001
        student.name = name;
        student.age = age;
        student.grade = grade;
        System.out.println(student);
        return student;
    }

    public static void printStudent(Student student) {
        System.out.println("이름: " + student.name + " 나이: " + student.age + " 성적: " + student.grade);
    }
}
```

**실행결과**

```java
ref.Student@6acbcfc0
ref.Student@6acbcfc0
ref.Student@5f184fc6
ref.Student@5f184fc6
이름: 학생1 나이: 15 성적: 90
이름: 학생2 나이: 16 성적: 80
```

## 변수와 초기화

### 변수의 종류

- 멤버 변수(필드): 클래스 선언
- 지역 변수: 메서드에 선언, 매개변수도 지역 변수의 한 종류이다.

멤버 변수, 필드 예시

```java
public class Student {
    String name;
    int age;
    int grade;
}
```

`name`, `age`, `grade`는 멤버 변수

**지역 변수 예시**

```java
public class ClassStart3 {
    public static void main(String[] agrgs) {
        Student student1;
        student = new Student();
        Student student2 = new Student();
    }
}
```

`student1`, `student2` 는 지역변수이다.

```java
package ref;

public class MethodChange1 {

    public static void main(String[] args) {
        int a = 10;
        System.out.println("메서드 호출 전: a = " + a);  //10
        changePrimitive(a);
        System.out.println("메서드 호출 후: a = " + a);  //10
    }

    static void changePrimitive(int x) {
        x = 20;
    }
}
```

`a`, `x`(매개변수)는 지역변수이다.

지역 변수는 이름 그대로 특정 지역에서만 사용되는 변수라는 뜻.

예를 들어서 변수 `x`는 `changePrimitive()` 메서드의 블록에서만 사용된다.

`changePirmitive()` 메서드가 끝나면 제거된다. `a`변수도 마찬가지 `main()`메서드가 끝나면 제거

### 변수의 값 초기화

- 멤버 변수: 자동 초기화
    - 인스턴스의 멤버 변수는 인스턴스를 생성할 때 자동으로 초기화
    - 숫자(`int`) = `0`, `boolean` = `false`, 참조형 = `null` (`null`은 참조할 대상이 없다는 뜻)
    - 개발자가 초기값을 직접 지정
- 지역 변수: 수동 초기화
    - 지역 변수는 항상 직접 초기화

멤버변수의 초기화

**InitData**

```java
package ref;

public class InitData {
    int value1;  // 초기화하지 않음
    int value2 = 10;  // 10으로 초기화
}
```

**InitMain**

```java
package ref;

public class InitMain {
    public static void main(String[] args) {
        InitData data = new InitData();
        System.out.println(data.value1);
        System.out.println(data.value2);
    }
}
```

**실행결과**

```java
value1 = 0
value2 = 10
```

`value1`은 초기값을 지정하지 않았지만 멤버 변수는 자동으로 초기화 된다. 숫자는 `0`으로 초기화

`value2`는 `10`으로 초기값을 지정해 두었기 때문에 객체를 생성할 때 `10`으로 초기화

### null

참조형 변수에는 항상 객체가 있는 위치를 가리키는 참조값이 들어간다. 그런데 아직 가리키는 대상이 없거나, 가리키는 대상을 나중에 입력하고 싶다면 `null`이라는 특별한 값을 넣는다.

**NullMain1**

```java
package ref;

public class NullMain1 {

    public static void main(String[] args) {
        Data data = null;
        System.out.println(data);
        data = new Data();
        System.out.println(data);
        data = null;
        System.out.println(data);
    }
}
```

**실행결과**

```java
null
ref.Data@6acbcfc0
null
```

**그림으로 알아보기**

**Data data = null**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/5122229c-99f0-45fc-aa21-e4fb523723dc/ffbedf3b-8e40-4d09-899f-03974b133078.png)

`Data` 타입을 받을 수있는 참조형 변수 `data` 를 만들었다. 그리고 여기에 `null`값을 할당했다. 따라서 `data` 변수에는 아직 가리키는 객체가 없다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/d9b850e6-b686-4609-9976-3a1e7fb1304e/7de3b9e4-0144-4088-83df-6c487ea3f880.png)

이후 새로운 `Data` 객체를 생성해서 그 참조값을 `data` 변수에 할당 했다. `data` 변수가 참조할 객체가 존재

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/21d1f4bd-352b-4882-9177-0cffdcf231ef/48222cc6-9d18-40cf-8b62-4008f9be1c58.png)

마지막에는 `data`에 다시 `null` 값을 할당했다. 이렇게 하면 `data` 변수는 앞서 만든 `Data`인스턴스 더는 참조 하지 않는다.

**GC - 아무도 참조하지 않는 인스턴스의 최후**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/6fe56060-5934-4ca3-b6bf-a29c441ddb7e/d47b73be-a936-47f3-afde-700f0c47e1f5.png)

C는 메모리를 수동으로 삭제해주어야 했다. 그러나 java에서는 GC(가비지 컬렉션)이 아무도 참조하지 않은 인스턴스가 있으면 더 이상 사용하지 않는 인스턴스라 판단하고 해당 인스턴스를 자동으로 메모리에서 제거해준다.

객체는 해당 객체를 참조하는 곳이 있으면, JVM이 종료할 때까지 계속 생존한다. 그런데 중간에 해당 객체를 참조하는 곳이 모두 사라지면 JVM은 필요없는 객체로 판단하고 GC(가비지 컬렉션)를 사용해서 제거한다.

### **NullPointerException**

참조값없이 객체를 찾아가면 발생하는 오류

`NullPointException` 은 이름 그대로 `null` 을 가리키다(Pointer)인데, 이때 발생하는 예외(Exception)다.

`null` 은 없다는 뜻이므로 결국 주소가 없는 곳을 찾아갈 때 발생.

객체를 참조할 때는 `.` (dot)을 사용. 이렇게 하면 참조값을 사용해서 해당 객체를 찾아갈 수 있음. 그러나 참조값이 `null`이라면 값이 없다는 뜻이므로, 찾아갈 수 있는 객체(인스턴스)가 없다. `NullPointerException` 은 이처럼 `null` 에 `.` 을 찍었을 때 발생.

**NullMain2**

```java
package ref;

public class NullMain2 {
    public static void main(String[] args) {
        Data data = null;
        data.value = 10;  //NullPointerException 예외 발생
        System.out.println("data = " + data.value);
    }
}
```

```java
data.value = 10;
null.value = 10;  //data에는 null값이 들어있다.
```

결과적으로 `null` 값은 참조할  주소가 존재하지 않는다.

따라서, 참조할 객체 인스턴스가 존재하지 않으므로 다음과 같이 `java.lang.NullPointerException`이 발생하고, 프로그램 종료.

예외가 발생했기 때문에 그 다음 로직은 수행되지 않는다.

**실행결과**

```java
Exception in thread "main" java.lang.NullPointerException: Cannot assign field "value" because "data" is null
	at ref.NullMain2.main(NullMain2.java:6)
```

### 멤버 변수와 null

지역변수의 경우에는 `null` 문제를 파악하는게 어렵지 않다. 그러나 멤버 변수가 `null`인 경우에는 주의

```java
package ref;

public class NullMain3 {
    public static void main(String[] args) {
        BigData bigData = new BigData();
        System.out.println("bigData.count = " + bigData);
        System.out.println("bigData.data = " + bigData.data);

        //NullPointException
        System.out.println("bigData.data.value = " + bigData.data.value);  //null
    }
}
```

**실행결과**

```java
Exception in thread "main" java.lang.NullPointerException: Cannot read field "value" because "bigData.data" is null
	at ref.NullMain3.main(NullMain3.java:9)
```

- 기본형 변수는 초기값이 `int`는 `0` , `boolean`의 경우 `False`, `String`은 `null`
- 참조형 변수는 초기값이 `null`
- 따라서 `bigData.data.value` 예외 발생과정

```java
bigData.data.value;  //bigData는 x001의 참조값을 갖는다.
x001.data.value;  //bigData.data = null
null.value;  //NullPointerException 발생
```

위 문제를 해결하기 위해 `bigData.data = new Data()`를 통해 참조값을 생성해준다.

**NullMain4**

```java
package ref;

public class NullMain4 {
    public static void main(String[] args) {
        BigData bigData = new BigData();
        bigData.data = new Data();
        System.out.println("bigData.count = " + bigData.count);
        System.out.println("bigData.data = " + bigData.data);

        //NullPointException
        System.out.println("bigData.data.value = " + bigData.data.value);  //null
    }
}
```

**실행결과**

```java
bigData.count = 0
bigData.data = ref.Data@34a245ab
bigData.data.value = 0
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6b8d40ba-5287-42be-84df-56b1c96a2c05/c5b13490-da39-4d44-adaf-9aedfc937081/3727465e-10c1-406c-a067-406c74deb122.png)

**실행과정**

```java
bigData.data.value;
x001.data.value;  //bigData는 x001을 참조
x002.value;  //x001.data는 x002를 참조
0  // 최종결과
```

## 문제와 풀이

### 문제: 상품 주문 시스템 개발 - 리팩토링

**문제설명**

`class1.ex.ProductOrderMain` 리팩토링

**요구사항**

`ProductOrder` 클래스는 다음과 같은 멤버 변수를 포함해야함.

- 상품명 (productName)
- 가격 (price)
- 주문수량 (quantity)

**예시 코드 구조**

```java
package ref.ex;

public class ProductOrder {
    String productName;
    int price;
    int quantity;
}
```

ref.ex.ProductOrderMain2 클래스 안에 main() 메서드를 포함하여, 여러 상품의 주문정보를 배열로 관리

다음과 같은 메서드 포함

- `static ProductOrder createOrder(String productName, int price, int quantity)`
    - `ProductOrder`를 생성하고 매개변수로 초기값을 설정. 마지막으로 `ProductOrder` 반환
- `static void printOrders(ProductOrder[] orders)`
    - 배열을 받아서 배열에 들어있는 전체 `ProductOrder`의 상품명, 가격, 수량 출력
- `static int getTotalAmount(ProductOrder[] orders)`
    - 배열을 받아서 배열에 들어있는 전체 `ProductOrder`의 총결제 금액 계산, 계산 결과 반환

**예시코드 구조**

```java
package ref.ex;

public class ProductOderMain2{
    public static void main(String[] agrs) {
    // 여러 상품 주문 정보를 담는 배열 생성
    // createOder()를 여러번 사용해서 상품 주문 정보들을 생성하고 배열에 저장
    // printOrders()를 사용해서 상품 주문 정보 출력
    // getTotalAmount()를 사용해서 총 결제 금액 계산
    // 총 결제 금액 출력
    }
}
```

**ProductOrderMain2_1 (내가 작성한 코드)**

```java
package ref.ex;

public class ProductOrderMain2_1 {
    public static void main(String[] args) {
        ProductOrder[] orders = new ProductOrder[3];
        ProductOrder order1 = createOrder("두부", 2000, 2);
        ProductOrder order2 = createOrder("김치", 5000, 1);
        ProductOrder order3 = createOrder("콜라", 1500, 2);
        orders[0] = order1;
        orders[1] = order2;
        orders[2] = order3;
        printOrders(orders);
        int totalAmount = getTotalAmount(orders);
        System.out.println("총 결제 금액: " + totalAmount);
    }

    public static ProductOrder createOrder(String productName, int price, int quantity) {
        ProductOrder order = new ProductOrder();
        order.productName = productName;
        order.price = price;
        order.quantity = quantity;
        return order;
    }

    public static void printOrders(ProductOrder[] orders) {
        for (ProductOrder order : orders) {
            System.out.println("상품명: " + order.productName + " 가격: " + order.price + " 수량: " + order.quantity);
        }
    }

    public static int getTotalAmount(ProductOrder[] orders) {
        int totalAmount = 0;
        for (ProductOrder order : orders) {
            totalAmount += order.quantity * order.price;
        }
        return totalAmount;
    }
}
```

**ProductOrderMain2_2 (제시된 코드)**

```java
package ref.ex;

public class ProductOrderMain2_2 {
    public static void main(String[] args) {
        ProductOrder[] orders = new ProductOrder[3];
        orders[0] = createOrder("두부", 2000, 2);
        orders[1] = createOrder("김치", 5000, 1);
        orders[2] = createOrder("콜라", 1500, 2);
        
        printOrders(orders);
        
        int totalAmount = getTotalAmount(orders);
        System.out.println("총 결제 금액: " + totalAmount);
    }

    public static ProductOrder createOrder(String productName, int price, int quantity) {
        ProductOrder order = new ProductOrder();
        order.productName = productName;
        order.price = price;
        order.quantity = quantity;
        return order;
    }

    public static void printOrders(ProductOrder[] orders) {
        for (ProductOrder order : orders) {
            System.out.println("상품명: " + order.productName + " 가격: " + order.price + " 수량: " + order.quantity);
        }
    }

    public static int getTotalAmount(ProductOrder[] orders) {
        int totalAmount = 0;
        for (ProductOrder order : orders) {
            totalAmount += order.quantity * order.price;
        }
        return totalAmount;
    }
}
```

**실행결과**

```java
상품명: 두부 가격: 2000 수량: 2
상품명: 김치 가격: 5000 수량: 1
상품명: 콜라 가격: 1500 수량: 2
총 결제 금액: 12000
```

### 문제: 상품 주문 시스템 개발 - 사용자 입력

**문제설명**

앞서 만든 상품 주문 시스템을 사용자 입력을 받도록 개선

**요구 사항**

- 아래 입력, 출력 예시를 참고해서 다음 사항을 적용
- 주문 수량 입력
    - 예) 입력할 주문의 개수 입략
- 가격, 수량, 상품명 입력
    - 입력시 상품 순서를 알 수 있게 “n번째 주문 정보를 입력하세요.” 라는 메세지 출력
- 입력이 끝나면 등록한 상품과 총 결제금액 출력

입력, 출력 예시

```java
입력할 주문의 개수를 입력하세요:3
1번째 주문 정보를 입력하세요.
상품명: 두부
가격: 2000
수량: 2
2번째 주문 정보를 입력하세요.
상품명: 김치
가격: 5000
수량: 1
3번째 주문 정보를 입력하세요.
상품명: 콜라
가격: 1500
수량: 2
상품명: 두부, 가격: 2000, 수량: 2
상품명: 김치, 가격: 5000, 수량: 1
상품명: 콜라, 가격: 1500, 수량: 2
12000
```

**ProductOrderMain3_1(내가 작성한 코드)**

```java
package ref.ex;

import java.util.Scanner;

public class ProductOrderMain3_1 {

    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.print("입력할 주문의 개수를 입력하세요:");
        int n = input.nextInt();
        ProductOrder[] orders = new ProductOrder[n];
        input.nextLine();

        for (int i = 0; i < orders.length; i++) {
            System.out.println((i + 1) + "번째 주문 정보를 입력하세요.");

            System.out.print("상품명: ");
            String productName = input.nextLine();
            System.out.print("가격: ");
            int price = input.nextInt();
            input.nextLine();
            System.out.print("수량: ");
            int quantity = input.nextInt();
            input.nextLine();

            orders[i] = createProduct(productName, price, quantity);
        }
        printOrders(orders);
    }

    public static ProductOrder createProduct(String productName, int price, int quantity) {
        ProductOrder order = new ProductOrder();
        order.productName = productName;
        order.price = price;
        order.quantity = quantity;

        return order;
    }

    public static int totalPrice(ProductOrder[] orders) {
        int totalPrice = 0;
        for (ProductOrder order : orders) {
            totalPrice += order.price * order.quantity;
        }
        return totalPrice;
    }

    public static void printOrders(ProductOrder[] orders) {
        for (ProductOrder order : orders) {
            System.out.println("상품명: " + order.productName + ", 가격: " + order.price + ", 수량: " + order.quantity);
        }
        System.out.println(totalPrice(orders));
        return;
    }
}
```

**ProductOrderMain3_2(제시된 코드)**

```java
package ref.ex;

import java.util.Scanner;

public class ProductOrderMain3_2 {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("입력할 주문의 개수를 입력하세요:");
        int n = scanner.nextInt();
        scanner.nextLine();  //입력 버퍼를 비우기 위한 코드

        ProductOrder[] orders = new ProductOrder[n];
        for (int i = 0; i < orders.length; i++) {
            System.out.println((i + 1) + "번째 주문 정보를 입력하세요.");

            System.out.print("상품명: ");
            String productName = scanner.nextLine();

            System.out.print("가격: ");
            int price = scanner.nextInt();

            System.out.print("수량: ");
            int quantity = scanner.nextInt();
            scanner.nextLine();  //입력 버퍼를 비우기 위한 코드

            orders[i] = createOrder(productName, price, quantity);
        }
        printOrders(orders);

        int totalAmount = getTotalAmount(orders);
        System.out.println("총 결제 금액: " + totalAmount);
    }

    public static ProductOrder createOrder(String productName, int price, int quantity) {
        ProductOrder order = new ProductOrder();
        order.productName = productName;
        order.price = price;
        order.quantity = quantity;
        return order;
    }

    public static void printOrders(ProductOrder[] orders) {
        for (ProductOrder order : orders) {
            System.out.println("상품명: " + order.productName + " 가격: " + order.price + " 수량: " + order.quantity);
        }
    }

    public static int getTotalAmount(ProductOrder[] orders) {
        int totalAmount = 0;
        for (ProductOrder order : orders) {
            totalAmount += order.quantity * order.price;
        }
        return totalAmount;
    }
}
```

**실행결과**

```java
입력할 주문의 개수를 입력하세요:3
        1번째 주문 정보를 입력하세요.
상품명: 두부
가격: 2000
수량: 2
        2번째 주문 정보를 입력하세요.
상품명: 김치
가격: 5000
수량: 1
        3번째 주문 정보를 입력하세요.
상품명: 콜라
가격: 1500
수량: 2
상품명: 두부, 가격: 2000, 수량: 2
상품명: 김치, 가격: 5000, 수량: 1
상품명: 콜라, 가격: 1500, 수량: 2
        12000
```