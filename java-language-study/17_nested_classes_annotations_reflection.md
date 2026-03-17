# 17일차 - 중첩 클래스, 애너테이션, 리플렉션

## 오늘의 목표

- 중첩 클래스의 종류를 구분한다.
- Java 애너테이션의 역할을 이해한다.
- 커스텀 애너테이션을 만들 수 있다.
- 리플렉션으로 클래스 정보를 읽고 메서드를 호출하는 기본 감각을 잡는다.

## 중첩 클래스란?

클래스 안에 또 다른 클래스를 정의하는 방식입니다.

종류:

- 정적 중첩 클래스(static nested class)
- 내부 클래스(inner class)
- 지역 클래스(local class)
- 익명 클래스(anonymous class)

## 정적 중첩 클래스

```java
class Outer {
    static class Nested {
        void hello() {
            System.out.println("nested");
        }
    }
}
```

사용:

```java
Outer.Nested n = new Outer.Nested();
```

외부 객체 없이 생성 가능합니다.

## 내부 클래스

```java
class Outer {
    class Inner {
        void hello() {
            System.out.println("inner");
        }
    }
}
```

사용:

```java
Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();
```

외부 객체와 연결됩니다.

## 지역 클래스

메서드 안에서 정의하는 클래스입니다.

```java
void work() {
    class LocalHelper {
        void run() {
            System.out.println("local");
        }
    }
}
```

## 익명 클래스

이름 없는 일회성 구현체입니다.

```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("anonymous");
    }
};
```

람다가 더 적합한 경우도 많지만, 여전히 익명 클래스가 필요한 상황이 있습니다.

## 애너테이션(Annotation)

코드에 메타데이터를 붙이는 문법입니다.

자주 보는 예:

- `@Override`
- `@Deprecated`
- `@SuppressWarnings`
- `@FunctionalInterface`

## `@Override`

오버라이딩 실수를 방지합니다.

## `@Deprecated`

더 이상 권장되지 않는 API에 표시합니다.

## 커스텀 애너테이션 만들기

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@interface MyMarker {
    String value();
}
```

## `@Retention`

애너테이션이 언제까지 유지되는지 지정합니다.

- `SOURCE`
- `CLASS`
- `RUNTIME`

리플렉션으로 읽으려면 보통 `RUNTIME`이 필요합니다.

## `@Target`

애너테이션을 어디에 붙일 수 있는지 지정합니다.

예:

- `TYPE`
- `METHOD`
- `FIELD`
- `PARAMETER`

## 리플렉션(Reflection)

실행 중 클래스 구조를 조사하고, 필드/메서드를 다룰 수 있는 기능입니다.

대표 클래스:

- `Class`
- `Field`
- `Method`
- `Constructor`

## 클래스 정보 얻기

```java
Class<?> clazz = String.class;
System.out.println(clazz.getName());
System.out.println(clazz.getSimpleName());
```

또는:

```java
Class<?> clazz = Class.forName("java.lang.String");
```

## 메서드 조회

```java
import java.lang.reflect.Method;

Method[] methods = String.class.getDeclaredMethods();
for (Method method : methods) {
    System.out.println(method.getName());
}
```

## 리플렉션으로 메서드 호출

```java
import java.lang.reflect.Method;

public class Main {
    public static void main(String[] args) throws Exception {
        String text = "java";
        Method method = String.class.getMethod("toUpperCase");
        Object result = method.invoke(text);
        System.out.println(result);
    }
}
```

## 필드 접근

```java
import java.lang.reflect.Field;

class Person {
    private String name = "Kim";
}

public class Main {
    public static void main(String[] args) throws Exception {
        Person p = new Person();
        Field field = Person.class.getDeclaredField("name");
        field.setAccessible(true);
        System.out.println(field.get(p));
    }
}
```

주의:

- 캡슐화를 깨뜨릴 수 있음
- 프레임워크 개발 등 특별한 상황에서 주로 사용

## 커스텀 애너테이션 읽기 예제

```java
import java.lang.reflect.Method;

class Demo {
    @MyMarker("hello")
    public void test() {
    }
}

public class Main {
    public static void main(String[] args) throws Exception {
        Method method = Demo.class.getMethod("test");
        MyMarker marker = method.getAnnotation(MyMarker.class);
        System.out.println(marker.value());
    }
}
```

## 언제 리플렉션을 쓰는가?

- 프레임워크
- DI 컨테이너
- 직렬화/매핑
- 테스트 도구
- 런타임 메타데이터 처리

일반 애플리케이션 로직에서는 과용하지 않는 것이 좋습니다.

## 실수 포인트

- 중첩 클래스 종류를 구분하지 못함
- 애너테이션을 단순 주석처럼 생각함
- `RetentionPolicy.RUNTIME`이 필요한 상황을 놓침
- 리플렉션으로 private 접근을 남용함
- 리플렉션 예외 처리를 무시함

## 체크 문제

1. 정적 중첩 클래스와 내부 클래스 차이는 무엇인가?
2. 애너테이션은 왜 필요한가?
3. 커스텀 애너테이션을 리플렉션으로 읽으려면 무엇이 필요한가?
4. 리플렉션은 언제 특히 유용한가?

## 직접 해볼 과제

### 과제 1

정적 중첩 클래스와 내부 클래스 예제를 각각 작성해 보세요.

### 과제 2

메서드에 붙는 커스텀 애너테이션을 만들고 읽어 보세요.

### 과제 3

리플렉션으로 특정 클래스의 메서드 이름 목록을 출력하세요.

## 오늘 정리

- 중첩 클래스는 구조화와 스코프 관리에 유용합니다.
- 애너테이션은 메타데이터 표현의 핵심입니다.
- 리플렉션은 강력하지만, 유지보수성과 안전성을 고려해 신중하게 써야 합니다.

