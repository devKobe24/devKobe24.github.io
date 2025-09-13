---
title: "☕️[Java] Class 클래스"
tags:
    - Java
    - Programming Language
    - Backend
date: "2024-09-20"
thumbnail: "/assets/img/thumbnail/JV.jpeg"
---

# ☕️[Java] Class 클래스.
- 자바에서 `Class` 클래스는 클래스의 정보(메타데이터)를 다루는데 사용됩니다.
- `Class` 클래스를 통해 개발자는 실행 중인 자바 애플리케이션 내에서 필요한 클래스의 속성과 메소드에 대한 정보를 조회하고 조작할 수 있습니다.

## 1️⃣ `Class` 클래스의 주요 기능
- **타입 정보 얻기.**
    - 클래스의 이름, 슈퍼클래스, 인터페이스, 접근 제한자 등과 같은 정보를 조회할 수 있습니다.
- **리플랙션.**
    - 클래스에 정의된 메소드, 필드, 생성자 등을 조회하고, 이들을 통해 객체 인스턴스를 생성하거나 메소드를 호출하는 등의 작업을 할 수 있습니다.
- **동적 로딩과 생성.**
    - `Class.forName()` 메서드를 사용하여 클래스를 동적으로 로드하고, `newInstance()` 메서드를 통해 새로운 인스턴스를 생성할 수 있습니다.
- **애노테이션 처리.**
    - 클래스에 적용된 애노테이션(annotation)을 조회하고 처리하는 기능을 제공합니다.

### 예시.
- 예를 들어, `String.class`는 `String` 클래스에 대한 `Class` 객체를 나타내며, 이를 통해 `String` 클래스에 대한 메타데이터를 조회하거나 조작할 수 있습니다.

```java
package langReview.clazz;

import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class ClassMetaMain {
    public static void main(String[] args) throws Exception {
        // Class 조회
        Class clazz = String.class; // 1. 클래스에서 조회.
        // Class clazz = new String().getClass(); // 2. 인스턴스에서 조회.
        // Class clazz = Class.forName("java.lang.String"); // 3. 문자열로 조회

        // 모든 필드 출력
        Field[] fields = clazz.getDeclaredFields();
        for (Field field : fields) {
            System.out.println("field = " + field);
            System.out.println("field.getType() = " + field.getType() + " " + field.getName());
        }

        // 모든 메서드 출력
        Method[] methods = clazz.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println("method = " + method);
        }

        // 상위 클래스 정보 출력
        System.out.println("Superclass: " + clazz.getSuperclass());

        // 인터페이스 정보 출력

        Class[] interfaces = clazz.getInterfaces();
        for (Class i : interfaces) {
            System.out.println("Interface: " + i.getName());
        }
    }
}
```

> **class vs clazz**
> **class는 자바의 예약어입니다. 따라서 패키지명, 변수명으로 사용할 수 없습니다.**
> 이런 이유로 자바 개발자들은 `class` 대신 `clazz`라는 이름을 관행으로 사용합니다.
> `clazz`는 `class`와 유사하게 들리고, 이 단어가 `class`를 의미한다는 것을 쉽게 알 수 있습니다.

**주의!**
- `main()` 옆에 `throws Exception`이 추가된 부분에 주의합시다.
    - 이 코드가 없으면 컴파일 오류가 발생합니다.

