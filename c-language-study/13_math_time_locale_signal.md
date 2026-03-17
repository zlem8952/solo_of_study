# 13일차 - `math.h`, `time.h`, `locale.h`, `signal.h`와 유용한 표준 헤더

## 오늘의 목표

- 수학 함수들을 범주별로 정리한다.
- 시간 관련 함수와 `struct tm`을 이해한다.
- 로케일과 신호 처리의 기본을 익힌다.
- `assert.h`, `errno.h`, `limits.h`, `float.h`처럼 자주 만나는 헤더도 함께 정리한다.

## `math.h` 함수 지도

### 기본 수학

| 함수 | 설명 |
| --- | --- |
| `sqrt` | 제곱근 |
| `cbrt` | 세제곱근 |
| `pow` | 거듭제곱 |
| `hypot` | 빗변 길이 |

### 삼각 함수

| 함수 | 설명 |
| --- | --- |
| `sin`, `cos`, `tan` | 삼각 함수 |
| `asin`, `acos`, `atan`, `atan2` | 역삼각 함수 |

### 지수/로그

| 함수 | 설명 |
| --- | --- |
| `exp`, `exp2` | 지수 함수 |
| `log`, `log10`, `log2` | 로그 |

### 반올림/절삭

| 함수 | 설명 |
| --- | --- |
| `floor` | 내림 |
| `ceil` | 올림 |
| `round` | 반올림 |
| `trunc` | 0 방향 절삭 |

### 나머지와 분해

| 함수 | 설명 |
| --- | --- |
| `fmod` | 실수 나머지 |
| `remainder` | IEEE 방식 나머지 |
| `modf` | 정수부/소수부 분리 |
| `frexp`, `ldexp` | 부동소수점 분해/조립 |

### 절댓값과 비교

| 함수 | 설명 |
| --- | --- |
| `fabs` | 실수 절댓값 |
| `fmin`, `fmax` | 최소/최대 |

### 분류/특수값

| 함수 | 설명 |
| --- | --- |
| `isfinite` | 유한값인가 |
| `isinf` | 무한대인가 |
| `isnan` | NaN인가 |
| `signbit` | 부호 비트 확인 |

## `math.h` 예제

```c
#include <math.h>
#include <stdio.h>

int main(void)
{
    double x = 2.0;

    printf("sqrt = %.2f\n", sqrt(x));
    printf("pow = %.2f\n", pow(x, 3.0));
    printf("sin = %.2f\n", sin(x));
    printf("floor = %.2f\n", floor(3.9));
    printf("round = %.2f\n", round(3.5));

    return 0;
}
```

Linux 계열에서는 링크 시 `-lm`이 필요할 수 있습니다.

```bash
gcc main.c -lm
```

## `time.h` 핵심 함수 지도

| 함수 | 설명 |
| --- | --- |
| `time` | 현재 시간(`time_t`) 획득 |
| `clock` | CPU 사용 시간 측정 |
| `difftime` | 두 시각 차이 계산 |
| `mktime` | `struct tm`을 `time_t`로 변환 |
| `localtime` | 지역 시간으로 변환 |
| `gmtime` | UTC 시간으로 변환 |
| `ctime` | 시간 문자열 변환 |
| `asctime` | `struct tm`을 문자열로 변환 |
| `strftime` | 원하는 형식의 시간 문자열 생성 |

## `time_t`와 `struct tm`

### `time_t`

시간을 표현하는 산술형 타입입니다.

### `struct tm`

시간 구성 요소를 저장합니다.

주요 멤버:

- `tm_year`: 1900부터의 연도 차이
- `tm_mon`: 0~11
- `tm_mday`: 1~31
- `tm_hour`: 0~23
- `tm_min`: 0~59
- `tm_sec`: 0~60
- `tm_wday`: 0~6
- `tm_yday`: 0~365

## 현재 시간 출력 예제

```c
#include <stdio.h>
#include <time.h>

int main(void)
{
    time_t now = time(NULL);
    struct tm *local = localtime(&now);

    if (local != NULL)
    {
        printf("%04d-%02d-%02d %02d:%02d:%02d\n",
               local->tm_year + 1900,
               local->tm_mon + 1,
               local->tm_mday,
               local->tm_hour,
               local->tm_min,
               local->tm_sec);
    }

    return 0;
}
```

## `strftime`

```c
char buf[100];
strftime(buf, sizeof(buf), "%Y-%m-%d %H:%M:%S", local);
printf("%s\n", buf);
```

## 실행 시간 측정 `clock`

