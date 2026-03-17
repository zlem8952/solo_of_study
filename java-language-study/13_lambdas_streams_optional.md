# 13일차 - 람다, 함수형 인터페이스, 스트림, `Optional`

## 오늘의 목표

- 람다식과 함수형 인터페이스를 이해한다.
- 메서드 참조를 사용할 수 있다.
- Stream API의 중간 연산과 최종 연산을 익힌다.
- `Optional`을 null 처리 도구로 적절히 사용할 수 있다.

## 람다식이란?

익명 함수를 더 간결하게 표현하는 문법입니다.

```java
(a, b) -> a + b
```

## 함수형 인터페이스

추상 메서드가 하나인 인터페이스입니다.

```java
@FunctionalInterface
interface Calculator {
    int calc(int a, int b);
}
```

사용:

```java
Calculator add = (a, b) -> a + b;
System.out.println(add.calc(3, 4));
```

## 표준 함수형 인터페이스

`java.util.function` 패키지에 많이 들어 있습니다.

| 인터페이스 | 의미 |
| --- | --- |
| `Predicate<T>` | T -> boolean |
| `Function<T, R>` | T -> R |
| `Consumer<T>` | T 소비, 반환 없음 |
| `Supplier<T>` | 값 공급 |
| `UnaryOperator<T>` | T -> T |
| `BinaryOperator<T>` | (T, T) -> T |

## 메서드 참조

람다를 더 간단히 표현할 수 있습니다.

```java
names.forEach(System.out::println);
```

형태:

- `ClassName::staticMethod`
- `object::instanceMethod`
- `ClassName::instanceMethod`
- `ClassName::new`

## 스트림(Stream)이란?

컬렉션 데이터를 선언형으로 처리하기 위한 API입니다.

핵심 아이디어:

- "어떻게 반복할까?"보다 "무엇을 할까?"에 집중

## 스트림 생성

```java
List<Integer> nums = List.of(1, 2, 3, 4, 5);
nums.stream();
```

배열:

```java
Arrays.stream(arr);
```

## 중간 연산

결과 스트림을 만드는 연산입니다.

- `filter`
- `map`
- `flatMap`
- `sorted`
- `distinct`
- `limit`
- `skip`

## 최종 연산

최종 결과를 만드는 연산입니다.

- `forEach`
- `collect`
- `toList`
- `count`
- `reduce`
- `anyMatch`
- `allMatch`
- `findFirst`

## 기본 예제

```java
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> nums = List.of(1, 2, 3, 4, 5, 6);

        nums.stream()
            .filter(n -> n % 2 == 0)
            .map(n -> n * 10)
            .forEach(System.out::println);
    }
}
```

## `map`

데이터를 다른 형태로 변환합니다.

```java
List<String> names = List.of("kim", "lee", "park");
List<String> upper = names.stream()
    .map(String::toUpperCase)
    .toList();
```

## `filter`

조건에 맞는 것만 남깁니다.

```java
List<Integer> result = nums.stream()
    .filter(n -> n > 10)
    .toList();
```

## `sorted`

정렬:

```java
students.stream()
    .sorted(Comparator.comparingInt(s -> s.score))
    .toList();
```

## `collect`

결과를 컬렉션이나 맵으로 모읍니다.

```java
import java.util.stream.Collectors;

List<String> result = names.stream()
    .filter(name -> name.length() >= 3)
    .collect(Collectors.toList());
```

자주 쓰는 Collector:

- `toList`
- `toSet`
- `joining`
- `groupingBy`
- `partitioningBy`
- `counting`
- `mapping`

## `reduce`

하나의 값으로 축약합니다.

```java
int sum = nums.stream()
    .reduce(0, Integer::sum);
```

## `flatMap`

중첩 구조를 펼칩니다.

```java
List<List<String>> groups = List.of(
    List.of("a", "b"),
    List.of("c", "d")
);

List<String> all = groups.stream()
    .flatMap(List::stream)
    .toList();
```

## `Optional`

값이 있을 수도, 없을 수도 있음을 표현하는 컨테이너입니다.

```java
Optional<String> name = Optional.of("Java");
Optional<String> empty = Optional.empty();
```

## `Optional` 주요 메서드

| 메서드 | 설명 |
| --- | --- |
| `isPresent` | 값 존재 여부 |
| `ifPresent` | 값이 있으면 실행 |
| `orElse` | 없으면 기본값 |
| `orElseGet` | 없으면 공급 함수 실행 |
| `orElseThrow` | 없으면 예외 |
| `map` | 값 변환 |
| `filter` | 조건 필터 |

예:

```java
String result = Optional.ofNullable(name)
    .map(String::trim)
    .filter(s -> !s.isEmpty())
    .orElse("기본값");
```

## `Optional` 사용 주의

- 모든 곳에 남용하지 않기
- 필드로 무조건 쓰는 것보다 반환 타입에서 의미 있게 쓰기
- `get()`만 바로 호출하는 습관은 피하기

## 예제: 학생 점수 필터링

```java
import java.util.List;

class Student {
    String name;
    int score;

    Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
}

public class Main {
    public static void main(String[] args) {
        List<Student> students = List.of(
            new Student("Kim", 90),
            new Student("Lee", 70),
            new Student("Park", 85)
        );

        students.stream()
            .filter(s -> s.score >= 80)
            .map(s -> s.name)
            .forEach(System.out::println);
    }
}
```

## 예제: 그룹화

```java
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<String> words = List.of("java", "jam", "code", "cat", "car");

        Map<Integer, List<String>> grouped = words.stream()
            .collect(Collectors.groupingBy(String::length));

        System.out.println(grouped);
    }
}
```

## 실수 포인트

- 람다와 익명 클래스를 구분 없이 이해함
- 스트림을 컬렉션 자체로 오해함
- 스트림 재사용을 시도함
- `Optional.get()`을 무심코 사용함
- 선언형 코드를 너무 복잡하게 써 가독성을 해침

## 체크 문제

1. 함수형 인터페이스란 무엇인가?
2. 스트림의 중간 연산과 최종 연산은 어떻게 다른가?
3. `map`과 `filter`는 각각 어떤 역할인가?
4. `Optional`은 언제 유용한가?

## 직접 해볼 과제

### 과제 1

정수 리스트에서 짝수만 골라 제곱한 뒤 합계를 구하세요.

### 과제 2

문자열 리스트를 길이 기준으로 그룹화하세요.

### 과제 3

사용자 이름이 null이거나 빈 문자열이면 `"guest"`를 반환하는 코드를 `Optional`로 작성하세요.

## 오늘 정리

- 람다와 스트림은 Java 현대 스타일의 핵심입니다.
- 선언형 데이터 처리에 익숙해지면 컬렉션 코드가 훨씬 간결해집니다.
- `Optional`은 null 처리 의도를 더 분명하게 만들어 줍니다.

