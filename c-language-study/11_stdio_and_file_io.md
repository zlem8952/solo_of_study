# 11일차 - 표준 입출력과 파일 입출력

## 오늘의 목표

- C의 스트림 개념을 이해한다.
- `printf`/`scanf` 계열과 문자/문자열 입출력 함수를 구분한다.
- 텍스트 파일과 바이너리 파일 입출력을 할 수 있다.
- 파일 위치 이동, 에러 확인, 버퍼링 개념까지 익힌다.

## 스트림(stream)이란?

C의 입출력은 `스트림`이라는 추상화로 다룹니다.

기본 스트림:

- `stdin`: 표준 입력
- `stdout`: 표준 출력
- `stderr`: 표준 오류 출력

파일 스트림은 `FILE *`로 다룹니다.

## `stdio.h`에서 자주 쓰는 함수 지도

| 분류 | 함수 |
| --- | --- |
| 형식 출력 | `printf`, `fprintf`, `sprintf`, `snprintf`, `vprintf`, `vfprintf`, `vsprintf`, `vsnprintf` |
| 형식 입력 | `scanf`, `fscanf`, `sscanf`, `vscanf`, `vfscanf`, `vsscanf` |
| 문자 입출력 | `getchar`, `putchar`, `fgetc`, `getc`, `fputc`, `putc`, `ungetc` |
| 문자열 입출력 | `fgets`, `fputs`, `puts` |
| 파일 열기/닫기 | `fopen`, `freopen`, `fclose` |
| 버퍼 입출력 | `fread`, `fwrite` |
| 위치 이동 | `fseek`, `ftell`, `rewind`, `fgetpos`, `fsetpos` |
| 상태 확인 | `feof`, `ferror`, `clearerr`, `perror` |
| 버퍼 제어 | `fflush`, `setbuf`, `setvbuf` |
| 파일 관리 | `remove`, `rename`, `tmpfile`, `tmpnam` |

## 형식 출력 함수

### `printf`

```c
printf("name=%s age=%d score=%.2f\n", name, age, score);
```

### `fprintf`

원하는 스트림으로 출력합니다.

```c
fprintf(stderr, "error: invalid input\n");
```

### `sprintf`, `snprintf`

문자열 버퍼에 출력합니다.

```c
char buf[100];
snprintf(buf, sizeof(buf), "value=%d", 42);
```

권장:

- `sprintf`보다 `snprintf`를 우선 고려
- 버퍼 크기를 넘지 않도록 제한

## 형식 입력 함수

### `scanf`

```c
int x;
scanf("%d", &x);
```

### `fscanf`

파일 스트림에서 형식 입력

```c
fscanf(fp, "%d", &x);
```

### `sscanf`

문자열에서 형식 입력

```c
sscanf("10 20", "%d %d", &a, &b);
```

주의:

- `scanf` 계열은 형식 문자열과 실제 인자 타입이 맞아야 합니다.
- 실패한 입력 개수를 반환하므로 반환값을 확인하는 습관이 좋습니다.

```c
if (scanf("%d", &x) != 1)
{
    fprintf(stderr, "정수 입력 실패\n");
}
```

## 문자 입출력

### `getchar`, `putchar`

```c
int ch = getchar();
putchar(ch);
```

`getchar`는 `int`로 받는 것이 좋습니다. EOF를 구분해야 하기 때문입니다.

### `fgetc`, `fputc`

```c
int ch = fgetc(fp);
fputc(ch, fp);
```

`getc`, `putc`도 비슷하지만 구현에 따라 매크로일 수 있습니다.

### `ungetc`

읽은 문자 하나를 다시 스트림에 밀어 넣습니다.

```c
ungetc(ch, fp);
```

## 문자열 입출력

### `fgets`

```c
char line[100];
fgets(line, sizeof(line), stdin);
```

특징:

- 최대 `size - 1`개 문자 읽음
- 줄바꿈이 들어올 수 있음
- 널 문자로 종료

### `fputs`

```c
fputs(line, stdout);
```

### `puts`

```c
puts("hello");
```

자동으로 줄바꿈을 추가합니다.

## `gets`는 왜 쓰면 안 되는가?

`gets`는 버퍼 크기를 알지 못해 매우 위험하며 표준에서 제거되었습니다. 문자열 입력은 `fgets`를 사용하세요.

## 파일 열기 `fopen`

```c
FILE *fp = fopen("data.txt", "r");
if (fp == NULL)
{
    perror("fopen");
    return 1;
}
```

### 파일 모드

| 모드 | 의미 |
| --- | --- |
| `"r"` | 읽기 전용, 파일이 있어야 함 |
| `"w"` | 쓰기 전용, 기존 파일 내용 삭제 또는 새 파일 생성 |
| `"a"` | 추가 쓰기, 파일 끝에 붙임 |
| `"r+"` | 읽기/쓰기 |
| `"w+"` | 읽기/쓰기, 기존 내용 삭제 |
| `"a+"` | 읽기/추가 쓰기 |
| `"rb"`, `"wb"`, `"ab"` | 바이너리 모드 |

Windows에서는 텍스트/바이너리 모드 차이가 특히 중요할 수 있습니다.

## 파일 닫기 `fclose`

```c
fclose(fp);
```

파일을 닫지 않으면 버퍼가 제대로 반영되지 않거나 자원이 낭비될 수 있습니다.

## 텍스트 파일 읽기 예제

