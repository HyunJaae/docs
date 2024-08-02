---
description: 99클럽 코테 스터디 12일차 TIL 입니다.
---

# \[99클럽 코테 스터디 12일차 TIL]  프로그래머스 - H-Index

## \[level 2] H-Index - 42747

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/42747)

#### 성능 요약

메모리: 78 MB, 시간: 0.93 ms

#### 구분

정렬

#### 문제 설명

H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과1에 따르면, H-Index는 다음과 같이 구합니다.

어떤 과학자가 발표한 논문 `n`편 중, `h`번 이상 인용된 논문이 `h`편 이상이고 나머지 논문이 h번 이하 인용되었다면 `h`의 최댓값이 이 과학자의 H-Index입니다.

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

**제한사항**

* 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
* 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.

**입출력 예**

| citations        | return |
| ---------------- | ------ |
| \[3, 0, 6, 1, 5] | 3      |

**입출력 예 설명**

이 과학자가 발표한 논문의 수는 5편이고, 그중 3편의 논문은 3회 이상 인용되었습니다. 그리고 나머지 2편의 논문은 3회 이하 인용되었기 때문에 이 과학자의 H-Index는 3입니다.

***

## 풀이 코드

```java
import java.util.Arrays;

class Solution {
    public int solution(int[] citations) {
        int len = citations.length;
        Arrays.sort(citations);
        
        for (int i = 0; i < citations.length; i++) {
            if (citations[i] >= len) return len;
            len--;
        }
        
        return 0;
    }
}
```

* Time: 0.93 ms
* Memory: 78 MB

푸는게 정말 어려웠다. 큰 힌트가 되었던 생각 중 하나가 **"H-Index 는 `citations` 길이보다 클 수 없다"** 였다.\
`citations` 의 원소 값이 얼마가 됐든 `citations` 의 길이 안에서 결과 값이 반환되어야 한다. 왜냐하면 문제 설명에 "논문 `n`편 중, `h`번 이상 인용된 논문이 `h`편 이상이고" 라고 했기 때문이다. `citations` 배열에 40이 5개 들어 있다면 그 값이 몇이 되었든 5 이하여야 한다.

***

## 돌아보기

문제를 좀 더 분석적으로 면밀히 파악할 필요가 있다. 차라리 스스로 시간을 정해두고 그 시간만큼은 문제에 집중하는게 좋을 것 같다.

***

\#99클럽 #코딩테스트준비 #개발자취업 #항해99 #TIL
