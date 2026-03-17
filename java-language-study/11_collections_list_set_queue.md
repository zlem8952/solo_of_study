# 11일차 - 컬렉션 프레임워크: `List`, `Set`, `Queue`, `Deque`

## 오늘의 목표

- 컬렉션 프레임워크의 전체 구조를 이해한다.
- `List`, `Set`, `Queue`, `Deque`의 차이를 구분한다.
- 대표 구현체들을 언제 선택하는지 감을 잡는다.
- 반복, 검색, 삭제, 정렬 기초를 익힌다.

## 컬렉션 프레임워크란?

여러 데이터를 다루기 위한 표준 API 집합입니다.

대표 인터페이스:

- `Collection`
- `List`
- `Set`
- `Queue`
- `Deque`
- `Map`은 별도의 계열이며 12일차에 다룹니다.

## 큰 구조

```text
Iterable
  └─ Collection
      ├─ List
      ├─ Set
      └─ Queue
          └─ Deque
```

## `List`

순서가 있고, 중복을 허용하는 컬렉션입니다.

대표 구현체:

- `ArrayList`
- `LinkedList`

### `ArrayList`

가장 많이 쓰입니다.

장점:

- 인덱스 접근 빠름
- 일반적인 데이터 저장에 무난

단점:

- 중간 삽입/삭제가 많으면 비용 큼

예:

```java
import java.util.ArrayList;
import java.util.List;

List<String> names = new ArrayList<>();
names.add("Kim");
names.add("Lee");
names.add("Park");

System.out.println(names.get(0));
System.out.println(names.size());
```

## `LinkedList`

연결 구조 기반 리스트입니다.

장점:

- 앞뒤 삽입/삭제에 유리한 경우가 있음

단점:

- 인덱스 접근이 느림

실무에서는 많은 경우 `ArrayList`가 먼저 고려됩니다.

## `Set`

중복을 허용하지 않는 컬렉션입니다.

대표 구현체:

- `HashSet`
- `LinkedHashSet`
- `TreeSet`

### `HashSet`

- 중복 없음
- 순서 보장 없음
- 빠른 검색에 강함

### `LinkedHashSet`

- 입력 순서 유지

### `TreeSet`

- 정렬된 상태 유지

## `Queue`

먼저 들어온 것이 먼저 나가는 구조(FIFO)에 적합합니다.

대표 구현체:

- `LinkedList`
- `PriorityQueue`
- `ArrayDeque`

### `PriorityQueue`

우선순위에 따라 꺼내는 큐입니다.

```java
import java.util.PriorityQueue;

PriorityQueue<Integer> pq = new PriorityQueue<>();
pq.offer(30);
pq.offer(10);
pq.offer(20);

while (!pq.isEmpty()) {
    System.out.println(pq.poll());
}
```

## `Deque`

양쪽 끝에서 넣고 뺄 수 있는 자료구조입니다.

대표 구현체:

- `ArrayDeque`
- `LinkedList`

스택처럼도, 큐처럼도 사용할 수 있습니다.

```java
import java.util.ArrayDeque;
import java.util.Deque;

Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);
stack.push(2);
System.out.println(stack.pop());
```

## 컬렉션 공통 메서드

많은 컬렉션이 공통으로 제공하는 메서드:

- `add`
- `remove`
- `contains`
- `isEmpty`
- `size`
- `clear`

## 반복 방법

### 향상된 `for`

```java
for (String name : names) {
    System.out.println(name);
}
```

### `forEach`

```java
names.forEach(System.out::println);
```

### `Iterator`

```java
import java.util.Iterator;

Iterator<String> it = names.iterator();
while (it.hasNext()) {
    String value = it.next();
    System.out.println(value);
}
```

## 반복 중 삭제

향상된 `for`나 일반 반복 중 직접 삭제하면 문제가 될 수 있습니다.

안전하게 하려면 `Iterator`를 사용합니다.

