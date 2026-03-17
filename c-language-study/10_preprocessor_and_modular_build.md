# 10일차 - 전처리기, 헤더 파일, 분할 컴파일

## 오늘의 목표

- 전처리기가 무엇을 하는지 이해한다.
- `#include`, `#define`, 조건부 컴파일을 사용할 수 있다.
- 헤더 파일과 소스 파일을 나누는 이유를 이해한다.
- include guard와 `extern` 선언의 역할을 안다.

## 전처리기란?

컴파일 전에 소스 코드를 가공하는 단계입니다.

대표 지시문:

- `#include`
- `#define`
- `#undef`
- `#if`, `#ifdef`, `#ifndef`, `#else`, `#elif`, `#endif`

전처리기는 문법 수준의 함수 호출이 아니라 `텍스트 치환`에 가깝습니다.

## `#include`

다른 파일의 내용을 현재 위치에 끼워 넣습니다.

```c
#include <stdio.h>
#include "myheader.h"
```

차이:

- `<...>`: 시스템 헤더 탐색
- `"..."`: 현재 프로젝트 기준으로 먼저 탐색

## `#define`

### 매크로 상수

```c
#define PI 3.1415926535
#define MAX_SIZE 100
```

주의:

- 타입이 없습니다.
- 단순 치환입니다.

## 함수형 매크로

```c
#define SQUARE(x) ((x) * (x))
```

괄호를 충분히 써야 예기치 않은 우선순위 문제를 피할 수 있습니다.

잘못된 예:

```c
#define SQUARE(x) x * x
```

`SQUARE(1 + 2)`는 `1 + 2 * 1 + 2`처럼 치환될 수 있습니다.

## 매크로의 부작용

```c
#define MAX(a, b) ((a) > (b) ? (a) : (b))
```

이 매크로에 `MAX(i++, j++)`처럼 넣으면 인자가 여러 번 평가될 수 있어 위험합니다.

가능하면 함수 또는 `inline` 함수를 고려하는 것이 좋습니다.

## 조건부 컴파일

```c
#ifdef DEBUG
printf("debug mode\n");
#endif
```

또는:

```c
#if VERSION >= 2
...
#else
...
#endif
```

## include guard

헤더가 여러 번 포함되어 재정의 문제가 생기는 것을 막습니다.

```c
#ifndef MY_MATH_H
#define MY_MATH_H

int add(int a, int b);

#endif
```

## 헤더와 소스 파일 분리

### 예시 구조

```text
main.c
math_utils.c
math_utils.h
```

### `math_utils.h`

```c
#ifndef MATH_UTILS_H
#define MATH_UTILS_H

int add(int a, int b);
int sub(int a, int b);

#endif
```

### `math_utils.c`

```c
#include "math_utils.h"

int add(int a, int b)
{
    return a + b;
}

int sub(int a, int b)
{
    return a - b;
}
```

### `main.c`

```c
#include <stdio.h>
#include "math_utils.h"

int main(void)
{
    printf("%d\n", add(3, 4));
    printf("%d\n", sub(7, 2));
    return 0;
}
```

컴파일:

```bash
gcc -std=c11 -Wall -Wextra -pedantic main.c math_utils.c -o app
```

## 헤더에 넣을 것 / 넣지 말아야 할 것

### 헤더에 보통 넣는 것

- 함수 선언
- 타입 선언(`struct`, `enum`, `typedef`)
- 필요한 매크로
- 외부에 공개할 상수

### 헤더에 함부로 넣으면 안 되는 것

- 전역 변수의 실제 정의
- 큰 함수 구현
- 불필요한 내부 전용 선언

전역 변수는 헤더에 보통 선언만:

```c
extern int g_count;
```

실제 정의는 `.c` 파일 하나에서만:

```c
int g_count = 0;
```

## `static`과 파일 내부 은닉

`.c` 파일 내부에서만 쓰는 함수는 `static`으로 숨길 수 있습니다.

```c
static int helper(int x)
{
    return x * 2;
}
```

이렇게 하면 다른 파일에서 보이지 않습니다.

## 매크로와 상수의 선택

```c
#define BUFFER_SIZE 256
```

또는

```c
const int buffer_size = 256;
```

선택 기준:

- 전처리 조건 등에 필요하면 매크로
- 타입 안정성과 디버깅 편의가 중요하면 `const`

## 자주 쓰는 전처리 매크로

| 매크로 | 의미 |
| --- | --- |
| `__FILE__` | 현재 파일명 |
| `__LINE__` | 현재 줄 번호 |
| `__DATE__` | 컴파일 날짜 |
| `__TIME__` | 컴파일 시간 |

예:

```c
printf("error at %s:%d\n", __FILE__, __LINE__);
```

## 조건부 디버그 로그 예제

```c
#include <stdio.h>

#ifdef DEBUG
#define LOG(fmt, ...) printf("[DEBUG] " fmt "\n", __VA_ARGS__)
#else
#define LOG(fmt, ...) ((void)0)
#endif

int main(void)
{
    int x = 42;
    LOG("x = %d", x);
    return 0;
}
```

주의:

- 가변 매크로는 매우 편리하지만 복잡해질수록 디버깅이 어려워집니다.

## 빌드 흐름 감각

분할 컴파일에서는 보통:

1. 각 `.c` 파일이 개별적으로 컴파일됨
2. 헤더를 통해 선언 정보를 공유함
3. 마지막에 링크되어 하나의 실행 파일이 됨

그래서 헤더와 구현의 역할을 구분하는 습관이 중요합니다.

## 헤더 설계 기본 원칙

- 헤더는 가능한 작고 명확하게 유지
- 필요한 선언만 공개
- 서로 순환 포함이 생기지 않게 주의
- 공용 타입은 공용 헤더에 둠
- 내부 전용 구현은 `.c` 파일에 둠

## 흔한 실수

- include guard 누락
- 헤더에 전역 변수 정의를 넣어 중복 정의 발생
- 함수형 매크로 괄호 부족
- 매크로 인자의 중복 평가 부작용 무시
- 내부 전용 함수까지 헤더에 공개

## 체크 문제

1. 전처리기는 컴파일의 어느 단계에서 동작하는가?
2. `#define`은 왜 타입이 없다고 하는가?
3. include guard가 필요한 이유는 무엇인가?
4. 헤더 파일에는 왜 보통 함수 선언만 두는가?

## 직접 해볼 과제

### 과제 1

사칙연산 함수를 `calc.h`, `calc.c`, `main.c`로 나누어 작성하세요.

### 과제 2

`DEBUG` 매크로가 정의됐을 때만 로그가 출력되도록 매크로를 작성하세요.

### 과제 3

원 넓이와 둘레를 계산하는 상수와 함수를 헤더/소스 분리 형태로 작성하세요.

## 오늘 정리

- 전처리기는 컴파일 전에 코드를 가공합니다.
- `#include`, `#define`, 조건부 컴파일은 매우 자주 사용됩니다.
- 헤더는 선언, 소스는 구현이라는 역할 분리가 중요합니다.
- 분할 컴파일 구조를 익히면 코드가 훨씬 커져도 관리가 쉬워집니다.

