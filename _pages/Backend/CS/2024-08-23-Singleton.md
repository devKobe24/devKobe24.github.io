---
title: "💾 [CS] 싱글톤 패턴"
tags:
    - CS
date: "2024-08-23"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾  [CS] 싱글톤 패턴.

## 1️⃣ 싱글톤 패턴(Singleton pattern)
- 싱글톤 패턴(singleton pattern)은 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴입니다.
- 하나의 클래스를 기반으로 여러 개의 개별적인 인스턴스를 만들 수 있지만, 그렇게 하지 않고 하나의 클래스를 기반으로 단 하나의 인스턴스를 만들어 이를 기반으로 로직을 만드는데 쓰입니다.
    - 보통 데이터베이스 연결 모듈에 많이 사용합니다.
- 하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈들이 공유하며 사용하기 때문에 인스턴스를 생성할 때 드는 비용이 줄어드는 장점이 있습니다.
    - 하지만 의존성이 높아진다는 단점이 있습니다.

## 2️⃣ Java에서의 싱글톤 패턴.
- Java에서 Singleton 패턴을 구현하는 방법은 여러 가지가 있지만, 가장 일반적으로 사용되는 방법 중 몇 가지를 소개하겠습니다.
    - Eager Initialization(즉시 초기화)
    - Lazy Initialization(지연 초기화)
    - Thread-safe Singleton(스레드 안전 싱글톤)
        - Synchronized Method
        - Double-checked Locking
    - Bill Pugh Singleton(Holder 방식)

### 1️⃣ Eager Initialization(즉시 초기화)
- 가장 간단한 방법으로, 클래스가 로드될 때 즉시 Singleton 인스턴스를 생성합니다.
```java
public class Singleton {
    // 유일한 인스턴스 생성
    private static final Singleton instance = new Singleton();
    
    // private 생성자: 외부에서 인스턴스 생성을 방지
    private Singleton() {}
    
    // 인스턴스를 반환하는 메서드
    public static Singleton getInstance() {
        return instance;
    }
}
```
- 이 방법은 간단하고 직관적이지만, 클래스가 로드될 때 바로 인스턴스가 생성되기 때문에, 인스턴스가 사용되지 않더라도 메모리를 차지하게 됩니다.

### 2️⃣ Lazy Initialization(지연 초기화)
- 인스턴스가 처음으로 필요할 때 생성되도록 합니다.
    - 이 방법은 초기화에 드는 비용이 큰 경우 유리합니다.
```java
public class Singleton {
    // 유일한 인스턴스를 저장할 변수 (초기에는 null)
    private static Singleton instance;
    
    // private 생성자: 외부에서 인스턴스 생성을 방지
    private Singleton() {}
    
    // 인스턴스를 반환하는 메서드 (필요할 때만 생성)
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
- 이 방법은 다중 스레드 환경에서 안전하지 않기 때문에, 추가적인 동기화가 필요합니다.

### 3️⃣ Thread-safe Singleton(스레드 안전 싱글톤)
- 다중 스레드 환경에서 안전하게 Lazy Initialization을 구현하려면 동기화를 사용합니다.

#### 1️⃣ Synchronized Method
```java
public class Singleton {
    private static Singleton instance;
    
    private Singleton() {}
    
    // synchronized 키워드로 스레드 안전하게 만듦
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
- 이 방법은 안전하지만, 성능에 약간의 영향을 줄 수 있습니다.
    - **`'synchronized'`** 로 인해 여러 스레드가 동시에 **'`getInstance()`'** 를 호출할 때 병목 현상이 발생할 수 있습니다.

#### 2️⃣ Double-checked Locking
- 이 방법은 성능과 스레드 안전성을 모두 고려한 최적화된 방식입니다.
```java
public class Singleton {
    private static volatile Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
- 여기서 **`'volatile'`** 키워드는 인스턴스 변수가 스레드 간에 올바르게 초기화되도록 보장합니다.

### 4️⃣ Bill Pugh Singleton(Holder 방식)
- 이 방법은 Lazy Initialization을 사용하면서도, 성능과 스레드 안전성을 모두 보장합니다.
```java
public class Singleton {
    
    private Singleton() {}
    
