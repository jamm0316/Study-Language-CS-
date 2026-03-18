##9. 다음 Java로 구현된 프로그램을 분석하여 그 실행 결과를 쓰시오. (단, 출력문의 출력 서식을 준수하시오.)

```java
public class Main {
    static interface F {
        int apply(int x) throw Exceptoin;
    }

public static int run (F f) {
    try {
        return f.apply(3);
    } catch (Exceptoin e) {
        return 7;
    }
}

public static void main (string[] args) {
    F f = (x) => {
        if (x > 2) {
            throw new Exception();
        }
        return x * 2;
    };
    System.out.print(run(f) + run((int n) -> n + 9);
}
```

<br><br><br><br><br><br>

sol)
1. 람다식을 이용해 `F f`의 `apply` 메서드를 재정의한다.
2. static method인 `run`에 1에서 정의한 f를 파라미터로 전달해 실행한다.
3. `try { return f.apply(3); }` 에 따라 f.apply(3)을 실행하며, 이때 1에저 정의한 람다식으로 3을 전달한다.
4. if (3 > 2) 이므로 `new Exceptoin`을 반환한다.
5. 다시 `public static int run (F f)` 메서드로 돌아와 `catch`에서 `return 7`을 반환하여 `run(f) = 7` 이된다.
6. `run((int n) -> n + 9`에 의해  F f의 apply()가 재정의 되고, `public static int run (F f)`의 `try { return f.apply(3); }`에 따라 3 + 9 = 12를 return 한다.
7. 따라서 7 + 12 = 19 이다.

**정답: 19**
