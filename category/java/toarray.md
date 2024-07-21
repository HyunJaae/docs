---
description: 알고리즘을 풀다가 리스트를 배열로 변환하는 과정에서 IntelliJ 가 경고한 내용의 이유를 알고 싶어 공부한 내용이다.
---

# toArray 함수 호출 시 빈 배열을 전달해야 하는 이유

### 의문

ArrayList 의 `toArray()` 함수는 List 자료형을 배열로 변환해주는 함수이다. 아래는 그 예시 코드이다.

```java
List<String> letters = new ArrayList<>();
letters.add("king candy");

return letters.toArray(new String[letters.size()]);
```

이 코드를 실행하면 아무 문제없이 실행되긴 하나 IntelliJ 에서 아래와 같은 warning 메세지를 표출한다.

<figure><img src="../../.gitbook/assets/toArray 초기화 warning.png" alt=""><figcaption><p>toArray() method warning message</p></figcaption></figure>

미리 크기가 지정된 배열을 인수로 전달하여 `toArray()` 함수를 호출했다고 경고한다. 그리고 빈 배열로 변경하는 것을 추천하고 있다. 변경하게 되면 아래와 같아진다.

```java
return letters.toArray(new String[0]);
```

`toArray()` 함수는 왜 빈 배열을 통해 초기화해야 하는걸까? IntelliJ 에서는 다음과 같이 설명하고 있다.

> In older Java versions, using a pre-sized array was recommended, as the reflection call necessary to create an array of proper size was quite slow. However, since late updates of OpenJDK 6, this call was intrinsified, making the performance of the empty array version the same, and sometimes even better, compared to the pre-sized version. Also, passing a pre-sized array is dangerous for a concurrent or synchronized collection as a data race is possible between the size and toArray calls. This may result in extra nulls at the end of the array if the collection was concurrently shrunk during the operation.

번역해보니 **Java 6 이전**에는 적합한 크기의 배열을 생성하는데 필요한 **리플렉션 호출이 느렸기 때문에** 오히려 **크기가 지정된 배열 사용을 추천**했으나 이후에는 **최적화**를 통해 **빈 배열 버전의 성능**이 크기 지정 배열 버전과 동일하거나 **오히려 더 나아졌다**고 한다. 또한 **크기 지정 배열을 전달하는 경우 동시성 이슈가 발생**할 수 있다고 한다. 추가로 stackoverflow 에서도 찾아보니 동일한 답변을 확인했다.

> There are two styles to convert a collection to an array: either using a pre-sized array (like `c.toArray(new String[c.size()]`)) or using an empty array (like `c.toArray(new String[0])`. In older Java versions using pre-sized array was recommended, as the reflection call which is necessary to create an array of proper size was quite slow. However since late updates of OpenJDK 6 this call was intrinsified, making the performance of the empty array version the same and sometimes even better, compared to the pre-sized version. Also passing pre-sized array is dangerous for a concurrent or synchronized collection as a data race is possible between the size and toArray call which may result in extra nulls at the end of the array, if the collection was concurrently shrunk during the operation. This inspection allows to follow the uniform style: either using an empty array (which is recommended in modern Java) or using a pre-sized array (which might be faster in older Java versions or non-HotSpot based JVMs).

### 왜 리플렉션 호출이 더 느린가?

그렇다면 왜 리플렉션이 호출이 더 느린걸까? 이에 대해 알아보니 리플렉션 호출은 **많은 런타임 검사를 필요**로 하고 이로 인한 **오버헤드가 크다**고 한다. 따라서 리플렉션을 통해 배열을 생성한다면 배열의 타입과 크기를 확인하고, 해당 타입의 배열을 추가하는 작업이 필요하기 때문에 시간이 더 걸릴 수 있다. **Java 6 이전**에는 미리 **크기가 지정된 배열을 전달함으로써 리플렉션 호출을 피해** 시간을 더 줄일 수 있었다고 한다.

### 어떤 동시성 이슈가 발생하는가?

아래는 ArrayList 클래스에서 구현된 `toArray()` 함수이다.

<figure><img src="../../.gitbook/assets/toArray 구현체.png" alt=""><figcaption><p>toArray implement</p></figcaption></figure>

해당 코드의 주석을 보면 배열이 리스트보다 더 많은 요소를 가진다면 배열의 남는 공간을 `null` 로 설정한다고 한다. 코드에 있는 조건문에서도 확인할 수 있다. 그럼 어떻게 동시성 이슈가 발생할 수 있는지 예를 들어보면:

1. 크기가 5인 리스트를 리스트 크기의 문자열 배열을 인수로 전달하여 `toArray()` 함수를 호출한다.
2. 배열을 복사하고 있는 동안 **다른 스레드가 컬렉션의 요소를 2개 삭제**한다.
3. 인수로 전달된 문자열 배열의 크기는 5이고 컬렉션의 크기는 5에서 요소 2개가 삭제되어 3이 된다.
4. **마지막 조건문을 타게 되어** 배열의 나머지 2개의 슬롯은 `null` 로 채워지게 된다.

결국 `toArray()` 함수 호출자는 예상하지 못했던 `null` 값이 포함된 배열을 반환 받게 된다. 만약 빈 배열을 전달했다면 `toArray()` 메서드는 정확한 크기의 배열을 생성하여 동시성 이슈가 발생할 가능성을 줄일 수 있었다.

### 결론

컬렉션을 배열로 변환하는 `toArray()` 함수를 호출할 때 성능 상의 이점과 동시성 이슈 발생 가능성을 줄이기 위해 빈 배열을 전달하자.

### 참고 자료

[stackoverflow](https://stackoverflow.com/questions/18136437/whats-the-use-of-new-string0-in-toarraynew-string0/65902425#65902425)\
IntelliJ
