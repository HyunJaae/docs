---
description: 람다와 스트림에 대해 다룹니다.
---

# Chapter 14. Lambda & Stream

## 1. 람다식

&#x20;JDK1.8부터 추가된 람다식(Lambda expression)으로 인해 자바는 객체지향 언어인 동시에 함수형 언어가 되었습니다.

### 1.1 람다식이란?

&#x20;<mark style="color:purple;background-color:green;">**람다식(Lambda expression)**</mark>은 간단히 말해서 <mark style="color:purple;background-color:green;">**메서드를 하나의 '식(expression)'**</mark>으로 표현한 것입니다.\
메서드를 람다식으로 표현하면 <mark style="color:purple;">**메서드의 이름과 반환 값이 없어지므로**</mark>, 람다식을 '익명 함수(anonymous function)' 이라고도 합니다. 아래는 람다식의 예시입니다.

```java
int[] arr = new int[5];
Arrays.setAll(arr, (i) -> (int)(Math.random()*5) + 1);
```

&#x20;모든 메서드는 클래스에 포함되어야 하므로 클래스도 새로 만들어야 하고, 객체도 생성해야만 비로소 이 메서드를 호출할 수 있습니다. 하지만 람다식은 이 모든 과정없이 오직 <mark style="color:purple;">**람다식 자체만으로도 이 메서드의 역할**</mark>을 대신할 수 있습니다.

&#x20;게다가 <mark style="color:purple;">**람다식은 메서드의 매개변수로 전달되어지는 것이 가능**</mark>하고, 메서드의 결과로 반환될 수도 있습니다. 람다식으로 인해 메서드를 변수처럼 다루는 것이 가능해진 것입니다.

### 1.2 람다식 작성하기

예를 들어 두 값 중에서 큰 값을 반환하는 메서드 max를 람다식으로 반환하면, 아래와 같습니다.

```java
int max(int a, int b) {
   return a > b ? a : b;
}
// 람다식
(int a, int b) -> {
    return a > b ? a : b;
}
```

반환 값이 있는 메서드의 경우, return문 대신 식(expression)으로 대신 할 수 있습니다. 이때는 문장(statement)이 아닌 <mark style="color:purple;">**식이므로 끝에 ; 을 붙이지 않습니다.**</mark>

```java
(int a, int b) -> a > b ? a : b
```

람다식에 선언된 매개변수의 <mark style="color:purple;">**타입은 추론이 가능한 경우 생략**</mark>할 수 있습니다. 람다식에 반환 타입이 없는 이유도 항상 추론이 가능하기 때문입니다.

```java
(a, b) -> a > b ? a : b
```

아래와 같이 선언된 매개변수가 하나뿐인 경우에는 괄호()를 생략할 수 있습니다. 단, <mark style="color:purple;">**매개변수의 타입이 있으면 괄호()를 생략할 수 없습니다.**</mark>

```java
a -> a * a // ok
int a -> a * a // error
(int a) -> a * a // ok
```

마찬가지로 <mark style="color:purple;">**중괄호{} 안의 문장이 하나일 때는 중괄호를 생략**</mark>할 수 있습니다. 이 때 문장의 끝에 ; 을 붙이지 않아야 한다는 것에 주의합시다.

```java
(String name, int i) -> {
  System.out.println(name + "-" + i);
}
// 중괄호 생략
(String name, int i) -> System.out.println(name + "-" + i)
```

그러나 중괄호 안의 문장이 <mark style="color:purple;">**return문일 경우 중괄호를 생략할 수 없습니다.**</mark>

```java
(int a, int b) -> { return a > b ? a : b; } // ok
(int a, int b) -> return a > b ? a : b // error
```

***
