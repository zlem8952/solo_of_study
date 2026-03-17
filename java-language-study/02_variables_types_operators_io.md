# 2일차 - 변수, 자료형, 연산자, 기본 입출력

## 오늘의 목표

- Java의 기본 자료형과 참조형 차이를 이해한다.
- 변수를 선언하고 초기화할 수 있다.
- 리터럴, 형 변환, 연산자를 익힌다.
- `Scanner`를 이용한 기본 입력을 배운다.

## 변수란?

값을 저장하기 위한 이름 붙은 공간입니다.

```java
int age = 20;
double height = 175.4;
char grade = 'A';
```

Java는 변수 타입이 명확해야 합니다.

## 기본 자료형(Primitive Type)

| 타입 | 크기 | 예시 |
| --- | --- | --- |
| `byte` | 1바이트 | `10` |
| `short` | 2바이트 | `300` |
| `int` | 4바이트 | `1000` |
| `long` | 8바이트 | `10000000000L` |
| `float` | 4바이트 | `3.14f` |
| `double` | 8바이트 | `3.141592` |
| `char` | 2바이트 | `'A'` |
| `boolean` | JVM 구현상 논리값 | `true`, `false` |

## 참조형(Reference Type)

기본형이 아닌 모든 타입은 참조형입니다.

예:

- `String`
- 배열
- 클래스 객체
- 인터페이스 타입

```java
String name = "Kim";
int[] numbers = {1, 2, 3};
```

참조형 변수는 실제 데이터 자체보다 `객체를 가리키는 참조`를 저장한다고 이해하면 좋습니다.

## 변수 선언과 초기화

```java
int a;
a = 10;

int b = 20;
```

Java는 지역 변수를 사용하기 전에 반드시 초기화해야 합니다.

```java
public class Main {
    public static void main(String[] args) {
        int x;
        System.out.println(x); // 컴파일 오류
    }
}
```

C와 달리 초기화되지 않은 지역 변수 사용을 컴파일 단계에서 막습니다.

## 리터럴

```java
10
3.14
'A'
"Hello"
true
```

### 정수 리터럴

```java
int decimal = 10;
int binary = 0b1010;
int hex = 0x1A;
int octal = 012;
```

### 큰 수 가독성

```java
int money = 1_000_000;
```

밑줄은 숫자 가독성을 높이기 위한 문법입니다.

## 상수 `final`

```java
final double PI = 3.141592;
```

한 번 대입하면 다시 바꿀 수 없습니다.

```java
PI = 4.0; // 오류
```

## `var`

Java 10 이후에는 지역 변수에 한해 타입 추론을 사용할 수 있습니다.

```java
var name = "Java";
var score = 95;
```

주의:

- 지역 변수에서만 사용
- 선언과 동시에 초기화 필요
- 코드가 더 읽기 어려워지면 남용하지 않는 것이 좋음

## 형 변환

### 자동 형 변환

```java
int a = 10;
double b = a;
```

작은 범위에서 큰 범위로는 자동 변환이 가능합니다.

### 명시적 형 변환

```java
double x = 3.9;
int y = (int) x;
```

`y`는 3이 됩니다.

## 오버플로 주의

```java
int max = Integer.MAX_VALUE;
System.out.println(max + 1);
```

Java의 정수는 고정 크기이므로 범위를 넘으면 값이 순환할 수 있습니다. C와 달리 정의되지 않은 동작은 아니지만, 여전히 버그입니다.

## 연산자

### 산술 연산자

```java
+  -  *  /  %
```

예:

```java
int a = 7;
int b = 3;
System.out.println(a + b);
System.out.println(a / b); // 2
System.out.println(a % b); // 1
```

## 정수 나눗셈

```java
System.out.println(7 / 2);     // 3
System.out.println(7 / 2.0);   // 3.5
```

## 비교 연산자

```java
== != > < >= <=
```

## 논리 연산자

```java
&& || !
```

단축 평가(short-circuit)가 적용됩니다.

