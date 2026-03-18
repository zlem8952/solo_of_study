# Java Language Study Pack

이 폴더는 20개의 분량으로 나눈 Java 학습용 마크다운 모음입니다. GitHub에 공개방식으로 설계되었으며, 문법 설명과 코드 예제를 충분히 넣어 초보자도 따라갈 수 있게 만들었습니다.

## 학습 기준

- 기준 버전: `현대 Java 17+ 기준`
- 범위: 문법, 객체지향, 예외 처리, 컬렉션, 제네릭, 람다, 스트림, 파일 입출력, 동시성, 리플렉션, 네트워크, JVM 개념
- 주의: Java는 버전에 따라 문법과 API가 조금씩 넓어집니다. 이 문서는 `핵심 Java SE 학습`에 초점을 둡니다.

## 추천 공부 방식

1. 하루에 문서 1개씩 읽습니다.
2. 예제를 직접 `javac`로 컴파일하고 `java`로 실행합니다.
3. 각 문서의 실수 포인트와 연습 과제를 반드시 확인합니다.
4. 처음 10일은 문법과 객체지향 이해에 집중하고, 11일 이후부터 라이브러리와 실전 감각을 키웁니다.
5. 20일을 한 바퀴 돈 뒤에는 11~20일차를 다시 보며 작은 프로젝트를 직접 만들어 봅니다.

## 목차

1. [01_java_overview_and_setup.md](./01_java_overview_and_setup.md) - Java 개요, JDK/JRE/JVM, 첫 프로그램
2. [02_variables_types_operators_io.md](./02_variables_types_operators_io.md) - 변수, 자료형, 연산자, 기본 입출력
3. [03_control_flow_arrays.md](./03_control_flow_arrays.md) - 조건문, 반복문, 배열
4. [04_methods_and_basic_design.md](./04_methods_and_basic_design.md) - 메서드, 오버로딩, 재귀, 설계 기초
5. [05_classes_objects_constructors.md](./05_classes_objects_constructors.md) - 클래스, 객체, 필드, 생성자
6. [06_encapsulation_packages_static_final.md](./06_encapsulation_packages_static_final.md) - 캡슐화, 접근 제어자, 패키지, `static`, `final`
7. [07_inheritance_polymorphism_object.md](./07_inheritance_polymorphism_object.md) - 상속, 다형성, `Object`
8. [08_abstract_classes_interfaces_records_enums.md](./08_abstract_classes_interfaces_records_enums.md) - 추상 클래스, 인터페이스, 레코드, 열거형
9. [09_exceptions_and_resource_management.md](./09_exceptions_and_resource_management.md) - 예외 처리, 사용자 정의 예외, `try-with-resources`
10. [10_strings_wrappers_formatting.md](./10_strings_wrappers_formatting.md) - 문자열, 래퍼 클래스, 포맷팅, 유틸리티
11. [11_collections_list_set_queue.md](./11_collections_list_set_queue.md) - 컬렉션 프레임워크: `List`, `Set`, `Queue`
12. [12_maps_generics_comparable_comparator.md](./12_maps_generics_comparable_comparator.md) - `Map`, 제네릭, 정렬 전략
13. [13_lambdas_streams_optional.md](./13_lambdas_streams_optional.md) - 람다, 함수형 인터페이스, 스트림, `Optional`
14. [14_io_files_nio_serialization.md](./14_io_files_nio_serialization.md) - 파일 입출력, NIO.2, 직렬화
15. [15_datetime_locale_regex.md](./15_datetime_locale_regex.md) - 날짜/시간, 로케일, 정규식
16. [16_concurrency_threads_executors_locks.md](./16_concurrency_threads_executors_locks.md) - 스레드, 실행기, 동기화, 동시성 유틸리티
17. [17_nested_classes_annotations_reflection.md](./17_nested_classes_annotations_reflection.md) - 중첩 클래스, 애너테이션, 리플렉션
18. [18_networking_http_modules_build.md](./18_networking_http_modules_build.md) - 네트워크, HTTP 클라이언트, 모듈, 빌드/배포
19. [19_jvm_memory_gc_testing_debugging.md](./19_jvm_memory_gc_testing_debugging.md) - JVM 메모리, GC, 테스트, 디버깅, 성능
20. [20_practice_projects_and_library_map.md](./20_practice_projects_and_library_map.md) - 실전 예제, 표준 라이브러리 지도, 학습 로드맵

## 빠른 실행 예시

```bash
javac Main.java
java Main
```

패키지가 있는 경우:

```bash
javac -d out src/com/example/Main.java
java -cp out com.example.Main
```

## 꼭 기억할 점

- Java는 C보다 메모리를 직접 다루는 범위는 줄어들지만, 대신 `객체지향`, `타입 시스템`, `라이브러리`, `동시성`, `JVM`을 이해해야 잘할 수 있습니다.
- 문법만 외우기보다 `왜 클래스로 나누는지`, `왜 인터페이스를 쓰는지`, `왜 컬렉션과 제네릭이 필요한지`를 연결해서 이해해야 실력이 빨리 늡니다.
- Java는 표준 라이브러리가 넓어서 함수 하나하나를 외우기보다 `어떤 문제를 어떤 패키지로 해결하는가`를 체계적으로 익히는 것이 중요합니다.

