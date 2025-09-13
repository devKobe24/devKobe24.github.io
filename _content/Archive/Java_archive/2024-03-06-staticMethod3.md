---
title: "☕️[Java] static 메서드 3"
tags:
    - Java
    - Programming Language
date: "2024-03-06"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# static 메서드 3

## 용어 정리

**멤버 메서드의 종류**
* **인스턴스 메서드**: `static`이 붙지 않은 멤버 메서드
* **클래스 메서드**: `static`이 붙은 멤버 메서드
    * 클래스 메서드, 정적 메서드, `static` 메서드등으로 부릅니다.

* `static`이 붙지 않은 멤버 메서드는 인스턴스를 생성해야 사용할 수 있고, 인스턴스에 소속되어 있습니다.
    * 따라서 인스턴스 메서드라고 합니다.
* `static`이 붙은 멤버 메서드는 인스턴스와 무관하게 클래스에 바로 접근해서 사용할 수 있고, 클래스 자체에 소속되어 있습니다.
    * 따라서 클래스 메서드라고 합니다.

> 참고로 방금 설명한 내용은 멤버 변수에도 똑같이 적용됩니다.

**정적 메서드 활용**
* 정적 메서드는 객체 생성이 필요 없이 메서드의 호출만으로 필요한 기능을 수행할 때 주로 사용합니다.
    * 예를 들어 간단한 메서드 하나로 끝나는 유틸리티성 메서드에 자주 사용합니다.
    * 수학의 여러가지 기능을 담은 클래스를 만들 수 있는데, 이 경우 인스턴스 변수 없이 입력한 값을 계산하고 반환하는 것이 대부분입니다.
        * 이럴 때 정적 메서드를 사용해서 유틸리티성 메서드를 만들면 좋습니다.

## 정적 메서드 접근 법
* `static` 메서드는 `static` 변수와 마찬가지로 클래스를 통해 바로 접근할 수 있고, 인스턴스를 통해서도 접근할 수 있습니다.

**DataCountMain - 추가**
```java
// 추가
// 인스턴스를 통한 접근
DecoData data3 = new DecoData();
data3.staticCall();

// 클래스를 통한 접근
DecoData.staticCall();
```

**실행 결과 - 추가된 부분**
```
staticValue=4
staticValue=5
```

* 둘의 차이는 없습니다. 둘다 결과적으로 정적 메서드에 접근합니다.

**인스턴스를 통한 접근 `data3.staticCall()`**
* 정적 메서드의 경우 인스턴스를 통한 접근은 추천하지 않습니다.
    * 왜냐하면 코드를 읽을 때 마치 인스턴스 메서드에 접근하는 것 처럼 오해할 수 있기 때문입니다.

**클래스를 통한 접근 `DecoData.staticCall()`**
* 정적 메서드는 클래스에서 공용으로 관리하기 때문에 클래스를 통해서 접근하는 것이 더 명확합니다.
    * 따라서 정적 메서드에 접근할 때는 클래스를 통해서 접근합시다.

## static import
* 정적 메서드를 사용할 때 해당 메서드를 다음과 같이 자주 호출해야 한다면 `static import` 기능을 고려합시다.
```java
DecoData.staticCall();
DecoData.staticCall();
DecoData.staticCall();
```

* 이 기능을 사용하면 다음과 같이 클래스명을 생략하고 메서드를 호출할 수 있습니다.
```java
staticCall();
staticCall();
staticCall();
```

**DecoDataMain - static import 적용**
```java
package static2;

// import static static2.DecoData.staticCall;
import static static2.DecoData.*;

public class DecoDataMain {
    
    public static void main(String[] args) {
        System.out.println("1.정적 호출");
        staticCall(); // 클래스 명 생략 가능
        ...
    }
}
```

* 특정 클래스의 정적 메서드 하나만 적용하려면 다음과 같이 생략할 메서드 명을 적어주면 됩니다.
```java
import static static2.DecoData.staticCall;
```

* 특정 클래스의 모든 정적 메서드에 적용하려면 다음과 같이 `*`을 사용하면 됩니다.
```java
import static static2.DecoData.*;
```

> 참고로 `import static`은 정적 메서드 뿐만 아니라 정적 변수에도 사용할 수 있습니다.

## main() 메서드는 정적 메서드
* 인스턴스 생성 없이 실행하는 가장 대표적인 메서드가 바로 `main()` 메서드입니다.
    * `main()` 메서드는 프로그램을 시작하는 시작점이 되는데, 생각해보면 객체를 생성하지 않아도 `main()` 메서드가 작동했습니다.
        * 이것은 `main()` 메서드가 `static`이기 때문입니다.

* 정적 메서드는 정적 메서드만 호출할 수 있습니다.
    * 따라서 정적 메서드인 `main()`이 호출하는 메서드에는 정적 메서드를 사용했습니다.
        * 물론 더 정확히 말하자면 정적 메서드는 같은 클래스 내부에서 정적 메서드만 호출할 수 있습니다.
            * 따라서 정적 메서드인 `main()` 메서드가 같은 클래스에서 호출하는 메서드도 정적 메서드로 선언해서 사용했습니다.

**main() 메서드와 static 메서드 호출 예**
```java
public class ValueDataMain {
    
    public static void main(String[] args) {
        ValueData valueData = new ValueData();
        add(valueData);
    }
    
    static void add(ValueData valueData) {
        valueData value++;
        System.out.println("숫자 증가 value=" + valueData.value);
    }
}
```
