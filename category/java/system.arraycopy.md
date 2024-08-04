---
description: System.arraycopy 메소드에 대해 알아봅니다.
---

# System.arraycopy()

Java 로 알고리즘을 풀다보면 배열의 값을 다른 배열에 할당해야 하는 경우가 있습니다. 이때 JNI 로 구현된 `System.arraycopy(Object src, int srcPos, Object dest, int destPos, int length)` 를 사용하면 더 빠른 속도와 간결함을 보장받을 수 있습니다.

## 정의

Java 문서에서는 아래와 같이 정의하고 있습니다.

> Copies an array from the specified source array, beginning at the specified position, to the specified position of the destination array. <...> The number of components copied is equal to the `length` argument. The components at positions `srcPos` through `srcPos+length-1` in the source array are copied into positions `destPos` through `destPos+length-1`, respectively, of the destination array.

원본 배열을 특정 위치에서 시작하여 대상 배열에 특정 위치에 복사합니다. 그리고 복사된 요소의 개수는 `length` 값과 동일합니다. 원본 배열의 `srcPos` \~ `srcPos + length - 1` 위치에 있는 요소는 각각 대상 배열의 `destPos` \~ `destPos + length - 1` 위치에 복사됩니다.

이해하기 쉽게 실제 코드를 통해 알아봅시다.

## 알아보기

### 사용 방법

#### 1. 사용하기

```java
    @Test
    @DisplayName("System.arraycopy() 메소드 사용하기")
    void systemArrayCopyTest() {
        // given
        int[] src = {1, 2, 3, 4, 5};
        int[] dest = new int[5];
        // when
        System.arraycopy(src, 0, dest, 0, 5);
        // then
        assertThat(src).isEqualTo(dest);
    }
```

위 코드에서 `src` 배열은 복사할 원본 배열이고 0 \~ 0 + 5 - 1 위치의 요소 `{1, 2, 3, 4, 5}` 가 대상 배열 `dest` 의 0 \~ 0 + 5 - 1 위치에 복사됩니다. 따라서 배열 `dest` 의 값은 원본 배열과 동일한 `{1, 2, 3, 4, 5}` 입니다.

#### 2. 원본 배열의 복사할 원소 개수 조절하기

```java
    @Test
    @DisplayName("원본 배열의 복사할 요소 개수 조절하기")
    void changeCopyLength() {
        // given
        int[] src = {1, 2, 3, 4, 5};
        int[] dest = new int[5];
        // when
        System.arraycopy(src, 0, dest, 0, 3);
        //then
        int[] result = {1, 2, 3, 0, 0};
        assertThat(dest).isEqualTo(result);
    }
```

원본 배열에서 복사할 요소의 개수를 정할 수 있습니다. `length` 의 길이를 `3` 으로 설정했습니다. 그러면 `src` 배열의 0 \~ 0 + 3 - 1 까지 위치의 요소가 `dest` 배열의 0 \~ 0 + 3 - 1 위치에 복사됩니다. 결과는 위 코드에서 `result` 배열과 동일합니다.

#### 3. 원본 배열의 시작 위치 변경하기

```java
    @Test
    @DisplayName("원본 배열의 시작 위치 변경하기")
    void changeCopyStartPoint() {
        // given
        int[] src = {1, 2, 3, 4, 5};
        int[] dest = new int[5];
        // when
        System.arraycopy(src, 2, dest, 0, 3);
        // then
        int[] result = {3, 4, 5, 0, 0};
        assertThat(dest).isEqualTo(result);
    }
```

원본 배열의 시작 위치를 변경할 수 있습니다. `System.arraycopy()` 메소드의 두번째 인자 값을 변경하면 됩니다. 파라미터 명은 `srcPos` 입니다. 위 코드에서 `src` 배열의 2 \~ 2 + 3 - 1 까지 위치의 요소가 `dest` 배열에 복사됩니다.

4. 대상 배열의 시작 위치 변경하기

```java
    @Test
    @DisplayName("대상 배열의 시작 위치 변경하기")
    void changeCopyEndPoint() {
        // given
        int[] src = {1, 2, 3, 4, 5};
        int[] dest = new int[5];
        // when
        System.arraycopy(src, 2, dest, 2, 3);
        // then
        int[] result = {0, 0, 3, 4, 5};
        assertThat(dest).isEqualTo(result);
    }
```

`System.arraycopy()` 메소드의 4번째 인자를 변경하면 됩니다. 파라미터 명은 `destPos` 입니다. 위 코드에서 `dest` 배열의 2 \~ 2 + 3 - 1 까지 위치에 복사됩니다.

5. 원본 배열의 시작 위치(`srcPos`) + 복사할 요소 개수(`length`) 크기가 원본 배열 크기보다 큰 경우 에러

```java
    @Test
    @DisplayName("원본 배열의 시작 위치 + 복사할 요소 개수의 크기가 원본 배열의 크기보다 큰 경우 에러")
    void outOfSrcArrayLength() {
        // given
        int[] src = {1, 2, 3, 4, 5};
        int[] dest = new int[5];
        // when
        assertThatThrownBy(() -> System.arraycopy(src, 3, dest, 0, 3))
                .isInstanceOf(IndexOutOfBoundsException.class); // then
    }
```

위 코드에서 원본 배열의 3번째 인덱스부터 3개 요소를 복사하는데 원본 배열의 크기는 5 이기 때문에 3번째, 4번째 인덱스 요소까지는 복사가 가능하지만 5번째 인덱스는 없으므로 `IndexOutOfBoundsException` 이 발생하게 된다.

