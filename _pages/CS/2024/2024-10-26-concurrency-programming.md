---
title: "💾 [CS] 동시성 프로그래밍(Concurrency Programming)이란 무엇일까요?"
tags:
    - CS
date: "2024-10-26"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 동시성 프로그래밍(Concurrency Programming)이란 무엇일까요?
- **동시성 프로그래밍(Concurrency Programming)은 여러 작업을 동시에 수행할 수 있도록 프로그램을 설계하는 기법입니다.** 
- 동시성(Concurrency)은 **작업의 실행 흐름을 겹치게 하거나 병렬로 처리**하여, **시스템의 효율성을 높이고 처리 시간을 줄이는 데 중점을 둡니다.**
- 동시성 프로그래밍(Concurrency Programming)은 **멀티스레딩, 멀티프로세싱을** 포함하여, **비동기 프로그래밍**과 같은 다양한 기법을 사용하여 시스템 자원을 최대한 활용할 수 있도록 합니다.

## 1️⃣ 동시성과 병렬성의 차이.
- 동시성과 병렬성은 종종 혼동되지만, **서로 다른 개념**입니다.

### 1️⃣ 동시성(Concurrency)
- **동시성(Concurrency)은 여러 작업이 실행되는 것처럼 보이게 하는 것을 의미합니다.**
    - 실제로는 **작업 간의 실행 시간을 쪼개어** 번갈아 가면서 실행하여, 여러 작업이 동시에 진행되는 것처럼 보이게 만듭니다.
- 예를 들어, 하나의 CPU 코어에서 두 개의 작업을 번갈아가며 빠르게 실행하면, 두 작업이 동시에 실행되는 것처럼 느껴집니다.
    - 이는 **멀티태스킹(Multitasking)의** 개념과 유사합니다.

### 2️⃣ 병렬성(Parallelism)
- **병렬성(Parallelism)**은 **여러 작업을 동시에 실행하는 것**을 의미합니다.
    - 동시성과 달리, 실제로 **여러 CPU 코어에서 여러 작업을 동시에** 처리합니다.
- 병렬성(Parallelism)은 멀티코어 프로세서 환경에서 주로 이루어지며, **하나의 작업을 여러 조각으로 나눠 여러 코어에서 동시에 처리할 수 있게 해줍니다**

## 2️⃣ 동시성 프로그래밍(Concurrency Programming)의 필요성

### 1️⃣ 효율적인 자원 사용.
- 컴퓨터 시스템은 **CPU, 메모리, I/O 장치**와 같은 다양한 자원이 있습니다.
    - 동시성 프로그래밍(Concurrency Programming)을 통해 이러한 자원을 효율적으로 사용할 수 있습니다.
- 예를 들어, 파일을 읽거나 네트워크 요청을 기다리는 동안 CPU가 유후 상태에 머무르지 않도록, 다른 작업을 실행할 수 있습니다.

> 📝 유후 상태(Idle State)
> 
> **CPU가 현재 실행할 작업이 없어서 대기하고 있는 상태를 의미합니다.**
> 즉, **CPU가 아무런 작업도 수행하지 않고 쉬고 있는 상태를** 뜻합니다.
> 
> 컴퓨터 시스템에서 CPU는 항상 **작업을 할당받아야 제대로 작동**할 수 있습니다.
> 그러나 **입출력 작업이 완료되기를 기다리거나, 다음 명령어가 준비되지 않은 경우** 등, 특정 상황에서는 CPU가 **유후 상태**로 머물게 됩니다.

### 2️⃣ 빠른 응답성.
- 동시성 프로그래밍(Concurrency Programming)은 **프로그램의 응답성을 향상**시킵니다.
    - 예를 들어, 사용자가 버튼을 클릭할 때마다 새로운 요청이 처리되도록 하고, 메인 스레드가 다른 작업을 수행하는 동안에도 UI가 멈추지 않게 할 수 있습니다.
- 이는 특히 **웹 서버**와 같은 환경에서 중요합니다.
    - 여러 사용자의 요청을 동시에 처리하여 빠르게 응답할 수 있습니다.

### 3️⃣ 배치 처리 및 병렬 계산.
- 동시성 프로그래밍(Concurrency Programming)은 **대량의 데이터를 병렬로 처리할 때 유리합니다.**
    - 예를 들어, 빅데이터 처리를 위해 여러 노드에서 데이터를 동시에 처리할 수 있습니다.

> 📝 노드(Node)
> 
> **컴퓨터 네트워크에서 데이터를 처리하거나 전송하는 장치나 시스템을 의미합니다.**
> 노드는 네트워크를 구성하는 **개별적인 장치로, 서버, 컴퓨터, 라우터, 스위치, 또는 클라이언트 등이 모두 노드가 될 수 있습니다.**
> **분산 시스템에서** 노드는 **각각 독립적으로 작업을 수행하거나, 서로 협력하여 큰 작업을 분담할 수 있습니다.**

## 3️⃣ 동시성 프로그래밍(Concurrency Programming)의 주요 개념.

### 1️⃣ 스레드(Thread)
- 스레드는 **프로세스 내에서 실행되는 작은 실행 단위**입니다.
    - 한 프로세스는 여러 스레드를 가질 수 있으며, 이들 스레드는 **메모리와 자원**을 공유합니다.
- 동시성 프로그래밍(Concurrency Programming)에서 **멀티스레딩(Multithreading)을** 통해 여러 스레드를 동시에 실행하여 작업을 동시에 처리할 수 있습니다.