    // SingletonHolder가 클래스 로드 시점에 초기화됨
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    
    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```
- 이 방법은 내부 정적 클래스가 JVM에 의해 클래스 로드 시 초기화되므로, 가장 권장되는 방식 중 하나입니다.
    - 클래스가 로드될 때 초기화가 이루어지므로, 동기화나 추가적인 코드 없이도 스레드 안전성을 보장할 수 있습니다.

## 3️⃣ Spring Boot와 MySQL 데이터베이스의 연결 그리고 싱글턴 패턴.
- Spring Boot에서 MySQL 데이터베이스를 연결할 때, 내부적으로 **'싱글턴 패턴'** 이 사용됩니다.
    - 그러나 이 패턴을 직접 구현할 필요는 없습니다.
        - **'Spring Framework'** 자체가 싱글턴 패턴을 활용하여 데이터베이스 연결 및 관리와 관련된 **'Bean(객체)'** 을 관리합니다.

### 1️⃣ Spring Boot와 싱글턴 패턴.
- Spring Framework는 기본적으로 각 Bean을 싱글턴 스코프로 관리합니다.
    - 이는 특정 클래스의 인스턴스가 애플리케이션 컨텍스트 내에서 한 번만 생성되어 애플리케이션 전반에서 공유됨을 의미합니다.

### 2️⃣ 데이터베이스 연결에서의 싱글턴 패턴 사용.
- 1. **DataSource Bean.**
    - Spring Boot에서 MySQL과 같은 데이터베이스에 연결할 때 **`'DataSource'`** 라는 **`'Bean'`** 을 생성하여 관리합니다.
    - 이 **`'DataSource'`** 객체는 데이터베이스 연결을 관리하는 역할을 하며, Spring은 이 **`'Bean'`** 을 싱글턴으로 생성하고 관리합니다.
        - 즉, Spring 애플리케이션 내에서는 **`'DataSource'`** 객체가 하나만 생성되어 모든 데이터베이스 연결 요청에서 재사용됩니다.

- 2. **EntityManagerFactory 및 SessionFactory.**
    - JPA나 Hibernate와 같은 ORM을 사용하는 경우, **`'EntityManagerFactory'`** 나 **`'SessionFactory'`** 와 같은 객체도 싱글턴 패턴에 의해 관리됩니다.
        - 이들 객체는 데이터베이스 연결을 처리하고 트랜잭션을 관리하며, 역시 Spring에 의해 싱글턴으로 관리됩니다.

- 3. **Spring의 싱글턴 관리.**
    - Spring은 개발자가 **`'Bean'`** 을 직접 싱글턴으로 관리할 필요가 없도록, 애플리케이션의 컨텍스트 내에서 **`'Bean'`** 을 싱글턴으로 관리합니다.
    - 데이터베이스와의 연결 관련 클래스들이 이 **`'Bean'`** 들로 구성되며, 이는 데이터베이스 연결이 효율적이고 일관되게 관리되도록 보장합니다.

### 3️⃣ 예시: Spring Boot에서 MySQL 연결 설정.
- Spring Boot에서 MySQL 데이터베이스를 연결하기 위한 일반적인 설정은 **`'application.properties'`** 파일이나 **`'application.yml'`** 파일에 데이터베이스 연결 정보를 추가하는 것입니다.

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

- 이 설정은 Spring Boot가 **`'DataSource'`** **`'Bean'`** 을 자동으로 생성하도록 하며, 이 **`'Bean'`** 은 애플리케이션 내에서 싱글턴으로 관리됩니다.

### 4️⃣ ✏️ 요약
- Spring Boot에서 MySQL과 같은 데이터베이스를 연결할 때, Spring은 내부적으로 싱글턴 패턴을 사용하여 데이터베이스 연결을 관리합니다.
    - **`'DataSource'`**, **`'EntityManagerFactory'`** 등의 객체가 싱글턴으로 관리되며, 이를 통해 애플리케이션 전반에 걸쳐 일관되고 효율적인 데이터베이스 연결 관리가 이루어집니다.
        - Spring 자체가 이 패턴을 처리하므로, 개발자는 별도로 싱글턴 패턴을 구현할 필요가 없습니다.

## 4️⃣ Java Servlet 컨테이너와 MySQL 데이터베이스 연결 그리고 싱글턴 패턴.
- Java Servlet 컨테이너에서 MySQL 데이터베이스를 연결할 때, 싱글턴 패턴이 일반적으로 사용됩니다.
    - 다만, 이 패턴은 애플리케이션 코드에서 직접 구현되는 것이 아니라, 서블릿 컨테이너나 데이터베이스 연결 관리 라이브러리에서 사용됩니다.

### 1️⃣ JDBC DataSource
- 서블릿 컨테이너(예: Tomcat, Jetty)에서 데이터베이스 연결을 설정할 때 보통 **`'DataSource'`** 를 사용합니다.
    - 이 **`'DataSource'`** 객체는 보통 싱글턴으로 관리되며, 데이터베이스 연결 풀을 제공합니다.
- **Connection Pooling**
    - 서블릿 컨테이너는 데이터베이스 연결을 관리하기 위해 연결 풀링(Connection pooling)을 사용합니다.
        - 연결 풀은 여러 데이터베이스 연결을 미리 생성하고 재사용하도록 관리합니다.
    - 연결 풀을 관리하는 객채는 **`'DataSource'`** 이고, 이는 애플리케이션 내에서 싱글턴으로 관리되어, 여러 서블릿에서 동일한 **`'DataSource'`** 객체를 사용하여 효율적으로 데이터베이스에 연결할 수 있습니다.

### 2️⃣ 싱글턴 패턴의 활용.
- **DataSource 객체**
    - 서블릿 컨테이너는 보통 **`'DataSource'`** 객체를 싱글턴으로 관리합니다.
        - **`'DataSource'`** 는 데이터베이스 연결 풀을 관리하며, 이 객체가 한 번만 생성되어 애플리케이션 전반에 걸쳐 재사용됩니다.
- **Connection 객체**
    - 각 요청마다 데이터베이스 연결이 필요할 때마다 새로운 **`'Connection'`** 객체가 생성되거나 풀에서 가져오게 됩니다.
        - 하지만 **`'DataSource'`** 자체는 싱글턴으로 관리되기 때문에, 동일한 **`'DataSource'`** 객체를 통해 연결이 이루어집니다.

### 3️⃣ 예시: Tomcat에서 DataSource 설정
- Tomcat과 같은 서블릿 컨테이너에서 MySQL 데이터베이스와의 연결을 설정하는 일반적인 방법은 **`'context.xml'`** 파일에서 **`'DataSource'`** 를 정의하는 것입니다.

```xml
<Context>
    <Resource name="jdbc/MyDB"
              auth="Container"
              type="javax.sql.DataSource"
              maxTotal="100"
              maxIdel="30"
              maxWaitMillis="10000"
              username="root"
              password="password"
              driverClassName="com.myslq.cj.jdbc.Driver"
              url="jdbc:mysql://localhost:3306/mydb"/>
</Context>
```
- 이 설정은 **`'jdbc/MyDB'`** 라는 JNDI 리소스를 정의하고, **`'DataSource'`** 객체를 생성하여 연결 풀링을 관리합니다.
    - 이 **`'DataSource'`** 는 Tomcat 내에서 싱글톤으로 관리됩니다.

### 4️⃣ 싱글턴 패턴의 이점.
- **효율성.**
    - 여러 서블릿이 동일한 **`'DataSource'`** 객체를 공유함으로써 메모리와 자원을 절약할 수 있습니다.
- **관리의 용이성.**
    - 데이터베이스 연결 관리를 중앙화할 수 있으며, 코드에서 직접 관리할 필요 없이 서블릿 컨테이너가 이를 담당합니다.

### 5️⃣ ✏️ 요약
- Java Servlet 컨테이너에서 MySQL 데이터베이스를 연결할 때, 싱글턴 패턴은 주로 **`DataSource`** 객체에 적용됩니다.
    - 이 **`DataSource`** 객체는 서블릿 컨테이너에 의해 싱글턴으로 관리되며, 데이터베이스 연결 풀을 통해 효율적으로 데이터베이스 연결을 처리합니다.
        - 이를 통해 애플리케이션 전반에 걸쳐 일관되고 성능이 최적화된 데이터베이스 연결 관리가 이루어집니다.

## 3️⃣ Java 애플리케이션 서버와 MySQL 데이터베이스의 연결 그리고 싱글턴 패턴.
- Java 애플리케이션 서버에서 MySQL 데이터베이스를 연결할 때, 싱글턴 패턴은 우요한 역할을 합니다.
    - 그러나 이 패넡은 애플리케이션 코드에서 직접 구현되지 않으며, 애플리케이션 서버나 데이터베이스 연결 관리 라이브러리에서 사용됩니다.

### 1️⃣ DataSource와 Connection Pooling
- Java 애플리게이션 서버(예: JBoss/WildFly, GlassFish, WebSphere)에서 데이터베이스를 연결할 때 일반적으로 **`'JDBC DataSource'`** 와 **`'Connection Pooling'`** 을 사용합니다.
    - 이때 DataSource 객체는 싱글턴으로 관리되며, 데이터베이스 연결의 효율성을 높이기 위해 연결 풀을 사용합니다.
- **DataSource 싱글턴 관리**
    - 애플리케이션 서버는 데이터베이스와의 연결을 관리하기 위해 DataSource를 생성합니다.
        - 이 DataSource 객체는 서버에서 싱글턴으로 관리됩니다.
            - 즉, 애플리케이션 전반에 걸쳐 동일한 DataSource 객체가 사용됩니다.
    - DataSource는 내부적으로 데이터베이스 연결 풀을 관리하며, 여러 클라이언트 요청에서 동일한 데이터베이스 연결 객체를 재사용합니다.
- **Connection 객체 관리**
    - 데이터베이스와의 실제 연결을 관리하는 Connection 객체는 매번 새로운 요청이 있을 때마다 DataSource에서 가져오지만, DataSource는 싱글턴으로 관리되므로 전체 애플리케이션에서 일관된 연결 풀이 사용됩니다.

### 2️⃣ Java EE 환경에서의 DataSource 관리
- Java EE 애플리케이션 서버에서는 **`'JNDI(Java Naming and Directory Interface)'`** 를 통해 DataSource를 관리합니다.
    - 이는 서버의 전역 설정에서 관리되며, 여러 애플리케이션이 동일한 데이터베이스 연결을 공유할 수 있도록 합니다.
- **JNDI를 통한 DataSource 설정 예시**
```xml
<Resource name="jdbc/MyDB"
          auth="Container"
          type="javax.sql.DataSource"
          maxTotal="100"
          maxIdel="30"
          maxWaitMillis="10000"
          username="root"
          password="password"
          driverClassName="com.mysql.cj.jdbc.Driver"
          url="jdbc:mysql://localhost:3306/mydb"/>
```
- 이 설정은 애플리케이션 서버가 싱글턴 DataSource 객체를 생성하고 관리하도록 합니다.

### 3️⃣ 싱글턴 패턴의 역할.
- **효율성**
    - 싱글턴으로 관리되는 DataSource는 애플리케이션 서버 전체에서 하나의 객체로 유지되며, 이를 통해 메모리와 자원 사용이 최적화됩니다.
- **일관성**
    - 동일한 데이터베이스 연결 풀을 사용하기 때문에 애플리케이션 전방에 걸쳐 데이터베이스 연결이 일관되게 관리됩니다.
- **관리 용이성**
    - 데이터베이스 연결 관리가 중앙화되어, 각 애플리게이션에서 따로 관리할 필요 없이 서버에서 통합 관리됩니다.

### 4️⃣ EJB와의 통합.
- JavaEE 환경에서 EJB(Enterprise JavaBeans)는 주로 애플리케이션 서버에서 관리되는 비즈니스 로직을 구현하는 데 사용됩니다.
- EJB에서 데이터베이스 연결을 사용할 때도 싱글턴 패턴이 적용된 DataSource를 통해 연결이 이루어집니다.
```java
@Stateless
public class MyService {
    
    @Resource(lookup = "java:/jdbc/MyDB")
    private DataSource dataSource;
    
    public void doSomething() {
        try (Connection connection = dataSource.getConnection()) {
            // 데이터베이스 작업 수행
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```
- 이 코드에서 **`'dataSource'`** 는 서버에 의해 관리되는 싱글턴 DataSource 객체를 참조하며, 이를 통해 데이터베이스 연결을 처리합니다.

### 5️⃣ ✏️ 요약,
- Java 애플리케이션 서버에서 MySQL 데이터베이스를 연결할 때, 싱글턴 패턴은 DataSource와 같은 중요한 객체 관리에 사용됩니다.
    - 이 패턴을 통해 애플리케이션 서버는 데이터베이스 연결을 효율적이고 일관되게 관리할 수 있으며, 연결 풀링을 통해 자원 사용을 최적화합니다.
        - 애플리케이션 서버가 DataSource를 싱글턴으로 관리함으로써, 서버 전반에 일관된 데이터베이스 연결을 제공하고 효율성을 극대화할 수 있습니다.
