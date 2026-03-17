# 12일차 - `Map`, 제네릭, `Comparable`, `Comparator`

## 오늘의 목표

- `Map` 계열 컬렉션을 이해한다.
- 제네릭 문법과 필요성을 익힌다.
- `Comparable`, `Comparator`를 이용해 정렬 전략을 설계할 수 있다.
- 와일드카드와 상한/하한 제한의 감각을 잡는다.

## `Map`이란?

키(key)와 값(value)을 쌍으로 저장하는 자료구조입니다.

대표 구현체:

- `HashMap`
- `LinkedHashMap`
- `TreeMap`

## `HashMap`

- 가장 많이 사용
- 키 중복 불가
- 순서 보장 없음
- 평균적으로 빠른 조회

```java
import java.util.HashMap;
import java.util.Map;

Map<String, Integer> scores = new HashMap<>();
scores.put("Kim", 90);
scores.put("Lee", 85);
scores.put("Park", 92);

System.out.println(scores.get("Kim"));
```

## `Map` 주요 메서드

| 메서드 | 설명 |
| --- | --- |
| `put` | 저장 |
| `get` | 조회 |
| `containsKey` | 키 존재 여부 |
| `containsValue` | 값 존재 여부 |
| `remove` | 삭제 |
| `size` | 개수 |
| `keySet` | 키 집합 |
| `values` | 값 컬렉션 |
| `entrySet` | 키-값 쌍 집합 |
| `getOrDefault` | 없을 때 기본값 |
| `putIfAbsent` | 없을 때만 저장 |

## `Map` 반복

```java
for (Map.Entry<String, Integer> entry : scores.entrySet()) {
    System.out.println(entry.getKey() + " -> " + entry.getValue());
}
```

## `LinkedHashMap`

- 입력 순서 유지

## `TreeMap`

- 키 정렬 유지
- 키는 `Comparable` 구현 또는 `Comparator` 제공 필요

## `Map.of`

간단한 불변 맵 생성:

```java
Map<String, Integer> map = Map.of(
    "Kim", 90,
    "Lee", 85
);
```

## 제네릭이란?

클래스나 메서드가 다룰 타입을 일반화하는 문법입니다.

```java
List<String> names = new ArrayList<>();
List<Integer> numbers = new ArrayList<>();
```

장점:

- 타입 안정성
- 형 변환 감소
- 컴파일 시 오류 발견

## 제네릭이 없던 경우의 문제

```java
List list = new ArrayList();
list.add("hello");
Integer x = (Integer) list.get(0); // 런타임 오류 가능
```

제네릭 사용:

```java
List<String> list = new ArrayList<>();
String s = list.get(0);
```

## 제네릭 클래스

```java
class Box<T> {
    private T value;

    public void set(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }
}
```

사용:

```java
Box<String> box = new Box<>();
box.set("Java");
System.out.println(box.get());
```

## 제네릭 메서드

```java
static <T> void printArray(T[] arr) {
    for (T item : arr) {
        System.out.println(item);
    }
}
```

## 와일드카드 `?`

```java
List<?> list;
```

의미:

- 어떤 타입인지 모르지만 어떤 타입의 리스트

## 상한 제한 `? extends`

```java
static double sum(List<? extends Number> nums) {
    double total = 0;
    for (Number n : nums) {
        total += n.doubleValue();
    }
    return total;
}
```

읽기 중심에 적합합니다.

## 하한 제한 `? super`

```java
static void addIntegers(List<? super Integer> list) {
    list.add(10);
    list.add(20);
}
```

쓰기 중심에 적합합니다.

기억법:

- Producer Extends
- Consumer Super

## 타입 소거(Type Erasure)

Java 제네릭은 런타임에 대부분 타입 정보가 지워집니다.

의미:

- 제네릭은 주로 컴파일 타임 타입 안전성 도구
- `new T()` 같은 표현이 안 되는 이유와 연결됨

## `Comparable`

자기 자신끼리의 기본 정렬 기준을 정의합니다.

```java
class Student implements Comparable<Student> {
    String name;
    int score;

    Student(String name, int score) {
        this.name = name;
        this.score = score;
    }

    @Override
    public int compareTo(Student other) {
        return Integer.compare(this.score, other.score);
    }
}
```

## `Comparator`

외부에서 정렬 전략을 정의합니다.

```java
import java.util.Comparator;

Comparator<Student> byName = Comparator.comparing(s -> s.name);
Comparator<Student> byScoreDesc = (a, b) -> Integer.compare(b.score, a.score);
```

## `Comparable` vs `Comparator`

### `Comparable`

- 클래스 내부의 기본 정렬 기준

### `Comparator`

- 상황별 다른 정렬 기준

## 정렬 예제

```java
import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;

class Student {
    String name;
    int score;

    Student(String name, int score) {
        this.name = name;
        this.score = score;
    }

    @Override
    public String toString() {
        return name + ":" + score;
    }
}

public class Main {
    public static void main(String[] args) {
        List<Student> students = new ArrayList<>();
        students.add(new Student("Kim", 90));
        students.add(new Student("Lee", 85));
        students.add(new Student("Park", 95));

        students.sort(Comparator.comparingInt(s -> s.score));
        System.out.println(students);
    }
}
```

## `Map` 실전 예제: 단어 개수 세기

```java
import java.util.HashMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        String[] words = {"java", "is", "fun", "java", "study", "fun"};
        Map<String, Integer> countMap = new HashMap<>();

        for (String word : words) {
            countMap.put(word, countMap.getOrDefault(word, 0) + 1);
        }

        System.out.println(countMap);
    }
}
```

## 실수 포인트

- `Map`을 컬렉션처럼 인덱스로 접근하려고 함
- 제네릭 없이 raw type을 사용함
- `HashMap`에서 순서를 기대함
- `Comparable`과 `Comparator`를 혼동함
- `? extends`에서 값을 자유롭게 넣을 수 있다고 착각함

## 체크 문제

1. `Map`은 어떤 문제를 해결하기 좋은가?
2. 제네릭이 왜 필요한가?
3. `Comparable`과 `Comparator`는 어떻게 다른가?
4. `? extends Number`는 어떤 의미인가?

## 직접 해볼 과제

### 과제 1

학생 이름과 점수를 `Map`에 저장하고 최고 점수를 찾으세요.

### 과제 2

제네릭 `Pair<K, V>` 클래스를 직접 만들어 보세요.

### 과제 3

학생 객체 리스트를 이름순, 점수순으로 각각 정렬해 보세요.

## 오늘 정리

- `Map`은 키-값 데이터 처리의 핵심입니다.
- 제네릭은 Java 타입 안전성을 크게 높여 줍니다.
- 정렬 전략은 `Comparable`과 `Comparator`를 구분해서 설계해야 합니다.

