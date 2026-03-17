# 1일차 - Java 개요와 개발 환경, 첫 프로그램

## 오늘의 목표

- Java가 어떤 언어인지 이해한다.
- `JDK`, `JRE`, `JVM`의 차이를 안다.
- 소스 코드가 컴파일되고 실행되는 흐름을 이해한다.
- 가장 기본적인 Java 프로그램 구조를 익힌다.

## Java란?

Java는 객체지향 중심의 범용 프로그래밍 언어입니다. 웹 백엔드, 기업 시스템, 안드로이드, 금융 시스템, 서버 애플리케이션, 데이터 처리 등 매우 넓은 분야에서 사용됩니다.

### Java의 특징

- 플랫폼 독립성을 강하게 추구합니다.
- 풍부한 표준 라이브러리를 제공합니다.
- 가비지 컬렉션으로 메모리 관리를 자동화합니다.
- 객체지향 설계와 대규모 프로젝트 구조화에 강합니다.
- 동시성 도구가 잘 갖춰져 있습니다.

## Write Once, Run Anywhere

Java는 소스 코드를 바로 기계어로 실행하는 것이 아니라, 먼저 `바이트코드(bytecode)`로 컴파일한 뒤 JVM이 실행합니다.

흐름:

1. `.java` 소스 작성
2. `javac`로 `.class` 바이트코드 생성
3. `java` 명령으로 JVM 위에서 실행

## JDK, JRE, JVM 차이

### JVM

- Java Virtual Machine
- 바이트코드를 실행하는 가상 머신

### JRE

- Java Runtime Environment
- JVM + Java 실행에 필요한 라이브러리

### JDK

- Java Development Kit
- JRE + 컴파일러(`javac`) + 개발 도구

정리하면:

- 실행만 하려면 JRE 개념이 중요
- 개발하려면 JDK가 필요

## 첫 프로그램

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, Java!");
    }
}
```

## 코드 해설

### `public class Main`

- `Main`이라는 클래스를 정의합니다.
- 파일 이름은 보통 `Main.java`여야 합니다.
- `public` 클래스 이름과 파일 이름은 맞춰야 합니다.

### `public static void main(String[] args)`

Java 프로그램의 시작점입니다.

각 부분 의미:

- `public`: 어디서든 접근 가능
- `static`: 객체를 만들지 않아도 실행 가능
- `void`: 반환값 없음
- `main`: 시작 메서드 이름
- `String[] args`: 명령행 인자 배열

### `System.out.println(...)`

- 콘솔에 문자열을 출력합니다.
- 끝에 자동으로 줄바꿈이 들어갑니다.

## 실행 방법

```bash
javac Main.java
java Main
```

결과:

```text
Hello, Java!
```

## Java 파일 구조 감각

아주 간단한 프로그램은 클래스 하나로 시작합니다.

```java
public class Main {
    public static void main(String[] args) {
        int x = 10;
        System.out.println(x);
    }
}
```

하지만 프로젝트가 커지면:

- 패키지
- 여러 클래스
- 여러 소스 파일
- 테스트 코드

로 나뉘게 됩니다.

## 주석

### 한 줄 주석

```java
// 한 줄 주석
```

### 여러 줄 주석

```java
/*
 여러 줄 주석
*/
```

### 문서 주석

```java
/**
 * 메서드나 클래스 설명
 */
```

이 형태는 Javadoc 문서 생성에 사용됩니다.

## Java의 문장과 블록

Java 문장은 보통 세미콜론 `;`으로 끝납니다.

```java
int age = 20;
System.out.println(age);
```

블록은 중괄호로 묶습니다.

```java
if (true) {
    System.out.println("yes");
}
```

## 출력 메서드 차이

```java
System.out.print("Hello");
System.out.print("Java");
```

줄바꿈 없음

```java
System.out.println("Hello");
System.out.println("Java");
```

줄바꿈 있음

```java
System.out.printf("name=%s age=%d%n", "Kim", 20);
```

형식 지정 출력

## 명명 규칙

Java는 관례가 매우 중요합니다.

- 클래스명: `PascalCase`
- 메서드/변수명: `camelCase`
- 상수명: `UPPER_SNAKE_CASE`
- 패키지명: 모두 소문자

예:

```java
class StudentManager
int studentCount
final int MAX_SIZE
package com.example.app;
```

## 패키지 개념 미리 보기

```java
package com.example.app;
```

- 클래스들을 논리적으로 묶는 이름 공간입니다.
- 파일 경로 구조와 연결됩니다.

패키지는 6일차, 18일차에서 더 자세히 다룹니다.

## Java의 특징적인 점

### 1. 모든 코드는 클래스 안에 있다

Java는 C처럼 함수만 따로 존재하지 않고, 메서드가 클래스 안에 들어갑니다.

### 2. 자동 메모리 관리

객체 생성 후 더 이상 참조되지 않으면 가비지 컬렉터가 회수합니다.

### 3. 강한 타입 검사

컴파일 단계에서 많은 오류를 잡아 줍니다.

## JShell

Java에는 간단한 실험을 위한 REPL 도구 `jshell`이 있습니다.

```bash
jshell
```

안에서:

```java
int x = 10;
System.out.println(x * 2);
```

초반 문법 실험에 매우 유용합니다.

## 흔한 컴파일 오류

### 파일 이름과 `public class` 이름 불일치

```java
public class Main {
}
```

파일명이 `Test.java`이면 오류

### 세미콜론 누락

```java
int x = 10
```

### 중괄호 불일치

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello");
}
```

## 예제: 자기소개 출력

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("이름: Kim");
        System.out.println("나이: 23");
        System.out.println("목표: Java 정복");
    }
}
```

## 체크 문제

1. `JDK`, `JRE`, `JVM`은 각각 무엇인가?
2. Java 프로그램이 실행되기까지 어떤 단계를 거치는가?
3. `main` 메서드의 형태는 왜 특별한가?
4. `print`, `println`, `printf`의 차이는 무엇인가?

## 직접 해볼 과제

### 과제 1

자신의 이름, 학교, 목표를 한 줄씩 출력하는 프로그램을 작성하세요.

### 과제 2

`printf`를 사용해 아래 형식으로 출력하세요.

```text
name = Kim
age = 20
height = 175.3
```

### 과제 3

`jshell`에서 정수 변수 두 개를 만들고 합을 출력해 보세요.

## 오늘 정리

- Java는 JVM 위에서 동작하는 객체지향 언어입니다.
- 개발에는 JDK가 필요합니다.
- 소스는 바이트코드로 컴파일된 뒤 JVM에서 실행됩니다.
- Java 학습의 시작은 `클래스`, `main`, `출력` 구조를 익히는 것입니다.

