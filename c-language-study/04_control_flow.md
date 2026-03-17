# 4일차 - 조건문, 반복문, 제어문

## 오늘의 목표

- `if`, `else if`, `else`를 자유롭게 사용할 수 있다.
- `switch`와 `case`의 특징을 이해한다.
- `for`, `while`, `do while` 반복문의 차이를 안다.
- `break`, `continue`, `return`, `goto`의 역할을 구분한다.

## 조건문 `if`

가장 기본적인 분기문입니다.

```c
if (조건식)
{
    실행문;
}
```

예:

```c
if (score >= 60)
{
    printf("합격\n");
}
```

## `if - else`

```c
if (score >= 60)
{
    printf("합격\n");
}
else
{
    printf("불합격\n");
}
```

## `else if`

```c
if (score >= 90)
{
    printf("A\n");
}
else if (score >= 80)
{
    printf("B\n");
}
else if (score >= 70)
{
    printf("C\n");
}
else
{
    printf("F\n");
}
```

## 중첩 `if`

```c
if (age >= 20)
{
    if (age < 30)
    {
        printf("20대\n");
    }
}
```

가능은 하지만 너무 깊게 중첩되면 읽기 어려워집니다. 조건을 논리 연산자로 합칠 수 있는지 먼저 생각하세요.

```c
if (age >= 20 && age < 30)
{
    printf("20대\n");
}
```

## `switch` 문

정수형 계열 값이나 열거형 등으로 여러 갈래를 나눌 때 사용합니다.

```c
switch (menu)
{
    case 1:
        printf("추가\n");
        break;
    case 2:
        printf("삭제\n");
        break;
    case 3:
        printf("조회\n");
        break;
    default:
        printf("잘못된 입력\n");
        break;
}
```

### `break`가 중요한 이유

`break`가 없으면 다음 `case`로 계속 떨어집니다. 이를 `fall-through`라고 합니다.

```c
switch (x)
{
    case 1:
        printf("one\n");
    case 2:
        printf("two\n");
        break;
}
```

`x`가 1이면 `one`, `two`가 둘 다 출력됩니다.

### 일부러 fall-through를 쓰는 경우

```c
switch (ch)
{
    case 'a':
    case 'A':
        printf("A 계열\n");
        break;
}
```

## 반복문 `while`

조건이 참인 동안 반복합니다.

```c
while (조건식)
{
    반복할 문장;
}
```

예:

```c
int i = 1;
while (i <= 5)
{
    printf("%d\n", i);
    i++;
}
```

## 반복문 `do while`

본문을 먼저 한 번 실행한 뒤 조건을 검사합니다.

```c
do
{
    실행문;
} while (조건식);
```

예:

```c
int x = 5;
do
{
    printf("%d\n", x);
    x--;
} while (x > 0);
```

## 반복문 `for`

초기식, 조건식, 증감식을 한 줄에 모아 반복을 표현합니다.

```c
for (초기식; 조건식; 증감식)
{
    반복문장;
}
```

예:

```c
for (int i = 0; i < 5; i++)
{
    printf("%d\n", i);
}
```

## `while`과 `for`의 선택 기준

- 반복 횟수가 명확하면 `for`
- 종료 조건 중심이면 `while`
- 최소 1번은 실행해야 하면 `do while`

## 무한 루프

```c
while (1)
{
    ...
}
```

또는

```c
for (;;)
{
    ...
}
```

반드시 적절한 탈출 조건을 고려해야 합니다.

## `break`

가장 가까운 반복문이나 `switch`를 종료합니다.

```c
for (int i = 0; i < 10; i++)
{
    if (i == 5)
    {
        break;
    }
    printf("%d\n", i);
}
```

## `continue`

현재 반복의 나머지를 건너뛰고 다음 반복으로 이동합니다.

```c
for (int i = 1; i <= 10; i++)
{
    if (i % 2 == 0)
    {
        continue;
    }
    printf("%d\n", i);
}
```

## `return`

함수를 종료하고 값을 돌려줍니다.

```c
if (x < 0)
{
    return 1;
}
```

## `goto`

다른 위치의 레이블로 점프합니다.

```c
goto end;

end:
printf("끝\n");
```

보통 남용하면 구조가 망가지므로 피합니다. 다만 오류 처리에서 한 번에 정리(cleanup)할 때 제한적으로 쓰이기도 합니다.

## 반복문 예제 1: 1부터 n까지 합

```c
#include <stdio.h>

int main(void)
{
    int n;
    int sum = 0;

    scanf("%d", &n);

    for (int i = 1; i <= n; i++)
    {
        sum += i;
    }

    printf("sum = %d\n", sum);
    return 0;
}
```

## 반복문 예제 2: 구구단

```c
#include <stdio.h>

int main(void)
{
    for (int i = 2; i <= 9; i++)
    {
        for (int j = 1; j <= 9; j++)
        {
            printf("%d x %d = %d\n", i, j, i * j);
        }
        printf("\n");
    }

    return 0;
}
```

## 반복문 예제 3: 소수 판별

```c
#include <stdio.h>

int main(void)
{
    int n;
    int is_prime = 1;

    scanf("%d", &n);

    if (n < 2)
    {
        is_prime = 0;
    }
    else
    {
        for (int i = 2; i * i <= n; i++)
        {
            if (n % i == 0)
            {
                is_prime = 0;
                break;
            }
        }
    }

    if (is_prime)
    {
        printf("소수입니다.\n");
    }
    else
    {
        printf("소수가 아닙니다.\n");
    }

    return 0;
}
```

## 중첩 루프와 시간 복잡도 감각

```c
for (int i = 0; i < n; i++)
{
    for (int j = 0; j < n; j++)
    {
        ...
    }
}
```

이런 구조는 대체로 `n * n`, 즉 `O(n^2)` 작업량을 가집니다. 데이터가 커지면 느려질 수 있습니다.

## 흔한 실수

### 무한 루프

```c
int i = 0;
while (i < 10)
{
    printf("%d\n", i);
}
```

`i`를 증가시키지 않아 무한 반복됩니다.

### 세미콜론 실수

```c
if (x > 0);
{
    printf("양수\n");
}
```

`if` 뒤 세미콜론 때문에 의도와 다르게 동작합니다.

### `switch`에서 `break` 누락

원하지 않는 fall-through가 발생합니다.

## 체크 문제

1. `if`와 `switch`는 어떤 상황에서 각각 더 적합한가?
2. `while`과 `do while`의 차이는 무엇인가?
3. `break`와 `continue`의 차이는 무엇인가?
4. 소수 판별에서 `break`를 넣는 이유는 무엇인가?

## 직접 해볼 과제

### 과제 1

정수를 입력받아 그 수의 팩토리얼을 계산하세요.

### 과제 2

별 삼각형, 역삼각형, 피라미드를 반복문으로 출력하세요.

### 과제 3

사용자가 0을 입력할 때까지 정수를 계속 입력받아 합계를 출력하세요.

## 오늘 정리

- 분기는 `if`, `switch`
- 반복은 `for`, `while`, `do while`
- 흐름 제어는 `break`, `continue`, `return`
- 반복문은 단순히 돌리는 것보다 `언제 끝나는지`를 분명히 설계하는 것이 중요합니다.

