---
title: "💾 [CS] I/O 바운드 작업(I/O-Bound Task)이란 무엇일까요?"
tags:
    - CS
date: "2024-10-24"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] I/O 바운드 작업(I/O-Bound Task)이란 무엇일까요?
- **I/O 바운드 작업(I/O-Bound Task)은 입출력(Input/Output)** 작업의 속도에 의해 **전체 작업의 성능에 제한되는 작업을 의미합니다.**
- 즉, 프로그램이 실행되는 동안 **CPU가 데이터를 처리하는 것보다, 데이터를 읽고 쓰는 작업(I/O)에서 더 많은 시간이 소비되는 상황을 말합니다.**

## 1️⃣ I/O 바운드 작업(I/O-Bound Task)의 예.

### 1️⃣ 파일 입출력.
- 하드 디스크에 파일을 **읽거나 쓰는 작업**은 I/O 바운드 작업(I/O-Bound Task)의 대표적인 예입니다.
    - 예를 들어, 대용량 파일을 읽어와서 처리하거나, 결과를 파일에 저장할 때, **디스크의 읽기/쓰기 속도**에 따라 작업의 전체 시간이 결정됩니다.

### 2️⃣ 네트워크 통신.
- 인터넷에서 **데이터를 다운로드하거나 업로드하는 작업은** 네트워크 속도에 의존합니다.
    - 예를 들어, 웹 페이지에서 데이터를 가져오거나 API 서버로 요청을 보내는 작업은 네트워크의 **지연 시간(latency)과 대역폭(bandwidth)에** 의해 제한됩니다.

### 3️⃣ 데이터베이스 쿼리.
- 데이터베이스에서 **데이터를 조회하거나 삽입하는 작업은** I/O 바운드 작업(I/O-Bound Task)입니다.
    - 디스크에 저장된 데이터를 읽어오거나, 데이터를 저장할 때 **디스크 속도와 데이터베이스의 처리 성능**이 작업의 속도를 좌우합니다.

## 2️⃣ I/O 바운드 vs CPU 바운드

### 1️⃣ I/O 바운드(I/O Bound)
- **작업의 성능이 I/O 속도에 의해 제한되는 상황입니다.**
    - **네트워크, 디스크, 데이터베이스 등의 입출력 장치에서 데이터를 읽거나 쓰는 속도가 전체 성능에 영향을 미칩니다.**
- 예를 들어, **파일 다운로드나 데이터베이스에서 대량의 데이터 조회가 I/O 바운드 작업입니다.**
    - 이 경우, CPU는 데이터 처리할 준비가 되어 있어도, **디스크나 네트워크에서 데이터를 받아오는 동안 대기하게 됩니다.**

### 2️⃣ CPU 바운드(CPU Bound)
- 작업의 성능이 **CPU의 처리 속도에 의해 제한되는 상황입니다.**
    - **복잡한 계산이나 데이터 처리 작업**이 많아 CPU가 대부분의 시간을 **계산 작업에 소비할 때 발생합니다.**
- 예를 들어, **암호화, 이미지 처리, 머신 러닝 모델 학습** 등이 CPU 바운드 작업입니다.
    - 이 경우, 작업 속도는 **CPU 처리 성능**에 의해 결정됩니다.

## 3️⃣ I/O Bound 작업 최적화.

### 1️⃣ 비동기 프로그래밍(Asynchronous Programming)
- **비동기 I/O(Asynchronous I/O)는** I/O 작업(I/O Task)이 완료될 때까지 **CPU가 대기하지 않고,** 다른 작업을 계속 수행항 수 있도록 합니다.
    - 이를 통해 **I/O 대기 시간을 줄이고,** CPU를 더 효율적으로 사용할 수 있습니다.
- 예를 들어, 파일을 다운로드하는 동안 다른 작업을 처리할 수 있어, 작업의 **전체 처리 속도**를 개선할 수 있습니다.

