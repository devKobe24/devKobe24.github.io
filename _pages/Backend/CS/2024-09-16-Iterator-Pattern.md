---
title: "💾 [CS] 이터레이터 패턴(Iterator pattern)"
tags:
    - CS
date: "2024-09-16"
thumbnail: "/assets/img/thumbnail/cs.jpeg"
---

# 💾 [CS] 이터레이터 패턴(Iterator pattern).

## 1️⃣ 이터레이터 패턴(Iterator pattern).
- 이터레이터 패턴(Iterator pattern)은 이터레이터(Iterator)를 사용하여 컬렉션(collection)의 요소들에 접근하는 디자인 패턴입니다.
    - 이를 통해 순회할 수 있는 여러 가지 자료형의 구조와는 상관없이 이터레이터라는 하나의 인터페이스로 순회가 가능합니다.

## 2️⃣ 이터레이터(Iterator)
- 이터레이터(Iterator)는 프로그래밍에서 컬렉션(예: 베열, 리스트, 셋 등) 내의 요소들을 순차적으로 접근할 수 있게 해주는 객체를 말합니다.
- 이터레이터는 주로 루프를 통해 컬렉션의 요소들을 하나씩 가져와 처리할 때 사용됩니다.

### 이터레이터의 핵심 기능 두 가지.
- **1. `next()` :** 이터레이터의 다음 요소를 반환합니다. 다음 요소가 없을 경우 예외를 발생시키거나 특정 값을 반환할 수 있습니다.
- **2. `hasNext()` :** 다음에 가져올 요소가 있는지 여부를 확인합니다. 다음 요소가 있으면 `true`를, 없으면 `false`를 반환합니다.

#### 예시.
- 자바에서의 이터레이터 사용 예시는 다음과 같습니다.

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("A");
        list.add("B");
        list.add("C");
        
        Iterator<String> iterator = list.iterator();
        
        while (iterator.hasNext()) {
            String element = iterator.next();
            System.out.println(element);
        }
    }
}
```

- 위의 코드에서 `list.iterator()`를 통해서 리스트의 이터레이터를 얻고, `while` 루프를 통해 `hasNext()`로 다음 요소가 있는지 확인하면서 `next()`를 사용해 각 요소를 하나씩 출력합니다.

- 이터레이터는 컬렉션의 요소를 순차적으로 탐색할 수 있는 표준화된 방법을 제공하기 때문에, 컬렉션이 무엇이든 상관 없이 동일한 방식으로 접근할 수 있습니다.
    - 또한, 이터레이터를 사용하면 컬렉션 내부 구현에 직접 접근하지 않고도 요소들을 탐색할 수 있기 때문에 컬렉션의 안전한 접근과 수정이 가능합니다.

## 3️⃣ 자바에서의 이터레이터 패턴.
- 자바에서 이터레이터 패턴(Iterator Pattern)은 컬렉션 내부 구조를 노출하지 않고도 그 요소들에 순차적으로 접근할 수 있도록 하는 디자인 패턴입니다.
- 이 패턴은 `java.util.Iterator` 인터페이스를 통해 자바의 표준 라이브러리에서 널리 사용됩니다.

### 1. 기본 개념.
- 이터레이터 패턴은 컬렉션의 내부 구조를 숨기면서 요소들에 접근할 수 있게 해줍니다.
- 이 패턴은 반복자가 컬렉션 요소들을 순차적으로 탐색할 수 있는 메서드들을 정의합니다.

### 2. Iterator 인터페이스.
- 자바의 `Iterator` 인터페이스는 세 가지 주요 메서드를 가지고 있습니다.
    - `boolean hasNext()` : 다음에 읽어올 요소가 있는지 확인합니다. 있으면 `true`, 없으면 `false`를 반환합니다.
    - `E next()` : 다음 요소를 반환하고, 이터레이터를 다음 위치로 이동시킵니다.
    - `void remove()` : 이터레이터가 마지막으로 반환한 요소를 컬렉션에서 제거합니다.(이 메서드는 선택적으로 구현될 수 있습니다.)

### 3. 사용 예시.
- 먼저, 컬렉션 클래스(예: `ArrayList`, `HashSet` 등)의 이터레이터를 사용하여 요소들을 반복 처리하는 예시를 보겠습니다.

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class IteratorPatternExample {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");
        names.add("Charlie");

        Iterator<String> iterator = names.iterator();

        while (iterator.hasNext()) {
            String name = iterator.next();
            System.out.println(name);
        }
    }
}
```

### 4. 커스텀 이터레이터 구현.
- 자신만의 컬렉션 클래스와 그에 대한 이터레이터를 직접 구현할 수도 있습니다.
    - 예를 들어, 간단한 `Book` 컬렉션과 그 이터레이터를 구현할 수 있습니다.

```java
import java.util.Iterator;
import java.util.NoSuchElementException;

class Book {
    private String title;
    
    public Book(String title) {
        this.title = title;
    }
    
    public String getTitle() {
        return title;
    }
}

class BookCollection implements Iterable<Book> {
    private Book[] books;
    private int index = 0;
    
    public BookCollection(int size) {
        books = new Book[size];
    }
    
    public void addBook(Book book) {
        if (index < books.length) {
            books[index++] = books;
        }
    }
    
    @Override
    public Iterator<Book> iterator() {
        return new BookIterator();
    }
    
    private class BookIterator implements Iterator<Book> {
        private int currentIndex = 0;
        
        @Override
        public boolean hasNext() {
            return currentIndex < book.length && books[currentIndex] != null;
        }
        
        @Override
        public Book next() {
            if (!hasNext()) {
                throw new NoSuchElementException();
            }
            return books[currentIntex++];
        }
        
        @Override
        public void remove() {
            throw new UnsupportedOperationException("Remove not supported");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        BookCollection bookCollection = new BookCollection(3);
        bookCollection.addBook(new Book("The Catcher in th Rye"));
        bookCollection.addBook(new Book("To Kill a Mockingbird"));
        bookCollection.addBook(new Book("1984"));
        
        for (Book book : bookCollection) {
            System.out.println(book.getTitle());
        }
    }
}
```

### 5. 동작 원리.
- `BookCollection` 클래스는 `Iterable<Book>`을 구현하여 이터레이터 패턴을 따릅니다.
- `iterator()` 메서드는 `BookIterator` 라는 내부 클래스를 반환하며, 이 클래스는 `Iterator<Book>`을 구현합니다.
- `BookIterator`는 `hasNext()`, `next()`, 그리고 `remove()` 메서드를 구현하여 컬렉션의 요소들을 반복 처리합니다.

### 6. 마무리.
- 이터레이터 패턴은 이처럼 내부 구조를 감추고, 표준화된 방법으로 컬렉션의 요소들을 탐색할 수 있게 해줍니다.
    - 이는 코드의 재사용성과 유지 보수성을 높이는 데 기여합니다.
