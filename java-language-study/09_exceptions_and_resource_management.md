# 9일차 - 예외 처리, 사용자 정의 예외, 자원 관리

## 오늘의 목표

- Java 예외 처리 구조를 이해한다.
- `try`, `catch`, `finally`, `throw`, `throws`를 사용할 수 있다.
- 체크 예외와 언체크 예외의 차이를 안다.
- `try-with-resources`로 자원을 안전하게 닫는 방법을 익힌다.

## 예외(Exception)란?

프로그램 실행 중 발생하는 비정상 상황을 표현하는 객체입니다.

예:

- 0으로 나누기
- 배열 범위 초과
- 파일 열기 실패
- 잘못된 입력

## 예외 계층 구조

대략적인 구조:

- `Throwable`
- `Error`
- `Exception`
- `RuntimeException`

### `Error`

보통 애플리케이션 코드에서 직접 처리하기 어려운 심각한 문제

### `Exception`

애플리케이션에서 처리 가능한 예외

## 체크 예외와 언체크 예외

### 체크 예외(Checked Exception)

컴파일러가 처리 또는 선언을 강제합니다.

예:

- `IOException`
- `SQLException`

### 언체크 예외(Unchecked Exception)

`RuntimeException` 계열

예:

- `NullPointerException`
- `IllegalArgumentException`
- `ArithmeticException`
- `ArrayIndexOutOfBoundsException`

## `try-catch`

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("0으로 나눌 수 없습니다.");
}
```

## `finally`

예외 발생 여부와 상관없이 마지막에 실행됩니다.

```java
try {
    System.out.println("작업");
} catch (Exception e) {
    System.out.println("예외");
} finally {
    System.out.println("항상 실행");
}
```

## `throw`

직접 예외를 발생시킵니다.

```java
if (age < 0) {
    throw new IllegalArgumentException("나이는 음수일 수 없습니다.");
}
```

## `throws`

메서드가 어떤 예외를 던질 수 있는지 선언합니다.

```java
import java.io.IOException;

static void readFile() throws IOException {
    ...
}
```

## 예외 메시지와 스택 트레이스

```java
catch (Exception e) {
    System.out.println(e.getMessage());
    e.printStackTrace();
}
```

`printStackTrace()`는 디버깅 때 매우 유용합니다.

## 여러 `catch`

```java
try {
    ...
} catch (NumberFormatException e) {
    ...
} catch (IllegalArgumentException e) {
    ...
}
```

## 멀티 캐치

```java
catch (IOException | SQLException e) {
    ...
}
```

## 사용자 정의 예외

```java
class InvalidScoreException extends Exception {
    public InvalidScoreException(String message) {
        super(message);
    }
}
```

사용:

```java
static void validateScore(int score) throws InvalidScoreException {
    if (score < 0 || score > 100) {
        throw new InvalidScoreException("점수는 0~100이어야 합니다.");
    }
}
```

## 언제 체크 예외를 쓸까?

보통 호출자가 복구 가능하고 반드시 대응해야 하는 상황에 어울립니다.

언체크 예외는:

- 잘못된 인자
- 프로그래밍 실수
- 전제 조건 위반

같은 경우에 많이 사용합니다.

## `try-with-resources`

자동으로 자원을 닫아 줍니다.

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
            System.out.println(br.readLine());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

`AutoCloseable` 또는 `Closeable`을 구현한 객체에 사용할 수 있습니다.

## 왜 `try-with-resources`가 좋은가?

- 코드가 짧음
- `finally`에서 직접 닫는 실수를 줄임
- 예외가 여러 번 겹칠 때도 정리가 더 안전함

## 예외를 삼키지 말기

나쁜 예:

```java
try {
    ...
} catch (Exception e) {
}
```

이렇게 하면 오류 원인을 추적하기 어렵습니다.

최소한:

- 로그 남기기
- 적절히 다시 던지기
- 사용자에게 의미 있는 메시지 제공

## `throws Exception` 남용 주의

학습 초반에는 편해 보여도, 나중에는 어떤 예외가 실제로 발생 가능한지 흐려집니다.

가능하면 더 구체적인 예외를 명시하는 것이 좋습니다.

## 예제: 문자열을 정수로 변환

```java
public class Main {
    static int parsePositiveInt(String value) {
        int number = Integer.parseInt(value);
        if (number <= 0) {
            throw new IllegalArgumentException("양수만 허용");
        }
        return number;
    }

    public static void main(String[] args) {
        try {
            System.out.println(parsePositiveInt("10"));
            System.out.println(parsePositiveInt("-3"));
        } catch (NumberFormatException e) {
            System.out.println("숫자 형식 오류");
        } catch (IllegalArgumentException e) {
            System.out.println("잘못된 값: " + e.getMessage());
        }
    }
}
```

## 예제: 사용자 정의 예외

```java
class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}

public class Main {
    static void checkAge(int age) throws InvalidAgeException {
        if (age < 0) {
            throw new InvalidAgeException("나이는 음수일 수 없습니다.");
        }
    }

    public static void main(String[] args) {
        try {
            checkAge(-1);
        } catch (InvalidAgeException e) {
            System.out.println(e.getMessage());
        }
    }
}
```

## 실수 포인트

- `try-catch`만 쓰고 예외 원인을 기록하지 않음
- 모든 예외를 무조건 잡아 버림
- `throws Exception`을 습관처럼 붙임
- 자원을 닫지 않음
- 체크 예외와 언체크 예외를 구분하지 못함

## 체크 문제

1. 체크 예외와 언체크 예외 차이는 무엇인가?
2. `throw`와 `throws`는 어떻게 다른가?
3. `finally`는 언제 실행되는가?
4. `try-with-resources`가 필요한 이유는 무엇인가?

## 직접 해볼 과제

### 과제 1

점수가 0~100 범위를 벗어나면 예외를 던지는 메서드를 작성하세요.

### 과제 2

문자열 배열을 정수로 변환하면서 잘못된 값은 예외 처리해 건너뛰는 프로그램을 작성하세요.

### 과제 3

파일 한 줄을 읽어 출력하는 코드를 `try-with-resources`로 작성하세요.

## 오늘 정리

- 예외는 "이상 상황"을 코드로 표현하는 방식입니다.
- 체크 예외와 언체크 예외는 처리 책임이 다릅니다.
- 자원 사용 코드는 `try-with-resources`를 우선 고려하는 습관이 좋습니다.

