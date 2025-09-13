---
title: "🆙[Cpp DataStructure] 안정성(stability) 확인"
tags:
    - Cpp
    - DataStructure
date: "2024-03-07"
thumbnail: "/assets/img/thumbnail/cpp.jpeg"
---

# 안정성(stability) 확인.

```cpp
#include <iostream>
#include <cassert>
#include <fstream>

using namespace std;

struct Element
{
	int key;
	char value;
};

void Print(Element* arr, int size)
{
	for (int i = 0; i < size; i++)
		cout << arr[i].key << " ";
	cout << endl;

	for (int i = 0; i < size; i++)
		cout << arr[i].value << " ";
	cout << endl;
}

int main()
{
    	// 안정성 확인(unstable)
	{
		Element arr[] = { {2, 'a'}, {2, 'b'}, {1, 'c' } };
		int size = sizeof(arr) / sizeof(arr[0]);

		Print(arr, size);

		int min_index;
		for(int i = 0; i < (size - 1); i++) 
		{
			min_index = i;
			for (int j = (i + 1); j < size; j++)
			{
				if (arr[j].key < arr[min_index].key) 
				{
					min_index = j;
				}
			}

			swap(arr[i], arr[min_index]);

			Print(arr, size);
		}
	}
	return 0;
}
```

**실행 결과**
```
2 2 1 
a b c 
1 2 2 
c b a 
1 2 2 
c b a
```

정렬의 안정성(stablity)이라는 개념이 있습니다.
* 이 개념은 `stable`한지 `unstable`한지로 구분합니다.
    * 즉, 안정적인지 불안정적인지로 구분합니다.

위 코드에서 보면 `Element` 배열을 정렬합니다.

이 `Èlement`를 보면 다음과 같습니다.
```cpp
struct Element
{
	int key;
	char value;
};
```
* `Element`의 `key`는 정수이고 `value`는 문자입니다.
    * 그래서 정렬할 때 `key`를 기준으로 정렬합니다.
        * 그러면 `swap`시 `value(문자)`는 함께 따라갑니다. -> 복사를 하니 따라가는 것 입니다.

```cpp
Element arr[] = { {2, 'a'}, {2, 'b'}, {1, 'c' } };
```
* 첫 번째 인덱스의 요소의 `key`는 `2`이고 `value`는 `'a'` 입니다.
* 두 번째 인덱스의 요소의 `key`는 `2`이고 `value`는 `'b'` 입니다.
* 세 번째 인덱스의 요소의 `key`는 `1`이고 `value`는 `'c'` 입니다.

"정렬시에는 `key`가 작은 순서대로 정렬되도록 할 것 입니다."
* 즉, `key`를 기준으로 정렬합니다.

여기서 주목해야 할 점은 "첫 번째 인덱스와 두 번째 인덱스의 요소의 Value"입니다.
* 두 인덱스의 요소의 `key` 값은 같으나 `value` 값은 다릅니다.

비교 로직을 보면 `key`값을 가지고 비교를 하지만 `swap` 시에는 `key`와 `value`를 모두 가지고 움직입니다.

**실행 결과를 보고 value와 안정성의 상관 관계에 대해 알아봅시다.**

**실행 결과**
```
2 2 1 
a b c 
1 2 2 
c b a 
1 2 2 
c b a
```
* 실행 결과를 보면 (2 a), (2, b), (1, c)에서 **"(1, c), (2, b), (2, a)"** 순으로 정렬된 것을 볼 수 있습니다.
    * 처음에는 (2 a), (2, b) 순서였지만 정렬 후에는 **"(2, b), (2, a)"** 로 순서가 바뀌어 정렬되었습니다.
        * value가 뒤바뀐 것을 알 수 있습니다.
            * key가 정렬이 되었으므로 정렬은 잘 되었다고 볼 수 있습니다.
            * **그러나 "key가 같은 값일 경우에는 value의 순서가 a, b에서 b, a 로 바뀌었습니다."**
                * **"여기서 stable한 것과 unstable한 것의 차이점이 나타납니다."**

**"키 값이 같아도 정렬시 벨류 값이 처음과 같이 유지가 된다면 그것은 stable하다라고 합니다, 그와 반대로 키 값이 같으나 처음과 달리 벨류의 순서가 바뀔 경우에는 unstable하다 라고 합니다."**
* 이것도 정렬 알고리즘을 분류하는 기준 중 하나입니다.