**실행 결과**
```bash
field = private final byte[] java.lang.String.value
field.getType() = class [B value
field = private final byte java.lang.String.coder
field.getType() = byte coder
field = private int java.lang.String.hash
field.getType() = int hash
field = private boolean java.lang.String.hashIsZero
field.getType() = boolean hashIsZero
field = private static final long java.lang.String.serialVersionUID
field.getType() = long serialVersionUID
field = static final boolean java.lang.String.COMPACT_STRINGS
field.getType() = boolean COMPACT_STRINGS
field = private static final java.io.ObjectStreamField[] java.lang.String.serialPersistentFields
field.getType() = class [Ljava.io.ObjectStreamField; serialPersistentFields
field = private static final char java.lang.String.REPL
field.getType() = char REPL
field = public static final java.util.Comparator java.lang.String.CASE_INSENSITIVE_ORDER
field.getType() = interface java.util.Comparator CASE_INSENSITIVE_ORDER
field = static final byte java.lang.String.LATIN1
field.getType() = byte LATIN1
field = static final byte java.lang.String.UTF16
field.getType() = byte UTF16
method = byte[] java.lang.String.value()
method = public boolean java.lang.String.equals(java.lang.Object)
method = public int java.lang.String.length()
method = public java.lang.String java.lang.String.toString()
method = static void java.lang.String.checkIndex(int,int)
method = public int java.lang.String.hashCode()
method = public void java.lang.String.getChars(int,int,char[],int)
method = public int java.lang.String.compareTo(java.lang.Object)
method = public int java.lang.String.compareTo(java.lang.String)
method = public int java.lang.String.indexOf(java.lang.String,int,int)
method = static int java.lang.String.indexOf(byte[],byte,int,java.lang.String,int)
method = public int java.lang.String.indexOf(java.lang.String,int)
method = public int java.lang.String.indexOf(int)
method = public int java.lang.String.indexOf(int,int)
method = public int java.lang.String.indexOf(int,int,int)
method = public int java.lang.String.indexOf(java.lang.String)
method = public static java.lang.String java.lang.String.valueOf(long)
method = public static java.lang.String java.lang.String.valueOf(char[])
method = public static java.lang.String java.lang.String.valueOf(java.lang.Object)
method = public static java.lang.String java.lang.String.valueOf(char[],int,int)
method = public static java.lang.String java.lang.String.valueOf(float)
method = public static java.lang.String java.lang.String.valueOf(double)
method = public static java.lang.String java.lang.String.valueOf(char)
method = public static java.lang.String java.lang.String.valueOf(boolean)
method = public static java.lang.String java.lang.String.valueOf(int)
method = byte java.lang.String.coder()
method = private static java.lang.Void java.lang.String.rangeCheck(char[],int,int)
method = static int java.lang.String.checkBoundsOffCount(int,int,int)
method = private static java.nio.charset.Charset java.lang.String.lookupCharset(java.lang.String) throws java.io.UnsupportedEncodingException
method = private static char java.lang.String.decode2(int,int)
method = private static int java.lang.String.decodeUTF8_UTF16(byte[],int,int,byte[],int,boolean)
method = private static int java.lang.String.scale(int,float)
method = private static int java.lang.String.decodeWithDecoder(java.nio.charset.CharsetDecoder,char[],byte[],int,int) throws java.nio.charset.CharacterCodingException
method = private static java.lang.String java.lang.String.newStringNoRepl1(byte[],java.nio.charset.Charset)
method = static java.lang.String java.lang.String.newStringUTF8NoRepl(byte[],int,int,boolean)
method = private static void java.lang.String.throwMalformed(byte[])
method = private static void java.lang.String.throwMalformed(int,int)
method = private static byte[] java.lang.String.encodeUTF8(byte,byte[],boolean)
method = private static byte[] java.lang.String.encode8859_1(byte,byte[],boolean)
method = private static byte[] java.lang.String.encode8859_1(byte,byte[])
method = private static byte[] java.lang.String.encodeASCII(byte,byte[])
method = private static byte[] java.lang.String.encodeWithEncoder(java.nio.charset.Charset,byte,byte[],boolean)
method = private static byte[] java.lang.String.safeTrim(byte[],int,boolean)
method = private static byte[] java.lang.String.encode(java.nio.charset.Charset,byte,byte[])
method = private static byte[] java.lang.String.getBytesNoRepl1(java.lang.String,java.nio.charset.Charset)
method = private static boolean java.lang.String.isASCII(byte[])
method = private static void java.lang.String.throwUnmappable(byte[])
method = private static void java.lang.String.throwUnmappable(int)
method = private static void java.lang.String.replaceNegatives(byte[],int)
method = private static boolean java.lang.String.isNotContinuation(int)
method = private static boolean java.lang.String.isMalformed3(int,int,int)
method = private static int java.lang.String.malformed3(byte[],int)
method = private static char java.lang.String.decode3(int,int,int)
method = private static boolean java.lang.String.isMalformed3_2(int,int)
method = private static int java.lang.String.decode4(int,int,int,int)
method = private static boolean java.lang.String.isMalformed4(int,int,int)
method = private static int java.lang.String.malformed4(byte[],int)
method = private static boolean java.lang.String.isMalformed4_2(int,int)
method = private static boolean java.lang.String.isMalformed4_3(int)
method = private static byte[] java.lang.String.encodeUTF8_UTF16(byte[],boolean)
method = boolean java.lang.String.isLatin1()
method = public char java.lang.String.charAt(int)
method = public int java.lang.String.codePointAt(int)
method = public int java.lang.String.codePointBefore(int)
method = public int java.lang.String.codePointCount(int,int)
method = public int java.lang.String.offsetByCodePoints(int,int)
method = static void java.lang.String.checkBoundsBeginEnd(int,int,int)
method = public byte[] java.lang.String.getBytes()
method = void java.lang.String.getBytes(byte[],int,int,byte,int)
method = void java.lang.String.getBytes(byte[],int,byte)
method = public void java.lang.String.getBytes(int,int,byte[],int)
method = public byte[] java.lang.String.getBytes(java.lang.String) throws java.io.UnsupportedEncodingException
method = public byte[] java.lang.String.getBytes(java.nio.charset.Charset)
method = public boolean java.lang.String.contentEquals(java.lang.CharSequence)
method = public boolean java.lang.String.contentEquals(java.lang.StringBuffer)
method = private boolean java.lang.String.nonSyncContentEquals(java.lang.AbstractStringBuilder)
method = public boolean java.lang.String.regionMatches(int,java.lang.String,int,int)
method = public boolean java.lang.String.regionMatches(boolean,int,java.lang.String,int,int)
method = public boolean java.lang.String.startsWith(java.lang.String)
method = public boolean java.lang.String.startsWith(java.lang.String,int)
method = static int java.lang.String.lastIndexOf(byte[],byte,int,java.lang.String,int)
method = public int java.lang.String.lastIndexOf(int)
method = public int java.lang.String.lastIndexOf(java.lang.String)
method = public int java.lang.String.lastIndexOf(int,int)
method = public int java.lang.String.lastIndexOf(java.lang.String,int)
method = public java.lang.String java.lang.String.substring(int,int)
method = public java.lang.String java.lang.String.substring(int)
method = public boolean java.lang.String.isEmpty()
method = public java.lang.String java.lang.String.replace(char,char)
method = public java.lang.String java.lang.String.replace(java.lang.CharSequence,java.lang.CharSequence)
method = public boolean java.lang.String.matches(java.lang.String)
method = public java.lang.String java.lang.String.replaceFirst(java.lang.String,java.lang.String)
method = public java.lang.String java.lang.String.replaceAll(java.lang.String,java.lang.String)
method = private java.lang.String[] java.lang.String.split(java.lang.String,int,boolean)
method = public java.lang.String[] java.lang.String.split(java.lang.String)
method = public java.lang.String[] java.lang.String.split(java.lang.String,int)
method = private java.lang.String[] java.lang.String.split(char,int,boolean)
method = public java.lang.String[] java.lang.String.splitWithDelimiters(java.lang.String,int)
method = static java.lang.String java.lang.String.join(java.lang.String,java.lang.String,java.lang.String,java.lang.String[],int)
method = public static java.lang.String java.lang.String.join(java.lang.CharSequence,java.lang.Iterable)
method = public static java.lang.String java.lang.String.join(java.lang.CharSequence,java.lang.CharSequence[])
method = public java.lang.String java.lang.String.toLowerCase()
method = public java.lang.String java.lang.String.toLowerCase(java.util.Locale)
method = public java.lang.String java.lang.String.toUpperCase()
method = public java.lang.String java.lang.String.toUpperCase(java.util.Locale)
method = public java.lang.String java.lang.String.trim()
method = public java.lang.String java.lang.String.strip()
method = public java.lang.String java.lang.String.stripLeading()
method = public java.lang.String java.lang.String.stripTrailing()
method = private int java.lang.String.indexOfNonWhitespace()
method = public java.util.stream.Stream java.lang.String.lines()
method = public java.lang.String java.lang.String.repeat(int)
method = private int java.lang.String.lastIndexOfNonWhitespace()
method = private static int java.lang.String.outdent(java.util.List)
method = public boolean java.lang.String.isBlank()
method = public char[] java.lang.String.toCharArray()
method = public static java.lang.String java.lang.String.format(java.util.Locale,java.lang.String,java.lang.Object[])
method = public static java.lang.String java.lang.String.format(java.lang.String,java.lang.Object[])
method = static void java.lang.String.repeatCopyRest(byte[],int,int,int)
method = public java.lang.Object java.lang.String.resolveConstantDesc(java.lang.invoke.MethodHandles$Lookup) throws java.lang.ReflectiveOperationException
method = public java.lang.String java.lang.String.resolveConstantDesc(java.lang.invoke.MethodHandles$Lookup)
method = public java.util.stream.IntStream java.lang.String.codePoints()
method = static java.lang.String java.lang.String.newStringNoRepl(byte[],java.nio.charset.Charset) throws java.nio.charset.CharacterCodingException
method = static byte[] java.lang.String.getBytesUTF8NoRepl(java.lang.String)
method = static byte[] java.lang.String.getBytesNoRepl(java.lang.String,java.nio.charset.Charset) throws java.nio.charset.CharacterCodingException
method = static int java.lang.String.decodeASCII(byte[],int,char[],int,int)
method = public boolean java.lang.String.equalsIgnoreCase(java.lang.String)
method = public int java.lang.String.compareToIgnoreCase(java.lang.String)
method = public boolean java.lang.String.endsWith(java.lang.String)
method = public java.lang.CharSequence java.lang.String.subSequence(int,int)
method = public java.lang.String java.lang.String.concat(java.lang.String)
method = public boolean java.lang.String.contains(java.lang.CharSequence)
method = public java.lang.String java.lang.String.indent(int)
method = public java.lang.String java.lang.String.stripIndent()
method = public java.lang.String java.lang.String.translateEscapes()
method = public java.util.stream.IntStream java.lang.String.chars()
method = public java.lang.Object java.lang.String.transform(java.util.function.Function)
method = public java.lang.String java.lang.String.formatted(java.lang.Object[])
method = public static java.lang.String java.lang.String.copyValueOf(char[],int,int)
method = public static java.lang.String java.lang.String.copyValueOf(char[])
method = public native java.lang.String java.lang.String.intern()
method = static void java.lang.String.checkOffset(int,int)
method = static java.lang.String java.lang.String.valueOfCodePoint(int)
method = public java.util.Optional java.lang.String.describeConstable()
method = private static java.lang.String java.lang.String.lambda$stripIndent$3(int,java.lang.String)
method = private static java.lang.String java.lang.String.lambda$indent$2(int,java.lang.String)
method = private static java.lang.String java.lang.String.lambda$indent$1(java.lang.String)
method = private static java.lang.String java.lang.String.lambda$indent$0(java.lang.String,java.lang.String)
Superclass: class java.lang.Object
Interface: java.io.Serializable
Interface: java.lang.Comparable
Interface: java.lang.CharSequence
Interface: java.lang.constant.Constable
Interface: java.lang.constant.ConstantDesc
```

