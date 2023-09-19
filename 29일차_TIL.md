# 내일배움캠프 29일차 TIL  
오전에는 코드카타를 진행하고 이후 숙련주차 강의를 수강했다. 강의를 전부 수강하고 개인과제를 시작하는 것이 목표였으나 일단 개인과제를 먼저 시작하고 강의 수강을 병행하는 것으로 계획을 수정해야겠다...

## 코드카타 - 기억에 남는 문제  
  # [unrated] 명예의 전당 (1) - 138477 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/138477) 

### 성능 요약

메모리: 31.1 MB, 시간: 2.09 ms

### 구분

코딩테스트 연습 > 연습문제

### 채점결과

Empty

### 문제 설명

<p>"명예의 전당"이라는 TV 프로그램에서는 매일 1명의 가수가 노래를 부르고, 시청자들의 문자 투표수로 가수에게 점수를 부여합니다. 매일 출연한 가수의 점수가 지금까지 출연 가수들의 점수 중 상위 k번째 이내이면 해당 가수의 점수를 명예의 전당이라는 목록에 올려 기념합니다.&nbsp;즉 프로그램 시작 이후 초기에 k일까지는 모든 출연 가수의 점수가 명예의 전당에 오르게 됩니다. k일 다음부터는 출연 가수의 점수가 기존의 명예의 전당 목록의 k번째 순위의 가수 점수보다 더 높으면, 출연 가수의 점수가 명예의 전당에 오르게 되고 기존의 k번째 순위의 점수는 명예의 전당에서 내려오게 됩니다.</p>

<p>이 프로그램에서는 매일 "명예의 전당"의 최하위 점수를 발표합니다. 예를 들어, <code>k</code> = 3이고, 7일 동안 진행된 가수의 점수가 [10, 100, 20, 150, 1, 100, 200]이라면, 명예의 전당에서 발표된 점수는 아래의 그림과 같이 [10, 10, 10, 20, 20, 100, 100]입니다.</p>

<p><img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/b0893853-7471-47c0-b7e5-1e8b46002810/%EA%B7%B8%EB%A6%BC1.png" title="" alt="그림1.png"></p>

<p>명예의 전당 목록의 점수의 개수 <code>k</code>, 1일부터 마지막 날까지 출연한 가수들의 점수인 <code>score</code>가 주어졌을 때, 매일 발표된 명예의 전당의 최하위 점수를 return하는 solution 함수를 완성해주세요.</p>

<h5>제한사항</h5>

<ul>
<li>3 ≤ <code>k</code> ≤ 100</li>
<li>7 ≤ <code>score</code>의 길이 ≤ 1,000

<ul>
<li>0 ≤ <code>score[i]</code> ≤ 2,000</li>
</ul></li>
</ul>

<hr>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>k</th>
<th>score</th>
<th>result</th>
</tr>
</thead>
        <tbody><tr>
<td>3</td>
<td>[10, 100, 20, 150, 1, 100, 200]</td>
<td>[10, 10, 10, 20, 20, 100, 100]</td>
</tr>
<tr>
<td>4</td>
<td>[0, 300, 40, 300, 20, 70, 150, 50, 500, 1000]</td>
<td>[0, 0, 0, 0, 20, 40, 70, 70, 150, 300]</td>
</tr>
</tbody>
      </table>
<hr>

<h5>입출력 예 설명</h5>

<p><strong>입출력 예 #1</strong></p>

<ul>
<li>문제의 예시와 같습니다.</li>
</ul>

<p><strong>입출력 예 #2</strong></p>

<ul>
<li>아래와 같이, [0, 0, 0, 0, 20, 40, 70, 70, 150, 300]을 return합니다.
<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/5175c32d-44d7-4b13-be47-360bbe6a553c/%EA%B7%B8%EB%A6%BC2.png" title="" alt="그림2.png"></li>
</ul>


> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

다른 케이스는 아무 문제 없었으나 중간에 런타임 에러가 발생하는 케이스가 2개 있었다. 추측으로는 아마 k가 score배열의 총 길이보다 클 경우를 고려하지 않아 Index out of range 에러가 발생한 것으로 생각되었는데, 어차피 answer배열의 요소를 업데이트 해주는 알고리즘 자체에는 문제가 없으므로 인덱스가 배열보다 커지는 경우를 예외처리하여 그때의 answer 배열을 리턴하도록 했다.  
```cs
using System;
using System.Collections.Generic;

public class Solution {
    public int[] solution(int k, int[] score) {
        int arrLength = score.Length;
        int[] answer = new int[arrLength];
        List<int> hallOfFame = new List<int>();
        int temp = 0;
        try
        {
            for(int i = 0; i < k; i++)
            {
                hallOfFame.Add(score[i]);
                hallOfFame.Sort();
                answer[i] = hallOfFame[0];
            }
            for(int i = k; i < arrLength; i++)
            {
                temp = score[i];
                if(temp>hallOfFame[0])
                {
                    hallOfFame[0] = temp;
                    hallOfFame.Sort();
                }
                answer[i] = hallOfFame[0];
            }
        return answer;
        }
        catch(Exception e)
        {
            return answer;
        }
        
    }
}
```
