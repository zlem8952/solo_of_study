# 12일차 - `string.h`, `ctype.h`, `stdlib.h` 핵심 함수 정리

## 오늘의 목표

- 문자열/메모리 함수들의 역할을 구분한다.
- 문자 분류/변환 함수를 익힌다.
- `stdlib.h`의 변환, 정렬, 탐색, 난수, 메모리 관리 함수들을 체계적으로 정리한다.
- 함수 이름만 외우지 말고 `언제 무엇을 써야 하는지` 연결해서 이해한다.

## `string.h` 핵심 함수 지도

### 길이, 복사, 연결

| 함수 | 설명 |
| --- | --- |
| `strlen` | 문자열 길이 반환 (`'\0'` 제외) |
| `strcpy` | 문자열 복사 |
| `strncpy` | 길이 제한 복사 |
| `strcat` | 문자열 뒤에 이어 붙이기 |
| `strncat` | 길이 제한 이어 붙이기 |

### 비교, 검색

| 함수 | 설명 |
| --- | --- |
| `strcmp` | 문자열 비교 |
| `strncmp` | 길이 제한 비교 |
| `strchr` | 문자 첫 위치 검색 |
| `strrchr` | 문자 마지막 위치 검색 |
| `strstr` | 부분 문자열 검색 |
| `strpbrk` | 문자 집합 중 하나 검색 |
| `strspn` | 허용 문자 집합 길이 |
| `strcspn` | 거부 문자 집합 전까지 길이 |

### 토큰 분리, 에러 문자열

| 함수 | 설명 |
| --- | --- |
| `strtok` | 구분자로 문자열 토큰 분리 |
| `strerror` | 에러 번호를 설명 문자열로 변환 |

### 메모리 함수

| 함수 | 설명 |
| --- | --- |
| `memcpy` | 메모리 복사 |
| `memmove` | 겹치는 영역도 안전하게 복사 |
| `memcmp` | 메모리 비교 |
| `memchr` | 메모리에서 바이트 검색 |
| `memset` | 메모리 특정 값으로 채우기 |

## `strlen`

```c
size_t len = strlen("banana"); // 6
```

문자열 길이에는 널 문자 `'\0'`가 포함되지 않습니다.

## `strcpy`, `strncpy`

```c
char dest[20];
strcpy(dest, "apple");
```

주의:

- 목적지 버퍼가 충분히 커야 합니다.
- `strncpy`는 항상 널 종료를 보장하지 않을 수 있어 주의가 필요합니다.

더 안전한 습관:

```c
snprintf(dest, sizeof(dest), "%s", src);
```

## `strcmp`

```c
if (strcmp(a, b) == 0)
{
    printf("같음\n");
}
```

반환값:

- 0: 같음
- 음수: 첫 문자열이 사전순으로 앞
- 양수: 첫 문자열이 사전순으로 뒤

## `strchr`, `strstr`

```c
char *p1 = strchr(str, 'a');
char *p2 = strstr(str, "ing");
```

찾지 못하면 `NULL`

## `strtok`

```c
char text[] = "red,green,blue";
char *token = strtok(text, ",");

while (token != NULL)
{
    printf("%s\n", token);
    token = strtok(NULL, ",");
}
```

주의:

- 원본 문자열을 수정합니다.
- 동시에 여러 문자열을 안전하게 처리해야 하면 다른 방법을 고려하세요.

## `memcpy`와 `memmove`

```c
memcpy(dest, src, n);
memmove(dest, src, n);
```

차이:

- `memcpy`: 겹치지 않는 메모리 복사
- `memmove`: 겹쳐도 안전

겹치는 예:

```c
char s[] = "abcdef";
memmove(s + 2, s, 4);
```

## `memset`

```c
int arr[10];
memset(arr, 0, sizeof(arr));
```

주의:

