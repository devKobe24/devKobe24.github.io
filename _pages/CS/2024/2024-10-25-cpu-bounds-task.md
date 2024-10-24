---
title: "💾 [CS] CPU 바운드 작업(CPU-Bound Task)이란 무엇일까요?"
tags:
    - CS
date: "2024-10-25"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] CPU 바운드 작업(CPU-Bound Task)이란 무엇일까요?
- **CPU 바운드 작업(CPU-Bound Task)은** 프로그램의 실행 속도가 **CPU의 처리 속도에 의해 제한되는 작업을 의미**합니다.
- 즉, **연산이나 계산 작업이 많아서** CPU가 **대부분의 시간을 계산 처리에 사용하며, CPU의 성능이 전체 작업의 성능을 결정짓는 상황입니다.**

## 1️⃣ CPU 바운드 작업(CPU-Bound Task)의 특징.

### 1️⃣ 복잡한 계산 작업.
- CPU 바운드 작업(CPU-Bound Task)은 일반적으로 **복잡한 수학 계산, 데이터 처리, 암호화, 머신 러닝 등** 많은 연산을 필요로 하는 작업이 포함됩니다.
- CPU가 **계속해서 계산 작업을 수행**하며, 다른 하드웨어(예: 디스크, 네트워크)의 속도에 의존하지 않고 CPU의 속도에 의해 성능이 결정됩니다.

### 2️⃣ CPU 사용률이 높음.
- CPU 바운드 작업(CPU-Bound Task)은 CPU의 **연산 자원(CPU 코어(CPU Core)와 클럭 속도(Clock Speed))을** 최대한 활용합니다.

> 📝 CPU Core
> 
> **중앙처리장치(Central Processing Unit, CPU) 내부에서 독립적으로 작업을 수행할 수 있는 처리 단위를 의미합니다.** 
> 각 코어(Core)는 **자신만의 연산 장치(ALU), 레지스터, 제어 장치 등을 갖추고 있어, 프로그램의 명령을 독립적으로 처리할 수 있습니다.**
> 현대의 CPU는 **여러 개의 Core(Multi Core)를** 포함하고 있어, 동시에 여러 작업을 병렬로 처리할 수 있습니다.

> 📝 클럭 속도(Clock Speed)
> 
> **클럭 속도(Clock Speed)는 CPU가 작업을 수행하는 속도를 나타내는 지표로, 1초 동안 CPU가 실행할 수 있는 명령어 사이클의 수를 의미합니다.**
> 클럭 속도(Clock Speed)는 **헤르츠(Hz)** 단위로 측정되며, **메가헤르츠(MHz)** 또는 **기가헤르츠(GHz)로** 표현됩니다.
> 
> 예를 들어, **1GHz는** CPU가 **1초에 10억 번의 명령어 사이클을 처리할 수 있다는 의미입니다.**
> **클럭 속도(Clock Speed)가 높을수록 CPU가 더 빠르게 작동하여, 더 많은 작업을 짧은 시간 안에 처리할 수 있습니다.**

- 프로그램을 실행할 때 **CPU 사용률(CPU Utilization)이** 높게 나타나며, CPU 성능이 전체 작업 속도에 큰 영향을 미칩니다.

## 2️⃣ CPU 바운드 작업(CPU-Bound Task)의 예.

### 1️⃣ 이미지 처리.
- 대량의 이미지를 변환하거나, 필터를 적용하는 작업은 **픽셀 단위로 많은 계산을 필요로 합니다.**
    - 이러한 작업은 CPU의 **계산 속도**에 의해 처리 속도가 결정됩니다.

### 2️⃣ 비디오 인코딩/디코딩.
- 비디오 파일을 인코딩하거나 디코딩하는 작업은 대량의 데이터 처리가 필요하고, 복잡한 알고리즘을 통해 **프레임을 압축하거나 복원해야 하기 때문에 CPU 바운드 작업(CPU-Bound Task)에 해당합니다.**

### 3️⃣ 암호화/복호화.
- 데이터를 **암호화하거나 복호화하는 과정은** CPU가 복잡한 계산을 수행해야 합니다.
    - 이는 주로 보안과 관련된 작업에서 자주 사용됩니다.

### 4️⃣ 과학 계산 및 데이터 분석.
- **데이터 분석, 수학적 모델링, 시뮬레이션 등 수많은 계산을 필요로 하는 작업도 CPU 바운드 작업입니다.**
    - 예를 들어, **대규모 데이터셋에서 통계적인 계산을 하는 작업은 CPU의 처리 성능에 크게 의존합니다.**

