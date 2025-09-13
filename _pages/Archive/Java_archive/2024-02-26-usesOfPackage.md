---
title: "☕️[Java] 패키지 활용"
tags:
    - Java
    - Programming Language
date: "2024-02-26"
thumbnail: "/assets/img/thumbnail/java.jpeg"
---

# 패키지 활용

실제 패키지가 어떤 식으로 사용되는지 예제를 통해서 알아봅시다.
* 실제 동작하는 코드는 아니지만, 큰 애플리케이션은 대략 이런식으로 패키지를 구성한다고 이해하면 됩니다.

> 참고로 이것은 정답이 가니고 프로젝트 규모와 아키텍처에 따라서 달라집니다.

**전체 구조도**
* `com.helloshop`
    * `user`
        * `User`
        * `UserService`
    * `product`
        * `Product`
        * `ProductService`
    * `order`
        * `Order`
        * `OrderService`
        * `OrderHistory`

**com.helloshop.user 패키지**
```java
package com.helloshop.user;

public class User {
    String userId;
    String name;
}
```

```java
package com.helloshop.user;

public class UserService {

}
```

**com.helloshop.product 패키지**
```java
package com.helloshop.product;

public class Product {
  String productId;
  int price;
}
```

```java
package com.helloshop.product;

public class ProductService {

}
```

**com.helloshop.order 패키지**
```java
package com.helloshop.order;

import com.helloshop.product.Product;
import com.helloshop.user.User;

public class Order {
  User user;
  Product product;

  public Order(User user, Product product) {
    this.user = user;
    this.product = product;
  }
}
```

```java
package com.helloshop.order;

import com.helloshop.product.Product;
import com.helloshop.user.User;

public class OrderService {

  public void order() {
    User user = new User();
    Product product = new Product();
    Order order = new Order(user, product);
  }
}
```

```java
package com.helloshop.order;

public class OrderHistory {

}
```

* 패키지를 구성할 때 서로 관련된 클래스는 하나의 패키지에 모으고, 관련이 적은 클래스는 다른 패키지로 분리하는 것이 좋습니다.
