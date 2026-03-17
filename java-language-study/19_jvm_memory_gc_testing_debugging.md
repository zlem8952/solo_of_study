# 19일차 - JVM 메모리, 가비지 컬렉션, 테스트, 디버깅, 성능 감각

## 오늘의 목표

- JVM 메모리 구조의 기본을 이해한다.
- 가비지 컬렉션이 어떤 문제를 해결하는지 안다.
- 테스트와 디버깅 습관을 정리한다.
- 성능 문제를 감으로 추측하지 않고 측정하는 태도를 익힌다.

## JVM 메모리 큰 그림

간단히 보면 다음 영역을 이해하면 됩니다.

- 스택(Stack)
- 힙(Heap)
- 메서드 영역/메타스페이스(Metaspace)

## 스택

- 메서드 호출 정보
- 지역 변수
- 매개변수

메서드 호출이 끝나면 스택 프레임이 사라집니다.

## 힙

객체와 배열이 주로 저장되는 영역입니다.

```java
Student s = new Student("Kim", 20);
```

`s` 참조 자체는 보통 스택에, 실제 객체는 힙에 있다고 이해하면 좋습니다.

## 메타스페이스

클래스 메타데이터가 저장되는 영역입니다.

## 클래스 로딩과 JIT 감각

JVM은 클래스를 로드하고, 자주 실행되는 코드는 JIT(Just-In-Time) 컴파일러가 최적화할 수 있습니다.

즉:

- Java는 단순 해석 실행만 하는 것이 아님
- 실행 중 최적화가 성능에 영향을 줌
- 너무 단순한 벤치마크는 오해를 만들 수 있음

## 가비지 컬렉션(GC)

더 이상 참조되지 않는 객체를 JVM이 자동으로 회수합니다.

장점:

- 수동 `free` 없음
- 메모리 해제 실수 감소

하지만:

- 메모리 누수 개념이 완전히 사라지는 것은 아닙니다.
- `static` 컬렉션에 계속 쌓아 두면 여전히 문제

## 메모리 누수 예시

```java
import java.util.ArrayList;
import java.util.List;

class Cache {
    static List<Object> store = new ArrayList<>();
}
```

필요 없는 객체를 계속 보관하면 GC가 회수할 수 없습니다.

## `StackOverflowError`

재귀가 너무 깊거나 종료 조건이 없으면 스택이 넘칠 수 있습니다.

```java
static void loop() {
    loop();
}
```

이것은 힙 부족이 아니라 호출 스택 문제입니다.

## 참조와 객체 수명

객체는 참조가 끊겨야 회수 후보가 됩니다.

```java
String s = new String("hello");
s = null;
```

바로 GC된다는 뜻은 아니지만, 수거 대상이 될 수 있습니다.

## `finalize`는 피하자

과거에는 정리 훅으로 쓰였지만, 현대 Java에서는 권장되지 않습니다. 자원 해제는 `try-with-resources`나 명시적 종료 메서드로 관리하는 것이 낫습니다.

## 참조 타입 심화

고급 주제로는 다음 참조 타입도 존재합니다.

- `WeakReference`
- `SoftReference`
- `PhantomReference`

캐시나 메모리 민감 구조에서 등장할 수 있습니다.

## 테스트란?

코드가 기대한 대로 동작하는지 검증하는 과정입니다.

### 테스트가 중요한 이유

- 리팩터링 안전성
- 버그 조기 발견
- 설계 품질 향상

## 단위 테스트(Unit Test)

작은 기능 단위를 독립적으로 검증합니다.

Java 표준 라이브러리만으로는 본격적인 테스트 프레임워크가 부족하므로, 실무에서는 JUnit을 많이 사용합니다.

하지만 원리 자체는 간단합니다.

```java
static int add(int a, int b) {
    return a + b;
}

public static void main(String[] args) {
    if (add(2, 3) != 5) {
        throw new AssertionError("테스트 실패");
    }
}
```

## 테스트 종류 감각

- 단위 테스트: 작은 기능 단위
- 통합 테스트: 여러 요소 연결 검증
- 회귀 테스트: 기존 기능이 깨지지 않았는지 확인

## `assert`

```java
assert value > 0 : "value must be positive";
```

실행 시 활성화:

```bash
java -ea Main
```

주의:

- `assert`는 사용자 입력 검증 수단이 아니라 개발 중 전제 조건 점검용

