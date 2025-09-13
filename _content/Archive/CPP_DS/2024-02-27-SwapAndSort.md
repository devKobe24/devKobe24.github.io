---
title: "🆙[Cpp DataStructure] 교환(Swap)과 정렬(Sort)"
tags:
    - Cpp
    - DataStructure
date: "2024-02-27"
thumbnail: "/assets/img/thumbnail/cpp.jpeg"
---

# Swap(교환)

먼저 아래의 코드를 보고 `a`와 `b`를 교환해봅시다.

```cpp
#include <iostream>

using namespace std;

int main()
{

	// Swap
	{
		int a = 3;
		int b = 2;

		cout << a << " " << b << endl;

		// TODO:

		cout << a << " " << b << endl;	
	}

	return 0;
}
```

**실행 결과**
```
3 2
3 2
```

**"TODO"** 에는 어떤 코드가 들어가야 할까요?

"우리가 양 손에 사과🍎와 레몬🍋을 들고 있다고 생각해봅시다."
* 그럼 왼손에는 사과🍎와 오른손에는 레몬🍋을 들고 있을 때 사과🍎와 레몬🍋을 바꾸려면 어떻게 해야할까요?
    * 저는 하나의 접시를 가져와 그 접시에 사과 또는 레몬을 잠시 올려두고 비어있는 손으로 과일을 옮긴 뒤 접시에 있는 과일을 집을것 입니다.
        * 그럼 코드도 똑같이 만들 수 있지 않을까요?

```cpp
#include <iostream>

using namespace std;

int main()
{

	// Swap
	{
		int a = 3;
		int b = 2;

		cout << a << " " << b << endl;

		// TODO:
        // 먼저 a를 사과 b를 레몬이라고 생각하고,
        // temp라는 접시를 만들어보겠습니다.
        // 그 접시에 a라는 사과를 올려보겠습니다.
        int temp = a;
        
        // 그럼 비어있는 손으로 레몬을 옮길 수 있게되었네요.
        // 비어있는 손으로 레몬을 옮겨보겠습니다.
        // a가 있던 손으로 b를 옮깁니다.
        a = b;
        
        // 이번에는 b가 있던 손이 비었네요.
        // b가 있던 손으로 접시(temp)에 있는 a를 들어보겠습니다.
        temp = a;

		cout << a << " " << b << endl;	
	}

	return 0;
}
```
**실행 결과**
```
3 2
2 3
```

* 양 손에 있던 사과(3)과 레몬(2)의 자리가 바뀌었습니다
    * **"즉, 교환(Swap)이 이루어졌습니다."**

하지만 항상 이렇게 3줄의 라인인
```cpp
int temp = a;
a = b;
b = temp;
```

* 위 코드처럼 코드를 만들어서 사용할 경우에는 매우 많은 교환(Swap)을 해야할 경우 코드의 양도 늘어나고 가독성도 좋지 않은 것 입니다.

**"그럼 이번에는 함수를 이용해서 두 숫자를 교환해봅시다."**

먼저, 위 교환 코드를 그대로 가져다가 사용해볼까요?
```cpp
void MySwap(int a, int b) {
    int temp = a;
    a = b;
    b = temp;
}
```

* 이 경우에는 리턴 값이 없기 때문에 불가능합니다.
    * 만약 리턴값이 int 형이라고 해도 리턴은 1개만 가능하기 때문에 어렵습니다.
        * cpp의 다른 기능인 구조체나 여러 기능을 사용해야 할 것입니다.

그러면 어떻게 해야할까요?
* **"C의 포인터와 CPP의 레퍼런스를 활용하면됩니다."**

**1. C의 포인터 활용**
```cpp
#include <iostream>

using namespace std;

void MySwapPtr(int* a, int* b) {
	int temp = *a;
	*a = *b;
	*b = temp;
}

int main()
{

	// Swap
	{
		int a = 3;
		int b = 2;

		cout << a << " " << b << endl;

		MySwapPtr(&a, &b);

		cout << a << " " << b << endl;	
	}

	return 0;
}
```

**실행 결과**
```
3 2
2 3
```

실행 결과는 올바르게 나왔습니다.

**"C 스타일의 포인터 사용은 `*(별)`을 사용하면 됩니다."**

```cpp
void MySwapPtr(int* a, int* b) {
	int temp = *a;
	*a = *b;
	*b = temp;
}
```
* temp에 먼저 a의 주소값을 넣어 줍니다.
    * 이후 a의 주소값에 b의 주소값을 넣어 줍니다.
        * b의 주소값에는 temp(a의 주소값)을 넣어줍니다.

**"이렇게 되면 각 주소값이 교환이 됩니다."**

C 스타일의 단점은 선언과 호출시에 나타납니다.

