# 15일차 - 날짜/시간 API, 로케일, 정규식

## 오늘의 목표

- `java.time` 패키지의 핵심 타입을 이해한다.
- 날짜/시간 계산과 포맷팅을 할 수 있다.
- 로케일과 숫자/날짜 형식 차이를 안다.
- 정규식으로 문자열 검증과 검색을 할 수 있다.

## 왜 `java.time`이 중요한가?

과거의 `Date`, `Calendar`는 사용성이 좋지 않았습니다. 현대 Java에서는 `java.time` 패키지가 표준입니다.

대표 클래스:

- `LocalDate`
- `LocalTime`
- `LocalDateTime`
- `ZonedDateTime`
- `Instant`
- `Duration`
- `Period`
- `DateTimeFormatter`

## `LocalDate`

날짜만 표현합니다.

```java
import java.time.LocalDate;

LocalDate today = LocalDate.now();
LocalDate birthday = LocalDate.of(2000, 5, 10);
```

## `LocalTime`

시간만 표현합니다.

```java
import java.time.LocalTime;

LocalTime now = LocalTime.now();
```

## `LocalDateTime`

날짜 + 시간

```java
import java.time.LocalDateTime;

LocalDateTime now = LocalDateTime.now();
```

## `ZonedDateTime`

시간대 정보 포함

```java
import java.time.ZoneId;
import java.time.ZonedDateTime;

ZonedDateTime seoul = ZonedDateTime.now(ZoneId.of("Asia/Seoul"));
```

## 날짜 연산

```java
LocalDate today = LocalDate.now();
System.out.println(today.plusDays(7));
System.out.println(today.minusMonths(1));
```

이 API는 불변입니다. 원본이 바뀌지 않습니다.

## `Period`와 `Duration`

### `Period`

날짜 단위 차이

```java
Period period = Period.between(startDate, endDate);
```

### `Duration`

시간 단위 차이

```java
Duration duration = Duration.between(startTime, endTime);
```

## 포맷팅

```java
import java.time.format.DateTimeFormatter;

DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String text = LocalDateTime.now().format(formatter);
System.out.println(text);
```

## 파싱

```java
String input = "2026-03-17";
LocalDate date = LocalDate.parse(input);
```

직접 패턴 지정:

```java
LocalDate date = LocalDate.parse("2026/03/17",
    DateTimeFormatter.ofPattern("yyyy/MM/dd"));
```

## `Locale`

지역/언어 설정을 표현합니다.

```java
import java.util.Locale;

Locale korean = Locale.KOREAN;
Locale us = Locale.US;
```

## 숫자/통화 포맷

```java
import java.text.NumberFormat;
import java.util.Locale;

NumberFormat currency = NumberFormat.getCurrencyInstance(Locale.US);
System.out.println(currency.format(12345.67));
```

## 날짜 포맷과 로케일

```java
DateTimeFormatter formatter =
    DateTimeFormatter.ofPattern("yyyy MMM dd", Locale.ENGLISH);
```

## 정규식(Regex)

문자열 패턴을 검색/검증하는 도구입니다.

### 간단한 사용

```java
String phone = "010-1234-5678";
boolean matched = phone.matches("\\d{3}-\\d{4}-\\d{4}");
```

## `Pattern`과 `Matcher`

복잡한 정규식 작업에 사용합니다.

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

Pattern pattern = Pattern.compile("\\d+");
Matcher matcher = pattern.matcher("abc123def45");

while (matcher.find()) {
    System.out.println(matcher.group());
}
```

## 자주 쓰는 정규식 기호

| 패턴 | 의미 |
| --- | --- |
| `.` | 아무 문자 하나 |
| `\\d` | 숫자 |
| `\\w` | 문자/숫자/밑줄 |
| `\\s` | 공백 |
| `+` | 1회 이상 |
| `*` | 0회 이상 |
| `?` | 0 또는 1회 |
| `{n}` | n회 |
| `{n,m}` | n~m회 |
| `^` | 시작 |
| `$` | 끝 |

## 이메일 검증 예시

```java
String email = "student@example.com";
boolean ok = email.matches("^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$");
```

정규식은 강력하지만, 복잡한 검증을 전부 정규식 하나에 넣으려 하면 가독성이 급격히 떨어질 수 있습니다.

## 예제: 남은 일수 계산

```java
import java.time.LocalDate;
import java.time.temporal.ChronoUnit;

public class Main {
    public static void main(String[] args) {
        LocalDate today = LocalDate.now();
        LocalDate exam = LocalDate.of(2026, 12, 1);

        long days = ChronoUnit.DAYS.between(today, exam);
        System.out.println("남은 일수: " + days);
    }
}
```

## 예제: 로그 라인에서 숫자 추출

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main {
    public static void main(String[] args) {
        String log = "id=120 score=95 rank=3";
        Matcher m = Pattern.compile("\\d+").matcher(log);

        while (m.find()) {
            System.out.println(m.group());
        }
    }
}
```

## 실수 포인트

- `Date`, `Calendar` 중심으로만 생각함
- `java.time` 객체가 불변이라는 점을 놓침
- 시간대(`ZoneId`) 문제를 무시함
- 정규식의 백슬래시 이스케이프를 잊음
- `matches()`가 전체 문자열 매칭이라는 점을 놓침

## 체크 문제

1. `LocalDate`와 `LocalDateTime` 차이는 무엇인가?
2. `Period`와 `Duration`은 어떻게 다른가?
3. 로케일은 왜 중요한가?
4. `matches()`와 `find()`는 어떤 차이가 있는가?

## 직접 해볼 과제

### 과제 1

생년월일을 입력받아 현재 나이를 계산하는 프로그램을 작성하세요.

### 과제 2

전화번호 형식을 정규식으로 검증하세요.

### 과제 3

현재 시간을 원하는 형식으로 포맷해 파일명으로 사용하는 문자열을 만들어 보세요.

## 오늘 정리

- 날짜/시간 처리는 현대 Java에서 `java.time`이 표준입니다.
- 로케일은 국가/언어에 따라 출력 형식을 다르게 만드는 핵심입니다.
- 정규식은 문자열 처리에 강력하지만 과하게 복잡하게 쓰지 않는 것이 중요합니다.

