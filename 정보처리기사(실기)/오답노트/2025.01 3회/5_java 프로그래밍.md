## 5. 다음 Java로 구현된 프로그램을 분석하여 그 결과를 쓰시오. (단, 출력문의 출력 서식을 준수하시오.)

```java
public class Main {
    public staic void main(string[] args) {
        int a = 5, b = 0;
        try {
            System.out.print(a/b);
        }
        catch (ArithmeticExceptoin e) {
            System.out.print("출력1");
        }
        catch (ArrayIndexOutOfBoundsException e) {
            System.out.print("출력2");
        }
        catch (NumberFormatExceptoin e) {
            System.out.print("출력3");
        }
        catch (Exceptoin e) {
            System.out.print("출력4");
        } finally {
            System.out.print("출력5");
        }
    }
}
```

<br><br><br><br><br>
sol) try-catch-finally
### 1. Exceptoin의 종류
- ArithmeticException : 산술 예외 -> 산술 연산이 유효하지 않을 떄 발생하는 예외이다.
  - 나누기 연산에서 분모가 0인 경우: 산술 연산에서는 0으로 나누는 것은 허용되지 않는다.
  - 정수 타입의 변수에서 최소값을 나타내는 상태에서 -1을 취하는 경우: `Integer.MIN_VALUE`의 절대값을 취하려할 떄 발생
- ArrayIndexOutOfBoundsException : 배열 범위 밖 인덱스 예외 -> 배열의 크기보다 크거나 음수 index에 대한 요청이 있을 떄 발생
- NumberFormatExceptoin: 숫자형 포멧 예외 -> 문자를 숫자로 변경시도하다가 에러 발생
  - 문자열을 숫자로 변겨할 경우 ex)123o
  - 변경하는 자료형 보다 범위가 큰 경우 ex)Integer.parseInt("9999999999999999999999999999999");
  - null 입력 시
  - 문자 앞뒤로 공백 존재할 시

### 2. try-catch-finally
- try-catch 구문 실행 후 finally는 반드시 실행된다.

**정답:
출력1출력5**
