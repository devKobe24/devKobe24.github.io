---
title: "📦[DS,Algorithm] 해시 테이블(Hash Table)"
tags:
    - DataStructure
    - Algorithm
date: "2024-05-25"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 1️⃣ 해시 테이블(Hash Table).

해시 테이블(Hash Table)은 데이터를 키-값 쌍(key-value pairs)으로 저장하는 자료구조입니다.

해시 테이블은 해시 함수를 사용하여 키를 해시 값으로 변환하고, 이 해시 값을 인덱스로 사용하여 배열에서 값을 저장하거나 검색합니다.

이를 통해 데이터에 빠르게 접근할 수 있습니다.

## 1️⃣ 해시 테이블의 구성 요소.

1. **키(key) :** 각 데이터를 식별하기 위한 고유한 식별자입니다.

2. **값(Value) :** 키와 연관된 데이터입니다.

3. **해시 함수(Hash Function) :** 키를 입력으로 받아 해시 값을 출력하는 함수입니다. 이 해시 값은 보통 정수이며, 배열의 인덱스로 사용됩니다.

4. **버킷(Bucket) :** 해시 값에 의해 인덱싱되는 배열의 각 위치입니다. 각 버킷은 하나의 키-값 쌍 또는 충돌 처리를 위한 데이터 구조(예: 연결 리스트)를 저장할 수 있습니다.

## 2️⃣ 해시 함수의 역할.

해시 함수는 키를 고정된 크기의 해시 값으로 매핑합니다.

이상적인 해시 함수는 키를 균등하게 분포시키고, 충돌을 최소화합니다.

## 3️⃣ 충동(Collision)과 충돌 해결 방법.

두 개 이상의 키가 동일한 해시 값을 가질 때 충돌이 발생합니다.

해시 테이블은 이러한 충돌을 처리하기 위한 여러가지 방법을 제공합니다.

1. **체이닝(Chaining) :** 각 버킷에 연결 리스트를 사용하여 동일한 해시 값을 갖는 모든 요소를 저장합니다. 충돌이 발생하면 해당 버킷의 리스트에 요소를 추가합니다.

2. **개방 주소법(Open Addressing) :** 충돌이 발생하면 다른 빈 버킷을 찾아 데이터를 저장합니다. 이를 위해 다양한 탐사 방법(예: 선형 탐사, 제곱 탐사, 이중 해싱)을 사용합니다.

## 4️⃣ 해시 테이블의 시간 복잡도.

- **검색(Search) :** O(1)(평균), O(n)(최악)
- **삽입(Insertion) :** O(1)(평균), O(n)(최악)
- **삭제(Deletion) :** O(1)(평균), O(n)(최악)

최악의 경우 시간 복잡도는 해시 충돌로 인해 모든 요소가 하나의 버킷에 저장될 때 발생합니다.

그러나, 좋은 해시 함수와 충돌 해결 방법을 사용하면 평균적으로 O(1)의 성능을 유지할 수 있습니다.

## 5️⃣ 해시 테이블의 장점과 단점.

**장점**
- 빠른 검색, 삽입, 삭제 성능(평균적으로 O(1))
- 키를 사용하여 데이터에 빠르게 접근 가능

**단점**
- 해시 함수의 성능에 의존
- 충돌 처이 필요
- 메모리 사용량이 증가할 수 있슴(특히 체이닝을 사용하는 경우)

## 💻 해시 테이블의 구현 예제.

아래는 Java에서 간단한 해시 테이블을 구현한 예제입니다.

```java

// HashTable
import java.util.LinkedList;

class HashTable {
  private class HashNode {
    String key;
    String value;
    HashNode next;

    public HashNode(String key, String value) {
      this.key = key;
      this.value = value;
    }
  }

  private LinkedList<HashNode>[] buckets;
  private int numBuckets;
  private int size;

  public HashTable() {
    numBuckets = 10; // 버킷의 초기 크기
    buckets = new LinkedList[numBuckets];
    size = 0;

    for (int i = 0; i < numBuckets; i++) {
      buckets[i] = new LinkedList<>();
    }
  }

  private int getBucketIndex(String key) {
    int hashCode = key.hashCode();
    int index = hashCode % numBuckets;
    return index < 0 ? index * -1 : index;
  }

  public void put(String key, String value) {
    int bucketIndex = getBucketIndex(key);
    LinkedList<HashNode> bucket = buckets[bucketIndex];

    for (HashNode node : bucket) {
      if (node.key.equals(key)) {
        node.value = value;
        return;
      }
    }

    bucket.add(new HashNode(key, value));
    size++;
  }

  public String get(String key) {
    int bucketIndex = getBucketIndex(key);
    LinkedList<HashNode> bucket = buckets[bucketIndex];

    for (HashNode node : bucket) {
      if (node.key.equals(key)) {
        return node.value;
      }
    }
    return null;
  }

  public String remove(String key) {
    int bucketIndex = getBucketIndex(key);
    LinkedList<HashNode> bucket = buckets[bucketIndex];

    HashNode prev = null;
    for (HashNode node : bucket) {
      if (node.key.equals(key)) {
        if (prev != null) {
          prev.next = node.next;
        } else {
          bucket.remove(node);
        }
        size--;
        return node.value;
      }
      prev = node;
    }
    return null;
  }

  public int size() {
    return size;
  }

  public boolean isEmpty() {
    return size == 0;
  }
}

// Main
public class Main {
  public static void main(String[] args) {
    HashTable hashTable = new HashTable();
    hashTable.put("one", "1");
    hashTable.put("two", "2");
    hashTable.put("three", "3");

    System.out.println("Value for key 'one': " + hashTable.get("one"));
    System.out.println("Value for key 'two': " + hashTable.get("two"));
    System.out.println("Removing key 'one': " + hashTable.remove("one"));
    System.out.println("Contains key 'one': " + (hashTable.get("one") != null));
  }
}

/*
출력
Value for key 'one': 1
Value for key 'two': 2
Removing key 'one': 1
Contains key 'one': false
*/
```
