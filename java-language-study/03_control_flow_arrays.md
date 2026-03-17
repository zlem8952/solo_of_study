# 3일차 - 조건문, 반복문, 배열

## 오늘의 목표

- `if`, `switch`, 반복문을 사용할 수 있다.
- 배열의 기본 구조와 사용법을 이해한다.
- 향상된 `for`문과 다차원 배열을 익힌다.
- Java의 `switch expression`과 전통적인 `switch` 차이를 안다.

## 조건문 `if`

```java
if (score >= 90) {
    System.out.println("A");
}
```

## `if - else`

```java
if (score >= 60) {
    System.out.println("합격");
} else {
    System.out.println("불합격");
}
```

## `else if`

```java
if (score >= 90) {
    System.out.println("A");
} else if (score >= 80) {
    System.out.println("B");
} else if (score >= 70) {
    System.out.println("C");
} else {
    System.out.println("F");
}
```

## `switch` 문

```java
switch (menu) {
    case 1:
        System.out.println("조회");
        break;
    case 2:
        System.out.println("추가");
        break;
    default:
        System.out.println("잘못된 입력");
}
```

### `switch`에서 사용할 수 있는 타입

- `byte`, `short`, `char`, `int`
- 래퍼 타입 일부
- `String`
- `enum`

## `switch expression`

현대 Java에서는 값을 반환하는 `switch` 표현식도 자주 씁니다.

```java
String grade = switch (score / 10) {
    case 10, 9 -> "A";
    case 8 -> "B";
    case 7 -> "C";
    default -> "F";
};
```

장점:

- `break` 실수를 줄임
- 값을 반환하는 코드가 더 명확함

## 반복문 `for`

```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}
```

## 반복문 `while`

```java
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}
```

## 반복문 `do while`

```java
int i = 0;
do {
    System.out.println(i);
    i++;
} while (i < 5);
```

본문을 최소 1번은 실행합니다.

## `break`, `continue`

```java
for (int i = 1; i <= 10; i++) {
    if (i == 5) {
        break;
    }
    System.out.println(i);
}
```

```java
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
        continue;
    }
    System.out.println(i);
}
```

## 배열이란?

같은 타입의 값을 여러 개 저장하는 고정 크기 컨테이너입니다.

```java
int[] numbers = new int[5];
```

기본값:

- `int[]` -> `0`
- `double[]` -> `0.0`
- `boolean[]` -> `false`
- 참조형 배열 -> `null`

## 배열 선언 방식

```java
int[] a;
int b[];
```

둘 다 가능하지만 Java에서는 `int[] a` 형태를 더 많이 사용합니다.

## 배열 초기화

```java
int[] scores = {90, 80, 70, 60};
```

또는:

```java
int[] scores = new int[] {90, 80, 70, 60};
```

## 배열 길이

```java
System.out.println(scores.length);
```

배열은 `length` 필드를 가집니다. 메서드가 아닙니다.

## 인덱스

```java
System.out.println(scores[0]);
System.out.println(scores[1]);
```

배열은 0부터 시작합니다.

## 배열 범위 초과

```java
int[] arr = {1, 2, 3};
System.out.println(arr[3]);
```

실행 시 `ArrayIndexOutOfBoundsException`이 발생합니다.

## 향상된 `for`문

```java
for (int score : scores) {
    System.out.println(score);
}
```

장점:

- 읽기 쉬움
- 인덱스 필요 없는 반복에 적합

단점:

- 현재 인덱스를 직접 사용하기 어려움

## 다차원 배열

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6}
};
```

출력:

```java
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
```

Java의 2차원 배열은 사실 "배열의 배열"입니다. 각 행 길이가 달라도 됩니다.

```java
int[][] jagged = {
    {1, 2},
    {3, 4, 5},
    {6}
};
```

## 예제: 1부터 n까지 합

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int sum = 0;

        for (int i = 1; i <= n; i++) {
            sum += i;
        }

        System.out.println(sum);
    }
}
```

## 예제: 최댓값 찾기

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = {8, 3, 15, 2, 9};
        int max = arr[0];

        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }

        System.out.println(max);
    }
}
```

## 예제: 구구단

```java
public class Main {
    public static void main(String[] args) {
        for (int i = 2; i <= 9; i++) {
            for (int j = 1; j <= 9; j++) {
                System.out.printf("%d x %d = %d%n", i, j, i * j);
            }
            System.out.println();
        }
    }
}
```

## `Arrays` 유틸리티 미리 보기

배열은 표준 라이브러리 `java.util.Arrays`와 함께 자주 사용합니다.

```java
import java.util.Arrays;

int[] arr = {3, 1, 2};
Arrays.sort(arr);
System.out.println(Arrays.toString(arr));
```

이 내용은 10일차, 12일차에서 다시 다룹니다.

## 실수 포인트

- `switch`에서 `break`를 빠뜨림
- 배열 길이를 `length()`로 착각함
- 배열 인덱스를 1부터 시작한다고 생각함
- 반복문 종료 조건을 잘못 써 무한 루프를 만듦
- 향상된 `for`문에서 원본 배열 값을 바꾸려는 실수를 함

## 체크 문제

1. `if`와 `switch`는 각각 언제 더 적합한가?
2. 전통적인 `switch`와 `switch expression` 차이는 무엇인가?
3. 배열의 길이는 어떻게 구하는가?
4. Java의 2차원 배열이 "배열의 배열"이라는 말은 무슨 뜻인가?

## 직접 해볼 과제

### 과제 1

정수를 입력받아 홀수인지 짝수인지 출력하세요.

### 과제 2

배열에 10개의 정수를 저장하고 평균을 구하세요.

### 과제 3

별 삼각형, 역삼각형, 피라미드를 반복문으로 출력하세요.

## 오늘 정리

- 조건문과 반복문은 프로그램 흐름 제어의 핵심입니다.
- 배열은 같은 타입의 값을 고정 크기로 저장합니다.
- `switch expression`, 향상된 `for`문처럼 현대 Java 문법에도 익숙해져야 합니다.

