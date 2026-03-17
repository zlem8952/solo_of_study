# 15일차 - 실전 예제, 디버깅, 정의되지 않은 동작, 표준 헤더 전체 지도

## 오늘의 목표

- C 학습을 실전 문제 해결로 연결한다.
- 디버깅과 경고 해석 습관을 익힌다.
- 정의되지 않은 동작(Undefined Behavior)을 체계적으로 정리한다.
- 표준 라이브러리 헤더 전체 지도를 한 번에 정리한다.

## C 실력이 느는 방식

C는 문법만 읽는다고 실력이 붙지 않습니다. 반드시 아래 4가지를 함께 해야 합니다.

1. 코드 직접 작성
2. 경고 없이 컴파일
3. 메모리/입력 경계 조건 테스트
4. 왜 동작하는지 메모리 관점으로 설명

## 추천 컴파일 옵션

```bash
gcc -std=c11 -Wall -Wextra -pedantic -O0 -g main.c -o main
```

옵션 의미:

- `-Wall -Wextra`: 경고 적극 활성화
- `-pedantic`: 표준에 엄격
- `-O0`: 디버깅 쉬움
- `-g`: 디버거용 정보 포함

## 가능하면 추가하고 싶은 옵션

```bash
gcc -std=c11 -Wall -Wextra -pedantic -fsanitize=address,undefined -g main.c -o main
```

AddressSanitizer/UBSan은 다음 문제를 잡는 데 도움을 줍니다.

- 버퍼 오버플로
- use-after-free
- 일부 정의되지 않은 동작

환경에 따라 지원 여부는 다를 수 있습니다.

## 정의되지 않은 동작(UB)이란?

표준이 결과를 보장하지 않는 동작입니다. "이상하게 돌아간다" 수준이 아니라, 어떤 결과도 믿을 수 없게 됩니다.

### 대표적인 UB 예시

#### 1. 초기화되지 않은 변수 사용

```c
int x;
printf("%d\n", x);
```

#### 2. 배열 범위 초과

```c
int arr[3] = {1, 2, 3};
arr[3] = 10;
```

#### 3. 널 포인터 역참조

```c
int *p = NULL;
*p = 10;
```

#### 4. 해제한 메모리 사용

```c
free(p);
printf("%d\n", *p);
```

#### 5. 부호 있는 정수 오버플로

```c
int x = INT_MAX;
x = x + 1;
```

#### 6. 같은 객체를 순서 없이 여러 번 수정

```c
i = i++;
```

또는

```c
arr[i] = i++;
```

이런 식은 초보 단계에서는 아예 쓰지 않는 것이 좋습니다.

## 디버깅 기본 순서

1. 입력을 가장 작은 예제로 줄인다.
2. 경고를 먼저 전부 해결한다.
3. 문제 지점 전후의 상태를 출력한다.
4. 포인터, 인덱스, 버퍼 크기를 확인한다.
5. 메모리 수명과 소유권을 점검한다.

## 디버깅 출력 예시

```c
printf("i=%d, sum=%d\n", i, sum);
printf("ptr=%p\n", (void *)ptr);
```

포인터는 `%p`와 `(void *)` 캐스팅으로 출력하는 습관이 좋습니다.

## `assert` 사용 전략

```c
assert(arr != NULL);
assert(size > 0);
```

용도:

- 프로그래머의 전제 조건 확인
- 내부 불변식 검사

사용자 입력 오류 처리와는 구분해야 합니다.

## 자주 보는 버그 유형

### 오프바이원(off-by-one)

```c
for (int i = 0; i <= n; i++)
```

`< n`이어야 할 가능성을 항상 의심해야 합니다.

### 입력 버퍼 문제

`scanf` 뒤 줄바꿈이 남아 다음 입력에 영향을 줄 수 있습니다.

### 문자열 버퍼 부족

```c
char name[5];
strcpy(name, "hello");
```

널 문자까지 포함하면 공간이 부족합니다.

### 잘못된 `sizeof`

```c
int *p = malloc(10 * sizeof(p));
```

이 표현이 항상 틀린 것은 아니지만, 의미가 불분명해질 수 있습니다. 보통은 `sizeof(*p)` 또는 `sizeof(int)`가 더 명확합니다.

## 실전 미니 프로젝트 1 - 성적 관리

핵심 연습 요소:

- 구조체
- 배열
- 함수 분리
- 평균 계산
- 파일 저장 확장 가능

```c
#include <stdio.h>

typedef struct
{
    char name[20];
    int kor;
    int eng;
    int math;
} Student;

double average(const Student *s)
{
    return (s->kor + s->eng + s->math) / 3.0;
}

void print_student(const Student *s)
{
    printf("%s: %.2f\n", s->name, average(s));
}

int main(void)
{
    Student list[3] = {
        {"Kim", 90, 80, 70},
        {"Lee", 100, 95, 90},
        {"Park", 75, 88, 92}
    };

    for (int i = 0; i < 3; i++)
    {
        print_student(&list[i]);
    }

    return 0;
}
```

## 실전 미니 프로젝트 2 - 파일 단어 수 세기