### 5️⃣ 게임 로직 및 물리 엔진.
- 게임 내의 **물리 연산, AI 처리, 게임 로직** 등은 많은 계산을 필요로 하며, 실시간으로 계산을 수행해야 하기 때문에 CPU 바운드 작업에 속합니다.

## 3️⃣ CPU 바운드 vs I/O 바운드

### 1️⃣ CPU 바운드
- 프로그램의 성능이 **CPU 처리 속도에 의해 제한됩니다.**
- **복잡한 계산**이나 **연산 작업을** 수행하는 경우 CPU 바운드가 됩니다.
- 예: 이미지 처리, 비디오 인코딩, 암호화 등.

### 2️⃣ I/O 바운드
- 프로그램의 성능이 **입출력(I/O) 작업의 속도에 의해 제한됩니다.**
- **네트워크 통신, 파일 읽기/쓰기, 데이터베이스 쿼리** 등 외부 장치의 속도에 따라 성능이 결정됩니다.
- 예: 파일 다운로드, 데이터베이스 조회, 웹 페이지 요청 등

## 4️⃣ CPU 바운드 작업의 최적화.

### 1️⃣ 병렬 처리(Parallel Processing)
- CPU 바운드 작업은 **멀티코어 CPU를** 사용하여 여러 코어에서 동시에 작업을 수행하도록 하여 성능을 향상시킬 수 있습니다.
- Java에서는 **멀티스레딩(Multithreading)을** 사용하여 여러 스레드에서 동시에 작업을 처리하도록 하거나 **Fork/Join Framework**를 사용하여 태스크를 분할하고 병렬로 처리할 수 있습니다.

### 👉 예시(Java에서 Fork/Join Framework)
```java
import java.util.concurrent.RecursiveTask;
import java.util.concurrent.ForkJoinPool;

public class SumTask extends RecursiveTask<Long> {
    private final int[] array;
    private final int start;
    private final int end;
    
    public SumTask(int[] array, int start, int end) {
        this.array = array;
        this.start = start;
        this.end = end;
    }
    
    @Override
    protected Long compute() {
        if (end - start <= 10) { // 작은 작업은 직접 계산
            long sum = 0;
            for (int i = start; i < end; i++) {
                sum += array[i];
            }
            return sum;
        } else { // 큰 작업은 분할
            int mid = (start + end) / 2;
            SumTask 
        }
    }
    
    public static void main(String[] args) {
        int[] array = new int[1000];
        ForkJoinPool pool = new ForkJoinPool();
        SumTask task = new SumTask(array, 0, array.length);
        long sum = pool.invoke(task);
        System.out.println("Sum: " + sum);
    }
}
```

### 2️⃣ 연산 최적화.
- 반복적인 계산을 줄이기 위해 **알고리즘을 최적화**하거나, **복잡한 연산을 간단하게 변경하여 성능을 향상 시킬 수 있습니다.**
- 예를 들어, 데이터가 이미 **정렬되어 있는지 확인하는 작업**에서는 매번 정렬을 수행하기보다 **정렬 여부를** 미리 체크하여 불필요한 연산을 줄이는 방식으로 최적화할 수 있습니다.

### 3️⃣ 캐싱(Caching)
- 반복적으로 계산되는 값들을 **캐시 메모리에 저장해두고,** 필요할 때마다 다시 계싼하지 않고 **저장된 값을 사용하여 성능을 높일 수 있습니다.**
- CPU 바운드 작업에서는 중복된 계산이 발생하지 않도록 **메모이제이션(Memoization)** 기법을 사용하여 성능을 향상시킬 수 있습니다.

## 5️⃣ 요약.
- **CPU 바운드 작업은 프로그램 성능이 CPU의 연산 처리 속도에 의해 제한되는 작업을 의미합니다.**
- 이러한 작업은 **복잡한 계산, 데이터 처리, 암호화** 등 CPU가 많은 연산을 수행해야 하는 경우가 많습니다.
- CPU 바운드 작업을 최적화하기 위해서는 **병렬 처리, 연산 최적화, 캐싱 등**의 기법을 활용하여 CPU 자원을 효율적으로 사용하고, 성능을 최대한 끌어올릴 수 있습니다.
- CPU 바운드 작업은 I/O 바운드 작업과 대조적으로, **연산 작업이 중심**이 되는 상황에서 발생합니다.