* 선언시에는 위와 같이 `*`을 모두 붙여줘야 합니다.
* 호출시에는 아래와 같이 `&`를 붙여줘야 합니다.

```cpp
MySwapPtr(&a, &b);
```

**"하지만 CPP 스타일의 래퍼런스 교환은 단순합니다."**

```cpp
// 선언시
void MySwapRef(int& a, int& b) {
	int temp = a;
	a = b;
	b = temp;
}

// 호출시
MySwapRef(a, b);
```

* 내부 구현은 똑같지만 별다른 어노테이션 없이 일반적인 변수 할당과 같이 해주면 래퍼런스 대입되고 교환이 이루어지는 것을 볼 수 있습니다.
* 호출시에도 어노테이션을 따로 붙일 필요없이 일반적인 매개변수(parameter)를 넣어주듯이 넣어주면 됩니다.

```cpp
#include <iostream>

using namespace std;

void MySwapRef(int& a, int& b) {
	int temp = a;
	a = b;
	b = temp;
}

int main()
{

	// Swap
	{
		int a = 3;
		int b = 2;

		cout << a << " " << b << endl;

		MySwapRef(a, b);

		cout << a << " " << b << endl;	
	}

	return 0;
}
```

**실행 결과**
```
3 2
2 3
```

**"이번에는 교환을 활용해서 정렬을 해보겠습니다."**

* 값과 상관 없이 항상 작은 값이 먼저 출력되게 하려면 어떻게 해야할까요?
    * 즉, 두 값이 같을 때는 순서가 상관이 없지만 큰 값이 먼저 출력되지 않게 해야합니다.

**먼저 두 값이 같지 않거나 큰 값이 먼저 출력 되었을 경우에 false를 출력하고 그와는 반대일 경우에는 true를 출력하는 코드를 작성해보겠습니다.**

```cpp
#include <iostream>

using namespace std;

// 정렬(sorting)
int main() {
	int arr[2];

	// TODO:
	for (int j = 0; j < 5; j++) {
		for (int i = 0; i < 5; i++) {
			arr[0] = i;
			arr[1] = j;

			cout << boolalpha;
			cout << arr[0] << " " << arr[1] << " "
				<< (arr[0] <= arr[1]) << endl;
		}
	}
	return 0;
}
```

* 먼저 배열을 선언합니다. 배열은 순서가 있기 때문입니다.
    * 그리고 2중 for문을 사용합니다. 첫 번째 for문이 1번 돌 때 두 번째 for문은 5번 돌게됩니다.
        * 그렇게 각각을 i와 j에 라는 변수의 이름으로 arr 배열 인덱스 0번째와 1번째에 넣어줍니다.

**실행 결과**
```
0 0 true
1 0 false
2 0 false
3 0 false
4 0 false
0 1 true
1 1 true
2 1 false
3 1 false
4 1 false
0 2 true
1 2 true
2 2 true
3 2 false
4 2 false
0 3 true
1 3 true
2 3 true
3 3 true
4 3 false
0 4 true
1 4 true
2 4 true
3 4 true
4 4 true
```

* 실행 결과 값이 작거나 같은 값이 인덱스 0번 즉 오름차순일 경우에는 true 입니다.
    * 그와는 반대로 큰 값이 인덱스 0번 즉 내림차순일 경우에는 false 입니다.

**"이제 두 값을 비교하여 오름차순으로 정렬되는 것을 확인하는 함수를 만들었으니 이번에는 실제 정렬을 해보도록하겠습니다."**

```cpp
#include <iostream>

using namespace std;

bool CheckSorted(int a, int b) {
	// TODO: ...
	if (a <= b) {
		return true;
	} else {
		return false;
	}
}

// 정렬(sorting)
int main() {
	int arr[2];

	// TODO:
	for (int j = 0; j < 5; j++) {
		for (int i = 0; i < 5; i++) {
			arr[0] = i;
			arr[1] = j;

			// swap 소개
			if (arr[0] > arr[1]) {
				swap(arr[0], arr[1]);
			}

			cout << boolalpha;
			cout << arr[0] << " " << arr[1] << " "
				<< (CheckSorted(arr[0], arr[1])) << endl;
			
		}
	}
	return 0;
}
```

**실행 결과**
```
0 0 true
0 1 true
0 2 true
0 3 true
0 4 true
0 1 true
1 1 true
1 2 true
1 3 true
1 4 true
0 2 true
1 2 true
2 2 true
2 3 true
2 4 true
0 3 true
1 3 true
2 3 true
3 3 true
3 4 true
0 4 true
1 4 true
2 4 true
3 4 true
4 4 true
```

* CPP에는 `swap`이라는 함수가 있습니다 `swap`을 사용할 경우에는 매개변수로 받은 두 값을 바꿔줍니다.
    * 따라서 위와 같은 실행 결과를 출력합니다.