```java
Iterator<String> it = names.iterator();
while (it.hasNext()) {
    String value = it.next();
    if (value.startsWith("K")) {
        it.remove();
    }
}
```

## 정렬

리스트 정렬:

```java
import java.util.Collections;

Collections.sort(numbers);
```

또는:

```java
numbers.sort(null);
```

정렬 전략은 12일차에서 `Comparable`, `Comparator`와 함께 더 자세히 다룹니다.

## `Collections` 유틸리티 클래스

자주 쓰는 메서드:

- `sort`
- `reverse`
- `shuffle`
- `max`
- `min`
- `frequency`

예:

```java
Collections.reverse(names);
Collections.shuffle(names);
```

## `ArrayList`와 배열 차이

### 배열

- 크기 고정
- 기본형 저장 가능

### `ArrayList`

- 크기 동적 증가
- 객체 타입 중심
- 제네릭과 함께 사용

기본형은 직접 넣을 수 없고 래퍼 타입을 사용합니다.

```java
List<Integer> list = new ArrayList<>();
```

## `List.of`, `Set.of`

불변 컬렉션을 간단하게 만들 수 있습니다.

```java
List<String> names = List.of("Kim", "Lee", "Park");
Set<Integer> nums = Set.of(1, 2, 3);
```

주의:

- 수정 불가

```java
names.add("Han"); // 예외
```

## 예제: 학생 이름 목록

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> students = new ArrayList<>();
        students.add("Kim");
        students.add("Lee");
        students.add("Park");

        for (String student : students) {
            System.out.println(student);
        }
    }
}
```

## 예제: 중복 제거

```java
import java.util.HashSet;
import java.util.Set;

public class Main {
    public static void main(String[] args) {
        Set<String> fruits = new HashSet<>();
        fruits.add("apple");
        fruits.add("banana");
        fruits.add("apple");

        System.out.println(fruits);
    }
}
```

## 예제: 큐 처리

```java
import java.util.ArrayDeque;
import java.util.Queue;

public class Main {
    public static void main(String[] args) {
        Queue<String> queue = new ArrayDeque<>();
        queue.offer("task1");
        queue.offer("task2");
        queue.offer("task3");

        while (!queue.isEmpty()) {
            System.out.println(queue.poll());
        }
    }
}
```

## 언제 무엇을 고를까?

- 순서 있고 자주 읽기: `ArrayList`
- 중복 제거: `HashSet`
- 입력 순서 유지한 중복 제거: `LinkedHashSet`
- 정렬된 집합: `TreeSet`
- FIFO 작업 처리: `ArrayDeque` 또는 `LinkedList`
- 우선순위 처리: `PriorityQueue`

## 실수 포인트

- 컬렉션과 배열을 같은 것으로 생각함
- `HashSet`에 순서를 기대함
- 반복 중 직접 삭제하다 예외 발생
- 기본형을 `List<int>`처럼 쓰려 함
- 불변 컬렉션을 수정하려고 함

## 체크 문제

1. `List`와 `Set` 차이는 무엇인가?
2. `ArrayList`와 `LinkedList`는 언제 다르게 선택하는가?
3. `HashSet`과 `TreeSet` 차이는 무엇인가?
4. 반복 중 안전하게 삭제하려면 무엇을 써야 하는가?

## 직접 해볼 과제

### 과제 1

정수 목록을 `ArrayList`에 저장하고 최댓값과 평균을 구하세요.

### 과제 2

문자열 배열의 중복을 제거해 `Set`으로 출력하세요.

### 과제 3

작업 이름을 큐에 넣고 하나씩 꺼내 처리하는 예제를 작성하세요.

## 오늘 정리

- 컬렉션 프레임워크는 Java 데이터 처리의 핵심입니다.
- `List`, `Set`, `Queue`, `Deque`는 역할이 분명히 다릅니다.
- 구현체는 성질을 이해하고 선택해야 합니다.

