# 5일차 - 배열과 문자열

## 오늘의 목표

- 배열의 메모리 구조를 이해한다.
- 1차원, 2차원 배열을 사용할 수 있다.
- 문자열이 `char` 배열이라는 사실을 이해한다.
- 문자열 입력과 처리에서 자주 하는 실수를 피한다.

## 배열이란?

같은 자료형의 데이터를 `연속된 메모리 공간`에 저장한 구조입니다.

```c
int scores[5] = {90, 80, 70, 60, 50};
```

이 배열은 `int` 5개가 연달아 저장됩니다.

### 인덱스

- 첫 번째 원소 인덱스는 `0`
- 마지막 원소 인덱스는 `크기 - 1`

```c
printf("%d\n", scores[0]);
printf("%d\n", scores[4]);
```

## 배열 선언과 초기화

```c
int arr[5];
int arr2[5] = {1, 2, 3, 4, 5};
int arr3[5] = {1, 2}; // 나머지는 0으로 초기화
int arr4[] = {10, 20, 30}; // 크기 자동 결정
```

## 배열의 메모리 특징

배열 원소는 연속적으로 저장되므로 인접 원소의 주소 차이는 자료형 크기만큼입니다.

```c
#include <stdio.h>

int main(void)
{
    int arr[3] = {10, 20, 30};

    printf("%p\n", (void *)&arr[0]);
    printf("%p\n", (void *)&arr[1]);
    printf("%p\n", (void *)&arr[2]);

    return 0;
}
```

## 배열 길이 구하기

같은 스코프에서 실제 배열일 때:

```c
size_t count = sizeof(arr) / sizeof(arr[0]);
```

주의:

- 이 공식은 `배열이 포인터로 변한 뒤`에는 더 이상 통하지 않습니다.
- 함수 인자로 전달되면 배열은 대부분 포인터처럼 동작합니다.

## 반복문과 배열

```c
for (int i = 0; i < 5; i++)
{
    printf("%d\n", scores[i]);
}
```

## 배열 범위 초과의 위험

```c
int arr[3] = {1, 2, 3};
arr[3] = 100; // 잘못된 접근
```

이 코드는 배열 범위를 벗어나므로 정의되지 않은 동작입니다.

## 2차원 배열

```c
int matrix[2][3] = {
    {1, 2, 3},
    {4, 5, 6}
};
```

접근:

```c
printf("%d\n", matrix[0][1]); // 2
printf("%d\n", matrix[1][2]); // 6
```

### 2차원 배열 출력

```c
for (int i = 0; i < 2; i++)
{
    for (int j = 0; j < 3; j++)
    {
        printf("%d ", matrix[i][j]);
    }
    printf("\n");
}
```

## 문자열은 `char` 배열

```c
char str[] = "Hello";
```

실제 저장:

```text
'H' 'e' 'l' 'l' 'o' '\0'
```

문자열 끝의 `'\0'`을 `널 문자`라고 합니다.

## 문자열 선언 방식 비교

```c
char a[] = "Hello";
char b[6] = {'H', 'e', 'l', 'l', 'o', '\0'};
```

두 방식은 같은 문자열 내용을 표현합니다.

## 문자열 출력

```c
printf("%s\n", str);
```

`%s`는 `'\0'`을 만날 때까지 출력합니다.

## 문자 하나씩 접근

```c
for (int i = 0; str[i] != '\0'; i++)
{
    printf("%c\n", str[i]);
}
```

## 문자열 입력의 기본

### `scanf("%s", ...)`

```c
char name[20];
scanf("%19s", name);
```

주의:

- 공백 전까지만 읽습니다.
- 배열 크기를 넘지 않도록 폭 제한을 넣는 습관이 좋습니다.

### `fgets`

문자열 입력에는 `fgets`가 더 안전합니다.

```c
char line[100];
fgets(line, sizeof(line), stdin);
```

`fgets`는 줄바꿈 문자까지 읽을 수 있습니다.

## 문자열 길이 직접 구하기

```c
#include <stdio.h>

int main(void)
{
    char str[] = "banana";
    int len = 0;

    while (str[len] != '\0')
    {
        len++;
    }

    printf("length = %d\n", len);
    return 0;
}
```

## 문자열 복사 직접 구현

```c
#include <stdio.h>

int main(void)
{
    char src[] = "apple";
    char dest[20];
    int i = 0;

    while (src[i] != '\0')
    {
        dest[i] = src[i];
        i++;
    }
    dest[i] = '\0';

    printf("%s\n", dest);
    return 0;
}
```

## 문자열 비교 직접 구현

```c
int string_compare(const char a[], const char b[])
{
    int i = 0;

    while (a[i] != '\0' && b[i] != '\0')
    {
        if (a[i] != b[i])
        {
            return a[i] - b[i];
        }
        i++;
    }

    return a[i] - b[i];
}
```

## 배열과 포인터의 관계 미리 보기

```c
int arr[3] = {10, 20, 30};
printf("%d\n", *arr);       // 10
printf("%d\n", *(arr + 1)); // 20
```

배열 이름은 많은 상황에서 첫 번째 원소의 주소처럼 사용됩니다. 이 내용은 6일차 포인터에서 자세히 다룹니다.

## 예제: 최댓값 찾기

```c
#include <stdio.h>

int main(void)
{
    int arr[5] = {8, 3, 15, 2, 9};
    int max = arr[0];

    for (int i = 1; i < 5; i++)
    {
        if (arr[i] > max)
        {
            max = arr[i];
        }
    }

    printf("max = %d\n", max);
    return 0;
}
```

## 예제: 문자열 뒤집기

```c
#include <stdio.h>

int main(void)
{
    char str[] = "coding";
    int len = 0;

    while (str[len] != '\0')
    {
        len++;
    }

    for (int i = len - 1; i >= 0; i--)
    {
        printf("%c", str[i]);
    }
    printf("\n");

    return 0;
}
```

## 실수 포인트

- 배열 인덱스를 1부터 시작한다고 착각함
- 배열 범위를 벗어남
- 문자열 끝의 `'\0'`을 잊음
- `scanf("%s", ...)`로 공백 포함 문자열을 읽으려 함
- 배열 크기보다 긴 문자열을 넣어 버퍼 오버플로를 일으킴

## 체크 문제

1. 배열 원소는 메모리에 어떻게 저장되는가?
2. 문자열 끝에 왜 `'\0'`이 필요한가?
3. `sizeof(arr) / sizeof(arr[0])`는 언제만 안전한가?
4. `scanf("%s", name)`보다 `fgets`가 더 안전한 이유는 무엇인가?

## 직접 해볼 과제

### 과제 1

10개의 정수를 배열에 저장하고 평균을 구하세요.

### 과제 2

문자열을 입력받아 모음(`a, e, i, o, u`)의 개수를 세세요.

### 과제 3

2차원 배열로 학생 3명의 국어, 영어, 수학 점수를 저장하고 총점과 평균을 출력하세요.

## 오늘 정리

- 배열은 같은 자료형의 연속 메모리입니다.
- 문자열은 `char` 배열이며 끝에 `'\0'`이 붙습니다.
- 배열 범위 초과는 C에서 매우 위험한 버그입니다.
- 문자열 처리는 항상 `버퍼 크기`를 먼저 생각해야 합니다.