6. 대상 배열의 시작 위치(`destPos`) + 복사할 요소 개수(`length`)의 크기가 대상 배열의 크기보다 큰 경우 에러

```java
    @Test
    @DisplayName("대상 배열의 시작 위치 + 복사할 요소 개수의 크기가 대상 배열의 크기보다 큰 경우 에러")
    void outOfDestArrayLength() {
        // given
        int[] src = {1, 2, 3, 4, 5};
        int[] dest = new int[5];
        // when
        assertThatThrownBy(() -> System.arraycopy(src, 0, dest, 3, 3))
                .isInstanceOf(IndexOutOfBoundsException.class); // then
    }
```

앞서 5. 원본 배열의 시작 위치(`srcPos`) + 복사할 요소 개수(`length`) 크기가 원본 배열 크기보다 큰 경우 에러 에서 설명한 이유와 동일하다.

### Arrays.copyOf() 와 비교하기

배열을 복사하는 또 다른 방법으로는 `Arrays` 클래스에서 제공하는 `copyOf()` 메소드가 있는데 해당 메소드도 결국 내부적으로 `System.arraycopy()` 메소드를 사용하고 있어 성능 상의 차이는 없습니다. 대신 `Arrays` 클래스가 제공하는 다양한 메소드가 있어 편리함이 있습니다.

여기에선 `arraycopy()` 메소드의 내부 구현 상의 추가 로직이 있어 `System.arraycopy()` 메소드와의 차이점에 대해 간략히 알아보겠습니다.

#### 1. 복사되는 대상 배열의 크기

<pre class="language-java"><code class="lang-java"><strong>    @Test
</strong>    @DisplayName("Arrays.copyOf() 메소드와 비교하기")
    void arraysCopyOf() {
        // given
        int[] src = {1, 2, 3, 4, 5};
        int[] dest = new int[3]; // 크기가 3이라는 것에 주목
        // when
        System.arraycopy(src, 0, dest, 0, 3);
        int[] result = Arrays.copyOf(src, 3);
        // then
        assertThat(dest).isEqualTo(result);
    }    
</code></pre>

위 코드를 보면 `src` 배열의 요소 `{1, 2, 3}` 을 `dest` 배열의 0 \~ 3 - 1 위치에 복사합니다. 앞선 예제에서는 대상 배열의 크기가 5 였고 비어 있는 자리는 0 으로 채워져 있었습니다. 하지만 `Arrays.copyOf()` 메소드는 아래와 같이 구현되어 있어 복사할 요소의 개수만큼의 크기를 가진 배열을 만들어 반환합니다.

```java
public static int[] copyOf(int[] original, int newLength) {
    if (newLength == original.length) {
        return original.clone();
    }
    int[] copy = new int[newLength]; // 복사할 요소의 개수만큼의 크기를 가진 배열을 만든다.
    System.arraycopy(original, 0, copy, 0,
                     Math.min(original.length, newLength));
    return copy;
}
```

그래서 위 코드의 `newLength` 가 복사할 원본 배열 `original` 의 크기보다 커도 상관이 없습니다. 그 차이만큼 0 으로 채워집니다. 아래는 그 예시 코드입니다.

```java
    @Test
    @DisplayName("Arrays.copyOf() 메소드에서 배열의 크기를 넘어가는 경우")
    void outOfBoundsArrayLength() {
        // given
        int[] src = {1, 2, 3, 4, 5};
        int[] dest = new int[8]; // 크기가 8이라는 것에 주목
        // when
        System.arraycopy(src, 0, dest, 0, 5);
        int[] result = Arrays.copyOf(src, 8);
        // then
        assertThat(dest).isEqualTo(result);
    }
```

원본 배열의 0 \~ 4번째 위치의 요소를 복사하고 나머지 위치는 0 으로 채웁니다. `System.arraycopy()` 메소드에선 `dest` 배열의 크기를 8 로 미리 설정해야 합니다. `Arrays.copyof()` 를 사용하면 조금 더 유연하게 배열을 복사할 수 있습니다.

2. 정해진 범위 복사하기

```java
    @Test
    @DisplayName("정해진 범위 복사하기")
    void copyOfRange() {
        // given
        int[] src = {1, 2, 3, 4, 5};
        // when
        int[] dest = Arrays.copyOfRange(src, 2, 8); // 8번째는 제외
        int[] result = {3, 4, 5, 0, 0, 0};
        // then
        assertThat(dest).isEqualTo(result);
    }
```

2번째 위치의 요소부터 7번째 요소의 값을 복사하고 범위를 넘어선 경우, 0으로 채웁니다.

***

## 마무리

지금까지 배열을 복사하는 방법에 대해 알아봤습니다. `Arrays` 클래스는 위에서 말한 배열 복사 외에도 `asList()` 나 `compare()` 메소드, `fill()`, `equals()` 메소드 등 이름만 봐도 유용할 것 같은 다양한 메소드를 제공하기 때문에 다양한 상황에서 활용도가 높을 것 같습니다.

대신 `System.arraycopy()` 메소드는 이미 복사 대상인 배열이 있다면 매번 새로운 배열을 만들어서 반환하는 `Arrays.copyOf()` 메소드보다 더 적합할 것 같습니다.

***

## 참조

* [Java 11 docs - System.arraycopy()](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/System.html#arraycopy\(java.lang.Object,int,java.lang.Object,int,int\))
* [Java 11 docs - Arrays](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Arrays.html)
