---
description: 99클럽 코테 스터디 4일차 TIL 입니다.
---

# \[99클럽 코테 스터디 4일차 TIL] JadenCase 문자열 만들기

## \[level 2] JadenCase 문자열 만들기 - 12951

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/12951?language=java)

#### 문제 설명

JadenCase란 모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열입니다. 단, 첫 문자가 알파벳이 아닐 때에는 이어지는 알파벳은 소문자로 쓰면 됩니다. (첫 번째 입출력 예 참고)\
문자열 s가 주어졌을 때, s를 JadenCase로 바꾼 문자열을 리턴하는 함수, solution을 완성해주세요.

**제한 조건**

* s는 길이 1 이상 200 이하인 문자열입니다.
* s는 알파벳과 숫자, 공백문자(" ")로 이루어져 있습니다.
  * 숫자는 단어의 첫 문자로만 나옵니다.
  * 숫자로만 이루어진 단어는 없습니다.
  * 공백문자가 연속해서 나올 수 있습니다.

**입출력 예**

| s                       | return                  |
| ----------------------- | ----------------------- |
| "3people unFollowed me" | "3people Unfollowed Me" |
| "for the last week"     | "For The Last Week"     |

***

## 풀이 코드

```java
class Solution {
    public String solution(String s) {
        StringBuilder answer = new StringBuilder();
        char prevC = ' '; // 이전 문자
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            // 문자가 알파벳인 경우
            if (c >= 'a' && c <= 'z' || c >= 'A' && c <= 'Z') {
                // 문자가 알파벳이면서 이전 문자가 공백이면 대문자 변환
                if (prevC == ' ') {
                    answer.append(String.valueOf(c).toUpperCase());
                    prevC = c; // 이전 문자에 첫 문자 할당
                } else { // 소문자 변환
                    answer.append(String.valueOf(c).toLowerCase());
                }
            } else { // 문자가 숫자 혹은 공백 문자인 경우
                answer.append(c);
                prevC = c; // 이전 문자에 숫자 혹은 공백 문자 할당
            }
        }
        return answer.toString();
    }
}
```

* Time: 0.98 ms
* Memory: 80 MB

### 풀이 과정

1. 공백으로 잘라서 단어의 앞 문자를 대문자로 변경한다.
2. 공백 문자가 연속으로 올 수 있기 때문에 공백으로 잘라서 단어만 추출할 수 없다.
3. 주어지는 문자열의 길이가 최대 200자이기 때문에 길이만큼 반복문을 돌아도 시간이 오래 걸리지 않는다.
4. 공백 문자 다음 문자는 무조건 단어의 첫 문자이다. 따라서 단어의 첫 문자는 이전 문자가 공백 문자다.
5. 이전 문자가 공백인 문자를 대문자로 변경한다.

### 개선하기

<pre class="language-java"><code class="lang-java">class Solution {
<strong>    public String solution(String s) {
</strong>        StringBuilder answer = new StringBuilder();
        String prev = " ";
        for (int i = 0; i &#x3C; s.length(); i++) {
            String c = String.valueOf(s.charAt(i));
            String convert = prev.equals(" ") ? c.toUpperCase() : c.toLowerCase();
            answer.append(convert);
            prev = c;
        }
        return answer.toString();
    }
}
</code></pre>

* Time: 0.32 ms
* Memory: 71.9 MB

첫 풀이 코드는 구현한 내가 봐도 조건식이 복잡해서 로직을 개선해봤다. 다행히 가독성뿐 아니라 성능까지 개선된걸 볼 수 있다.

개선 포인트는 읽으려는 문자가 알파벳인지 숫자인지는 굳이 구분할 필요가 없다는 것이다. 숫자든 알파벳이든 단어의 첫 문자면 대문자로 변환하면 되고 공백인지만 구분하면 된다. 그리고 조건에 상관없이 모든 문자를 이전 문자에 할당한다.

### 다른 사람의 풀이

```java
class Solution {
    public String solution(String s) {
        String answer = "";
        String[] sp = s.toLowerCase().split("");
        boolean flag = true;

        for(String ss : sp) {
            answer += flag ? ss.toUpperCase() : ss;
            flag = ss.equals(" ") ? true : false;
        }

        return answer;
    }
}
```

* Time: 4.03 ms
* Memory: 90.7 MB

다른 분의 풀이 코드인데 마음에 든 코드였다. 모든 문자를 소문자로 변환하고 이전 문자가 공백 문자인지를 `boolean` 타입으로 구분해서 간결한 코드를 작성했다. 코드만으로도 단번에 이해가 돼서 보기 좋은 코드다.

***

## 돌아보기

권장 시간(30분) 안에 풀지 못했다. 아래와 같이 작성하고 한참을 헤매다 질문하기 내역에서 테스트 케이스를 확인하고 풀 수 있었다.

```java
class Solution {
    public String solution(String s) {
        if (s.isBlank()) return "";
        StringBuilder answer = new StringBuilder();
                String[] strings = s.replaceAll("\\W+", " ").strip().split(" ");

        for (String string : strings) {
            String upperCase = String.valueOf(string.charAt(0)).toUpperCase();
            answer.append(upperCase).append(string.substring(1).toLowerCase()).append(" ");
        }
        return answer.toString().strip();
    }
}
```

처음 문제를 보고 연속된 공백 문자가 나올 수 있다는 제한 조건에 사로잡혀서 하나의 공백 문자로 변경하도록 했다. 그리고 문제에도 없는데 앞 뒤 공백이 없어야 된다는 강박에 빠져서 `strip()` 메소드를 사용했다. 문제를 풀 때마다 새로운 함정에 빠지는 것 같아 재미있다.

{% hint style="info" %}
**`trim()` 과 `strip()` 의 차이**

`trim()` 메소드는 공백 문자(ASCII 값이 32 이하인 문자, 주로 공백, 탭, 줄 바꿈 그리고 캐리지 리턴)를 제거한다.

`strip()` 메소드는 유니코드의 공백들을 모두 제거하고 Java11 부터 지원된다.

유니코드에는 우리가 일반적으로 많이 사용하는 스페이스('\u0020'), 탭('\u0009') 등 외에도 더 많은 공백 문자들이 있다.
{% endhint %}

### 다음에는

1. 문제에 없는 조건을 만들어내지 말기
2. 시간에 쫓기지 말고 천천히 풀기

***

\#99클럽 #코딩테스트준비 #개발자취업 #항해99 #TIL