- 바이트 단위 채움입니다.
- 정수 배열을 1로 채운다고 `memset(arr, 1, sizeof(arr))`를 쓰면 원하는 값 1이 들어가는 것이 아닙니다.

## `ctype.h` 함수 지도

문자 분류 함수:

| 함수 | 설명 |
| --- | --- |
| `isalpha` | 영문자인가 |
| `isdigit` | 숫자인가 |
| `isalnum` | 영문자 또는 숫자인가 |
| `isspace` | 공백 문자인가 |
| `islower` | 소문자인가 |
| `isupper` | 대문자인가 |
| `isxdigit` | 16진수 문자인가 |
| `ispunct` | 구두점 문자인가 |
| `isprint` | 출력 가능한가 |
| `iscntrl` | 제어 문자인가 |
| `isgraph` | 출력 가능하고 공백이 아닌가 |

문자 변환 함수:

| 함수 | 설명 |
| --- | --- |
| `tolower` | 소문자로 변환 |
| `toupper` | 대문자로 변환 |

예:

```c
if (isalpha((unsigned char)ch))
{
    ...
}
```

주의:

- `ctype` 함수에는 `unsigned char` 범위 값 또는 `EOF`를 넘기는 것이 안전합니다.

## `ctype.h` 예제

```c
#include <ctype.h>
#include <stdio.h>

int main(void)
{
    char str[] = "Abc123!";

    for (int i = 0; str[i] != '\0'; i++)
    {
        if (isalpha((unsigned char)str[i]))
        {
            printf("%c is alpha\n", str[i]);
        }
        if (isdigit((unsigned char)str[i]))
        {
            printf("%c is digit\n", str[i]);
        }
    }

    return 0;
}
```

## `stdlib.h` 핵심 함수 지도

### 숫자 변환

| 함수 | 설명 |
| --- | --- |
| `atoi`, `atol`, `atoll` | 문자열을 정수로 변환 |
| `atof` | 문자열을 실수로 변환 |
| `strtol`, `strtoll` | 문자열을 긴 정수로 변환 |
| `strtoul`, `strtoull` | 문자열을 부호 없는 정수로 변환 |
| `strtod`, `strtof`, `strtold` | 문자열을 실수로 변환 |

권장:

- `atoi`보다 오류 확인이 가능한 `strtol` 계열을 선호

### 메모리 관리

| 함수 | 설명 |
| --- | --- |
| `malloc` | 메모리 할당 |
| `calloc` | 0으로 초기화된 메모리 할당 |
| `realloc` | 메모리 크기 재조정 |
| `free` | 메모리 해제 |

### 난수, 정렬, 탐색

| 함수 | 설명 |
| --- | --- |
| `rand`, `srand` | 의사 난수 생성 |
| `qsort` | 범용 정렬 |
| `bsearch` | 이진 탐색 |

### 절댓값, 나눗셈

| 함수 | 설명 |
| --- | --- |
| `abs`, `labs`, `llabs` | 절댓값 |
| `div`, `ldiv`, `lldiv` | 몫과 나머지를 함께 계산 |

### 종료/환경

| 함수 | 설명 |
| --- | --- |
| `exit` | 프로그램 종료 |
| `atexit` | 종료 시 실행할 함수 등록 |
| `abort` | 비정상 종료 |
| `getenv` | 환경 변수 읽기 |
| `system` | 셸 명령 실행 |

### 멀티바이트 관련

| 함수 | 설명 |
| --- | --- |
| `mblen`, `mbtowc`, `wctomb` | 멀티바이트/와이드 문자 변환 기초 |
| `mbstowcs`, `wcstombs` | 문자열 변환 |

초보 단계에서는 문자/문자열의 기본 개념을 먼저 익히고, 멀티바이트는 필요할 때 다시 보는 것을 권장합니다.

## `strtol` 자세히 보기

```c
char *end;
long value = strtol("123abc", &end, 10);
```

결과:

- `value`는 123
- `end`는 `"abc"`를 가리킴