```c
#include <stdio.h>

int main(void)
{
    FILE *fp = fopen("input.txt", "r");
    char line[256];

    if (fp == NULL)
    {
        perror("fopen");
        return 1;
    }

    while (fgets(line, sizeof(line), fp) != NULL)
    {
        printf("%s", line);
    }

    fclose(fp);
    return 0;
}
```

## 텍스트 파일 쓰기 예제

```c
#include <stdio.h>

int main(void)
{
    FILE *fp = fopen("output.txt", "w");

    if (fp == NULL)
    {
        perror("fopen");
        return 1;
    }

    fprintf(fp, "apple %d\n", 10);
    fprintf(fp, "banana %d\n", 20);

    fclose(fp);
    return 0;
}
```

## `fscanf` 예제

```c
char name[50];
int price;

while (fscanf(fp, "%49s %d", name, &price) == 2)
{
    printf("%s %d\n", name, price);
}
```

## 바이너리 입출력 `fread`, `fwrite`

```c
size_t fwrite(const void *ptr, size_t size, size_t count, FILE *stream);
size_t fread(void *ptr, size_t size, size_t count, FILE *stream);
```

예:

```c
int nums[5] = {1, 2, 3, 4, 5};
fwrite(nums, sizeof(int), 5, fp);
```

읽기:

```c
int nums[5];
fread(nums, sizeof(int), 5, fp);
```

주의:

- 바이너리 파일은 사람이 읽기 어렵습니다.
- 구조체를 바이너리로 그대로 저장할 때는 패딩, 엔디언, 버전 호환성을 주의해야 합니다.

## 파일 위치 이동

### `fseek`

```c
fseek(fp, 0, SEEK_SET); // 처음으로
fseek(fp, 0, SEEK_END); // 끝으로
```

### `ftell`

```c
long pos = ftell(fp);
```

### `rewind`

```c
rewind(fp);
```

### `fgetpos`, `fsetpos`

더 이식성 있는 위치 저장/복원 도구입니다.

```c
fpos_t pos;
fgetpos(fp, &pos);
fsetpos(fp, &pos);
```

## 파일 크기 구하기 예제

```c
fseek(fp, 0, SEEK_END);
long size = ftell(fp);
rewind(fp);
```

## EOF와 오류 확인

### `feof`

파일 끝에 도달했는지 확인

### `ferror`

읽기/쓰기 오류가 났는지 확인

### `clearerr`

오류/EOF 상태를 초기화

주의:

`while (!feof(fp))` 패턴은 초보자가 자주 잘못 쓰는 방식입니다. 보통 실제 읽기 함수의 반환값을 기준으로 반복해야 합니다.

좋은 예:

```c
while (fgets(line, sizeof(line), fp) != NULL)
{
    ...
}
```

## `fflush`

출력 버퍼를 비웁니다.

```c
fflush(stdout);
```

주의:

- 출력 스트림에서 사용은 의미가 있습니다.
- `fflush(stdin)`은 표준 C에서 정의된 동작이 아닙니다.

## 버퍼링 제어

### `setbuf`

```c
setbuf(stdout, NULL); // 비버퍼링
```

### `setvbuf`

```c
setvbuf(fp, NULL, _IOFBF, 1024);
```

일반 학습 단계에서는 개념만 알아도 충분합니다.

## 파일 삭제/이름 변경

```c
remove("old.txt");
rename("temp.txt", "final.txt");
```

## 임시 파일

```c
FILE *tmp = tmpfile();
```

`tmpnam`도 있지만 충돌과 안전성 측면에서 주의가 필요합니다.

## 예제: 파일 복사 프로그램

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    FILE *src = fopen("source.txt", "r");
    FILE *dst = fopen("dest.txt", "w");
    int ch;

    if (src == NULL || dst == NULL)
    {
        perror("fopen");
        if (src != NULL) fclose(src);
        if (dst != NULL) fclose(dst);
        return EXIT_FAILURE;
    }

    while ((ch = fgetc(src)) != EOF)
    {
        fputc(ch, dst);
    }

    fclose(src);
    fclose(dst);
    return EXIT_SUCCESS;
}
```

## 실수 포인트

- 파일 열기 실패 검사 누락
- `scanf` 반환값 확인 안 함
- 버퍼 크기보다 큰 문자열을 읽음
- `while (!feof(fp))` 형태 사용
- 텍스트/바이너리 모드 차이를 무시함
- 파일을 열고 닫지 않음

## 체크 문제

1. `stdin`, `stdout`, `stderr`는 각각 무엇인가?
2. `printf`와 `fprintf`의 차이는 무엇인가?
3. `fgets`가 `scanf("%s", ...)`보다 안전한 이유는 무엇인가?
4. `fread`, `fwrite`는 어떤 상황에서 유용한가?

## 직접 해볼 과제

### 과제 1

텍스트 파일의 줄 수, 단어 수, 문자 수를 세는 프로그램을 작성하세요.

### 과제 2

사용자로부터 문장을 입력받아 파일에 저장한 뒤 다시 읽어 출력하세요.

### 과제 3

정수 100개를 바이너리 파일에 저장하고 다시 읽어 합계를 구하세요.

## 오늘 정리

- `stdio.h`는 콘솔 입출력과 파일 입출력의 중심입니다.
- 스트림과 버퍼 개념을 이해하면 함수들의 역할이 정리됩니다.
- 파일 처리에서는 `실패 검사`, `버퍼 크기`, `닫기`가 매우 중요합니다.

