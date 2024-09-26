---
title: "💾 [CS] 옵저버 패턴(Observer pattern)"
tags:
    - CS
date: "2024-09-05"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 옵저버 패턴(Observer pattern).
- 옵저버 패턴(observer pattern)은 주체가 어떤 객체(subject)의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴입니다.

<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/cs-observer-pattern.png?raw=true">

- 여기서 주체란 객체의 상태 변화를 보고 있는 관찰자이며, 옵저버들이란 이 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 '추가 변화 사항'이 생기는 객체들을 의미합니다.

<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/cs-observer-pattern-2.png?raw=true">

- 또한, 위의 그림처럼 주체와 객체를 따로 두지 않고 상태가 변경되는 객체를 기반으로 구축하기도 합니다.
- 옵저버 패턴을 활용한 서비스로는 트위터가 있습니다.

<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/cs-twitter-obser-pattern.png?raw=true">

- 위의 그림처럼 내가 어떤 사람인 주체를 '팔로우' 했다면 주체가 포스팅을 올리게 되면 알림이 '팔로워'에게 가야합니다.

<img src = "https://github.com/devKobe24/images2/blob/main/CS_IMG/cs-observer-pattern-structure.png?raw=true">

- 또한, 옵저버 패턴은 주로 이벤트 기반 시스템에 사용하며 MVC(Model-View-Controller) 패턴에도 사용됩니다.
    - 예를 들어 주체라고 볼 수 있는 모델(model)에서 변경 사항이 생겨 update() 메서드로 옵저버인 뷰에 알려주고 이를 기반으로 컨트롤러(controller) 등이 작동하는 것입니다.

## 1️⃣ 자바에서의 옵저버 패턴.

```java
// Observer
public interface Observer {
    void update();
}

// Subject
public interface Subject {
    void register(Observer obj);
    void unregister(Observer obj);
    void notifyObservers();
    Object getUpdate(Observer obj);
}

// Topic
import java.util.ArrayList;
import java.util.List;

public class Topic implements Subject {
    private List<Observer> observers;
    private String message;

    public Topic() {
        this.observers = new ArrayList<>();
        this.message = "";
    }
    @Override
    public void register(Observer obj) {
        if (!observers.contains(obj)) {
            observers.add(obj);
        }
    }

    @Override
    public void unregister(Observer obj) {
        observers.remove(obj);
    }

    @Override
    public void notifyObservers() {
        this.observers.forEach(Observer::update);
    }

    @Override
    public Object getUpdate(Observer obj) {
        return this.message;
    }

    public void postMessage(String msg) {
        System.out.println("Message sended to Topic: " + msg);
        this.message = msg;
        notifyObservers();
    }
}

// TopicSubscriber
public class TopicSubscriber implements Observer {

	private String name;
	private Subject topic;

	public TopicSubscriber(String name, Subject topic) {
		this.name = name;
		this.topic = topic;
	}

	@Override
	public void update() {
		String msg = (String) topic.getUpdate(this);
		System.out.println(name + ":: got message >> " + msg);
	}
}

// Main
public class Main {

	public static void main(String[] args) {
		Topic topic = new Topic();
		Observer a = new TopicSubscriber("a", topic);
		Observer b = new TopicSubscriber("b", topic);
		Observer c = new TopicSubscriber("c", topic);
		topic.register(a);
		topic.register(b);
		topic.register(c);

		topic.postMessage("nice to meet you");
	}
}
```

**실행 결과**
```bash
Message sended to Topic: nice to meet you
a:: got message >> nice to meet you
b:: got message >> nice to meet you
c:: got message >> nice to meet you
```

- `topic`을 기반으로 옵저버 패턴을 구현했습니다.
    - 여기서 `topic`은 주체이자 객체가 됩니다.
        - `class Topic implements Subject`를 통해 `Subject interface`를 구현했고 `Observer a = new TopicSubscriber("a", topic);` 으로 옵저버를 선언할 때 해당 이름과 어떠한 토픽의 옵저버가 될 것인지를 정했습니다.

### 자바: 상속과 구현
- 위의 코드에 나온 `implements` 등 자바의 상속과 구현의 특징과 차이에 대해 알아보겠습니다.
    - **상속(extends)**
        - 자식 클래스가 부모 클래스의 메서드 등을 상속받아 사용하며 자식 클래스에서 추가 및 확장을 할 수 있는 것을 말합니다.
            - 이로 인해 재사용성, 중복성의 최소화가 이루어집니다.
    - **구현(Implements)**
        - 부모 인터페이스(Interface)를 자식 클래스에서 재정의하여 구현하는 것을 말합니다.
            - 상속과는 달리 반드시 부모 클래스의 메서드를 재정의하여 구현해야 합니다.
    - **상속과 구현의 차이**
        - 상속은 일반 클래스, `abstract` 클래스를 기반으로 구현하며, 구현은 인터페이스를 기반으로 구현합니다.
