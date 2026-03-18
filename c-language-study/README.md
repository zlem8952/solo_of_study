# C Language Study Pack

이 폴더는 15개의 분량으로 나눈 C 언어 학습용 마크다운 문서 모음입니다. GitHub 공개방식으로 설계되었으며, 초보자도 따라갈 수 있게 설명과 코드 예제를 충분히 넣었습니다.

## 학습 기준

- 기준 표준: `표준 C(C11/C17 중심)`
- 범위: 문법, 포인터, 함수, 구조체, 전처리기, 표준 라이브러리 핵심 함수, 파일 입출력, 디버깅, 실전 예제
- 제외: 운영체제별 비표준 함수와 컴파일러 전용 확장 문법은 일부 언급만 하고, 핵심은 `표준 C`에 맞춥니다.

## 추천 공부 방법

1. 하루에 문서 1개씩 읽습니다.
2. 문법만 보지 말고 예제를 직접 컴파일합니다.
3. 문서 끝의 실수 포인트와 연습 과제를 반드시 확인합니다.
4. 이해가 잘 안 되는 부분은 이전 문서로 돌아가 반복합니다.
5. 15일을 한 바퀴 돈 뒤에는 `11~15일차`를 중심으로 다시 반복합니다.

## 목차

1. [01_c_overview_and_setup.md](./01_c_overview_and_setup.md) - C 언어 개요, 첫 프로그램, 컴파일 흐름
2. [02_types_variables_input_output.md](./02_types_variables_input_output.md) - 자료형, 변수, 상수, 입력과 출력
3. [03_operators_and_expressions.md](./03_operators_and_expressions.md) - 연산자, 형 변환, 표현식
4. [04_control_flow.md](./04_control_flow.md) - 조건문, 반복문, 제어문
5. [05_arrays_strings.md](./05_arrays_strings.md) - 배열, 문자열, 메모리 배치
6. [06_pointers.md](./06_pointers.md) - 포인터의 핵심 개념과 사용법
7. [07_functions_scope_storage.md](./07_functions_scope_storage.md) - 함수, 재귀, 스코프, 저장 기간
8. [08_struct_union_enum_typedef.md](./08_struct_union_enum_typedef.md) - 구조체, 공용체, 열거형, typedef
9. [09_dynamic_memory_and_errors.md](./09_dynamic_memory_and_errors.md) - 동적 메모리, 명령행 인자, 오류 처리
10. [10_preprocessor_and_modular_build.md](./10_preprocessor_and_modular_build.md) - 전처리기, 헤더, 분할 컴파일
11. [11_stdio_and_file_io.md](./11_stdio_and_file_io.md) - 표준 입출력과 파일 입출력
12. [12_string_ctype_stdlib.md](./12_string_ctype_stdlib.md) - 문자열/문자/유틸리티 함수
13. [13_math_time_locale_signal.md](./13_math_time_locale_signal.md) - 수학, 시간, 로케일, 신호 처리
14. [14_advanced_headers_and_patterns.md](./14_advanced_headers_and_patterns.md) - 고급 헤더, 가변 인자, 비트, 현대 표준 기능
15. [15_projects_debugging_and_study_guide.md](./15_projects_debugging_and_study_guide.md) - 실전 예제, 디버깅, 테스트, 학습 로드맵

## 빠른 컴파일 예시

```bash
gcc -std=c11 -Wall -Wextra -pedantic main.c -o main
./main
```

Windows의 `gcc` 환경이라면:

```bash
gcc -std=c11 -Wall -Wextra -pedantic main.c -o main.exe
main.exe
```

## 꼭 기억할 점

- C는 문법이 단순해 보여도 `메모리`, `포인터`, `정의되지 않은 동작(Undefined Behavior)`을 이해하지 않으면 금방 막힙니다.
- `왜 이렇게 동작하는가`를 항상 메모리 관점으로 생각해야 실력이 빨리 늘어납니다.
- 라이브러리 함수는 외우는 것보다 `언제 어떤 헤더에서 무엇을 써야 하는지` 체계적으로 정리하는 것이 더 중요합니다.

