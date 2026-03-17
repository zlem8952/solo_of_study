# 7일차 - 함수, 재귀, 스코프, 저장 기간

## 오늘의 목표

- 함수를 선언, 정의, 호출할 수 있다.
- 함수 프로토타입의 필요성을 이해한다.
- 값 전달과 주소 전달의 차이를 다시 정리한다.
- 스코프, 수명, 저장 클래스, `static`, `extern`의 개념을 이해한다.

## 함수란?

함수는 `특정 작업을 수행하는 코드 묶음`입니다. 코드를 재사용하고, 문제를 작게 나누고, 테스트하기 쉽게 만들어 줍니다.

## 함수의 기본 구조

```c
반환형 함수이름(매개변수목록)
{
    함수 본문
}
```

예:

```c
int add(int a, int b)
{
    return a + b;
}
```

## 함수 선언과 정의

### 선언(프로토타입)

```c
int add(int a, int b);
```

### 정의

```c
int add(int a, int b)
{
    return a + b;
}
```

선언은 컴파일러에게 "이런 함수가 있다"라고 먼저 알려주는 역할을 합니다.

## 함수 호출

```c
int result = add(3, 4);
```

## 반환형 `void`

값을 반환하지 않는 함수는 `void`를 사용합니다.

```c
void print_line(void)
{
    printf("---------------\n");
}
```

## 매개변수와 인자

- 매개변수(parameter): 함수 정의 쪽 변수
- 인자(argument): 실제 호출할 때 넘기는 값

```c
int add(int a, int b) // a, b는 매개변수
{
    return a + b;
}

int main(void)
{
    int result = add(10, 20); // 10, 20은 인자
    return 0;
}
```

## 값 전달(call by value)

C는 기본적으로 `값 전달`입니다.

```c
void increase(int x)
{
    x++;
}
```

원본은 바뀌지 않습니다.

```c
int n = 5;
increase(n);
printf("%d\n", n); // 5
```

원본을 바꾸려면 포인터를 써야 합니다.

## 함수 분리 예제

```c
#include <stdio.h>

int max_of_two(int a, int b)
{
    if (a > b)
    {
        return a;
    }
    return b;
}

int main(void)
{
    printf("%d\n", max_of_two(7, 12));
    return 0;
}
```

## 함수 원형이 필요한 이유

```c
#include <stdio.h>

int main(void)
{
    printf("%d\n", square(5));
    return 0;
}

int square(int x)
{
    return x * x;
}
```

위 코드는 함수 선언이 없으면 컴파일러가 미리 알지 못해 문제가 될 수 있습니다. 그래서 보통 위쪽에 프로토타입을 둡니다.

```c
int square(int x);
```

## 재귀 함수

함수가 자기 자신을 호출하는 방식입니다.

### 팩토리얼

```c
int factorial(int n)
{
    if (n <= 1)
    {
        return 1;
    }
    return n * factorial(n - 1);
}
```

### 재귀의 핵심 두 가지

1. 종료 조건(base case)
2. 문제를 더 작은 형태로 줄이는 재귀 단계

종료 조건이 없으면 무한 재귀로 스택이 넘칠 수 있습니다.

## 지역 변수와 전역 변수

### 지역 변수

함수 안에서 선언되며, 그 블록 안에서만 보입니다.

```c
void func(void)
{
    int x = 10;
}
```

### 전역 변수

함수 밖에서 선언되며, 여러 함수에서 접근할 수 있습니다.

```c
int g_count = 0;
```

전역 변수는 편리하지만 의존성이 커지고 디버깅이 어려워질 수 있어 남용을 피해야 합니다.

## 스코프(scope)

이름이 보이는 범위입니다.

### 블록 스코프

```c
if (1)
{
    int x = 10;
}
```

`x`는 블록 밖에서 보이지 않습니다.

### 파일 스코프

함수 밖에서 선언한 식별자는 파일 전체에서 보입니다.

## 저장 기간(storage duration)

