# 18일차 - 네트워크, HTTP 클라이언트, 모듈, 빌드/배포

## 오늘의 목표

- Java의 기본 네트워크 API를 이해한다.
- `Socket`, `ServerSocket`, `HttpClient`의 역할을 안다.
- Java 모듈 시스템의 기본 구조를 이해한다.
- `javac`, `jar` 기반 빌드/배포 흐름을 익힌다.

## 네트워크 기본 개념

Java는 표준 라이브러리로 네트워크 프로그래밍을 지원합니다.

대표 패키지:

- `java.net`
- `java.net.http`

## `InetAddress`

호스트 이름과 IP 주소를 다룰 때 사용합니다.

```java
import java.net.InetAddress;

InetAddress address = InetAddress.getByName("example.com");
System.out.println(address.getHostAddress());
```

## `Socket`

TCP 클라이언트 연결에 사용합니다.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

public class Main {
    public static void main(String[] args) throws Exception {
        try (Socket socket = new Socket("example.com", 80);
             PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
             BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()))) {

            out.println("GET / HTTP/1.1");
            out.println("Host: example.com");
            out.println();

            String line;
            while ((line = in.readLine()) != null) {
                System.out.println(line);
            }
        }
    }
}
```

## `ServerSocket`

서버 측에서 연결을 기다립니다.

```java
import java.net.ServerSocket;
import java.net.Socket;

public class Main {
    public static void main(String[] args) throws Exception {
        try (ServerSocket server = new ServerSocket(5000)) {
            Socket client = server.accept();
            System.out.println("클라이언트 연결: " + client.getInetAddress());
            client.close();
        }
    }
}
```

## UDP

UDP 통신은 `DatagramSocket`, `DatagramPacket`을 사용합니다.

TCP보다 가볍지만 신뢰성 보장이 다릅니다.

감각적으로 정리하면:

- TCP: 연결, 순서, 신뢰성이 중요할 때
- UDP: 빠르고 단순한 전송이 중요할 때

## `URI`, `URL`

네트워크 주소를 표현하는 표준 클래스입니다.

```java
import java.net.URI;

URI uri = URI.create("https://example.com/api");
System.out.println(uri.getHost());
```

## `HttpClient`

현대 Java에서 HTTP 요청을 보낼 때 매우 유용합니다.

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class Main {
    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://example.com"))
            .GET()
            .build();

        HttpResponse<String> response =
            client.send(request, HttpResponse.BodyHandlers.ofString());

        System.out.println(response.statusCode());
        System.out.println(response.body());
    }
}
```

자주 함께 다루는 요소:

- 상태 코드
- 헤더
- 요청 메서드(`GET`, `POST`)
- 타임아웃
- 본문 처리 방식

예를 들어 헤더와 타임아웃도 지정할 수 있습니다.

```java
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://example.com/api"))
    .header("Accept", "application/json")
    .timeout(java.time.Duration.ofSeconds(5))
    .GET()
    .build();
```

## 비동기 HTTP

```java
client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
    .thenApply(HttpResponse::body)
    .thenAccept(System.out::println)
    .join();
```

## 모듈 시스템(Java Platform Module System)

Java 9부터 도입된 모듈 시스템입니다.

핵심 파일:

```java
module com.example.app {
    requires java.net.http;
    exports com.example.app;
}
```

장점:

- 의존성 명시
- 캡슐화 강화
- 큰 프로젝트 구조화에 도움

## 클래스패스와 모듈패스

### 클래스패스

전통적인 클래스 탐색 방식입니다.

### 모듈패스

모듈 시스템 기반 탐색 방식입니다.

초반에는 클래스패스를 먼저 확실히 이해하고, 이후 모듈 구조를 보는 편이 부담이 적습니다.

## 패키지와 모듈 차이

### 패키지

- 클래스 묶음

### 모듈

- 패키지 여러 개와 의존성을 아우르는 더 큰 단위

## 기본 빌드 흐름

### 단일 파일

```bash
javac Main.java
java Main
```

### 패키지 구조

```bash
javac -d out src/com/example/app/Main.java
java -cp out com.example.app.Main
```

## JAR 만들기

```bash
jar --create --file app.jar -C out .
```

실행 가능한 JAR:

```bash
jar --create --file app.jar --main-class=com.example.app.Main -C out .
java -jar app.jar
```

## 매니페스트

JAR에는 메타데이터를 담는 매니페스트가 포함될 수 있습니다.

예:

- 메인 클래스
- 버전 정보

## 배포할 때 같이 생각할 것

- 실행 진입점이 무엇인지
- 외부 설정 파일 위치
- 로그 파일 경로
- 의존성을 JAR에 어떻게 포함할지
- 실행할 Java 버전이 맞는지

## Maven / Gradle

표준 라이브러리는 아니지만 Java 생태계에서 매우 중요합니다.

### Maven

- XML 기반 빌드 설정

### Gradle

- 더 유연한 빌드 스크립트

학습 초기에는 `javac`/`jar` 흐름을 이해한 뒤, 이후 Maven/Gradle을 배우는 편이 좋습니다.

의존성 관리가 필요한 순간:

- 외부 라이브러리 사용
- 테스트 프레임워크 추가
- 반복 빌드 자동화
- 멀티 모듈 프로젝트 구성

## 예제: 간단한 클라이언트-서버 감각

클라이언트는 요청을 보내고, 서버는 포트를 열어 요청을 기다립니다. 실제 서버 애플리케이션은 여기에:

- 멀티스레드 처리
- 요청 파싱
- 라우팅
- 보안

이 추가됩니다.

## 실수 포인트

- 네트워크 예외를 무시함
- 소켓/스트림을 닫지 않음
- HTTP 요청/응답이 문자열만으로 끝난다고 생각함
- 패키지와 모듈 개념을 혼동함
- 빌드 도구를 쓰기 전에 컴파일 기본 흐름을 이해하지 않음
- 클래스패스 문제를 단순 경로 오타와 구분하지 못함
- 네트워크 코드에 타임아웃을 두지 않음

## 체크 문제

1. `Socket`과 `ServerSocket`은 각각 어떤 역할인가?
2. `HttpClient`는 언제 쓰는가?
3. 모듈 시스템의 장점은 무엇인가?
4. 실행 가능한 JAR은 어떻게 만드는가?

## 직접 해볼 과제

### 과제 1

`HttpClient`로 웹 페이지를 가져와 상태 코드와 길이를 출력하세요.

### 과제 2

간단한 `ServerSocket` 프로그램으로 포트 연결을 확인하세요.

### 과제 3

작은 패키지 구조 프로젝트를 컴파일해서 JAR로 묶어 보세요.

## 오늘 정리

- Java는 표준 라이브러리만으로도 기본 네트워크 프로그래밍이 가능합니다.
- `HttpClient`는 현대 Java에서 HTTP 처리의 핵심 도구입니다.
- 모듈과 빌드 흐름까지 이해하면 Java 프로젝트 구조가 훨씬 명확해집니다.
