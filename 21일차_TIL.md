# 내일배움캠프 21일차 TIL
오늘 오전에는 코드카타 후 정렬과 탐색 알고리즘을 공부했다. 오후에는 개인과제 해설이 있었고 계속 알고리즘 공부를 했다.    

## 코드카타 - 기억에 남는 문제  
# [level 1] 삼총사 - 131705 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/131705) 

### 성능 요약

메모리: 31.5 MB, 시간: 0.22 ms

### 구분

코딩테스트 연습 > 연습문제

### 채점결과

Empty

### 문제 설명

<p>한국중학교에 다니는 학생들은 각자 정수 번호를 갖고 있습니다. 이 학교 학생 3명의 정수 번호를 더했을 때 0이 되면 3명의 학생은 삼총사라고 합니다. 예를 들어, 5명의 학생이 있고, 각각의 정수 번호가 순서대로 -2, 3, 0, 2, -5일 때, 첫 번째, 세 번째, 네 번째 학생의 정수 번호를 더하면 0이므로 세 학생은 삼총사입니다. 또한, 두 번째, 네 번째, 다섯 번째 학생의 정수 번호를 더해도 0이므로 세 학생도 삼총사입니다. 따라서 이 경우 한국중학교에서는 두 가지 방법으로 삼총사를 만들 수 있습니다.</p>

<p>한국중학교 학생들의 번호를 나타내는 정수 배열 <code>number</code>가 매개변수로 주어질 때, 학생들 중 삼총사를 만들 수 있는 방법의 수를 return 하도록 solution 함수를 완성하세요.</p>

<hr>

<h5>제한사항</h5>

<ul>
<li>3 ≤ <code>number</code>의 길이 ≤ 13</li>
<li>-1,000 ≤ <code>number</code>의 각 원소 ≤ 1,000</li>
<li>서로 다른 학생의 정수 번호가 같을 수 있습니다.</li>
</ul>

<hr>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>number</th>
<th>result</th>
</tr>
</thead>
        <tbody><tr>
<td>[-2, 3, 0, 2, -5]</td>
<td>2</td>
</tr>
<tr>
<td>[-3, -2, -1, 0, 1, 2, 3]</td>
<td>5</td>
</tr>
<tr>
<td>[-1, 1, -1, 1]</td>
<td>0</td>
</tr>
</tbody>
      </table>
<hr>

<h5>입출력 예 설명</h5>

<p><strong>입출력 예 #1</strong></p>

<ul>
<li>문제 예시와 같습니다.</li>
</ul>

<p><strong>입출력 예 #2</strong></p>

<ul>
<li>학생들의 정수 번호 쌍 (-3, 0, 3), (-2, 0, 2), (-1, 0, 1), (-2, -1, 3), (-3, 1, 2) 이 삼총사가 될 수 있으므로, 5를 return 합니다.</li>
</ul>

<p><strong>입출력 예 #3</strong></p>

<ul>
<li>삼총사가 될 수 있는 방법이 없습니다.</li>
</ul>


> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

알고리즘 공부가 부족했던 터라, for문을 3중으로 사용해서 숫자 3개의 모든 조합을 만들고, 숫자들의 합이 0일 때마다 answer를 하나씩 더하는 방식으로 구현했다. 추후 오늘 아침에 알고리즘 공부를 해보니 이렇게 짠 코드는 O(n^3)의 시간 복잡성을 가진다는 것을 알게 되었다...  
```cs
using System;

public class Solution {
    public int solution(int[] number) {
        int answer = 0;
        int checkTrio = 0;
        int numLength = number.Length;
        
        for(int i = 0; i <= numLength - 3; i++)
        {
            for(int j = i+1; j <= numLength - 2; j++)
            {
                for(int k = j+1; k <= numLength -1 ; k++)
                {
                    checkTrio += (number[i] + number[j] + number [k]);
                    answer += (checkTrio == 0) ? 1 : 0;
                    checkTrio = 0;
                }
                
            }
        }
        

        return answer;
    }
}
```
문제를 풀고 다른 사람들의 풀이를 보니 DFS 알고리즘을 활용해서 푼 사례도 있었다. 상황에 따라 어떤 알고리즘을 사용해야 할지, 어떤 알고리즘들이 있는지 많은 공부가 필요하겠다는 생각이 들었다.  