### 👉 예시 (Java 비동기 I/O)
```java
import java.nio.file.*;
import java.nio.charset.StandardCharsets;
import java.util.concurrent.CompletableFuture;

public class AsyncFileRead {
    public static void main(String[] args) {
        CompletableFuture.runAsync(() -> {
            try {
                String content = Files.readString(Paths.get("example.txt")), StandardCharsets.UTF_8);
                System.out.println(content);
            } catch (Exception e) {
                e.printStackTrace();
            }
        });
        
        System.out.println("파일 읽기 요청 완료");
    }
}
```

- 위 코드는 **비동기**로 파일을 읽기 때문에, 파일을 읽는 동안 다른 작업을 수행할 수 있습니다.

### 2️⃣ 멀티스레딩(Multithreading)
- 여러 **스레드(Thread)를** 사용하여 동시에 I/O 작업을 수행할 수 있습니다.
    - 이를 통해 I/O 작업이 대기하는 동안 다른 스레드가 **CPU를 사용**하여 효율성을 높일 수 있습니다.
- 예를 들어, 네트워크에서 여러 데이터를 동시에 가져와야 할 때, 각 데이터를 개별 스레드(Thread)에서 처리하도록 하여 **병렬 처리**를 구현할 수 있습니다.

### 👉 예시(Java 멀티스레딩)
```java
public class MultiThreadedDownload {
    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> downloadFile("http://example.com/file1"));
        Thread thread2 = new Thread(() -> downloadFile("http://example.com/file2"));
        
        thread1.start();
        thread2.start();
    }
    
    public static void downloadFile(String url) {
        // 네트워크 파일 다운로드 작업 수행
        System.out.println("Downloading from " + url);
    }
}
```

### 3️⃣ 캐싱(Caching).
- 자주 사용하는 데이터를 **메모리에 저장해두고, 필요할 때마다 빠르게 접근할 수 있도록 합니다.** 
    - 이를 통해 디스크나 네트워크에 **불필요한 I/O 요청**을 줄일 수 있습니다.
- 예를 들어, 웹 페이지를 로드할 때, 정적인 리소스를 **캐싱(Caching)하면** 네트워크 요청을 줄이고 더 빠르게 로드할 수 있습니다.

### 4️⃣ 효율적인 데이터 처리.
- **데이터를 대량으로 한 번에 처리하여 I/O 요청을 줄입니다.**
    - 예를 즐어, 데이터베이스에 한 번에 여러 행을 삽입하거나 조회하는 **배치 작업(batch processing)을** 통해, 개별적으로 여러 번 처리하는 것보다 더 효율적일 수 있습니다.

### 👉 예시(Java 데이터베이스 배치 처리)
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;

public class BatchProcessingExample {
    public static void main(String[] args) {
        try (Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "user", "password")) {
            String query = "INSERT INTO students (name, age) VALUES (?, ?)";
            PreparedStatement pstmt = conn.prepareStatement(query);
            
            conn.setAutoCommit(false);
            
            for (int i = 0; i < 100; i++) {
                pstmt.setString(1, "Student " + i);
                pstmt.setInt(2, 20 + i);
                pstmt.addBatch();
            }
            
            pstmt.executeBatch();
            conn.commit();
            
            System.out.println("Batch insertion completed.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## 4️⃣ 요약.
- **I/O 바운드 작업**은 **입출력 작업의 속도**에 의해 성능이 제한되는 작업을 말합니다.
    - 파일 읽기/쓰기, 네트워크 통신, 데이터베이스 조회 등이 대표적인 I/O 바운드 작업입니다.
        - 이러한 작업은 CPU가 데이터를 처리하는 것보다 **입출력 장치에서 데이터를 가져오거나 보내는 시간이 더 오래 걸리는** 경우가 많아, CPU는 대기 상태에 머무르게 됩니다.
        - 이를 개선하기 위해 **비동기 프로그래밍, 멀티스레딩, 캐싱** 등의 기법을 활용하여 **입출력 효율을 높이고, 전체 시스템 성능을 향상 시킬 수 있습니다.**