이 방식은 `atoi`보다 실패 검사가 쉽습니다.

## `rand`, `srand`

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(void)
{
    srand((unsigned int)time(NULL));

    for (int i = 0; i < 5; i++)
    {
        printf("%d\n", rand());
    }
    return 0;
}
```

주의:

- `rand`는 간단한 학습용이나 게임의 가벼운 용도에는 쓸 수 있지만 보안용 난수로는 부적절합니다.

## `qsort`

```c
#include <stdio.h>
#include <stdlib.h>

int compare_ints(const void *a, const void *b)
{
    int x = *(const int *)a;
    int y = *(const int *)b;

    if (x < y) return -1;
    if (x > y) return 1;
    return 0;
}

int main(void)
{
    int arr[] = {5, 2, 9, 1, 3};
    int n = sizeof(arr) / sizeof(arr[0]);

    qsort(arr, n, sizeof(arr[0]), compare_ints);

    for (int i = 0; i < n; i++)
    {
        printf("%d ", arr[i]);
    }
    printf("\n");
    return 0;
}
```

## `bsearch`

배열이 미리 정렬되어 있어야 합니다.

```c
int key = 3;
int *found = bsearch(&key, arr, n, sizeof(arr[0]), compare_ints);
```

## `div`

```c
div_t result = div(10, 3);
printf("quot=%d rem=%d\n", result.quot, result.rem);
```

## `getenv`

```c
char *path = getenv("PATH");
if (path != NULL)
{
    printf("%s\n", path);
}
```

## `system`

```c
system("dir");
```

주의:

- 외부 명령 실행은 보안상 위험할 수 있습니다.
- 사용자 입력을 그대로 조합해 넘기면 매우 위험합니다.

## 문자열 함수 실전 예제

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char s[50] = "Hello";

    strcat(s, " C");
    printf("%s\n", s);
    printf("len = %zu\n", strlen(s));

    if (strstr(s, "C") != NULL)
    {
        printf("contains C\n");
    }

    return 0;
}
```

## 문자 함수 실전 예제

```c
#include <ctype.h>
#include <stdio.h>

int main(void)
{
    char text[] = "Hello 2026!";
    int letters = 0;
    int digits = 0;

    for (int i = 0; text[i] != '\0'; i++)
    {
        if (isalpha((unsigned char)text[i]))
        {
            letters++;
        }
        if (isdigit((unsigned char)text[i]))
        {
            digits++;
        }
    }

    printf("letters=%d digits=%d\n", letters, digits);
    return 0;
}
```

## 실수 포인트

- `strcpy`, `strcat` 사용 시 버퍼 크기를 확인하지 않음
- `strcmp(a, b)`가 1 또는 -1만 반환한다고 오해함
- `memcpy`와 `memmove` 차이를 모름
- `atoi` 실패를 제대로 판별하지 못함
- `rand`를 보안 난수처럼 사용함

## 체크 문제

1. `strlen`은 널 문자를 포함해서 세는가?
2. `strcmp`의 반환값은 어떻게 해석해야 하는가?
3. `memcpy` 대신 `memmove`가 필요한 경우는 언제인가?
4. `atoi`보다 `strtol`이 더 안전한 이유는 무엇인가?

## 직접 해볼 과제

### 과제 1

문자열에서 공백을 제외한 문자 수를 세는 프로그램을 작성하세요.

### 과제 2

문장을 단어 단위로 분리해 출력하는 프로그램을 `strtok`로 작성하세요.

### 과제 3

정수 배열을 `qsort`로 오름차순 정렬한 뒤 `bsearch`로 특정 값을 찾으세요.

## 오늘 정리

- `string.h`는 문자열과 메모리 조작의 핵심입니다.
- `ctype.h`는 문자 판별과 변환에 유용합니다.
- `stdlib.h`는 변환, 메모리, 정렬, 탐색, 종료 처리까지 매우 폭넓은 기능을 제공합니다.

