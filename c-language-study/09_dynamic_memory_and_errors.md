# 9일차 - 동적 메모리, 명령행 인자, 오류 처리

## 오늘의 목표

- 스택 메모리와 힙 메모리의 차이를 이해한다.
- `malloc`, `calloc`, `realloc`, `free`를 사용할 수 있다.
- 메모리 누수, 이중 해제, 댕글링 포인터를 피하는 습관을 익힌다.
- `argc`, `argv`를 사용해 명령행 인자를 처리할 수 있다.
- 오류 처리의 기본 도구를 익힌다.

## 정적/자동 메모리와 동적 메모리

지금까지 사용한 일반 지역 변수와 배열은 보통 자동 저장 기간 객체로, 함수 블록이 끝나면 사라집니다.

```c
void func(void)
{
    int arr[100];
}
```

반면 프로그램 실행 중에 필요한 크기를 그때그때 확보하고 싶을 때는 동적 메모리를 씁니다.

## 힙 메모리란?

- 실행 중 필요할 때 요청
- 직접 해제해야 함
- 크기를 유연하게 정할 수 있음

## `malloc`

지정한 바이트 수만큼 메모리를 할당합니다.

```c
int *arr = malloc(5 * sizeof(int));
```

주의:

- 성공하면 메모리 주소 반환
- 실패하면 `NULL` 반환
- 초기화되지 않은 메모리

정확한 사용:

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *arr = malloc(5 * sizeof(int));
    if (arr == NULL)
    {
        fprintf(stderr, "메모리 할당 실패\n");
        return 1;
    }

    for (int i = 0; i < 5; i++)
    {
        arr[i] = (i + 1) * 10;
    }

    for (int i = 0; i < 5; i++)
    {
        printf("%d\n", arr[i]);
    }

    free(arr);
    arr = NULL;
    return 0;
}
```

## `calloc`

배열 형태로 메모리를 할당하고 `0`으로 초기화합니다.

```c
int *arr = calloc(5, sizeof(int));
```

## `realloc`

기존 할당 메모리 크기를 변경합니다.

```c
int *temp = realloc(arr, 10 * sizeof(int));
if (temp == NULL)
{
    free(arr);
    return 1;
}
arr = temp;
```

주의:

- `realloc` 실패 시 원래 포인터는 그대로 살아 있습니다.
- 그래서 반환값을 바로 원본 포인터에 덮어쓰지 않는 습관이 좋습니다.

## `free`

할당한 메모리를 반환합니다.

```c
free(arr);
arr = NULL;
```

`NULL`로 바꿔두면 이미 해제한 메모리를 실수로 다시 참조할 위험을 줄일 수 있습니다.

## 자주 터지는 메모리 버그

### 메모리 누수

할당만 하고 해제하지 않는 경우

```c
int *p = malloc(100 * sizeof(int));
```

`free(p);`를 하지 않으면 누수입니다.

### 이중 해제

```c
free(p);
free(p); // 위험
```

### 댕글링 포인터

```c
free(p);
printf("%d\n", *p); // 위험
```

### 할당 크기 계산 실수

```c
int *p = malloc(10); // int 10개가 아님
```

올바른 방식:

```c
int *p = malloc(10 * sizeof(int));
```

## 동적 배열 예제

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int n;
    scanf("%d", &n);

    int *arr = malloc(n * sizeof(int));
    if (arr == NULL)
    {
        fprintf(stderr, "allocation failed\n");
        return 1;
    }

    for (int i = 0; i < n; i++)
    {
        arr[i] = i * i;
    }

    for (int i = 0; i < n; i++)
    {
        printf("%d ", arr[i]);
    }
    printf("\n");

    free(arr);
    return 0;
}
```

## 2차원 동적 메모리 개념

방법은 여러 가지가 있습니다.

### 행 포인터 배열 방식

```c
int **matrix = malloc(rows * sizeof(int *));
```

각 행마다 다시 `malloc`합니다.

### 한 덩어리 방식

```c
int *data = malloc(rows * cols * sizeof(int));
```

필요에 따라 `data[i * cols + j]`로 접근합니다.

학습 초반에는 한 덩어리 방식이 메모리 구조 이해에 더 도움이 됩니다.

