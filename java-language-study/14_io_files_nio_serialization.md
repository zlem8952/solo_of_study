# 14일차 - 파일 입출력, NIO.2, 직렬화

## 오늘의 목표

- Java의 바이트 스트림과 문자 스트림을 구분한다.
- `File`, `Path`, `Files`를 사용해 파일을 읽고 쓸 수 있다.
- 버퍼링과 NIO.2 API의 장점을 이해한다.
- 직렬화의 개념과 주의점을 익힌다.

## Java I/O 큰 그림

### 바이트 스트림

- `InputStream`
- `OutputStream`

이미지, 바이너리 파일, 일반 바이트 단위 처리에 사용

### 문자 스트림

- `Reader`
- `Writer`

텍스트 처리에 적합

## 기본 파일 읽기

```java
import java.io.FileReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try (FileReader reader = new FileReader("data.txt")) {
            int ch;
            while ((ch = reader.read()) != -1) {
                System.out.print((char) ch);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 버퍼링

매번 한 글자씩 처리하기보다 버퍼를 두면 효율이 좋습니다.

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 주요 I/O 클래스

| 분류 | 클래스 |
| --- | --- |
| 바이트 입력 | `InputStream`, `FileInputStream`, `BufferedInputStream` |
| 바이트 출력 | `OutputStream`, `FileOutputStream`, `BufferedOutputStream` |
| 문자 입력 | `Reader`, `FileReader`, `BufferedReader` |
| 문자 출력 | `Writer`, `FileWriter`, `BufferedWriter`, `PrintWriter` |
| 객체 입출력 | `ObjectInputStream`, `ObjectOutputStream` |

## 파일 쓰기

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("out.txt"))) {
            bw.write("Hello");
            bw.newLine();
            bw.write("Java");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## `PrintWriter`

텍스트 출력이 편리합니다.

```java
import java.io.IOException;
import java.io.PrintWriter;

public class Main {
    public static void main(String[] args) {
        try (PrintWriter out = new PrintWriter("result.txt")) {
            out.println("name=Kim");
            out.printf("score=%.2f%n", 95.5);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## `File`

기존 Java 파일 API입니다.

```java
import java.io.File;

File file = new File("data.txt");
System.out.println(file.exists());
System.out.println(file.length());
```

하지만 현대 Java에서는 `Path`와 `Files`를 더 많이 사용합니다.

## NIO.2의 핵심: `Path`, `Paths`, `Files`

```java
import java.nio.file.Path;
import java.nio.file.Paths;

Path path = Paths.get("data.txt");
```

또는:

```java
Path path = Path.of("data.txt");
```

## `Files` 유틸리티

자주 쓰는 메서드:

- `exists`
- `readString`
- `writeString`
- `readAllLines`
- `copy`
- `move`
- `delete`
- `createDirectories`
- `list`

## `Files.readString`

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;

public class Main {
    public static void main(String[] args) throws IOException {
        String text = Files.readString(Path.of("data.txt"));
        System.out.println(text);
    }
}
```

## `Files.writeString`

```java
Files.writeString(Path.of("output.txt"), "Hello Java");
```

## 디렉터리 다루기

```java
Files.createDirectories(Path.of("logs/app"));
```

목록 조회:

```java
Files.list(Path.of("."))
    .forEach(System.out::println);
```

## 바이트 단위 파일 복사

```java
Files.copy(Path.of("a.txt"), Path.of("b.txt"));
```

## 문자셋(charset)

텍스트 파일은 인코딩이 중요합니다.

```java
import java.nio.charset.StandardCharsets;

Files.writeString(Path.of("utf8.txt"), "안녕하세요", StandardCharsets.UTF_8);
```

## 직렬화(Serialization)

객체 상태를 바이트 형태로 저장/복원하는 기능입니다.

```java
import java.io.Serializable;

class User implements Serializable {
    private static final long serialVersionUID = 1L;
    String name;
    int age;

    User(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

저장:

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("user.ser"))) {
    out.writeObject(new User("Kim", 20));
}
```

읽기:

```java
import java.io.FileInputStream;
import java.io.ObjectInputStream;

try (ObjectInputStream in = new ObjectInputStream(new FileInputStream("user.ser"))) {
    User user = (User) in.readObject();
    System.out.println(user.name);
}
```

## 직렬화 주의점

- 클래스 구조 변경 시 호환성 문제
- 보안 이슈
- 모든 객체를 무조건 직렬화하는 설계는 위험

실무에서는 JSON, DB, 명시적 DTO 변환이 더 적합한 경우가 많습니다.

## `transient`

직렬화에서 제외할 필드

```java
transient String password;
```

## 예제: 텍스트 파일 복사

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;

public class Main {
    public static void main(String[] args) throws IOException {
        Path source = Path.of("input.txt");
        Path target = Path.of("copy.txt");

        Files.copy(source, target);
        System.out.println("복사 완료");
    }
}
```

## 예제: 로그 파일 작성

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardOpenOption;

public class Main {
    public static void main(String[] args) throws IOException {
        Path log = Path.of("app.log");

        Files.writeString(
            log,
            "application started\n",
            StandardOpenOption.CREATE,
            StandardOpenOption.APPEND
        );
    }
}
```

## 실수 포인트

- 스트림을 닫지 않음
- 텍스트와 바이트 처리를 구분하지 않음
- 문자셋 문제를 무시함
- 오래된 `File` API만 고집하고 `Path`/`Files`를 모르고 넘어감
- 직렬화를 장기 저장 포맷처럼 오해함

## 체크 문제

1. 바이트 스트림과 문자 스트림 차이는 무엇인가?
2. `BufferedReader`가 왜 유용한가?
3. `File`보다 `Path`/`Files`가 더 현대적인 이유는 무엇인가?
4. 직렬화 사용 시 어떤 점을 주의해야 하는가?

## 직접 해볼 과제

### 과제 1

텍스트 파일의 줄 수를 세는 프로그램을 작성하세요.

### 과제 2

사용자 입력을 파일에 저장하고 다시 읽어 출력하세요.

### 과제 3

간단한 `User` 객체를 직렬화했다가 다시 읽어 오는 예제를 작성하세요.

## 오늘 정리

- Java I/O는 스트림 계층 구조를 이해하는 것이 핵심입니다.
- 현대 Java에서는 `Path`와 `Files`를 적극적으로 써야 생산성이 높습니다.
- 파일 처리 코드는 예외 처리와 자원 관리가 항상 함께 가야 합니다.

