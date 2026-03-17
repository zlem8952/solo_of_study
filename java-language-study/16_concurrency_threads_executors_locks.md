# 16일차 - 스레드, 실행기, 동기화, 동시성 유틸리티

## 오늘의 목표

- Java 동시성의 기본 개념을 이해한다.
- `Thread`, `Runnable`, `Callable`, `ExecutorService`를 사용할 수 있다.
- `synchronized`, `Lock`, `volatile`의 역할을 구분한다.
- 동시성 컬렉션과 원자 클래스, `CompletableFuture` 개념을 익힌다.

## 프로세스와 스레드

### 프로세스

실행 중인 프로그램 단위

### 스레드

프로세스 안에서 실제 작업을 수행하는 실행 흐름

Java 프로그램은 기본적으로 메인 스레드에서 시작합니다.

## 스레드 생성 방식 1 - `Thread`

```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("작업 실행");
    }
}

public class Main {
    public static void main(String[] args) {
        Thread t = new MyThread();
        t.start();
    }
}
```

## 스레드 생성 방식 2 - `Runnable`

더 일반적으로 많이 사용합니다.

```java
public class Main {
    public static void main(String[] args) {
        Runnable task = () -> System.out.println("작업 실행");
        Thread t = new Thread(task);
        t.start();
    }
}
```

## `start()`와 `run()` 차이

```java
t.start();
```

새 스레드 시작

```java
t.run();
```

그냥 현재 스레드에서 메서드 호출

초보자가 자주 혼동합니다.

## `sleep`, `join`

```java
Thread.sleep(1000);
t.join();
```

- `sleep`: 현재 스레드 잠시 대기
- `join`: 다른 스레드 종료까지 기다림

## `Callable`과 `Future`

결과를 반환하는 작업에 사용합니다.

```java
import java.util.concurrent.Callable;

Callable<Integer> task = () -> 1 + 2;
```

`Runnable`은 반환값이 없고, `Callable`은 반환값과 예외 처리가 가능합니다.

## `ExecutorService`

실무에서는 직접 `Thread`를 계속 만드는 대신, 스레드 풀을 관리하는 실행기를 많이 씁니다.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

ExecutorService executor = Executors.newFixedThreadPool(4);
executor.submit(() -> System.out.println("작업"));
executor.shutdown();
```

## `Future`

비동기 작업 결과를 나중에 받습니다.

```java
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) throws Exception {
        ExecutorService executor = Executors.newSingleThreadExecutor();

        Future<Integer> future = executor.submit(() -> 10 + 20);
        System.out.println(future.get());

        executor.shutdown();
    }
}
```

## 공유 자원 문제

여러 스레드가 같은 데이터를 동시에 수정하면 경쟁 상태(race condition)가 발생할 수 있습니다.

```java
class Counter {
    int value = 0;

    void increase() {
        value++;
    }
}
```

여러 스레드가 동시에 `increase()`하면 결과가 틀릴 수 있습니다.

## `synchronized`

한 번에 하나의 스레드만 들어오게 하는 기본 동기화 도구입니다.

```java
class Counter {
    private int value = 0;

    public synchronized void increase() {
        value++;
    }

    public synchronized int getValue() {
        return value;
    }
}
```

블록 단위도 가능:

```java
synchronized (lock) {
    ...
}
```

## `volatile`

변수 변경이 스레드 사이에 즉시 보이도록 가시성(visibility)을 보장하는 데 사용합니다.

```java
private volatile boolean running = true;
```

주의:

- `volatile`은 원자적 증가를 보장하지 않습니다.
- `count++` 문제를 해결하지 못합니다.

## `AtomicInteger`

원자적 연산이 필요할 때 사용합니다.

```java
import java.util.concurrent.atomic.AtomicInteger;

AtomicInteger count = new AtomicInteger();
count.incrementAndGet();
```

## `Lock`

더 유연한 명시적 락입니다.

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

Lock lock = new ReentrantLock();
lock.lock();
try {
    ...
} finally {
    lock.unlock();
}
```

## `wait`, `notify`, `notifyAll`

전통적인 스레드 협력 방식입니다.

```java
synchronized (lock) {
    lock.wait();
    lock.notifyAll();
}
```

직접 사용 가능하지만 난이도가 높습니다. 현대 코드에서는 `BlockingQueue`, `ExecutorService`, `CompletableFuture` 같은 더 높은 수준 API를 우선 고려하는 경우가 많습니다.

## 동시성 컬렉션

`java.util.concurrent` 패키지에는 스레드 안전 컬렉션이 있습니다.

대표 예:

- `ConcurrentHashMap`
- `CopyOnWriteArrayList`
- `BlockingQueue`

## `BlockingQueue`

생산자-소비자 패턴에 자주 사용됩니다.

```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

BlockingQueue<String> queue = new ArrayBlockingQueue<>(10);
queue.put("task");
String value = queue.take();
```

## `CompletableFuture`

비동기 작업 조합에 매우 유용합니다.

```java
import java.util.concurrent.CompletableFuture;

public class Main {
    public static void main(String[] args) {
        CompletableFuture.supplyAsync(() -> "Java")
            .thenApply(String::toUpperCase)
            .thenAccept(System.out::println)
            .join();
    }
}
```

## 스레드 안전성 설계 원칙

- 공유 상태를 줄이기
- 불변 객체 선호
- 동기화 범위를 최소화
- 고수준 동시성 도구 사용
- `static` 가변 상태를 신중히 다루기

## 예제: 안전한 카운터

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicInteger;

public class Main {
    public static void main(String[] args) throws InterruptedException {
        AtomicInteger count = new AtomicInteger();
        ExecutorService executor = Executors.newFixedThreadPool(4);

        for (int i = 0; i < 1000; i++) {
            executor.submit(count::incrementAndGet);
        }

        executor.shutdown();
        executor.awaitTermination(1, TimeUnit.SECONDS);

        System.out.println(count.get());
    }
}
```

## 실수 포인트

- `start()` 대신 `run()` 호출
- 스레드를 직접 과도하게 생성
- 공유 상태를 보호하지 않음
- `volatile`만으로 복합 연산 문제를 해결할 수 있다고 오해
- `ExecutorService`를 `shutdown()`하지 않음

## 체크 문제

1. `Runnable`과 `Callable`은 어떻게 다른가?
2. `synchronized`와 `volatile`은 어떤 차이가 있는가?
3. 왜 `ExecutorService`를 많이 쓰는가?
4. `AtomicInteger`는 어떤 문제를 해결하는가?

## 직접 해볼 과제

### 과제 1

두 개의 스레드를 실행해 서로 다른 메시지를 출력하게 하세요.

### 과제 2

1000번 증가하는 카운터를 멀티스레드 환경에서 안전하게 구현하세요.

### 과제 3

`CompletableFuture`를 사용해 비동기 계산 두 개를 실행한 뒤 결과를 합쳐 보세요.

## 오늘 정리

- 동시성은 Java의 강력한 분야지만 가장 실수하기 쉬운 영역이기도 합니다.
- `ExecutorService`, 원자 클래스, 동시성 컬렉션 같은 고수준 도구를 익히는 것이 중요합니다.
- 공유 상태와 가시성, 원자성 차이를 분명히 이해해야 합니다.

