---
title: "📦[DS,Algorithm] 이진 트리(Binary Tree)"
tags:
    - DataStructure
    - Algorithm
date: "2024-05-27"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 1️⃣ 이진 트리(Binary Tree).

**이진 트리(Binary Tree)** 는 각 노드가 최대 두 개의 자식 노드를 가질 수 있는 트리 구조입니다.

이 두 자식 노드는 일반적으로 **왼쪽 자식(Left Child)** 과 **오른쪽 자식(Right Child)** 이라고 불립니다.

이진 트리는 다양한 응용 프로그램에서 중요한 자료구조입니다.

## 1️⃣ 이진 트리의 구성 요소.

- **노드(Node) :** 데이터를 저장하는 기본 단위입니다.
- **루트(Root) :** 트리의 최상위 노드입니다.
- **자식(Child) :** 특정 노드로부터 연결된 하위 노드입니다.
- **부모(Parent) :** 특정 노드를 가리키는 상위 노드입니다.
- **잎(Leaf) :** 자식 노드가 없는 노드입니다.
- **서브트리(Subtree) :** 특정 노드와 그 노드의 모든 자식 노드로 구성된 트리입니다.

## 2️⃣ 이진 트리의 종류.

1. **포화 이진 트리(Full Binary Tree) :** 모든 노드가 0개 또는 2개의 자식 노드를 가지는 트리입니다.

2. **완전 이진 트리(Complete Binary Tree) :** 마지막 레벨을 제외한 모든 레벨이 완전히 채워져 있으며, 마지막 레벨의 노드는 왼쪽부터 채워져 있는 트리입니다.

3. **높이 균형 이진 트리(Height-balanced binary Tree) :** AVL 트리와 같이 각 노드의 왼쪽 서브트리와 오른쪽 서브트리의 높이 차이가 1 이하인 트리입니다.

4. **이진 탐색 트리(Binary Search Tree, BST) :** 왼쪽 서브트리의 모든 노드가 루트 노드보다 작고, 오른쪽 서브 트리의 모든 노드가 루트 노드보다 큰 트리입니다.

## 3️⃣ 이진 트리의 주요 연산 및 시간 복잡도.

1. **삽입(Insertion) :** 새로운 노드를 트리에 추가합니다.
    - 일반적인 경우 시간 복잡도 : O(log n)(이진 탐색 트리에서)
    - 최악의 경우 시간 복잡도 : O(n)(편향된 트리에서)

2. **삭제(Deletion) :** 트리에서 특정 노드를 제거합니다.
    - 일반적인 경우 시간 복잡도 : O(log n)(이진 탐색 트리에서)
    - 최악의 경우 시간 복잡도: O(n)(편향된 트리에서)

3. **탐색(Search) :** 트리에서 특정 값을 찾습니다.
    - 일반적인 경우 시간 복잡도: O(log n)(이진 탐색 트리에서)
    - 최악의 경우 시간 복잡도 : O(n)(편향된 트리에서)

4. **순회(Traversal) :** 트리의 모든 노드를 방문합니다. 순회 방법에는 전위(Preorder), 중위(Inorder), 후위(Postorder) 순회가 있습니다.
    - 시간 복잡도: O(n)(모든 노드를 방문하기 때문에)

## 4️⃣ 이진 트리의 예제
**이진 탐색 트리(BST)의 구현**
```java
// TreeNode
public class TreeNode {
  int data;
  TreeNode left;
  TreeNode right;

  public TreeNode(int data) {
    this.data = data;
    this.left = null;
    this.right = null;
  }
}

// BinarySearchTree
public class BinarySearchTree {
  private TreeNode root;

  public BinarySearchTree() {
    this.root = null;
  }

  // 삽입 연산
  public void insert(int data) {
    root = insertRec(root, data);
  }

  private TreeNode insertRec(TreeNode root, int data) {
    if (root == null) {
      root = new TreeNode(data);
      return root;
    }

    if (data < root.data) {
      root.left = insertRec(root.left, data);
    } else if (data > root.data) {
      root.right = insertRec(root.right, data);
    }
    return root;
  }

  // 탐색 연산
  public boolean search(int data) {
    return searchRec(root, data);
  }

  private boolean searchRec(TreeNode root, int data) {
    if (root == null) {
      return false;
    }

    if (root.data == data) {
      return true;
    }

    if (data < root.data) {
      return searchRec(root.left, data);
    } else {
      return searchRec(root.right, data);
    }
  }

  // 중위 순회(Inorder Traversal)
  public void inorder() {
    inorderRec(root);
  }

  private void inorderRec(TreeNode root) {
    if (root != null) {
      inorderRec(root.left);
      System.out.print(root.data + " ");
      inorderRec(root.right);
    }
  }
}

// Main
public class Main {

  public static void main(String[] args) {
    BinarySearchTree bst = new BinarySearchTree();

    bst.insert(50);
    bst.insert(30);
    bst.insert(20);
    bst.insert(40);
    bst.insert(70);
    bst.insert(60);
    bst.insert(80);

    System.out.println("Inorder traversal of the BST:");
    bst.inorder(); // 출력: 20 30 40 50 60 70 80

    System.out.println("\nSearch for 40: " + bst.search(40)); // 출력: true
    System.out.println("Search for 90: " + bst.search(90)); // 출력: false
  }
}
```
