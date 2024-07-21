---
description: Kotlin 을 공부하면서 Java 와 다른 점을 정리한 글입니다.
---

# Java 와 다른 Kotlin

### <mark style="color:purple;">1. 변수 선언</mark>

* Java

```java
final String name = "Dex";
int age = 12;
```

* Kotlin

```kotlin
// 상수 변수 선언
val name: String = "Dex"
// 변수 선언
var age: Int = 12
// 추론 타입
val name = "Dex"
var age = 12
```

1. 세미콜론이 없다.
2. 추론 타입이 있어 자료형을 명시하지 않아도 된다.
3. 원시 타입이 없다.
   * 컴파일 시 때에 따라 원시 타입 또는 객체 타입으로 컴파일된다.
4. 자료형을 명시할 때는 `:` 뒤에 자료형을 작성한다.

### <mark style="color:purple;">2. 함수 선언</mark>

* Java

```java
public void main(String[] args) {
	System.out.println("Hello, World!");
}
```

* Kotlin

```kotlin
fun birthdayGreeting(name: String = "Rover", age: Int): String {
	val nameGreeting = "Happy Birthday, $name!"
	val ageGreeting = "You are now $age years old!"
	return "$nameGreeting\n$ageGreeting"
}

fun main() {
	println(birthdayGreeting(age = 3))
}
```

```kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}

fun sum(a: Int, b: Int) = a + b
```

1. `fun` 키워드로 함수를 선언한다.
2. 반환 타입을 지정하지 않을 경우 `Unit` 타입으로 반환된다.(`void` 타입과 동일)
3. 반환 타입 지정 시 `:` 뒤에 작성한다.
4. 반환 타입을 지정하고 반환 값이 없으면 에러가 발생한다.
5. <mark style="background-color:green;">매개 변수는 상수 값</mark>이며 타입을 선언해야 한다.
6. 인수에 이름을 지정할 수 있다.
7. 인수의 기본 값을 설정할 수 있다. (`name: String = "Rover"`)
8. 함수 본문을 표현식으로도 표현할 수 있다.

### <mark style="color:purple;">3. 클래스와 인스턴스 생성</mark>

* 클래스 생성

```kotlin
class Rectangle(var height: Double, var length: Double) {
    var perimeter = (height + length) * 2
}
```

* 인스턴스 생성

```kotlin
fun main() {
    val rectangle = Rectangle(5.0, 2.0)
    println("The perimeter is ${rectangle.perimeter}")
}
```

1. 인스턴스 생성 시 `new` 키워드를 붙이지 않아도 된다.

* 상속

```kotlin
open class Shape

class Rectangle(var height: Double, var length: Double) : Shape() { ... }
```

1. 상속할 클래스는 `open` 키워드를 사용해야 한다.

### <mark style="color:purple;">4. 문자열 템플릿</mark>

* Java

```java
int a = 10;
System.out.println("a = " + a);
```

* Kotlin

```kotlin
var a = 10
val s1 = "a is $a"

a = 20
println("${s1.replace("is", "was")}, but now is $a")
// -> a was 10, but now is 20
```

```kotlin
val a = "abc"
println("$a.length is ${a.length}")
// -> abc.length is 3
```

1. `${}` 를 통해 문자열 내에 표현식을 사용할 수 있다.
2. `$` 키워드를 통해 문자열에 바로 변수를 사용할 수 있다.

### <mark style="color:purple;">5. for loop</mark>

* Java

```java
List<String> fruits = List.of("apple", "banana", "kiwifruit");
for (int i = 0; i < fruits.size(); i++) {
    System.out.println(fruits.get(i));
}
for (String fruit : fruits) System.out.println(fruit);
```

* Kotlin

```kotlin
val items = listOf("apple", "banana", "kiwifruit")
for (item in items) println(item)
```

1. `in` 키워드를 통해 리스트를 순회한다. 향상된 for 문과 비슷하다.

### <mark style="color:purple;">6. when 표현식</mark>

```kotlin
fun describe(obj: Any): String =
    when(obj) {
        1 -> "One"
        "Hello" -> "Greeting"
        is Long -> "Long"
        !is String -> "Not a String"
        else -> "Unknown"
    }
```

1. Java 에서의 `Object` 타입이 Kotlin 에서는 `Any` 타입이다.
2. Java 에서는 `Object` 또는 `boolean` 타입을 switch 문에서 사용할 수 없으나 Kotlin 에서는 가능하다.
3. `instance of` 와 같은 역할인 `is` 연산자가 있다.
