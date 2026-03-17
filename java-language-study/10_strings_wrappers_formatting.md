# 10일차 - 문자열, 래퍼 클래스, 포맷팅, 유틸리티 클래스

## 오늘의 목표

- `String`의 불변성과 주요 메서드를 이해한다.
- `StringBuilder`, `StringBuffer` 차이를 안다.
- 래퍼 클래스와 박싱/언박싱을 이해한다.
- 문자열 포맷팅, 숫자 변환, 자주 쓰는 유틸리티를 익힌다.

## `String`

Java에서 문자열은 `String` 객체입니다.

```java
String s = "Hello";
```

## `String`은 불변(immutable)

```java
String s = "Hello";
s.concat(" Java");
System.out.println(s); // Hello
```

원본은 바뀌지 않습니다.

새 값을 다시 대입해야 합니다.

```java
s = s.concat(" Java");
```

## 문자열 풀(String Pool)

리터럴 문자열은 재사용될 수 있습니다.

```java
String a = "hi";
String b = "hi";
System.out.println(a == b); // true 가능
```

하지만 내용 비교는 항상 `equals`를 우선 생각해야 합니다.

```java
String x = new String("hi");
System.out.println(a.equals(x)); // true
System.out.println(a == x);      // false 가능
```

## `String` 주요 메서드

| 메서드 | 설명 |
| --- | --- |
| `length()` | 길이 |
| `charAt()` | 특정 위치 문자 |
| `substring()` | 부분 문자열 |
| `contains()` | 포함 여부 |
| `equals()` | 내용 비교 |
| `equalsIgnoreCase()` | 대소문자 무시 비교 |
| `indexOf()` | 위치 검색 |
| `startsWith()` | 시작 문자열 |
| `endsWith()` | 끝 문자열 |
| `replace()` | 치환 |
| `split()` | 분리 |
| `trim()` / `strip()` | 앞뒤 공백 제거 |
| `toUpperCase()` / `toLowerCase()` | 대소문자 변환 |
| `isBlank()` | 공백 문자열 여부 |

## 문자열 예제

```java
String text = "  Java Study  ";

System.out.println(text.length());
System.out.println(text.trim());
System.out.println(text.contains("Study"));
System.out.println(text.substring(2, 6));
```

## 문자열 결합 성능

반복문에서 `+`를 많이 쓰면 비효율적일 수 있습니다.

```java
String result = "";
for (int i = 0; i < 1000; i++) {
    result += i;
}
```

이럴 때는 `StringBuilder`가 더 적합합니다.

## `StringBuilder`

가변 문자열을 효율적으로 다룹니다.

```java
StringBuilder sb = new StringBuilder();
sb.append("Java");
sb.append(" ");
sb.append("Study");
System.out.println(sb.toString());
```

주요 메서드:

- `append`
- `insert`
- `delete`
- `reverse`
- `setCharAt`
- `toString`

## `StringBuffer`

`StringBuilder`와 비슷하지만 동기화가 적용됩니다.

일반적인 단일 스레드 상황에서는 보통 `StringBuilder`가 더 많이 쓰입니다.

## 래퍼 클래스(Wrapper Class)

기본형을 객체처럼 다루게 해 주는 클래스입니다.

| 기본형 | 래퍼 클래스 |
| --- | --- |
| `int` | `Integer` |
| `long` | `Long` |
| `double` | `Double` |
| `boolean` | `Boolean` |
| `char` | `Character` |

## 오토박싱 / 오토언박싱

```java
Integer a = 10; // 오토박싱
int b = a;      // 오토언박싱
```

## 숫자 변환

```java
int n = Integer.parseInt("123");
double d = Double.parseDouble("3.14");
String s = String.valueOf(100);
```

## 래퍼 클래스 유용 메서드

```java
Integer.max(3, 7);
Integer.compare(10, 20);
Double.isNaN(value);
Character.isDigit('7');
```

## `Math` 클래스

자주 쓰는 수학 기능을 제공합니다.

```java
Math.max(10, 20);
Math.min(10, 20);
Math.abs(-5);
Math.sqrt(16);
Math.pow(2, 3);
Math.random();
Math.round(3.6);
```

## 형식 지정 출력

```java
System.out.printf("name=%s age=%d score=%.2f%n", "Kim", 20, 95.5);
```

### 자주 쓰는 포맷

| 포맷 | 의미 |
| --- | --- |
| `%s` | 문자열 |
| `%d` | 정수 |
| `%f` | 실수 |
| `%c` | 문자 |
| `%b` | 불리언 |
| `%n` | 줄바꿈 |

## `String.format`

문자열로 만들어서 반환합니다.

```java
String msg = String.format("합계=%d 평균=%.2f", sum, avg);
```

## 텍스트 블록(Text Block)

여러 줄 문자열을 더 편하게 작성할 수 있습니다.

```java
String json = """
        {
          "name": "Kim",
          "age": 20
        }
        """;
```

## `Objects` 유틸리티

`java.util.Objects`는 null 안전 비교 등에 유용합니다.

```java
Objects.equals(a, b);
Objects.requireNonNull(name);
```

## `Arrays` 유틸리티

```java
import java.util.Arrays;

int[] arr = {3, 1, 2};
Arrays.sort(arr);
System.out.println(Arrays.toString(arr));
```

자주 쓰는 메서드:

- `sort`
- `toString`
- `equals`
- `binarySearch`
- `copyOf`
- `fill`

## 예제: 문자열 뒤집기

```java
public class Main {
    public static void main(String[] args) {
        String text = "java";
        String reversed = new StringBuilder(text).reverse().toString();
        System.out.println(reversed);
    }
}
```

## 예제: 문자열 분석

```java
public class Main {
    public static void main(String[] args) {
        String email = "student@example.com";

        System.out.println(email.contains("@"));
        System.out.println(email.substring(0, email.indexOf("@")));
        System.out.println(email.toUpperCase());
    }
}
```

## 실수 포인트

- 문자열 비교를 `==`로 함
- 반복문에서 `+`를 남용해 성능 저하
- `Integer.parseInt`가 실패할 수 있다는 점을 무시함
- 오토언박싱 과정의 `NullPointerException`을 놓침
- `String`이 변경된다고 착각함

## 체크 문제

1. `String`이 불변이라는 말은 무슨 뜻인가?
2. `StringBuilder`는 언제 써야 하는가?
3. 래퍼 클래스는 왜 필요한가?
4. 문자열 비교에 `equals`를 써야 하는 이유는 무엇인가?

## 직접 해볼 과제

### 과제 1

문자열에서 모음 개수를 세는 프로그램을 작성하세요.

### 과제 2

사용자 이름과 점수를 받아 `String.format`으로 메시지를 만드세요.

### 과제 3

문자열 배열을 정렬해 출력하는 프로그램을 `Arrays.sort`로 작성하세요.

## 오늘 정리

- `String`은 매우 자주 쓰이지만 불변이라는 점이 핵심입니다.
- 반복 조작에는 `StringBuilder`가 유리합니다.
- 래퍼 클래스와 유틸리티 클래스를 익히면 Java 코드 생산성이 크게 올라갑니다.

