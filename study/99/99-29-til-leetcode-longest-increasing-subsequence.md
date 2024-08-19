---
description: 99클럽 코테 스터디 29일차 TIL 입니다.
---

# \[99클럽 코테 스터디 29일차 TIL]  LeetCode - Longest Increasing Subsequence

### [300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

#### Medium

***

Given an integer array `nums`, return _the length of the longest **strictly increasing subsequence**_.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [10,9,2,5,3,7,101,18]
</strong><strong>Output: 4
</strong><strong>Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [0,1,0,3,2,3]
</strong><strong>Output: 4
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: nums = [7,7,7,7,7,7,7]
</strong><strong>Output: 1
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= nums.length <= 2500`
* `-104 <= nums[i] <= 104`

&#x20;

Follow up: Can you come up with an algorithm that runs in `O(n log(n))` time complexity?

***

## 풀이 코드

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] len = new int[nums.length];
        Arrays.fill(len, 1);

        for (int i = 1; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) { // 나보다 작고
                    len[i] = Math.max(len[i], len[j] + 1); // 수열 길이 가장 큰 값 + 1
                }
            }
        }

        int max = 0;
        for (int l : len) {
            max = Math.max(max, l);
        }

        return max;
    }
}
```

* Time: 36 ms
* Space: 44.3 MB

해당 문제는 주어진 숫자 배열에서 가장 긴 증가 수열을 구하면 되는 문제다. 처음엔 수열을 공비수열로 생각해서 예시부터 왜 그렇게 나오는지 이해를 못했고 꽤 헤맸다.

어떻게 풀어야할지 처음 생각한 방식은 주어진 배열의 **각 수가 가장 마지막 숫자일 때의 수열 크기를 담은 배열(len)**을 만들고 그 중 가장 큰 값을 반환하는 것으로 생각했다. 이를 위해서 수를 뽑을 때 그 이전까지의 수 중에 작은 수가 있는지 확인한다. 여기까지만 생각하면 아래와 같은 문제가 생긴다.

* input : \[10, 9, 2, 5, 3, 7, 100]
* len : \[1, 1, 1, 2, 2, 4, 5]
* expected : 4
* output : 5

숫자 `7` 일 때의 수열은 `[2, 3, 7]` 로 길이는 `3` 이어야 하고 `100` 일 때는 `[2, 3, 7, 100]` 으로 길이가 `4` 여야 한다. 이 문제를 해결하기 위해 유심히 여러 케이스를 확인해보니 주어진 배열에서 뽑은 각 수가 마지막 숫자일 때의 가장 큰 증가 수열 크기는 이전까지의 뽑은 수 중 나보다 작으면서 가장 큰 증가 수열의 크기 + 1 인 것으로 확인했다. 예를 들어,

* input : \[10, 9, 2, 5, 3, 4, 100]
* len : \[10보다 작은 수 없음, 9보다 작은 수 없음, 2보다 작은 수 없음, 5보다 작은 2의 증가 수열의 크기 + 1, 3보다 작은 2의 증가 수열의 크기 + 1, 4보다 작은 2 와 3 중 증가 수열의 크기가 가장 큰 3의 증가 수열의 크기 + 1, 100보다 작은 2, 5, 3, 4 중 증가 수열의 크기가 가장 큰 4의 증가 수열의 크기 + 1]
* \-> len : \[1, 1, 1, 1 + 1, 1 + 1, (1 + 1) + 1, ((1 + 1) + 1) + 1]
* \-> len : \[1, 1, 1, 2, 2, 3, 4]

이해가 됐으면 좋겠습니다. 따라서 위 풀이 코드에 적혀 있는 주석의 조건식이 완성된다. 이 문제를 이분 탐색으로도 풀 수 있다고 한다. 아래 풀이는 클럽장님이 공유해주신 풀이로 풀이를 따라 예시 값을 넣다보면 이해가 된다.

### 이분 탐색 풀이

```java
자바- 미들러 이분탐색 풀이
// case 1
// 초기 dp  -> []
// 10 -> [10]
// 9 -> [9](9는 10보다 작아서)
// 2 -> [2](2는 9보다 작아서)
// 5 -> [2, 5]
// 3 -> [2, 3](5를 3으로 대체)
// 7 -> [2, 3, 7]
// 101 -> [2, 3, 7, 101]
// 18 -> [2, 3, 7, 18](101을 18으로 대체).


// case 2
// 0 -> [0]
// 1 -> [0, 1]
// 0 -> [0 ,1]
// 3 -> [0, 1, 3]
// 2 -> [0, 1, 2]
// 3 -> [0, 1, 2, 3]

class Solution {
     public static int lengthOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }

        // dp 배열은 LIS의 현재 길이를 저장
        int[] dp = new int[nums.length];
        int length = 0;  // 현재 LIS의 길이

        for (int num : nums) {
            // 이분 탐색을 사용하여 num이 dp 배열에서 적절한 위치를 찾기
            int left = 0, right = length;
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (dp[mid] < num) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            // left는 num이 들어가야 할 위치
            dp[left] = num;

            // length는 dp 배열의 현재 유효 길이
            if (left == length) {
                length++;
            }
        }
        return length;
    }
}
```

* Time: 3 ms
* Space: 43.91 MB

성능부터가 엄청 개선됐다.

***

## 돌아보기

이분 탐색에 대해 공부해야겠다. 그리고 오늘 정기 모임에서 클럽장님들의 풀이 문제 수를 들었는데 적어도 500문제는 풀어야 할 것 같다. 하루에 5문제씩 100일 정도하면 코딩 테스트를 통과할 수 있지 않을까 싶다.

### 다음에는

* 완전 탐색이 나오면 이분 탐색으로 풀어보기

***

\#99클럽 #코딩테스트준비 #개발자취업 #항해99 #TIL
