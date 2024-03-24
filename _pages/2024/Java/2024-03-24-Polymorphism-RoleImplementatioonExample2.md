---
title: "☕️[Java] 다형성 - 역할 구현 예제 2"
tags:
    - Java
    - Programming Language
date: "2024-03-24"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 다형성 - 역할과 구현 예제 2
새로운 Model3 차량을 추가해야 하는 요구사항이 들어왔습니다.

이 요구사항을 맞추려면 기존에 `Driver` 코드를 많이 변경해야 합니다.

드라이버는 `K3Car`도 운전할 수 있고, `Model3Car`도 운전할 줄 있어야 합니다.

> 참고로 돌을 동시에 운전하는 것은 아닙니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%92%E1%85%A1%E1%86%AF%E1%84%80%E1%85%AA%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A62.png?raw=true">

```java
package poly.car0;

public class CarMain0 {

  public static void main(String[] args) {
    Driver driver = new Driver();
    K3Car k3Car = new K3Car();
    driver.setK3Car(k3Car);
    driver.drive();

    // 추가
    Model3Car model3Car = new Model3Car();
    driver.setK3Car(null);
    driver.setModel3Car(model3Car);
    driver.drive();
  }
}
```

* K3를 운전하던 운전자가 Model3로 차량을 변경해서 운전하는 코드입니다.
* `driver.setK3Car(null)`을 통해서 기존 `K3Car`의 참조를 제거합니다.
* `driver.setModel3Car(model3Car)`를 통해서 새로운 `model3Car`의 참조를 추가합니다.
* `driver.drive()`를 호출합니다.

**실행 결과**
```
자동차를 운전합니다.
K3Car.startEngine
K3Car.pressAccelerator
K3Car.offEngine
자동차를 운전합니다.
Model3Car.startEngine
Model3Car.pressAccelerator
Model3Car.offEngine
```

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%92%E1%85%A1%E1%86%AF%E1%84%80%E1%85%AA%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A62%E1%84%86%E1%85%A6%E1%84%86%E1%85%A9%E1%84%85%E1%85%B5%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B7.png?raw=true">

여기서 새로운 차량을 추가한다면 또 다시 `Driver` 코드를 많이 변경해야 합니다.

만약 운전할 수 있는 차량의 종류가 계속 늘어난다면 점점 더 변경해야 하는 코드가 많아질 것입니다.

<img src = "https://github.com/devKobe24/images/blob/main/%E1%84%83%E1%85%A1%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%A7%E1%86%A8%E1%84%92%E1%85%A1%E1%86%AF%E1%84%80%E1%85%AA%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A62%E1%84%86%E1%85%A6%E1%84%86%E1%85%A9%E1%84%85%E1%85%B5%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B72.png?raw=true">

* **"이 코드의 본질적인 문제는 자동차가 늘어나는데, 자동차 운전자의 코드를 계속해서 뜯어 고쳐야한다는 것 입니다."**
* **이런 문제가 생기는 이유는 "역할과 구현을 분리하지 않았기 때문입니다".**