## 디버깅 기본 순서

1. 문제를 재현 가능한 최소 입력으로 줄인다.
2. 예외 메시지와 스택 트레이스를 먼저 읽는다.
3. 변수 상태를 출력하거나 디버거로 확인한다.
4. 최근에 바뀐 코드, 경계값, null 가능성을 점검한다.

## 스택 트레이스 읽기

예외가 발생하면 보통:

- 어떤 예외인지
- 어디서 발생했는지
- 어떤 호출 경로로 왔는지

를 알려줍니다.

이것을 읽는 습관이 매우 중요합니다.

## 디버거에서 자주 보는 기능

- 중단점(breakpoint)
- 한 줄씩 실행(step over)
- 메서드 안으로 들어가기(step into)
- 변수 값 보기
- 호출 스택 확인

## 로깅

단순 `System.out.println`도 도움이 되지만, 실제 프로젝트에서는 로깅 프레임워크를 많이 사용합니다.

표준 라이브러리에는 `java.util.logging`이 있습니다.

```java
import java.util.logging.Logger;

Logger logger = Logger.getLogger("app");
logger.info("started");
```

로그에 자주 담는 것:

- 시작/종료 이벤트
- 주요 입력 값
- 예외 메시지
- 처리 시간

## 성능 측정 태도

감으로 "이게 느릴 것 같다"를 판단하면 자주 틀립니다.

원칙:

- 먼저 병목을 측정
- 자료구조 선택 점검
- 불필요한 객체 생성 줄이기
- 문자열 처리 방식 확인
- I/O와 네트워크 지연 고려

## `System.nanoTime`

간단한 측정에 사용 가능

```java
long start = System.nanoTime();
// 작업
long end = System.nanoTime();
System.out.println(end - start);
```

정밀 벤치마크는 JMH 같은 도구가 더 적합합니다.

## 성능과 관련한 흔한 포인트

- 반복문에서 문자열 `+` 남용
- 잘못된 컬렉션 선택
- 박싱/언박싱 과다
- 불필요한 동기화
- 불필요한 객체 생성

## JVM 옵션 미리 보기

자주 보게 되는 옵션:

- `-Xms`: 초기 힙 크기
- `-Xmx`: 최대 힙 크기
- `-ea`: assert 활성화

예:

```bash
java -Xms256m -Xmx1024m -ea Main
```

## 프로파일링 도구 감각

문제가 복잡해지면 다음 도구들이 등장합니다.

- VisualVM
- Java Flight Recorder
- Java Mission Control
- 힙 덤프 분석 도구

출력문만으로 안 잡히는 병목은 이런 도구로 봐야 합니다.

## 예제: 간단 성능 비교 감각

```java
public class Main {
    public static void main(String[] args) {
        long start = System.nanoTime();

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 100000; i++) {
            sb.append(i);
        }

        long end = System.nanoTime();
        System.out.println("elapsed(ns) = " + (end - start));
    }
}
```

## 실수 포인트

- GC가 있으니 메모리 문제는 없다고 생각함
- 스택 트레이스를 읽지 않고 감으로 수정함
- 성능 문제를 측정 없이 추측함
- 테스트 없는 리팩터링을 반복함
- `assert`를 항상 실행된다고 오해함
- 재귀 오류와 힙 메모리 문제를 같은 것으로 생각함
- warm-up 없는 성능 측정을 그대로 믿음

## 체크 문제

1. 스택과 힙은 어떻게 다른가?
2. GC가 있어도 메모리 누수가 생길 수 있는 이유는 무엇인가?
3. 디버깅에서 가장 먼저 확인해야 하는 것은 무엇인가?
4. `System.nanoTime()`은 언제 유용한가?

## 직접 해볼 과제

### 과제 1

반복 문자열 결합과 `StringBuilder`의 실행 시간을 간단히 비교해 보세요.

### 과제 2

의도적으로 `NullPointerException`을 만들고 스택 트레이스를 읽어 보세요.

### 과제 3

간단한 메서드에 `assert`를 넣고 `-ea` 옵션으로 실행해 보세요.

## 오늘 정리

- JVM 메모리 구조를 이해하면 객체 동작 감각이 좋아집니다.
- GC가 메모리 관리를 도와주지만, 설계 책임까지 대신해 주는 것은 아닙니다.
- 테스트, 디버깅, 측정 습관이 Java 실력을 크게 좌우합니다.
