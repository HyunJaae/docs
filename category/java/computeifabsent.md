---
description: ComputeIfAbsent 메소드에 대해 알아봅니다.
---

# computeIfAbsent 메소드를 알아보자

```java
Map<String, List<String>> results = new HashMap<>();

String key = "KEY";

if (!results.containsKey(key)) {
    results.put(key, new ArrayList<>());
}

results.get(key).add(s);
```

알고리즘을 풀다가 위와 같은 로직을 작성할 때가 있는데 한 줄로 처리 가능한 유용한 메소드가 있어 알아봤습니다.

`Map` 인터페이스에 `default` 메소드로 선언되어 있는 `computeIfAbsent` 라는 메소드인데 하는 역할은 위 코드와 동일합니다. `Map` 에 `key` 에 해당하는 값이 없다면 해당 `key` 와 전달된 값을 `Map` 에 넣고 있는 경우엔 `key` 에 해당하는 값을 가져와서 추가로 처리하는 것입니다. 위에선 리스트가 그 값이 되기 때문에 `add` 메소드를 통해서 `s` 라는 값을 추가하는걸 볼 수 있습니다.

위 로직을 `computeIfAbsent` 메소드를 사용하여 수정하면 아래와 같습니다.

```java
Map<String, List<String>> results = new HashMap<>();

String key = "KEY";

results.computeIfAbsent(key, k -> new ArrayList<>()) // key 에 해당하는 값이 없으면 새로운 리스트를 값으로 해서 저장하고 가져온다.
       .add(s); // key 에 해당하는 값인 리스트에 s 값을 할당한다.
```

`computeIfAbsent` 는 아래와 같이 구현되어 있습니다.

```java
default V computeIfAbsent(K key,
            Function<? super K, ? extends V> mappingFunction) {
        Objects.requireNonNull(mappingFunction);
        V v;
        if ((v = get(key)) == null) {
            V newValue;
            if ((newValue = mappingFunction.apply(key)) != null) {
                put(key, newValue);
                return newValue;
            }
        }

        return v;
    }
```

로직을 보면 `key` 로 가져온 값 `v` 가 `null` 이고 파라미터로 전달된 `mappingFunction` 함수를 실행하여 얻은 결과 값 `newValue` 가 `null` 이 아니면 `key` 와 결과 값을 맵에 할당하고 해당 결과 값을 반환합니다.

`v` 가 `null` 이 아니거나 `newValue` 가 `null` 이면 `v` 를 그대로 반환합니다. 값을 그대로 반환한다고 보면 됩니다.

앞으로도 알고리즘을 풀다보면 `Map` 을 쓸 일이 많을텐데 `computeIfAbsent` 메소드를 활용해보면 가독성을 해치지 않으면서 코드 줄 수를 줄일 수 있을 것 같습니다.

***

참고 자료 : [Java docs](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Map.html#computeIfAbsent\(K,java.util.function.Function\))
