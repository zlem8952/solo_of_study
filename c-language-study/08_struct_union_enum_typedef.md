# 8일차 - 구조체, 공용체, 열거형, typedef

## 오늘의 목표

- 여러 데이터를 하나로 묶는 구조체를 이해한다.
- 구조체 배열, 중첩 구조체, 포인터와 함께 쓰는 방법을 익힌다.
- 공용체와 열거형의 특징을 이해한다.
- `typedef`로 타입 별칭을 만드는 이유를 안다.

## 구조체 `struct`

서로 다른 자료형의 데이터를 하나로 묶는 사용자 정의 타입입니다.

```c
struct Student
{
    char name[20];
    int age;
    double score;
};
```

## 구조체 변수 선언

```c
struct Student s1;
```

멤버 접근:

```c
s1.age = 20;
s1.score = 95.5;
```

## 초기화

```c
struct Student s1 = {"Kim", 20, 95.5};
```

## 구조체 예제

```c
#include <stdio.h>

struct Student
{
    char name[20];
    int age;
    double score;
};

int main(void)
{
    struct Student s = {"Lee", 21, 88.5};

    printf("name: %s\n", s.name);
    printf("age: %d\n", s.age);
    printf("score: %.1f\n", s.score);

    return 0;
}
```

## 구조체 배열

```c
struct Student students[3] = {
    {"Kim", 20, 90.0},
    {"Lee", 21, 85.5},
    {"Park", 19, 92.0}
};
```

출력:

```c
for (int i = 0; i < 3; i++)
{
    printf("%s %d %.1f\n",
           students[i].name,
           students[i].age,
           students[i].score);
}
```

## 구조체 안에 구조체

```c
struct Date
{
    int year;
    int month;
    int day;
};

struct Event
{
    char title[30];
    struct Date when;
};
```

접근:

```c
event.when.year = 2026;
```

## 구조체 포인터와 `->`

포인터가 구조체를 가리킬 때는 `(*p).age` 대신 `p->age`를 많이 씁니다.

```c
struct Student s = {"Han", 22, 77.0};
struct Student *p = &s;

printf("%d\n", p->age);
printf("%.1f\n", p->score);
```

## 구조체를 함수에 전달

### 값으로 전달

```c
void print_student(struct Student s)
{
    printf("%s\n", s.name);
}
```

복사가 일어납니다.

### 포인터로 전달

```c
void update_score(struct Student *s, double score)
{
    s->score = score;
}
```

큰 구조체는 포인터로 전달하는 것이 효율적입니다.

### 읽기 전용 포인터

```c
void print_student(const struct Student *s)
{
    printf("%s %d %.1f\n", s->name, s->age, s->score);
}
```

## 구조체 정렬과 패딩

구조체는 멤버 사이에 패딩 바이트가 들어갈 수 있습니다.

```c
struct Example
{
    char a;
    int b;
};
```

겉보기에는 `1 + 4 = 5`바이트 같지만, 실제 크기는 정렬 때문에 더 클 수 있습니다.

```c
printf("%zu\n", sizeof(struct Example));
```

이것은 시스템과 컴파일러 규칙에 따라 달라질 수 있습니다.

## 공용체 `union`

같은 메모리 공간을 여러 타입이 공유합니다.

```c
union Data
{
    int i;
    float f;
    char str[20];
};
```

특징:

- 멤버들이 같은 메모리 위치를 공유
- 한 시점에 하나의 해석 방식만 유효하다고 생각해야 함
- 메모리 절약이나 저수준 해석에 사용

예:

```c
union Data d;
d.i = 10;
printf("%d\n", d.i);
```

다른 멤버에 값을 넣으면 이전 해석은 달라질 수 있습니다.

## 열거형 `enum`

관련된 정수 상수 집합을 이름으로 관리할 수 있습니다.

```c
enum Color
{
    RED,
    GREEN,
    BLUE
};
```

기본적으로 `RED = 0`, `GREEN = 1`, `BLUE = 2`

```c
enum Color c = GREEN;
```

값을 직접 지정할 수도 있습니다.

```c
enum Status
{
    STATUS_OK = 200,
    STATUS_NOT_FOUND = 404,
    STATUS_ERROR = 500
};
```

## `typedef`

타입에 별칭을 부여합니다.

```c
typedef unsigned int uint;
```

구조체와 함께 자주 사용:

```c
typedef struct
{
    char name[20];
    int age;
} Person;
```

이제 `struct` 키워드 없이:

```c
Person p;
```

## 구조체 + 열거형 조합

```c
typedef enum
{
    STUDENT_ACTIVE,
    STUDENT_LEAVE,
    STUDENT_GRADUATED
} StudentStatus;

typedef struct
{
    char name[20];
    int age;
    StudentStatus status;
} Student;
```

## 비트 필드 기초

구조체 멤버의 비트 수를 제한할 수 있습니다.

```c
struct FlagSet
{
    unsigned int is_admin : 1;
    unsigned int is_guest : 1;
    unsigned int level : 3;
};
```

주의:

- 구현 의존적인 부분이 있으므로 바이너리 호환성에 민감한 경우 조심해야 합니다.

## 예제: 좌표 구조체

```c
#include <stdio.h>

typedef struct
{
    int x;
    int y;
} Point;

void move_point(Point *p, int dx, int dy)
{
    p->x += dx;
    p->y += dy;
}

int main(void)
{
    Point pt = {3, 4};
    move_point(&pt, 2, -1);
    printf("(%d, %d)\n", pt.x, pt.y);
    return 0;
}
```

## 예제: 학생 평균 계산

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

int main(void)
{
    Student s = {"Kim", 90, 80, 100};
    printf("%s 평균: %.2f\n", s.name, average(&s));
    return 0;
}
```

## 실수 포인트

- 구조체 멤버 접근에서 `.`과 `->`를 혼동함
- 큰 구조체를 값으로 무심코 복사함
- 공용체를 사용할 때 어떤 멤버가 현재 유효한지 추적하지 않음
- `typedef`가 새로운 타입 시스템을 만드는 것으로 오해함

## 체크 문제

1. 구조체와 배열의 차이는 무엇인가?
2. 구조체 포인터에서 왜 `->`를 쓰는가?
3. 공용체는 왜 메모리를 절약할 수 있는가?
4. 열거형은 왜 코드 가독성을 높이는가?

## 직접 해볼 과제

### 과제 1

책 정보를 저장하는 구조체를 만들고 제목, 저자, 가격을 출력하세요.

### 과제 2

학생 5명의 정보를 구조체 배열에 저장하고 평균 점수 순으로 출력해 보세요.

### 과제 3

`enum`을 이용해 요일을 정의하고 현재 요일 이름을 출력하는 코드를 작성하세요.

## 오늘 정리

- 구조체는 여러 종류의 데이터를 하나로 묶습니다.
- 구조체 포인터에는 `->`를 사용합니다.
- 공용체는 메모리를 공유합니다.
- 열거형과 `typedef`를 적절히 쓰면 코드가 훨씬 읽기 쉬워집니다.