핵심 연습 요소:

- 파일 입력
- 문자 분류
- 상태 추적

```c
#include <ctype.h>
#include <stdio.h>

int main(void)
{
    FILE *fp = fopen("input.txt", "r");
    int ch;
    int in_word = 0;
    int words = 0;

    if (fp == NULL)
    {
        perror("fopen");
        return 1;
    }

    while ((ch = fgetc(fp)) != EOF)
    {
        if (isspace((unsigned char)ch))
        {
            in_word = 0;
        }
        else if (!in_word)
        {
            in_word = 1;
            words++;
        }
    }

    fclose(fp);
    printf("words = %d\n", words);
    return 0;
}
```

## 실전 미니 프로젝트 3 - 단일 연결 리스트 맛보기

핵심 연습 요소:

- 구조체 자기참조
- 동적 메모리
- 포인터 연결

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node
{
    int value;
    struct Node *next;
} Node;

Node *push_front(Node *head, int value)
{
    Node *node = malloc(sizeof(*node));
    if (node == NULL)
    {
        return head;
    }

    node->value = value;
    node->next = head;
    return node;
}

void print_list(const Node *head)
{
    while (head != NULL)
    {
        printf("%d ", head->value);
        head = head->next;
    }
    printf("\n");
}

void free_list(Node *head)
{
    while (head != NULL)
    {
        Node *next = head->next;
        free(head);
        head = next;
    }
}

int main(void)
{
    Node *head = NULL;
    head = push_front(head, 30);
    head = push_front(head, 20);
    head = push_front(head, 10);

    print_list(head);
    free_list(head);
    return 0;
}
```

## 표준 헤더 전체 지도

아래 표는 표준 C에서 자주 만나는 헤더를 전체적으로 정리한 것입니다.

| 헤더 | 핵심 내용 |
| --- | --- |
| `assert.h` | `assert` |
| `ctype.h` | 문자 분류/변환 |
| `errno.h` | 오류 코드 `errno` |
| `float.h` | 부동소수점 한계값 매크로 |
| `inttypes.h` | 고정 폭 정수 입출력 매크로 |
| `iso646.h` | 대체 토큰 매크로 |
| `limits.h` | 정수 한계값 매크로 |
| `locale.h` | 로케일 설정 |
| `math.h` | 수학 함수 |
| `setjmp.h` | 비지역 점프 |
| `signal.h` | 신호 처리 |
| `stdalign.h` | 정렬 관련 지원 |
| `stdarg.h` | 가변 인자 |
| `stdatomic.h` | 원자 연산 |
| `stdbool.h` | 불리언 |
| `stddef.h` | `size_t`, `ptrdiff_t`, `offsetof` |
| `stdint.h` | 고정 폭 정수 타입 |
| `stdio.h` | 표준 입출력, 파일 입출력 |
| `stdlib.h` | 메모리, 변환, 난수, 정렬, 종료 |
| `stdnoreturn.h` | 반환하지 않는 함수 |
| `string.h` | 문자열/메모리 함수 |
| `tgmath.h` | 타입 일반화 수학 매크로 |
| `threads.h` | 스레드 API |
| `time.h` | 시간 함수 |
| `uchar.h` | 유니코드 코드 단위 타입 |
| `wchar.h` | 와이드 문자/문자열 |
| `wctype.h` | 와이드 문자 분류/변환 |

## 비표준 함수에 대한 태도

실제 인터넷 글이나 강의에서는 다음 같은 비표준 함수를 자주 볼 수 있습니다.

- `getch`
- `stricmp`
- `itoa`
- `sleep` 또는 `Sleep`

이런 함수는 운영체제나 컴파일러에 따라 다르므로, 먼저 `표준 C`를 확실히 익힌 뒤에 필요할 때 따로 공부하는 것이 좋습니다.

## 2회독 추천 순서

15일을 한 번 끝낸 뒤에는 아래 순서로 다시 보면 효율이 좋습니다.

1. 6일차 포인터
2. 9일차 동적 메모리
3. 11일차 파일 입출력
4. 12일차 문자열/표준 라이브러리
5. 14일차 고급 기능

## 직접 해볼 최종 과제

### 과제 1

학생 정보를 파일에 저장하고 다시 읽어 출력하는 작은 프로그램을 작성하세요.

### 과제 2

명령행 인자로 숫자를 여러 개 받아 정렬 후 출력하는 프로그램을 작성하세요.

### 과제 3

문자열 검색, 파일 입력, 구조체 배열을 조합해 간단한 연락처 관리 프로그램을 작성하세요.

## 마지막 정리

- C는 문법보다 `메모리와 실행 흐름` 이해가 더 중요합니다.
- 함수와 헤더는 "이름"보다 "역할 분류"로 기억해야 오래 갑니다.
- 경고 없는 코드, 경계 조건 테스트, 메모리 점검 습관이 실력을 결정합니다.
- 이 문서 세트를 한 바퀴 돈 뒤 직접 프로젝트를 만들기 시작하면 실력이 빠르게 올라갑니다.

