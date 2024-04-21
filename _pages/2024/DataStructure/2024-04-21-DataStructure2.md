---
title: "📦[DataStructure] 복합 자료 구조"
tags:
    - DataStructure
date: "2024-04-21"
thumbnail: "/assets/img/thumbnail/ds.jpeg"
---

# 복합 자료 구조.
다양한 프로그래밍 언어가 **복합(composite) 자료 구조**를 만들 수 있는 기능을 제공합니다.

예를 들어, 여러 개별 변수를 한 그룹으로 엮은 구조체(struct)나 객체(object)가 복합 자료 구조에 속합니다.
- 복합 자료 구조는 관련있는 데이터 조각을 한데 모아서 한꺼번에 전달할 수 있는 손쉬운 방법을 제공합니다.
    - 예를 들어, 우리가 시음한 커피 종류에 대한 정보를 모은 CoffeeRecord를 정의할 수 있습니다.

```
CoffeeRecord {
    String: Name
    String: Brand
    Integer: Rating
    Float: Cost_Per_Pound
    Boolean: Is_Dark_Roast
    String: Other_Notes
}
````

- 커피의 속성을 추적하기 위해 변수 여섯 개를 따로따로 유지하는 대신, 모든 정보를 하나의 복합 자료 구조 CoffeeRecord에 저장합니다.

속성이 추가되면 복합 자료 구조를 사용하는 것이 더 중요해집니다.
- 복합 자료 구조가 없다면 수백 개의 관련 변수를 전달하는 방식으로 처리해야 하는데, 이는 변수를 잘못된 순서로 함수에 전달하는 등 프로그래머의 실수를 야기할 가능성도 더 높습니다.

자바(Java)나 파이썬(Python)을 비롯한 많은 프로그래밍 언어에서, 복합 데이터는 자신의 데이터와 작동에 대한 함수를 모두 포함하는 '객체(object)'가 될 수 있습니다.

객체의 함수는 파이썬의 self 참조처럼 특별한 구문을 사용해 해당 객체 자신의 데이터에 접급합니다.

객체는 내부 데이터를 객체 외부에서 공객적으로 접근하도록 허용할지 아니면 비공개적으로 객체 내부 함수에서만 접근하게 할지를 지정하는 가시성(visibility) 규칙을 제공할 수 있습니다.

복합 자료 구조나 객체를 사용하는 코드에서는 다음 예제처럼 **'변수이름.필드이름'** 이라는 구문을 사용해 복합 자료 구조의 필드에 접근합니다.

```
last_record.name = "Sublime Blend"
````
- 이 코드는 커피 기록에 있는 lastest_record 레코드의 name 필드를 Sublime Blend로 설정합니다.
