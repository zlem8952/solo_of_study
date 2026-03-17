# 5일차 - 클래스, 객체, 필드, 생성자

## 오늘의 목표

- 클래스와 객체의 차이를 이해한다.
- 필드와 메서드를 가지는 클래스를 직접 만들 수 있다.
- 생성자와 `this`의 역할을 익힌다.
- 인스턴스 멤버와 클래스 멤버의 감각을 잡는다.

## 클래스와 객체

### 클래스

설계도 또는 타입입니다.

### 객체

클래스를 바탕으로 실제 생성된 실체입니다.

예를 들어:

- 클래스: `Student`
- 객체: `new Student("Kim", 20)`

## 가장 기본적인 클래스

```java
class Student {
    String name;
    int age;

    void introduce() {
        System.out.println("이름: " + name + ", 나이: " + age);
    }
}
```

## 객체 생성

```java
Student s1 = new Student();
s1.name = "Kim";
s1.age = 20;
s1.introduce();
```

`new`는 객체를 생성하고 그 참조를 반환합니다.

## 필드(field)

객체가 가지는 데이터입니다.

```java
String name;
int age;
```

필드는 객체의 상태(state)를 표현합니다.

## 메서드(method)

객체가 수행할 수 있는 동작입니다.

```java
void study() {
    System.out.println(name + "이 공부합니다.");
}
```

## 객체 지향의 기본 사고

좋은 클래스는:

- 데이터(필드)
- 행동(메서드)

을 함께 묶습니다.

## 생성자(constructor)

객체를 만들 때 초기 상태를 설정하는 특별한 메서드입니다.

```java
class Student {
    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

특징:

- 클래스 이름과 같음
- 반환 타입 없음

## `this`

현재 객체 자신을 가리킵니다.

```java
this.name = name;
```

왼쪽 `this.name`은 필드, 오른쪽 `name`은 매개변수입니다.

## 생성자 오버로딩

```java
class Student {
    String name;
    int age;

    Student() {
        this("이름없음", 0);
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

## `this(...)`

같은 클래스의 다른 생성자를 호출합니다.

```java
Student() {
    this("기본값", 0);
}
```

생성자 첫 줄에만 올 수 있습니다.

## 예제: `Student` 클래스

```java
class Student {
    String name;
    int age;
    double score;

    Student(String name, int age, double score) {
        this.name = name;
        this.age = age;
        this.score = score;
    }

    void printInfo() {
        System.out.printf("%s %d %.1f%n", name, age, score);
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student("Kim", 20, 95.5);
        Student s2 = new Student("Lee", 22, 88.0);

        s1.printInfo();
        s2.printInfo();
    }
}
```

## 객체 참조와 `null`

```java
Student s = null;
```

객체를 가리키지 않는 상태입니다.

```java
s.printInfo();
```

이렇게 호출하면 `NullPointerException`이 발생합니다.

## 인스턴스 변수와 지역 변수

```java
class Counter {
    int value; // 인스턴스 변수

    void increase() {
        int step = 1; // 지역 변수
        value += step;
    }
}
```

차이:

- 인스턴스 변수는 객체 상태
- 지역 변수는 메서드 안에서만 사용

## 객체 배열

```java
Student[] students = new Student[3];
students[0] = new Student("Kim", 20, 90);
students[1] = new Student("Lee", 21, 85);
students[2] = new Student("Park", 19, 92);
```

주의:

```java
Student[] students = new Student[3];
System.out.println(students[0].name);
```

배열 칸에는 처음에 `null`이 들어 있으므로 바로 사용하면 오류입니다.

## 클래스 분리

실제 프로젝트에서는 보통 클래스를 파일별로 분리합니다.

`Student.java`

```java
public class Student {
    String name;
    int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        Student s = new Student("Kim", 20);
        System.out.println(s.name);
    }
}
```

## `toString` 미리 보기

객체를 출력할 때 사람이 읽기 좋은 문자열을 만들고 싶으면 `toString()`을 재정의합니다.

```java
class Student {
    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{name='" + name + "', age=" + age + "}";
    }
}
```

이 내용은 7일차에 더 자세히 다룹니다.

## 설계 감각

클래스를 만들 때 생각할 질문:

- 이 객체가 표현하는 대상은 무엇인가?
- 어떤 데이터가 필요한가?
- 어떤 동작을 제공해야 하는가?
- 어떤 필드는 외부에서 바로 바꾸면 위험한가?

이 마지막 질문이 6일차 캡슐화로 이어집니다.

## 실수 포인트

- 클래스와 객체를 같은 개념으로 생각함
- 객체 배열을 만들고 객체까지 자동 생성됐다고 착각함
- 생성자 이름을 클래스 이름과 다르게 쓰려 함
- `null` 상태에서 메서드를 호출함
- `this`의 의미를 헷갈림

## 체크 문제

1. 클래스와 객체의 차이는 무엇인가?
2. 생성자는 왜 필요한가?
3. `this.name = name`에서 `this`는 무엇을 의미하는가?
4. 객체 배열을 만들었을 때 내부 요소는 처음에 어떤 상태인가?

## 직접 해볼 과제

### 과제 1

`Book` 클래스를 만들고 제목, 저자, 가격 필드를 넣은 뒤 출력 메서드를 작성하세요.

### 과제 2

`Rectangle` 클래스를 만들어 너비와 높이로 넓이를 구하는 메서드를 작성하세요.

### 과제 3

학생 3명을 객체 배열에 저장하고 전체 정보를 출력하세요.

## 오늘 정리

- 클래스는 설계도, 객체는 실제 생성된 인스턴스입니다.
- 필드는 상태, 메서드는 동작을 표현합니다.
- 생성자와 `this`를 이해하면 Java 객체 생성의 기초가 잡힙니다.