## 명령행 인자 `argc`, `argv`

```c
int main(int argc, char *argv[])
{
    ...
}
```

- `argc`: 인자 개수
- `argv`: 인자 문자열 배열

예를 들어:

```bash
./main hello 123
```

이면:

- `argc == 3`
- `argv[0] == "./main"`
- `argv[1] == "hello"`
- `argv[2] == "123"`

## 명령행 인자 출력 예제

```c
#include <stdio.h>

int main(int argc, char *argv[])
{
    for (int i = 0; i < argc; i++)
    {
        printf("argv[%d] = %s\n", i, argv[i]);
    }
    return 0;
}
```

## 문자열을 숫자로 바꾸기

초보자는 `atoi`를 자주 보지만, 더 안전하게는 `strtol` 계열을 쓰는 편이 좋습니다.

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
    if (argc < 2)
    {
        fprintf(stderr, "사용법: ./main number\n");
        return 1;
    }

    long value = strtol(argv[1], NULL, 10);
    printf("%ld\n", value);
    return 0;
}
```

## 오류 출력 스트림 `stderr`

일반 출력은 `stdout`, 오류 메시지는 `stderr`로 내보내는 습관이 좋습니다.

```c
fprintf(stderr, "파일 열기 실패\n");
```

## `perror`

시스템 또는 라이브러리 오류 원인을 메시지와 함께 출력할 때 유용합니다.

```c
perror("fopen failed");
```

## `errno`

오류 원인을 코드 값으로 확인할 수 있습니다.

```c
#include <errno.h>
```

하지만 `errno`는 함수가 실패했을 때만 검사해야 의미가 있습니다.

## 프로그램 종료 함수

### `exit`

```c
exit(EXIT_FAILURE);
```

### `EXIT_SUCCESS`, `EXIT_FAILURE`

`stdlib.h`에 정의된 매크로입니다.

```c
return EXIT_SUCCESS;
```

## `assert`

디버깅 중 논리 조건을 검사할 때 사용합니다.

```c
#include <assert.h>

assert(ptr != NULL);
```

릴리스 빌드에서 비활성화될 수 있으므로, 실제 사용자 오류 처리와 같은 것은 `assert`에만 의존하면 안 됩니다.

## 예제: 평균 계산 프로그램

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
    if (argc < 2)
    {
        fprintf(stderr, "사용법: %s n1 n2 n3 ...\n", argv[0]);
        return EXIT_FAILURE;
    }

    long sum = 0;

    for (int i = 1; i < argc; i++)
    {
        sum += strtol(argv[i], NULL, 10);
    }

    printf("평균 = %.2f\n", (double)sum / (argc - 1));
    return EXIT_SUCCESS;
}
```

## 안전한 습관 정리

- `malloc` 결과는 항상 검사
- 할당 크기는 `개수 * sizeof(타입)` 형태로 계산
- `realloc`은 임시 포인터로 받기
- `free` 후에는 `NULL` 대입 고려
- 해제된 메모리를 다시 쓰지 않기
- 오류 메시지는 `stderr` 사용

## 체크 문제

1. `malloc`과 `calloc`의 차이는 무엇인가?
2. `realloc` 결과를 바로 원본 포인터에 넣지 않는 이유는 무엇인가?
3. 메모리 누수와 댕글링 포인터는 어떻게 다른가?
4. `argc`, `argv`는 각각 무엇을 의미하는가?

## 직접 해볼 과제

### 과제 1

사용자에게 정수 개수를 입력받아 동적 배열을 만들고 평균을 계산하세요.

### 과제 2

명령행 인자로 받은 수들 중 최댓값을 출력하는 프로그램을 작성하세요.

### 과제 3

크기를 점점 늘려 가며 동적 배열에 숫자를 저장하는 간단한 프로그램을 `realloc`으로 작성하세요.

## 오늘 정리

- 동적 메모리는 유연하지만 직접 관리해야 합니다.
- `malloc`, `calloc`, `realloc`, `free`는 C 메모리 관리의 핵심 함수입니다.
- 메모리 버그는 C에서 가장 치명적인 실수 중 하나입니다.
- 명령행 인자를 이용하면 프로그램을 더 실용적으로 만들 수 있습니다.