```c
clock_t start = clock();

for (volatile long i = 0; i < 10000000; i++)
{
}

clock_t end = clock();
double elapsed = (double)(end - start) / CLOCKS_PER_SEC;
printf("elapsed = %.6f\n", elapsed);
```

## `locale.h`

프로그램의 문자, 숫자, 날짜 형식 등을 지역 설정에 맞게 다룰 때 사용합니다.

주요 함수:

| 함수 | 설명 |
| --- | --- |
| `setlocale` | 현재 로케일 설정 |
| `localeconv` | 로케일별 숫자/통화 형식 정보 |

예:

```c
#include <locale.h>
#include <stdio.h>

int main(void)
{
    setlocale(LC_ALL, "");
    printf("locale set\n");
    return 0;
}
```

## `signal.h`

비동기 신호를 다룹니다.

주요 함수:

| 함수 | 설명 |
| --- | --- |
| `signal` | 신호 처리기 등록 |
| `raise` | 현재 프로세스에 신호 발생 |

예:

```c
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>

void handle_signal(int sig)
{
    printf("signal received: %d\n", sig);
}

int main(void)
{
    signal(SIGINT, handle_signal);
    raise(SIGINT);
    return 0;
}
```

주의:

- 신호 처리기 안에서는 매우 제한된 작업만 안전할 수 있습니다.
- 복잡한 로직은 신호 처리기 밖으로 넘기는 설계를 고려해야 합니다.

## `assert.h`

디버깅 중 전제 조건을 검사할 때 사용합니다.

```c
#include <assert.h>

assert(ptr != NULL);
```

`NDEBUG`가 정의되면 `assert`는 제거될 수 있습니다.

## `errno.h`

오류 코드 저장에 사용됩니다.

```c
#include <errno.h>
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    char *end;
    errno = 0;
    long value = strtol("999999999999999999999999", &end, 10);

    if (errno != 0)
    {
        perror("strtol");
    }

    printf("%ld\n", value);
    return 0;
}
```

## `limits.h`

정수형 한계값 매크로를 제공합니다.

예:

- `CHAR_BIT`
- `INT_MIN`, `INT_MAX`
- `LONG_MIN`, `LONG_MAX`
- `ULLONG_MAX`

## `float.h`

부동소수점 한계와 정밀도 관련 매크로를 제공합니다.

예:

- `FLT_MIN`, `FLT_MAX`
- `DBL_MIN`, `DBL_MAX`
- `DBL_EPSILON`

## `tgmath.h`

일반화 수학 매크로를 제공합니다. 타입에 따라 적절한 수학 함수를 선택해 줍니다. 학습 초기에는 반드시 필요하지는 않지만 표준 헤더의 일부로 알아두면 좋습니다.

## 예제: 날짜 차이 계산

```c
#include <stdio.h>
#include <time.h>

int main(void)
{
    struct tm a = {0};
    struct tm b = {0};

    a.tm_year = 2026 - 1900;
    a.tm_mon = 2;
    a.tm_mday = 1;

    b.tm_year = 2026 - 1900;
    b.tm_mon = 2;
    b.tm_mday = 16;

    time_t t1 = mktime(&a);
    time_t t2 = mktime(&b);

    printf("%.0f days\n", difftime(t2, t1) / (60 * 60 * 24));
    return 0;
}
```

## 실수 포인트

- `math.h` 함수에 필요한 링크 옵션을 잊음
- `struct tm`의 `tm_year`, `tm_mon` 기준값을 헷갈림
- `assert`를 사용자 입력 검증 도구처럼 사용함
- `errno`를 실패 여부 확인 없이 무조건 읽음
- 신호 처리기 안에서 복잡한 작업을 함

## 체크 문제

1. `sqrt`, `pow`, `round`는 각각 무엇을 하는가?
2. `time_t`와 `struct tm`의 차이는 무엇인가?
3. `strftime`는 왜 유용한가?
4. `limits.h`와 `float.h`는 어떤 정보를 제공하는가?

## 직접 해볼 과제

### 과제 1

반지름을 입력받아 넓이와 둘레를 계산하는 프로그램을 `math.h`로 작성하세요.

### 과제 2

현재 시간을 읽어 `YYYY/MM/DD HH:MM:SS` 형식으로 출력하세요.

### 과제 3

두 날짜를 입력받아 날짜 차이를 계산하는 프로그램을 작성하세요.

## 오늘 정리

- `math.h`는 계산 함수, `time.h`는 시간 처리의 핵심입니다.
- `locale.h`, `signal.h`, `assert.h`, `errno.h`는 자주 보이진 않아도 중요합니다.
- 표준 헤더는 각각 역할이 분명하므로 `문제 유형 -> 헤더` 연결로 기억하는 것이 좋습니다.

