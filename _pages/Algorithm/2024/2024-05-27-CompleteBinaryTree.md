---
title: "📦[DS,Algorithm] 완전 이진 트리(Complete Binary Tree)"
tags:
    - DataStructure
    - Algorithm
date: "2024-05-27"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 1️⃣ 완전 이진 트리(Complete Binary Tree).

완전 이진 트리(Complete Binary Tree)는 이진 트리의 한 종류입니다.

## 1️⃣ 완전 이진 트리(Complete Binary Tree)의 특성.

1. **모든 레벨이 완전히 채워져 있다.**
    - 마지막 레벨을 제외한 모든 레벨의 노드가 최대 개수로 채워져 있습니다.
    - 마지막 레벨의 노드들은 왼쪽부터 오른쪽으로 채워져 있습니다.

2. **노드의 배치**
    - 트리의 높이가 **'h'** 라면, 마지막 레벨을 제외한 모든 레벨에는 **'2^k'** 개의 노드가 있습니다. 여기서 **'k'** 는 해당 레벨의 깊이 입니다.
    - 마지막 레벨에는 1개 이상 **'2^h'** 개 이하의 노드가 있으며, 이 노드들은 왼쪽부터 채워집니다.

## 2️⃣ 완전 이친 트리의 예.
```
        1
      /   \
     2     3
    / \   / \
   4   5 6   7
  / \
 8   9
```

위의 트리는 완전 이진 트리의 예입니다.

모든 레벨이 완전히 채워져 있고, 마지막 레벨의 노드들은 왼쪽부터 오른쪽으로 채워져있습니다.

## 3️⃣ 완전 이진 트리의 속성.

1. **노드 수**
    - 높이가 **'h'** 인 완전 이진 트리는 최대 **'2^(h+1) - 1'** 개의 노드를 가질 수 있습니다.
    - 마지막 레벨을 제외한 모든 노드는 **'2^h - 1'** 개의 노드를 가집니다.

2. **높이**
    - 노드 수가 **'n'** 인 완전 이진 트리의 높이는 **'O(log n)'** 입니다.

3. **배열 표현**
    - 완전 이진 트리는 배열을 사용하여 쉽게 표현할 수 있습니다. 이는 힙 자료구조에서 많이 사용됩니다.

## 4️⃣ 배열을 통한 완전 이진 트리 표현

완전 이진 트리는 배열을 사용하여 효율적으로 표현할 수 있습니다.

노드의 인덱스를 기준으로 부모-자식 관계를 쉽게 파악할 수 있습니다.

**노드의 인덱스 규칙**
- **루트 노드 :** 인덱스 0
- **인덱스 'i'의 오른쪽 자식 노드 :** '2*i + 1'
- **인덱스 'i'의 부모 노드 :** '(i - 1) / 2'

## 5️⃣ 예제 코드
아래는 완전 이진 트리를 배열로 표현하고, 이를 출력하는 간단한 예제 코드입니다.
```java
public class CompleteBinaryTree {

  public static void main(String[] args) {
    int[] tree = {1, 2, 3, 4, 5, 6, 7, 8, 9};

    // 트리 출력
    printTree(tree);
  }

  // 배열로 표현된 완전 이진 트리 출력
  public static void printTree(int[] tree) {
    for (int i = 0; i < tree.length; i++) {
      int leftChildIndex = 2 * i + 1;
      int rightChildIndex = 2 * i + 2;

      System.out.print("Node " + tree[i] + ": ");

      if (leftChildIndex < tree.length) {
        System.out.print("Left Child: " + tree[leftChildIndex] + ", ");
      } else {
        System.out.print("Left Child: null, ");
      }

      if (rightChildIndex < tree.length) {
        System.out.print("Right Child: " + tree[rightChildIndex]);
      } else {
        System.out.print("Right Child: null");
      }
      System.out.println();
    }
  }
}

/* 출력
Node 1: Left Child: 2, Right Child: 3
Node 2: Left Child: 4, Right Child: 5
Node 3: Left Child: 6, Right Child: 7
Node 4: Left Child: 8, Right Child: 9
Node 5: Left Child: null, Right Child: null
Node 6: Left Child: null, Right Child: null
Node 7: Left Child: null, Right Child: null
Node 8: Left Child: null, Right Child: null
Node 9: Left Child: null, Right Child: null
*/
```

### 설명

1. **트리 배열 초기화 :** **`int[] tree = {1, 2, 3, 4, 5, 6, 7, 8, 9};`**
    - 완전 이진 트리를 배열로 표현합니다.

2. **트리 출력 :** **`printTree(tree)`**
    - 배열로 표현된 완전 이진 트리를 출력하는 함수입니다.
    - 각 노드에 대해 왼쪽 자식과 오른쪽 자식을 출력합니다.

### 시간 복잡도

- **삽입(Insertion) :** O(log n)
- **삭제(Deletion) :** O(log n)
- **탐색(Search) :** O(n) (일반적으로 완전 이진 트리는 탐색보다 삽입/삭제가 주된 연산입니다.)

> 완전 이진 트리는 데이터의 구조적 특성 때문에 힙과 같은 자료구조에서 많이 사용됩니다.
> 이는 효율적인 삽입 및 삭제 연산을 제공하며, 배열을 통한 표현이 간편하여 다양한 알고리즘에서 유용하게 사용됩니다.
