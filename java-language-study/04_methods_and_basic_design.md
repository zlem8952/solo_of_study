# 4일차 - 메서드, 오버로딩, 재귀, 설계 기초

## 오늘의 목표

- 메서드를 선언하고 호출할 수 있다.
- 매개변수와 반환값의 의미를 이해한다.
- 오버로딩, 가변 인자, 재귀를 익힌다.
- Java가 값을 어떻게 전달하는지 정확히 이해한다.

## 메서드란?

특정 작업을 수행하는 코드 묶음입니다. Java에서는 모든 메서드가 클래스 안에 존재합니다.

## 메서드 기본 구조

```java
반환타입 메서드이름(매개변수목록) {
    실행 코드
}
```

예:

```java
static int add(int a, int b) {
    return a + b;
}
```

## 메서드 호출

```java
int result = add(3, 4);
```

## `void` 메서드

값을 반환하지 않는 경우 `void`를 사용합니다.

```java
static void printLine() {
    System.out.println("-----");
}
```

## 매개변수와 인자

- 매개변수(parameter): 메서드 정의 쪽 변수
- 인자(argument): 실제 호출 시 전달하는 값

```java
static int add(int a, int b) {
    return a + b;
}

public static void main(String[] args) {
    int sum = add(10, 20);
}
```

## 메서드 분리 예제

```java
public class Main {
    static int square(int x) {
        return x * x;
    }

    public static void main(String[] args) {
        System.out.println(square(5));
    }
}
```

## 메서드 오버로딩

같은 이름의 메서드를 매개변수 형태만 다르게 여러 개 정의할 수 있습니다.

```java
static int add(int a, int b) {
    return a + b;
}

static double add(double a, double b) {
    return a + b;
}
```

오버로딩 판단 기준:

- 매개변수 개수
- 매개변수 타입
- 매개변수 순서

반환 타입만 다른 것은 오버로딩이 아닙니다.

## 재귀 메서드

메서드가 자기 자신을 호출하는 방식입니다.

```java
static int factorial(int n) {
    if (n <= 1) {
        return 1;
    }
    return n * factorial(n - 1);
}
```

중요:

- 종료 조건이 반드시 있어야 함
- 너무 깊으면 `StackOverflowError` 가능

## 가변 인자 `varargs`

인자 개수가 달라질 수 있는 메서드에 사용합니다.

```java
static int sum(int... values) {
    int total = 0;
    for (int value : values) {
        total += value;
    }
    return total;
}
```

호출:

```java
sum(1, 2, 3);
sum(10, 20, 30, 40);
```

주의:

- 메서드당 가변 인자는 하나만
- 마지막 매개변수여야 함

## Java는 값 전달(Call by Value)

이 부분은 매우 중요합니다.

### 기본형 전달

```java
static void change(int x) {
    x = 100;
}
```

원본은 바뀌지 않습니다.

### 참조형 전달

```java
static void changeName(Person p) {
    p.name = "Lee";
}
```

객체 참조의 복사본이 전달됩니다. 즉, 참조 자체는 복사되지만 그 참조가 가리키는 객체 내부는 수정될 수 있습니다.

## 예제: 배열 합계 메서드

```java
public class Main {
    static int sumArray(int[] arr) {
        int sum = 0;
        for (int value : arr) {
            sum += value;
        }
        return sum;
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 3, 4, 5};
        System.out.println(sumArray(nums));
    }
}
```

## 메서드에서 배열 반환

```java
static int[] createArray() {
    return new int[] {1, 2, 3};
}
```

## 메서드 설계 기본 원칙

좋은 메서드는 보통 다음 특성을 가집니다.

- 하나의 역할에 집중
- 이름만 봐도 역할이 보임
- 너무 길지 않음
- 입력과 출력이 분명함

나쁜 예:

- 입력도 받고 계산도 하고 출력도 하고 파일 저장도 함

좋은 예:

- `readInput`
- `calculateAverage`
- `printResult`

처럼 역할 분리

## 메서드와 `return`

```java
static int max(int a, int b) {
    if (a > b) {
        return a;
    }
    return b;
}
```

`return`은 값을 돌려주고 메서드를 즉시 종료합니다.

## `main`도 메서드다

```java
public static void main(String[] args) {
}
```

따라서 다른 메서드를 호출하며 프로그램 흐름을 구성할 수 있습니다.

## 예제: 소수 판별 메서드

```java
public class Main {
    static boolean isPrime(int n) {
        if (n < 2) {
            return false;
        }
        for (int i = 2; i * i <= n; i++) {
            if (n % i == 0) {
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        System.out.println(isPrime(17));
    }
}
```

## 예제: 재귀로 문자열 뒤집기

```java
public class Main {
    static String reverse(String s) {
        if (s.length() <= 1) {
            return s;
        }
        return reverse(s.substring(1)) + s.charAt(0);
    }

    public static void main(String[] args) {
        System.out.println(reverse("java"));
    }
}
```

## 실수 포인트

- 반환 타입과 실제 반환값이 맞지 않음
- 오버로딩과 단순 이름 중복을 혼동함
- 재귀 종료 조건을 빼먹음
- 메서드 하나에 너무 많은 일을 넣음
- Java가 "참조 전달"이라고 잘못 외움

## 체크 문제

1. 메서드 오버로딩은 무엇인가?
2. Java가 값 전달이라는 말은 정확히 무슨 뜻인가?
3. 가변 인자는 언제 유용한가?
4. 좋은 메서드 설계의 핵심은 무엇인가?

## 직접 해볼 과제

### 과제 1

세 정수 중 가장 큰 값을 반환하는 메서드를 작성하세요.

### 과제 2

정수 배열의 평균을 계산하는 메서드를 작성하세요.

### 과제 3

재귀로 1부터 n까지 합을 구하는 메서드를 작성하세요.

## 오늘 정리

- 메서드는 코드를 구조화하고 재사용하는 핵심 도구입니다.
- 오버로딩, 가변 인자, 재귀는 Java 메서드의 중요한 기능입니다.
- Java의 인자 전달 방식은 기본형과 참조형 모두 "값 전달"입니다.

