# 7일차 - 상속, 다형성, `Object` 클래스

## 오늘의 목표

- 상속과 다형성의 핵심을 이해한다.
- 메서드 오버라이딩과 오버로딩 차이를 구분한다.
- 업캐스팅, 다운캐스팅, `instanceof`를 익힌다.
- `Object`의 주요 메서드(`toString`, `equals`, `hashCode`)를 이해한다.

## 상속이란?

기존 클래스의 특성을 이어받아 새로운 클래스를 만드는 것입니다.

```java
class Animal {
    void sound() {
        System.out.println("동물 소리");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("멍멍");
    }
}
```

## `extends`

```java
class Dog extends Animal {
}
```

의미:

- `Dog`는 `Animal`의 자식 클래스
- `Animal`은 부모(슈퍼) 클래스

## 상속의 장점

- 코드 재사용
- 공통 기능 추상화
- 다형성 기반 설계 가능

하지만 "코드 재사용이 편해 보인다"는 이유로 무조건 상속을 쓰면 안 됩니다. 관계가 진짜 `is-a`인지 생각해야 합니다.

## 오버라이딩(Overriding)

부모 클래스의 메서드를 자식 클래스에서 다시 정의하는 것입니다.

```java
class Animal {
    void sound() {
        System.out.println("동물 소리");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("멍멍");
    }
}
```

`@Override`는 꼭 붙이는 습관이 좋습니다. 실수한 경우 컴파일러가 잡아 줍니다.

## 오버로딩과 오버라이딩 차이

### 오버로딩

- 같은 이름
- 다른 매개변수
- 같은 클래스 안에서도 가능

### 오버라이딩

- 상속 관계
- 같은 시그니처
- 자식이 부모 메서드 재정의

## `super`

부모 클래스의 생성자나 멤버를 가리킵니다.

```java
class Animal {
    protected String name;

    Animal(String name) {
        this.name = name;
    }
}

class Dog extends Animal {
    Dog(String name) {
        super(name);
    }
}
```

## 생성자와 상속

자식 객체를 생성할 때 부모 생성자도 먼저 호출됩니다.

명시하지 않으면 기본적으로 `super()`가 호출됩니다.

## 다형성(Polymorphism)

부모 타입 참조로 자식 객체를 다룰 수 있는 성질입니다.

```java
Animal a = new Dog();
a.sound();
```

실행 결과는 실제 객체 타입인 `Dog`의 `sound()`를 따릅니다.

이것이 `동적 바인딩`입니다.

## 업캐스팅과 다운캐스팅

### 업캐스팅

```java
Animal a = new Dog();
```

자식 -> 부모

자동 변환 가능

### 다운캐스팅

```java
Dog d = (Dog) a;
```

부모 -> 자식

명시적 형 변환 필요

주의:

```java
Animal a = new Animal();
Dog d = (Dog) a; // 런타임 오류
```

`ClassCastException` 가능

## `instanceof`

안전한 타입 확인에 사용합니다.

```java
if (a instanceof Dog) {
    Dog d = (Dog) a;
    d.bark();
}
```

현대 Java에서는 패턴 매칭 형태도 사용할 수 있습니다.

```java
if (a instanceof Dog d) {
    d.bark();
}
```

## 상속보다 조합(Composition)

상속은 강한 결합을 만듭니다. 단순 재사용 목적이라면 조합이 더 좋은 경우가 많습니다.

예:

```java
class Engine {
    void start() { }
}

class Car {
    private Engine engine = new Engine();
}
```

`Car is-a Engine`은 아니지만 `Car has-a Engine`입니다.

## `Object` 클래스

Java의 모든 클래스는 결국 `Object`를 상속합니다.

주요 메서드:

- `toString()`
- `equals(Object obj)`
- `hashCode()`
- `getClass()`

## `toString`

객체를 문자열로 표현할 때 사용됩니다.

```java
class Student {
    private String name;

    Student(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Student{name='" + name + "'}";
    }
}
```

## `equals`

객체의 "내용상 같은가"를 정의할 때 재정의합니다.

```java
import java.util.Objects;

class Student {
    private String name;
    private int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Student other)) return false;
        return age == other.age && Objects.equals(name, other.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

## `hashCode`

`equals`를 재정의했다면 보통 `hashCode`도 함께 재정의해야 합니다.

이유:

- `HashSet`, `HashMap` 같은 컬렉션에서 일관성 유지

## 예제: 다형성 배열

```java
class Animal {
    void sound() {
        System.out.println("동물 소리");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("멍멍");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("야옹");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal[] animals = {new Dog(), new Cat()};

        for (Animal animal : animals) {
            animal.sound();
        }
    }
}
```

## `final`과 상속 제한

- `final class`는 상속 불가
- `final method`는 오버라이딩 불가

설계 의도를 강하게 고정하고 싶을 때 사용합니다.

## 실수 포인트

- 상속과 단순 코드 복붙 대체를 혼동함
- 오버라이딩인데 `@Override`를 안 붙여 실수를 놓침
- `equals`만 재정의하고 `hashCode`는 안 만듦
- 다운캐스팅을 무조건 안전하다고 생각함
- 모든 관계를 상속으로 모델링하려 함

## 체크 문제

1. 상속은 어떤 관계를 표현하는가?
2. 오버라이딩과 오버로딩은 어떻게 다른가?
3. 다형성이 왜 중요한가?
4. `equals`와 `hashCode`는 왜 함께 생각해야 하는가?

## 직접 해볼 과제

### 과제 1

`Shape` 부모 클래스와 `Circle`, `Rectangle` 자식 클래스를 만들고 넓이 메서드를 오버라이딩하세요.

### 과제 2

`Person` 클래스를 만들고 `toString`, `equals`, `hashCode`를 재정의하세요.

### 과제 3

동물 배열에 여러 자식 객체를 담고 공통 메서드를 호출하는 예제를 작성하세요.

## 오늘 정리

- 상속은 공통 특성을 확장하는 도구입니다.
- 다형성은 Java 설계의 핵심입니다.
- `Object` 메서드를 올바르게 재정의하면 컬렉션과 비교 동작이 안정됩니다.

