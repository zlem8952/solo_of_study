# 8일차 - 추상 클래스, 인터페이스, 레코드, 열거형, 봉인 클래스

## 오늘의 목표

- 추상 클래스와 인터페이스의 차이를 이해한다.
- 인터페이스 기반 설계가 왜 중요한지 안다.
- `enum`, `record`, `sealed` 같은 현대 Java 타입 설계를 익힌다.

## 추상 클래스

직접 객체로 만들기보다, 공통 기반을 제공하기 위한 클래스입니다.

```java
abstract class Animal {
    abstract void sound();

    void sleep() {
        System.out.println("잠을 잔다");
    }
}
```

특징:

- `abstract` 메서드를 가질 수 있음
- 일반 메서드도 가질 수 있음
- 인스턴스 필드 보유 가능
- 직접 `new Animal()` 불가

## 추상 클래스 상속

```java
class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("멍멍");
    }
}
```

## 인터페이스

클래스가 반드시 제공해야 할 동작 규약을 정의합니다.

```java
interface Flyable {
    void fly();
}
```

구현:

```java
class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("난다");
    }
}
```

## 추상 클래스 vs 인터페이스

### 추상 클래스

- 공통 상태(필드)와 공통 구현을 함께 제공 가능
- 강한 "기반 클래스" 개념

### 인터페이스

- 능력/역할 규약 정의에 적합
- 다중 구현 가능
- 느슨한 결합 설계에 매우 유리

실무에서는 상속보다 인터페이스 중심 설계가 더 유연한 경우가 많습니다.

## 인터페이스의 메서드 종류

현대 Java 인터페이스는 여러 메서드 형태를 가질 수 있습니다.

```java
interface MyInterface {
    void work();                 // 추상 메서드

    default void log() {
        System.out.println("default log");
    }

    static void help() {
        System.out.println("help");
    }
}
```

`default` 메서드는 기존 구현체를 깨지 않고 기능을 추가할 때 유용합니다.

## 함수형 인터페이스

추상 메서드가 하나인 인터페이스입니다.

```java
@FunctionalInterface
interface Calculator {
    int calc(int a, int b);
}
```

람다와 함께 매우 많이 사용됩니다. 13일차에서 자세히 다룹니다.

## 다중 구현

Java 클래스는 클래스 상속은 하나만 가능하지만, 인터페이스는 여러 개 구현할 수 있습니다.

```java
class SmartPhone implements Camera, MusicPlayer, InternetCapable {
}
```

## `enum`

고정된 상수 집합을 표현합니다.

```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

사용:

```java
Day today = Day.MONDAY;
```

### enum의 장점

- 타입 안전성
- 오타 감소
- 의미 있는 코드

## enum에 필드와 메서드 넣기

```java
enum Level {
    LOW(1), MEDIUM(2), HIGH(3);

    private final int priority;

    Level(int priority) {
        this.priority = priority;
    }

    public int getPriority() {
        return priority;
    }
}
```

## `record`

데이터 중심 클래스를 간결하게 표현하기 위한 타입입니다.

```java
record Person(String name, int age) {
}
```

자동으로 제공되는 것:

- 생성자
- 접근자 메서드
- `toString`
- `equals`
- `hashCode`

사용:

```java
Person p = new Person("Kim", 20);
System.out.println(p.name());
```

레코드는 불변 데이터 모델에 매우 적합합니다.

## 일반 클래스와 record 차이

### 일반 클래스

- 자유도가 높음
- 변경 가능한 상태 설계 가능

### record

- 데이터 전달 객체에 적합
- 간결함
- 불변성 중심

## `sealed` 클래스 / 인터페이스

상속 또는 구현 가능한 타입을 제한할 수 있습니다.

```java
sealed class Shape permits Circle, Rectangle {
}

final class Circle extends Shape {
}

final class Rectangle extends Shape {
}
```

장점:

- 타입 계층을 통제 가능
- switch와 패턴 매칭 등과 결합 시 안정성 향상

## 예제: 인터페이스 기반 결제

```java
interface Payment {
    void pay(int amount);
}

class CardPayment implements Payment {
    @Override
    public void pay(int amount) {
        System.out.println("카드 결제: " + amount);
    }
}

class CashPayment implements Payment {
    @Override
    public void pay(int amount) {
        System.out.println("현금 결제: " + amount);
    }
}

public class Main {
    static void checkout(Payment payment, int amount) {
        payment.pay(amount);
    }

    public static void main(String[] args) {
        checkout(new CardPayment(), 10000);
        checkout(new CashPayment(), 5000);
    }
}
```

## 예제: enum과 switch

```java
enum OrderStatus {
    READY, SHIPPING, DONE, CANCELLED
}

public class Main {
    public static void main(String[] args) {
        OrderStatus status = OrderStatus.SHIPPING;

        switch (status) {
            case READY -> System.out.println("준비 중");
            case SHIPPING -> System.out.println("배송 중");
            case DONE -> System.out.println("완료");
            case CANCELLED -> System.out.println("취소");
        }
    }
}
```

## 실수 포인트

- 추상 클래스와 인터페이스를 구분 없이 사용함
- 인터페이스를 단순 "문법"으로만 보고 설계 의미를 놓침
- enum 대신 문자열 상수 남발
- record를 상태가 자주 바뀌는 객체에 억지로 사용
- sealed 타입 사용 시 허용 목록을 잘못 구성

## 체크 문제

1. 추상 클래스와 인터페이스는 어떻게 다른가?
2. 인터페이스 기반 설계가 왜 유연한가?
3. enum은 문자열 상수보다 어떤 장점이 있는가?
4. record는 어떤 상황에 특히 잘 맞는가?

## 직접 해볼 과제

### 과제 1

`Movable` 인터페이스를 만들고 `Car`, `Robot` 클래스가 구현하게 하세요.

### 과제 2

요일 enum을 만들고 주말/평일 판별 메서드를 추가하세요.

### 과제 3

`record Product(String name, int price)`를 만들고 출력해 보세요.

## 오늘 정리

- 추상 클래스는 공통 기반, 인터페이스는 규약 설계에 강합니다.
- enum, record, sealed는 현대 Java에서 매우 중요한 타입 설계 도구입니다.
- 객체지향 설계는 "어떤 타입이 무엇을 약속하는가"를 표현하는 작업입니다.

