# 6일차 - 캡슐화, 접근 제어자, 패키지, `static`, `final`

## 오늘의 목표

- 캡슐화가 왜 중요한지 이해한다.
- 접근 제어자의 차이를 안다.
- 패키지와 `import`를 사용할 수 있다.
- `static`과 `final`의 역할을 구분할 수 있다.

## 캡슐화란?

객체의 내부 상태를 외부에서 무분별하게 건드리지 못하게 하고, 필요한 방식으로만 접근하게 만드는 설계 원칙입니다.

좋은 이유:

- 잘못된 값이 들어가는 것을 막을 수 있음
- 클래스 내부 구현을 바꿔도 외부 코드 영향이 줄어듦
- 유지보수가 쉬워짐

## 나쁜 예

```java
class BankAccount {
    public int balance;
}
```

외부에서:

```java
account.balance = -999999;
```

이런 식으로 잘못된 값이 들어갈 수 있습니다.

## 좋은 예

```java
class BankAccount {
    private int balance;

    public void deposit(int amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public int getBalance() {
        return balance;
    }
}
```

## 접근 제어자

| 제어자 | 같은 클래스 | 같은 패키지 | 자식 클래스 | 다른 패키지 |
| --- | --- | --- | --- | --- |
| `private` | O | X | X | X |
| 기본(package-private) | O | O | X | X |
| `protected` | O | O | O | 일부 가능 |
| `public` | O | O | O | O |

## `private`

해당 클래스 내부에서만 접근 가능

## `public`

어디서든 접근 가능

## package-private

아무 접근 제어자도 쓰지 않으면 같은 패키지에서만 접근 가능합니다.

```java
class Helper {
}
```

## `protected`

같은 패키지 + 상속 관계에서 접근 가능

초보 단계에서는 무분별하게 쓰기보다 `private`과 `public` 중심으로 먼저 익히는 것이 좋습니다.

## Getter / Setter

캡슐화와 함께 자주 나오는 패턴입니다.

```java
class Person {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        if (name != null && !name.isBlank()) {
            this.name = name;
        }
    }
}
```

중요:

- setter를 무조건 만들 필요는 없습니다.
- 읽기 전용이어야 한다면 getter만 두는 것이 더 좋을 수 있습니다.

## 불변 객체(immutable object) 기초

객체 생성 후 상태가 바뀌지 않도록 설계하는 방식입니다.

```java
final class User {
    private final String name;
    private final int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

장점:

- 상태 추적이 쉬움
- 동시성 환경에서 안전성이 높아짐

## 패키지(package)

관련 클래스를 묶는 이름 공간입니다.

```java
package com.example.school;
```

보통 파일 맨 위에 작성합니다.

디렉터리 구조 예시:

```text
src/com/example/school/Student.java
```

## `import`

다른 패키지의 클래스를 사용할 때 편의를 위해 가져옵니다.

```java
import java.util.Scanner;
import java.util.ArrayList;
```

또는:

```java
import java.util.*;
```

하지만 실무에서는 보통 필요한 것만 명시적으로 import하는 편이 읽기 쉽습니다.

## `static`

객체가 아니라 클래스 자체에 속하는 멤버입니다.

### static 필드

```java
class Counter {
    static int count = 0;

    Counter() {
        count++;
    }
}
```

모든 객체가 하나의 `count`를 공유합니다.

### static 메서드

```java
class MathUtil {
    static int add(int a, int b) {
        return a + b;
    }
}
```

호출:

```java
MathUtil.add(3, 4);
```

## static 메서드에서 주의할 점

```java
class Test {
    int value = 10;

    static void printValue() {
        System.out.println(value); // 오류
    }
}
```

`static` 메서드는 인스턴스 필드에 직접 접근할 수 없습니다.

## static 초기화 블록

```java
class Config {
    static int value;

    static {
        value = 100;
        System.out.println("static init");
    }
}
```

클래스가 처음 로딩될 때 한 번 실행됩니다.

## `final`

### final 변수

한 번만 대입 가능

```java
final int MAX = 100;
```

### final 메서드

오버라이딩 금지

```java
class Animal {
    public final void breathe() {
        System.out.println("숨쉰다");
    }
}
```

### final 클래스

상속 금지

```java
final class SecurityManagerExample {
}
```

## `static final`

상수에 자주 사용합니다.

```java
class Constants {
    public static final double PI = 3.141592;
}
```

관례:

- 상수명은 `UPPER_SNAKE_CASE`

## 예제: 계좌 클래스 캡슐화

```java
class Account {
    private String owner;
    private int balance;

    public Account(String owner, int balance) {
        this.owner = owner;
        this.balance = Math.max(balance, 0);
    }

    public void deposit(int amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public boolean withdraw(int amount) {
        if (amount > 0 && balance >= amount) {
            balance -= amount;
            return true;
        }
        return false;
    }

    public int getBalance() {
        return balance;
    }
}
```

## 예제: `static` 카운터

```java
class User {
    private static int count = 0;
    private final String name;

    public User(String name) {
        this.name = name;
        count++;
    }

    public static int getCount() {
        return count;
    }
}

public class Main {
    public static void main(String[] args) {
        new User("Kim");
        new User("Lee");
        System.out.println(User.getCount());
    }
}
```

## 실수 포인트

- 모든 필드를 `public`으로 열어 버림
- `static`과 인스턴스 멤버를 혼동함
- `final`을 "무조건 상수"라고만 이해함
- 패키지 구조와 파일 경로를 맞추지 않음
- getter/setter를 기계적으로 다 생성함

## 체크 문제

1. 캡슐화가 필요한 이유는 무엇인가?
2. `private`과 `public`의 차이는 무엇인가?
3. `static` 필드와 인스턴스 필드는 어떻게 다른가?
4. `final` 변수, 메서드, 클래스는 각각 어떤 의미인가?

## 직접 해볼 과제

### 과제 1

`Car` 클래스를 만들고 속도 필드는 `private`으로 숨긴 뒤 가속/감속 메서드로만 바꾸게 하세요.

### 과제 2

생성된 객체 수를 세는 `static` 카운터를 가진 클래스를 작성하세요.

### 과제 3

불변 객체 형태의 `Point` 클래스를 만들어 보세요.

## 오늘 정리

- 캡슐화는 객체를 안전하고 유지보수 가능하게 만드는 핵심 원칙입니다.
- 접근 제어자는 설계 의도를 코드에 담는 수단입니다.
- `static`은 클래스 소속, `final`은 변경 제한이라는 감각으로 이해하면 됩니다.