```java
if (obj != null && obj.toString().length() > 0) {
    ...
}
```

## 대입과 증감

```java
= += -= *= /= %=
++ --
```

```java
int x = 5;
System.out.println(++x); // 6
System.out.println(x++); // 6 출력 후 x는 7
```

복잡한 식에서 증감 연산자 남용은 피하는 편이 좋습니다.

## 문자열 결합

```java
String name = "Kim";
int age = 20;
System.out.println(name + " " + age);
```

문자열과 숫자를 `+`로 결합할 수 있습니다.

## 기본 출력

```java
System.out.println("Hello");
System.out.print("Java");
System.out.printf("score = %.2f%n", 95.5);
```

`%n`은 운영체제에 맞는 줄바꿈입니다.

## `Scanner`로 입력받기

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int age = sc.nextInt();
        double score = sc.nextDouble();

        System.out.println(age);
        System.out.println(score);
    }
}
```

## `Scanner` 주요 메서드

| 메서드 | 의미 |
| --- | --- |
| `nextInt()` | 정수 입력 |
| `nextLong()` | long 입력 |
| `nextDouble()` | 실수 입력 |
| `next()` | 공백 전까지 문자열 |
| `nextLine()` | 한 줄 전체 문자열 |
| `nextBoolean()` | boolean 입력 |

## `nextLine()` 주의

```java
int age = sc.nextInt();
String line = sc.nextLine();
```

이 경우 `nextInt()` 뒤 줄바꿈이 남아 있어 `line`이 빈 문자열이 될 수 있습니다.

보통:

```java
int age = sc.nextInt();
sc.nextLine();
String line = sc.nextLine();
```

처럼 중간 줄바꿈을 소비해야 합니다.

## 문자열과 문자의 차이

```java
char ch = 'A';
String str = "A";
```

- `char`는 문자 하나
- `String`은 문자열 객체

## `==`와 `equals` 미리 보기

기본형 비교는 `==`로 하지만, 문자열/객체 내용 비교는 보통 `equals`를 사용합니다.

```java
String a = "hi";
String b = new String("hi");

System.out.println(a == b);      // 참조 비교
System.out.println(a.equals(b)); // 내용 비교
```

이 내용은 7일차, 10일차에서 더 자세히 다룹니다.

## 예제: 원의 넓이 구하기

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        final double PI = 3.141592;

        System.out.print("반지름: ");
        double radius = sc.nextDouble();

        System.out.printf("넓이 = %.2f%n", PI * radius * radius);
    }
}
```

## 예제: 성적 평균

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int kor = sc.nextInt();
        int eng = sc.nextInt();
        int math = sc.nextInt();

        double avg = (kor + eng + math) / 3.0;
        System.out.printf("평균 = %.2f%n", avg);
    }
}
```

## 실수 포인트

- `long` 리터럴에 `L`을 안 붙임
- `float` 리터럴에 `f`를 안 붙임
- 정수 나눗셈을 실수처럼 기대함
- `Scanner`에서 `nextLine()` 줄바꿈 문제를 놓침
- 문자열 비교를 `==`로 해버림

## 체크 문제

1. 기본형과 참조형의 차이는 무엇인가?
2. `final`은 어떤 의미인가?
3. `7 / 2`와 `7 / 2.0`의 결과는 왜 다른가?
4. `Scanner`에서 `next()`와 `nextLine()`은 어떻게 다른가?

## 직접 해볼 과제

### 과제 1

두 정수를 입력받아 합, 차, 곱, 몫, 나머지를 출력하세요.

### 과제 2

이름과 나이를 입력받아 `printf`로 출력하세요.

### 과제 3

섭씨를 입력받아 화씨로 바꾸는 프로그램을 작성하세요.

## 오늘 정리

- Java는 기본형과 참조형을 구분합니다.
- 변수는 타입이 명확해야 하며, 지역 변수는 반드시 초기화해야 합니다.
- `Scanner`와 출력 메서드를 익히면 간단한 콘솔 프로그램을 만들 수 있습니다.

