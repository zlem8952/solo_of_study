# 20일차 - 실전 예제, 표준 라이브러리 지도, 학습 로드맵

## 오늘의 목표

- 20일 동안 배운 내용을 프로젝트 감각으로 연결한다.
- Java 표준 라이브러리의 큰 지도를 정리한다.
- 이후 어떤 순서로 반복 학습해야 할지 정리한다.

## Java 실력이 늘어나는 방식

문법을 읽는 것만으로는 실력이 잘 붙지 않습니다. 아래 4가지를 함께 해야 합니다.

1. 직접 작성
2. 직접 실행
3. 예외와 경계값 테스트
4. 설계 이유 설명

## 추천 미니 프로젝트 1 - 학생 관리 프로그램

핵심 연습 요소:

- 클래스 설계
- 리스트 사용
- 정렬
- 파일 저장 확장 가능

```java
import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;

class Student {
    private final String name;
    private final int score;

    Student(String name, int score) {
        this.name = name;
        this.score = score;
    }

    public String getName() {
        return name;
    }

    public int getScore() {
        return score;
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
        students.add(new Student("Lee", 80));
        students.add(new Student("Park", 95));

        students.sort(Comparator.comparingInt(Student::getScore).reversed());

        for (Student student : students) {
            System.out.println(student);
        }
    }
}
```

## 추천 미니 프로젝트 2 - 간단한 할 일(To-do) CLI

핵심 연습 요소:

- 리스트
- 메뉴 분기
- 메서드 분리
- 파일 저장 확장

기능 예시:

- 할 일 추가
- 목록 보기
- 완료 처리
- 삭제

## 추천 미니 프로젝트 3 - 단어 빈도 분석기

핵심 연습 요소:

- 파일 입력
- 문자열 처리
- `Map`
- 정렬

예제 핵심:

```java
import java.util.HashMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        String text = "java java study code code code";
        String[] words = text.split("\\s+");
        Map<String, Integer> count = new HashMap<>();

        for (String word : words) {
            count.put(word, count.getOrDefault(word, 0) + 1);
        }

        System.out.println(count);
    }
}
```

## 추천 미니 프로젝트 4 - HTTP 데이터 조회기

핵심 연습 요소:

- `HttpClient`
- JSON 응답 확인
- 예외 처리
- 비동기 확장 가능

## 표준 라이브러리 큰 지도

Java는 표준 라이브러리가 넓기 때문에, 함수 하나씩 외우기보다 패키지 역할을 기억하는 것이 훨씬 중요합니다.

| 패키지/영역 | 핵심 내용 |
| --- | --- |
| `java.lang` | 문자열, 수학, 기본 타입 래퍼, 예외, 스레드 기본 |
| `java.util` | 컬렉션, 유틸리티, `Scanner`, `Optional`, `Objects`, `Arrays` |
| `java.util.function` | 함수형 인터페이스 |
| `java.util.stream` | Stream API |
| `java.time` | 날짜/시간 API |
| `java.io` | 전통적인 스트림 기반 I/O |
| `java.nio.file` | 파일/경로/NIO.2 |
| `java.net` | 소켓, URL/URI |
| `java.net.http` | HTTP 클라이언트 |
| `java.util.concurrent` | 실행기, 락, 동시성 컬렉션 |
| `java.util.regex` | 정규식 |
| `java.math` | `BigInteger`, `BigDecimal` |
| `java.text` | 형식화, 일부 로케일 관련 도구 |
| `java.lang.reflect` | 리플렉션 |
| `java.lang.annotation` | 애너테이션 메타데이터 |
| `java.security` | 해시, 서명, 보안 관련 API |
| `java.sql` | JDBC 기반 데이터베이스 연결 API |

## 추가로 꼭 알아둘 클래스

### `BigDecimal`

돈 계산처럼 부동소수점 오차를 피해야 할 때 중요합니다.

```java
import java.math.BigDecimal;

BigDecimal a = new BigDecimal("0.1");
BigDecimal b = new BigDecimal("0.2");
System.out.println(a.add(b));
```

