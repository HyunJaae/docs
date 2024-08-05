---
description: 99클럽 코테 스터디 15일차 TIL 입니다.
---

# \[99클럽 코테 스터디 15일차 TIL]  LeetCode - Prefix and Suffix Search

### [745. Prefix and Suffix Search](https://leetcode.com/problems/prefix-and-suffix-search/)

#### Hard

***

Design a special dictionary that searches the words in it by a prefix and a suffix.

Implement the `WordFilter` class:

* `WordFilter(string[] words)` Initializes the object with the `words` in the dictionary.
* `f(string pref, string suff)` Returns _the index of the word in the dictionary,_ which has the prefix `pref` and the suffix `suff`. If there is more than one valid index, return **the largest** of them. If there is no such word in the dictionary, return `-1`.

&#x20;

**Example 1:**

<pre><code><strong>Input
</strong>["WordFilter", "f"]
[[["apple"]], ["a", "e"]]
<strong>Output
</strong>[null, 0]
<strong>Explanation
</strong>WordFilter wordFilter = new WordFilter(["apple"]);
wordFilter.f("a", "e"); // return 0, because the word at index 0 has prefix = "a" and suffix = "e".
</code></pre>

&#x20;

**Constraints:**

* `1 <= words.length <= 104`
* `1 <= words[i].length <= 7`
* `1 <= pref.length, suff.length <= 7`
* `words[i]`, `pref` and `suff` consist of lowercase English letters only.
* At most `104` calls will be made to the function `f`.

### 문제 요약

주어진 문자열 배열에서 인수로 전달할 접두사와 접미사로 이루어진 문자열의 인덱스를 반환하는데 중복될 경우, 더 큰 인덱스 값을 반환하면 되는 문제입니다.

***

## 풀이 코드

```java
class WordFilter {

    private final String[] words;

    public WordFilter(String[] words) {
        this.words = words;
    }
    
    public int f(String pref, String suff) {
        int idx = -1;
        for (int i = words.length - 1; i > -1; i--) {
            String word = words[i];
            if (word.charAt(0) != pref.charAt(0)) continue;
            if (word.startsWith(pref) && word.endsWith(suff)) {
                idx = i;
                break;
            }
        }
        return idx;
    }
}

/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter obj = new WordFilter(words);
 * int param_1 = obj.f(pref,suff);
 */
```

* Time: 1363 ms
* Space: 57.7 MB

제한 사항이 문자열 배열의 크기가 최대 10만개까지여서 시간 초과로 계속 실패했다. 중복될 경우 더 큰 인덱스 값을 반환하면 되니까 일단 문자열 배열을 뒤에서부터 순회하도록 했다.

그래도 시간 초과가 떠서 문자열의 첫번째 문자와 접두사의 첫번째 문자가 다르면 비교할 의미가 없기 때문에 바로 `continue` 시켜주는 조건문을 작성하니까 정말 턱걸이로 통과했다.

해당 문제는 Trie 알고리즘을 활용하거나 `HashMap` 에 문자열에 대한 모든 접두사와 접미사 조합을 `key` 로 추가하는 방법도 있다.

두 방법에 대해선 스스로 더 공부한 후에 공유하도록 하겠다.

***

## 돌아보기

이중 반복문을 너무 배제시켰던 것 같다. 시간 복잡도를 생각하면서 풀어봐야겠다.

### 다음에는

* 이중 `for` 문과 친해지기
* 시간 복잡도도 생각하기
* 자료구조 좀 활용하기

***

\#99클럽 #코딩테스트준비 #개발자취업 #항해99 #TIL
