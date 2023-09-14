# 내일배움캠프 26일차 TIL  
오전에는 코드카타 후, 문제를 풀면서 배운 새로운 개념들을 공부했다. 오후에는 팀 프로젝트 발표가 있었고, 발표 이후에는 C# 문법을 복습했다.  

## 코드카타 - 기억에 남는 문제  
# [level 1] 두 개 뽑아서 더하기 - 68644 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/68644) 

### 성능 요약

메모리: 31 MB, 시간: 2.51 ms

### 구분

코딩테스트 연습 > 월간 코드 챌린지 시즌1

### 채점결과

Empty

### 문제 설명

<p>정수 배열 numbers가 주어집니다. numbers에서 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해서 만들 수 있는 모든 수를 배열에 오름차순으로 담아 return 하도록 solution 함수를 완성해주세요.</p>

<hr>

<h5>제한사항</h5>

<ul>
<li>numbers의 길이는 2 이상 100 이하입니다.

<ul>
<li>numbers의 모든 수는 0 이상 100 이하입니다.</li>
</ul></li>
</ul>

<hr>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>numbers</th>
<th>result</th>
</tr>
</thead>
        <tbody><tr>
<td><code>[2,1,3,4,1]</code></td>
<td><code>[2,3,4,5,6,7]</code></td>
</tr>
<tr>
<td><code>[5,0,2,7]</code></td>
<td><code>[2,5,7,9,12]</code></td>
</tr>
</tbody>
      </table>
<hr>

<h5>입출력 예 설명</h5>

<p>입출력 예 #1</p>

<ul>
<li>2 = 1 + 1 입니다. (1이 numbers에 두 개 있습니다.)</li>
<li>3 = 2 + 1 입니다.</li>
<li>4 = 1 + 3 입니다.</li>
<li>5 = 1 + 4 = 2 + 3 입니다.</li>
<li>6 = 2 + 4 입니다.</li>
<li>7 = 3 + 4 입니다.</li>
<li>따라서 <code>[2,3,4,5,6,7]</code> 을 return 해야 합니다.</li>
</ul>

<p>입출력 예 #2</p>

<ul>
<li>2 = 0 + 2 입니다.</li>
<li>5 = 5 + 0 입니다.</li>
<li>7 = 0 + 7 = 5 + 2 입니다.</li>
<li>9 = 2 + 7 입니다.</li>
<li>12 = 5 + 7 입니다.</li>
<li>따라서 <code>[2,5,7,9,12]</code> 를 return 해야 합니다.</li>
</ul>


> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

첫 시도에는 이중 포문을 사용해서 가능한 모든 조합의 합을 리스트에 넣고, 이를 정렬했다. 이후 해당 리스트로 반복문을 사용해 반복되는 숫자를 제거하려 했으나 리스트의 요소 수가 반복문 중에 변경되면 에러가 발생할 수 있기 때문에 새로운 리스트를 선언하여 값을 그대로 복사해주고, 반복문을 통해 한쪽 리스트에 반복되는 숫자가 있는지 검사하고, 반복되는 숫자가 있으면 해당 숫자를 다른 리스트에서 찾아
제거하는 방식으로 구현했었다. 테스트 케이스는 통과했으나 제출해보니 형편없는 정확성을 자랑했다. 아무래도 반복되는 숫자를 검사하고 제거하는 과정에서 반복되는 숫자가 3개 이상일 경우 제대로 제거되지 않는 것 같았다. 그래서 애초에 반복되는 숫자를 검사하는 단계를 없애기 위해, 두 숫자 조합의 합들을 담을 컬렉션으로 리스트를 사용하는 것이 아니라, 중복되는 숫자가 있으면 요소를 추가하지 않는
해시셋을 사용해보았고, 문제를 해결할 수 있었다.  
```cs
using System;
using System.Collections.Generic;
using System.Linq;

public class Solution {
    public int[] solution(int[] numbers) {
        int[] answer = new int[] {};
        HashSet<int> answerHash = new HashSet<int>();
        List<int> answerList = new List<int>();

        int sum = 0;
        for(int i = 0; i < numbers.Length-1; i++)
        {
            for(int j = i + 1; j < numbers.Length; j++)
            {
                sum = 0;
                sum = numbers[i] + numbers[j];
                answerHash.Add(sum);
            }
        }
        answerList = answerHash.ToList();
        answerList.Sort();
        answer = answerList.ToArray();
        return answer;
    }
}
```