### 2️⃣ 락(Lock)
- 여러 스레드가 **공유 자원에 동시에 접근하면 데이터 충돌(Race Condition)이** 발생할 수 있습니다.
    - 이를 방지하기 위해, 공유 자원에 접근할 때 **락을 사용**하여 한 번에 하나의 스레드만 자원에 접근하도록 합니다.
- 하지만 락을 잘못 사용할 경우 **데드락(DeadLock)이나 라이블록(Livelock)** 같은 문제가 발생할 수 있스므로, 주의가 필요합니다.

### 3️⃣ 뮤텍스(Mutex)와 세마포어(Semaphore)
- **뮤텍스(Mutex)는 두 개 이상의 스레드가 동시에 자원에 접근하는 것을 방지하기 위해 사용되는 락의 일종입니다.**
    - 특정 스레드가 자원을 사용하면, 다른 스레드는 그 자원이 **해제될 때까지 대기해야 합니다.**
- **세마포어(Semaphore)는 리소스의 접근 가능한 수량을 관리하여, 여러 스레드가 리소스레 접근하도록 제어할 수 있습니다.** 
    - 제한된 수의 스레드만 리소스에 접근할 수 있게끔 허용합니다.

### 4️⃣ 비동기 프로그래밍(Asynchronous Programming)
- 비동기 프로그래밍(Asynchronous Programming)은 **동시성 프로그래밍(Concurrency Programming)의 한 형태로, 특정 작업이 완료될 때 까지 대기하지 않고 다른 작업을 먼저 수행하도록 합니다.**
- 예를 들어, 네트워크 요청을 보내고 응답을 기다리는 동안, 프로그램은 다른 작업을 수행할 수 있습니다.
- Java에서는 `CompletableFuture`, JavaScript에서는 **`Promise`를** 사용하여 비동기 프로그래밍(Asynchronous Programming)을 쉽게 구현할 수 있습니다.

## 4️⃣ 동시성 프로그래밍(Concurrency Programming)의 예시.

### 1️⃣ 멀티스레딩 예시.
```java
class MyThread extends Thread {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getId() + ": " + i);
            try {
                Thread.sleep(500); // 0.5초 대기
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        MyThread t2 = new MYThread();
        t1.start(); // t1 스레드 시작
        t2.start(); // t2 스레드 시작
    }
}
```

- 위 코드는 두 개의 스레드를 생성하고, 동시에 실행시켜 **동시성을 구현하는 예시입니다.**

### 2️⃣ 비동기 프로그래밍 예시.
```java
import java.util.concurrent.CompletableFuture;

public class AsyncExample {
    public static void main(String[] args) {
        CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
            try {
                Thread.sleep(1000);
                System.out.println("비동기 작업 완료!");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        
        System.out.println("메인 스레드 작업 실행 중...");
        future.join(); // 비동기 작업이 끝날 때까지 대기
    }
}
```

- 이 코드는 비동기적으로 실행되는 작업을 만들고, 메인 스레드가 작업을 기다리지 않고 **다른 작업을 수행할 수 있게 합니다.**

## 5️⃣ 동시성 프로그래밍(Concurrency Programming)의 장점.

### 1️⃣ 자원 효율성 극대화.
- CPU가 작업을 대기하는 동안 유휴 상태로 있지 않고, 다른 작업을 수행할 수 있어 시스템 자원의 효율성을 높일 수 있습니다.

### 2️⃣ 빠른 응답성
- 여러 작업을 동시에 수행하거나, 특정 작업을 기다리는 동안에도 프로그램이 응답할 수 있도록 하여, 사용자의 **경험을 개선 합니다.**

### 3️⃣ 병렬 처리
- 여러 코어를 사용하는 멀티코어 프로세서 환경에서 **병렬 처리를 통해 작업 속도를 크게 향상시킬 수 있습니다.**

## 6️⃣ 동시성 프로그래밍(Concurrency Programming)의 단점.

### 1️⃣ 복잡성 증가.
- 여러 스레드가 동시에 작업을 수행할 때 **동기화 문제가 발생할 수 있어, 프로그램의 복잡성이 증가합니다.**
    - 개발자는 **데드락(DeadLock), 레이스 컨디션(Race Condition) 등 여러 문제를 해결해야 합니다.**

### 2️⃣ 디버깅의 어려움.
- 동시성 코드의 디버깅은 **다른 시검에 따라 다른 결과가 발생할 수 있기 때문에 예측하기 어려운 버그가 나타날 수 있습니다.**

### 3️⃣ 성능 오버헤드.
- 스레드를 생성하고 관리하는 데에도 비용이 발생하며, 동기화를 위해 **락을 사용**하면 성능이 저하될 수 있습니다.

## 6️⃣ 요약.
- **동시성 프로그래밍(Concurrency Programming)은** 여러 작업을 동시에 수행할 수 있도록 설계된 프로그래밍 방식으로, **멀티스레딩(Multilthreading), 비동기 프로그래밍(Asyncronous Programming), 멀티프로세싱(Multiprocessing)을** 포함하여 시스템 자원을 효율적으로 사용하고 프로그램의 **응답성을 높이는 것을 목표로 합니다.**
- 동시성 프로그래밍(Concurrency Programming)은 CPU가 작업을 대기하는 동안 유휴 상태로 있지 않고, 다른 작업을 수행하여 **성능을 최적화**합니다.
    - 그러나 코드의 **복잡성, 디버깅의 어려움, 성능 오버헤드** 등고 함께 고려해야 합니다.