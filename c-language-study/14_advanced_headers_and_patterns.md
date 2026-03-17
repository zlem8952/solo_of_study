# 14일차 - 고급 헤더, 가변 인자, 타입 한정자, 현대 표준 기능

## 오늘의 목표

- 초급 문법을 넘어 표준 C의 고급 기능을 정리한다.
- `stdarg.h`, `stdint.h`, `stdbool.h`, `stddef.h` 등의 역할을 익힌다.
- `volatile`, `restrict`, `_Atomic`, `_Static_assert`, `_Generic` 같은 키워드와 기능을 이해한다.
- `setjmp.h`, `threads.h`, `stdatomic.h`의 기본 개념을 잡는다.

## `stdbool.h`

C99 이후에는 `_Bool`과 `stdbool.h`를 이용해 불리언 표현을 더 읽기 쉽게 쓸 수 있습니다.

```c
#include <stdbool.h>

bool is_valid = true;
if (is_valid)
{
    ...
}
```

실제로는:

- `bool` -> `_Bool`
- `true` -> 1
- `false` -> 0

## `stdint.h`

비트 폭이 명확한 정수형을 제공합니다.

자주 쓰는 타입:

| 타입 | 의미 |
| --- | --- |
| `int8_t`, `uint8_t` | 8비트 정수 |
| `int16_t`, `uint16_t` | 16비트 정수 |
| `int32_t`, `uint32_t` | 32비트 정수 |
| `int64_t`, `uint64_t` | 64비트 정수 |
| `intptr_t`, `uintptr_t` | 포인터를 저장 가능한 정수 |

장점:

- 파일 포맷, 네트워크 프로토콜, 임베디드 코드에서 크기가 분명함

## `inttypes.h`

고정 폭 정수형의 입출력을 돕는 매크로를 제공합니다.

```c
#include <inttypes.h>
#include <stdint.h>
#include <stdio.h>

int32_t x = 123;
printf("%" PRId32 "\n", x);
```

이식성을 높이는 데 도움이 됩니다.

## `stddef.h`

핵심 타입과 매크로를 제공합니다.

| 항목 | 설명 |
| --- | --- |
| `size_t` | 크기 표현용 부호 없는 정수 |
| `ptrdiff_t` | 포인터 차이 표현용 정수 |
| `NULL` | 널 포인터 상수 |
| `offsetof` | 구조체 멤버 오프셋 계산 |

예:

```c
#include <stddef.h>
#include <stdio.h>

struct Item
{
    char c;
    int n;
};

int main(void)
{
    printf("%zu\n", offsetof(struct Item, n));
    return 0;
}
```

## `stdarg.h` - 가변 인자 함수

함수 인자 개수가 고정되지 않을 때 사용합니다.

주요 매크로:

| 매크로 | 설명 |
| --- | --- |
| `va_list` | 가변 인자 목록 타입 |
| `va_start` | 가변 인자 처리 시작 |
| `va_arg` | 다음 인자 읽기 |
| `va_end` | 정리 |
| `va_copy` | 목록 복사 |

예제:

```c
#include <stdarg.h>
#include <stdio.h>

int sum_many(int count, ...)
{
    va_list ap;
    int sum = 0;

    va_start(ap, count);
    for (int i = 0; i < count; i++)
    {
        sum += va_arg(ap, int);
    }
    va_end(ap);

    return sum;
}

int main(void)
{
    printf("%d\n", sum_many(4, 10, 20, 30, 40));
    return 0;
}
```

주의:

- 가변 인자는 타입 안전성이 약합니다.
- 인자 개수와 타입을 호출자와 피호출자가 정확히 약속해야 합니다.

## `setjmp.h`

일반 함수 반환 흐름을 뛰어넘는 비지역 점프를 지원합니다.

함수:

| 함수 | 설명 |
| --- | --- |
| `setjmp` | 현재 실행 환경 저장 |
| `longjmp` | 저장된 위치로 점프 |

개념 예시:

```c
#include <setjmp.h>
#include <stdio.h>

jmp_buf env;

void test(void)
{
    longjmp(env, 1);
}

int main(void)
{
    if (setjmp(env) == 0)
    {
        printf("first\n");
        test();
    }
    else
    {
        printf("jumped back\n");
    }

    return 0;
}
```

주의:

- 자원 정리가 꼬일 수 있습니다.
- 현대 코드에서는 무분별한 사용을 피하고, 정말 필요한 경우에만 사용합니다.

## `volatile`

변수가 프로그램 코드 바깥 요인에 의해 바뀔 수 있음을 컴파일러에 알립니다.

```c
volatile int flag;
```

주로 사용되는 곳:

- 메모리 매핑 I/O
- 인터럽트 처리
- 신호 처리와의 상호작용

주의:

- `volatile`은 스레드 동기화를 해결해 주지 않습니다.
- 원자성이나 메모리 순서를 보장하지 않습니다.

## `restrict`

포인터가 가리키는 대상이 다른 포인터와 겹치지 않는다고 약속할 때 사용합니다.

```c
void add_arrays(int *restrict out,
                const int *restrict a,
                const int *restrict b,
                size_t n)
{
    for (size_t i = 0; i < n; i++)
    {
        out[i] = a[i] + b[i];
    }
}
```

장점:

- 컴파일러 최적화에 도움

주의:

- 약속을 깨면 정의되지 않은 동작 가능

## `_Atomic`과 `stdatomic.h`

멀티스레드 환경에서 원자적 접근이 필요한 경우 사용합니다.