객체가 메모리에 존재하는 기간입니다.

### 자동 저장 기간

일반 지역 변수는 블록에 들어올 때 생성되고, 블록을 벗어나면 사라집니다.

```c
void func(void)
{
    int x = 10;
}
```

### 정적 저장 기간

프로그램 시작부터 종료까지 존재합니다.

- 전역 변수
- `static` 지역 변수

## `static` 지역 변수

```c
#include <stdio.h>

void counter(void)
{
    static int count = 0;
    count++;
    printf("%d\n", count);
}

int main(void)
{
    counter(); // 1
    counter(); // 2
    counter(); // 3
    return 0;
}
```

특징:

- 선언 위치는 함수 안
- 하지만 수명은 프로그램 종료까지 유지
- 초기화는 한 번만 수행

## `static` 전역 변수 / 함수

파일 밖으로 노출되지 않게 제한할 수 있습니다.

```c
static int internal_value = 10;

static void helper(void)
{
    ...
}
```

이것은 `이 파일 내부에서만 사용`하겠다는 의미입니다.

## `extern`

다른 파일에 정의된 전역 변수나 함수를 참조할 때 사용합니다.

파일 `a.c`:

```c
int g_value = 100;
```

파일 `b.c`:

```c
extern int g_value;
```

## 함수와 배열 인자

```c
int sum(int arr[], int size)
{
    int total = 0;
    for (int i = 0; i < size; i++)
    {
        total += arr[i];
    }
    return total;
}
```

`int arr[]`는 함수 인자에서 사실상 `int *arr`와 비슷하게 동작합니다.

## 함수 포인터 기초

함수의 주소를 저장하는 포인터입니다.

```c
int add(int a, int b)
{
    return a + b;
}

int (*fp)(int, int) = add;
printf("%d\n", fp(3, 4));
```

함수 포인터는 콜백 패턴, 정렬 비교 함수 등에서 유용합니다.

## 예제: 재귀로 거듭제곱

```c
int power(int base, int exp)
{
    if (exp == 0)
    {
        return 1;
    }
    return base * power(base, exp - 1);
}
```

## 예제: 배열 평균 함수

```c
#include <stdio.h>

double average(const int arr[], int size)
{
    int sum = 0;

    for (int i = 0; i < size; i++)
    {
        sum += arr[i];
    }

    return (double)sum / size;
}

int main(void)
{
    int scores[] = {90, 80, 70, 100};
    printf("%.2f\n", average(scores, 4));
    return 0;
}
```

## 실수 포인트

- 함수 선언 없이 먼저 호출함
- 지역 변수 주소를 반환함
- 재귀 종료 조건을 빼먹음
- 전역 변수를 너무 많이 사용함
- `static`과 `extern`의 역할을 혼동함

### 특히 위험한 코드

```c
int *bad_function(void)
{
    int x = 10;
    return &x; // 위험
}
```

지역 변수 `x`는 함수 종료와 함께 사라지므로 그 주소를 반환하면 안 됩니다.

## 체크 문제

1. 함수 프로토타입은 왜 필요한가?
2. C가 기본적으로 값 전달이라는 말은 무슨 뜻인가?
3. `static` 지역 변수와 일반 지역 변수의 차이는 무엇인가?
4. `extern`은 언제 사용하는가?

## 직접 해볼 과제

### 과제 1

정수 배열의 최솟값을 반환하는 함수를 작성하세요.

### 과제 2

재귀로 피보나치 수열의 n번째 값을 구하는 함수를 작성하세요.

### 과제 3

`static` 지역 변수를 사용해 호출 횟수를 세는 함수를 작성하세요.

## 오늘 정리

- 함수는 코드를 재사용하고 구조화하는 핵심 도구입니다.
- C는 기본적으로 값 전달입니다.
- 스코프는 `보이는 범위`, 저장 기간은 `살아 있는 시간`입니다.
- `static`, `extern`은 분할 컴파일에서 매우 중요합니다.