`double`은 이진 부동소수점이라 정확한 소수 계산에 한계가 있습니다.

### `BigInteger`

아주 큰 정수 계산에 사용합니다.

```java
import java.math.BigInteger;

BigInteger big = new BigInteger("12345678901234567890");
System.out.println(big.multiply(BigInteger.TEN));
```

### `Path`, `Files`

파일 시스템 작업의 현대 표준입니다.

```java
java.nio.file.Path path = java.nio.file.Path.of("data.txt");
System.out.println(java.nio.file.Files.exists(path));
```

### `ConcurrentHashMap`

멀티스레드 환경에서 자주 등장하는 맵입니다.

### `Pattern`

문자열 검증과 검색에서 반복적으로 등장합니다.

## 표준 라이브러리 학습 전략

1. `java.lang`, `java.util` 먼저 확실히
2. 컬렉션 + 제네릭 + 스트림 연결
3. 파일 I/O, 날짜/시간, 정규식 학습
4. 동시성, 네트워크, 리플렉션 확장
5. 이후 `java.math`, `java.net.http`, `java.util.concurrent` 반복

## 다시 보면 좋은 순서

20일을 한 바퀴 돈 뒤에는 아래 순서로 반복하는 것이 효율이 좋습니다.

1. 5일차 클래스/객체
2. 7일차 상속/다형성
3. 11일차 컬렉션
4. 12일차 제네릭/정렬
5. 13일차 스트림
6. 16일차 동시성

## 면접/실무에서 자주 나오는 질문

- `==`와 `equals` 차이
- `ArrayList`와 `LinkedList` 차이
- `HashMap` 동작 원리 기초
- 추상 클래스와 인터페이스 차이
- 오버로딩과 오버라이딩 차이
- checked / unchecked exception 차이
- `String`, `StringBuilder` 차이
- `synchronized`와 `volatile` 차이
- JVM 메모리 구조
- `Comparable`과 `Comparator` 차이
- `HashMap`과 `ConcurrentHashMap` 차이
- `record`와 일반 클래스 차이

## 실전 코딩 습관

- null 가능성을 항상 생각
- 컬렉션 타입은 인터페이스로 선언
- 예외 메시지와 스택 트레이스 확인
- 작은 메서드로 역할 분리
- 불변 객체를 우선 고려
- 무분별한 상속보다 인터페이스와 조합 우선

## 최종 과제 아이디어

### 과제 1

학생 관리 CLI를 만들고 파일 저장 기능까지 추가하세요.

### 과제 2

로그 파일을 읽어 에러 횟수와 사용자 ID 통계를 내는 프로그램을 작성하세요.

### 과제 3

HTTP API를 호출해 응답 데이터를 가공하는 콘솔 프로그램을 작성하세요.

### 과제 4

간단한 멀티스레드 작업 큐를 만들고 처리 속도를 측정해 보세요.

### 과제 5

CSV 파일을 읽어 통계를 계산하고 결과를 정렬해 출력하는 프로그램을 작성하세요.

## 2회독 추천 방식

두 번째 학습에서는 다음처럼 보는 것이 좋습니다.

1. 예제를 전부 직접 다시 작성
2. 각 일차별 과제를 최소 1개 이상 실제로 구현
3. 공통 도메인(`Student`, `Order`, `User`)으로 여러 문서를 연결
4. 마지막에는 하나의 콘솔 프로젝트로 합치기

## 마지막 정리

- Java는 문법 자체보다 `타입 시스템`, `객체지향`, `표준 라이브러리`, `JVM` 이해가 중요합니다.
- 좋은 Java 코드는 문법이 화려한 코드가 아니라, 읽기 쉽고 역할이 분명하며 예외와 상태를 잘 다루는 코드입니다.
- 이 문서 세트를 한 바퀴 돈 뒤, 작은 프로젝트를 직접 만들어 보면 실력이 빠르게 올라갑니다.