- `Class` 클래스는 다음과 같이 3가지 방법으로 조회할 수 있습니다.
```java
Class clazz = String.class; // 1. 클래스에서 조회.
Class clazz = new String().getClass(); // 2. 인스턴스에서 조회.
Class clazz = Class.forName("java.lang.String"); // 3. 문자열로 조회
```

### Class 클래스의 주요 기능.
- **getDeclaredFields()**
    - 클래스의 모든 필드를 조회합니다.
- **getDeclaredMethods()**
    - 클래스의 모든 메서드를 조회합니다.
- **getSuperclass()**
    - 클래스의 부모 클래스를 조회합니다.
- **getInterfaces()**
    - 클래스의 인터페이스들을 조회합니다.

## 2️⃣ 클래스 생성하기.
- `Class` 클래스에는 클래스의 모든 정보가 들어있습니다.
- 이 정보를 기반으로 인스턴스를 생성하거나, 메서드를 호출하고, 필드의 값도 변경할 수 있습니다.
    - 아래의 예제에서는 간단하게 인스턴스를 생성해보겠습니다.

```java
package langReview.clazz;

public class Hello {
    public String hello() {
        return "hello!";
    }
}
```

```java
package langReview.clazz;

public class ClassCreateMain {
    public static void main(String[] args) throws Exception {
        Class helloClass = Hello.class;
        //Class helloClass = Class.forName("lang.clazz.Hello");

        Hello hello = (Hello) helloClass.getDeclaredConstructor().newInstance();
        String result = hello.hello();
        System.out.println("result = " + result);
    }
}
```

**실행 결과**
```bash
result = hello!
```

**getDeclaredConstructor().newInstance()**
- `getDeclaredConstructor()` : 생성자를 선택합니다.
- `newInstance()` : 선택된 생성자를 기반으로 인스턴스를 생성합니다.

**리플랙션 - reflection**
- `Class`를 사용하면 클래스의 메타 정보를 기반으로 클래스에 정의된 메소드, 필드, 생성자 등을 조회하고, 이들을 통해 객체 인스턴스를 생성하거나 메소드를 호출하는 작업을 할 수 있습니다.
    - 이런 작업을 리플랙션이라고 합니다.
- 추가로 애노테이션 정보를 읽어서 특별한 기능을 수행할 수 있습니다.
- 최신 프레임워크들은 이런 기능을 적극 활용합니다.
