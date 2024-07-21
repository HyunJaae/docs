---
description: Java8 부터 도입된 함수형 인터페이스와 자바에서 기본 제공하는 함수를 알아보겠습니다.
---

# Functional Interface

### 1. **@FunctionalInterface**

```java
@FunctionalInterface // 함수형 인터페이스 정의 시 견고한 관리 가능
public interface RunSomething {
​
    void doIt();
​
    static void printName() {
        System.out.println("Devlee");
    }
​
    default void printAge() {
        System.out.println("3");
    }
}
```

&#x20;함수형 인터페이스는 **하나의 추상 메서드(SAM : Single Abstract Method)를 가지고 있는 인터페이스**를 말합니다. 추가로 Java8 부터 인터페이스는 `static` 메서드와 `default` 메서드를 포함할 수 있고 함수형 인터페이스에서 해당 메서드들은 몇 개를 추가해도 상관이 없습니다.

&#x20;함수형 인터페이스 생성 시 `@FunctionalInterface` 어노테이션을 사용하면 해당 인터페이스가 함수형 인터페이스가 맞는지 검사합니다. 따라서 인터페이스 검증과 가독성을 위해 함수형 인터페이스 생성 시 `@FunctionalInterface` 어노테이션 사용을 권장합니다.

***

### 2. 람다 표현식(Lambda Expressions)

&#x20;함수형 인터페이스를 사용하면 Java8 부터 추가된 **람다 표현식(Lambda Expressions)**으로 구현체를 생성하여 사용할 수 있습니다. 아래는 그 예시입니다.

```java
public class Foo {
​
    public static void main(String[] args) {
        // Java8 이전에는 아래와 같이 구현해서 사용했어야 합니다. - 익명 내부 클래스(anonymous inner class)
        RunSomething runSomething = new RunSomething() {
            @Override
            public void doIt() {
                System.out.println("Hello");
            }
        };
      
        // Java8 이후
        RunSomething runSomethingLambda = () -> System.out.println("Hello");
        runSomethingLambda.doIt(); // -> Hello
    }
}
```

***

### 3. Java 에서 기본으로 제공하는 함수형 인터페이스

&#x20;Java 에서는 기본적으로 제공하는 함수형 인터페이스가 있습니다. 그 중 자주 사용하는 인터페이스는 아래와 같습니다.

| Interface            | Description                                          |
| -------------------- | ---------------------------------------------------- |
| Function\<T, R>      | Input : T, Output : R                                |
| BiFunction\<T, U, R> | 입력 값을 2개 받습니다.                                       |
| Consumer\<T>         | Return 값이 없고 받기만 합니다.                                |
| Supplier\<T>         | 입력 값이 없고 return 값만 있습니다.                             |
| Predicate\<T>        | 입력 값을 받고 boolean 값을 반환합니다.                           |
| UnaryOperator\<T>    | 입력 값의 타입과 반환 값의 타입이 같은 경우에 사용합니다.                    |
| BinaryOperator\<T>   | BiFunctional 의 2개의 입력 값과 1개의 반환 값의 타입이 같은 경우에 사용합니다. |

아래는 코드 예시입니다.

```java
import java.util.function.*;
​
public class Foo {
​
    public static void main(String[] args) {
        // Function<T, R>
        Function<Integer, Integer> plus10 = (i) -> i + 10;
        Function<Integer, Integer> multiply2 = (i) -> i * 2;
        // compse 와 andThen 으로 조합도 가능합니다.
        System.out.println(multiply2.compose(plus10).apply(2));
        System.out.println("andThen.apply(2) = " + plus10.andThen(multiply2).apply(2));
        // BiFunction<T, U, R>
        BiFunction<Integer, String, String> printAge = (i, s) -> i + s;
        System.out.println("printAge.apply(3, 살) = " + printAge.apply(3, "살"));
        // Consumer<T>
        Consumer<Integer> printT = (i) -> System.out.println(i);
        printT.accept(10);
        // Supplier<T>
        Supplier<Integer> get10 = () -> 10;
        System.out.println("get10.get() = " + get10.get());
        // Predicate<T>
        Predicate<String> startWithDev = (s) -> s.startsWith("dev");
        System.out.println(startWithDev.test("devlee"));
        // negate() 는 반대의 결과 값을 반환합니다. or(), and() 연산자도 있습니다.
        System.out.println(startWithDev.negate().test("devlee"));
        // UnaryOperator<T>, Function<T, R> 인터페이스를 상속 받습니다.
        UnaryOperator<Integer> plus10Unary = (i) -> i + 10;
        System.out.println(plus10Unary.apply(2));
        // BinaryOperator<T>
        BinaryOperator<Integer> multiply = (i, j) -> i * j;
        System.out.println(multiply.apply(2, 8));
    }
}
```

* 그 외 인터페이스 : [Java Docs](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)