주요 도구:

| 항목 | 설명 |
| --- | --- |
| `_Atomic` | 원자 타입 지정 |
| `atomic_init` | 초기화 |
| `atomic_load` | 읽기 |
| `atomic_store` | 쓰기 |
| `atomic_fetch_add` | 원자적 증가 |

예:

```c
#include <stdatomic.h>
#include <stdio.h>

int main(void)
{
    atomic_int counter;
    atomic_init(&counter, 0);

    atomic_fetch_add(&counter, 1);
    printf("%d\n", atomic_load(&counter));
    return 0;
}
```

## `threads.h`

표준 C11 스레드 API입니다. 일부 환경에서는 지원이 제한될 수 있습니다.

주요 항목:

| 항목 | 설명 |
| --- | --- |
| `thrd_t` | 스레드 타입 |
| `thrd_create` | 스레드 생성 |
| `thrd_join` | 스레드 종료 대기 |
| `mtx_t` | 뮤텍스 |
| `mtx_init`, `mtx_lock`, `mtx_unlock`, `mtx_destroy` | 뮤텍스 제어 |
| `cnd_t` | 조건 변수 |
| `tss_t` | 스레드 지역 저장소 |

학습 팁:

- 개념은 알아두되, 실제 프로젝트에서는 플랫폼/라이브러리 지원을 먼저 확인하세요.

## `_Thread_local`

스레드마다 독립된 저장 공간을 가집니다.

```c
_Thread_local int tls_value = 0;
```

## `_Static_assert`

컴파일 시점 조건 검사를 합니다.

```c
_Static_assert(sizeof(int) >= 4, "int must be at least 4 bytes");
```

런타임이 아니라 컴파일 단계에서 오류를 낼 수 있어 유용합니다.

## `_Generic`

타입에 따라 다른 동작을 선택하는 C11 기능입니다.

```c
#include <stdio.h>

#define type_name(x) _Generic((x), \
    int: "int", \
    double: "double", \
    char *: "char *", \
    default: "other")

int main(void)
{
    printf("%s\n", type_name(10));
    printf("%s\n", type_name(3.14));
    return 0;
}
```

매크로 기반 generic programming에 사용됩니다.

## 지정 초기화(designated initializer)

구조체/배열 초기화를 더 명확하게 할 수 있습니다.

```c
struct Point
{
    int x;
    int y;
};

struct Point p = {.y = 20, .x = 10};
```

배열에도 사용 가능:

```c
int arr[10] = {[0] = 1, [3] = 7, [9] = 100};
```

## 컴파운드 리터럴(compound literal)

익명 객체를 즉석에서 만드는 기능입니다.

```c
int *p = (int[]){1, 2, 3};
```

구조체에도 사용 가능:

```c
print_point(&(struct Point){.x = 3, .y = 4});
```

## 유연 배열 멤버(flexible array member)

구조체 마지막에 크기 없는 배열을 둘 수 있습니다.

```c
struct Buffer
{
    size_t len;
    char data[];
};
```

동적 메모리와 함께 사용하면 가변 크기 구조를 효율적으로 만들 수 있습니다.

## `stdalign.h`, `stdnoreturn.h`, `uchar.h`, `wchar.h`, `wctype.h`

### `stdalign.h`

- 정렬 관련 매크로/키워드 지원

### `stdnoreturn.h`

- 반환하지 않는 함수 표시

```c
noreturn void fatal_error(void);
```

### `uchar.h`

- 유니코드 코드 단위 타입(`char16_t`, `char32_t`) 관련

### `wchar.h`, `wctype.h`

- 와이드 문자와 와이드 문자열 처리

학습 초반에는 필수는 아니지만 표준 헤더 범위에 포함되므로 존재를 알아두는 것이 좋습니다.

## 함수 포인터 활용 패턴

```c
int apply(int a, int b, int (*op)(int, int))
{
    return op(a, b);
}
```

이 패턴은 전략 선택, 콜백, 정렬 비교 함수 등에 많이 사용됩니다.

## 실수 포인트

- 가변 인자 함수에서 타입 불일치를 일으킴
- `volatile`을 동기화 도구라고 오해함
- `restrict` 약속을 지키지 않음
- `setjmp/longjmp`로 자원 정리를 망가뜨림
- 고정 폭 정수형 출력에 서식 문자열을 잘못 사용함

## 체크 문제

1. `stdbool.h`를 쓰면 어떤 장점이 있는가?
2. `stdint.h`는 왜 중요한가?
3. `stdarg.h`를 사용할 때 가장 조심해야 할 점은 무엇인가?
4. `volatile`과 `_Atomic`은 어떻게 다른가?

## 직접 해볼 과제

### 과제 1

가변 인자로 여러 정수의 평균을 구하는 함수를 작성하세요.

### 과제 2

`stdint.h`를 사용해 32비트 정수 배열의 합을 구하고 출력 매크로를 이용해 출력하세요.

### 과제 3

지정 초기화와 컴파운드 리터럴을 사용한 구조체 예제를 만들어 보세요.

## 오늘 정리

- 표준 C는 생각보다 넓고, 고급 기능도 꽤 많습니다.
- `stdarg`, `stdint`, `stdbool`, `stddef`는 실무에서도 자주 등장합니다.
- `volatile`, `restrict`, `_Atomic`은 이름만 비슷해 보여도 목적이 완전히 다릅니다.
- 고급 기능은 외우기보다 `필요한 문제 상황과 연결`해 기억하는 것이 좋습니다.

