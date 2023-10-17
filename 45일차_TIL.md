# 내일배움캠프 45일차 TIL  
오전에 코드카타 후,  팀 프로젝트 개발을 진행했다.  

## 코드카타 - 기억에 남는 문제    
# [level 1] 문자열 나누기 - 140108 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/140108) 

### 성능 요약

메모리: 31.5 MB, 시간: 2.48 ms

### 구분

코딩테스트 연습 > 연습문제

### 채점결과

정확성: 100.0<br/>합계: 100.0 / 100.0

### 제출 일자

2023년 10월 2일 10:16:24

### 문제 설명

<p>문자열 <code>s</code>가 입력되었을 때 다음 규칙을 따라서 이 문자열을 여러 문자열로 분해하려고 합니다.</p>

<ul>
<li>먼저 첫 글자를 읽습니다. 이 글자를 x라고 합시다.</li>
<li>이제 이 문자열을 왼쪽에서 오른쪽으로 읽어나가면서, x와 x가 아닌 다른 글자들이 나온 횟수를 각각 셉니다. 처음으로 두 횟수가 같아지는 순간 멈추고, 지금까지 읽은 문자열을 분리합니다.</li>
<li><code>s</code>에서 분리한 문자열을 빼고 남은 부분에 대해서 이 과정을 반복합니다. 남은 부분이 없다면 종료합니다.</li>
<li>만약 두 횟수가 다른 상태에서 더 이상 읽을 글자가 없다면, 역시 지금까지 읽은 문자열을 분리하고, 종료합니다.</li>
</ul>

<p>문자열 <code>s</code>가 매개변수로 주어질 때, 위 과정과 같이 문자열들로 분해하고, 분해한 문자열의 개수를 return 하는 함수 solution을 완성하세요.</p>

<hr>

<h5>제한사항</h5>

<ul>
<li>1 ≤ <code>s</code>의 길이 ≤ 10,000</li>
<li><code>s</code>는 영어 소문자로만 이루어져 있습니다.</li>
</ul>

<hr>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>s</th>
<th>result</th>
</tr>
</thead>
        <tbody><tr>
<td>"banana"</td>
<td>3</td>
</tr>
<tr>
<td>"abracadabra"</td>
<td>6</td>
</tr>
<tr>
<td>"aaabbaccccabba"</td>
<td>3</td>
</tr>
</tbody>
      </table>
<hr>

<h5>입출력 예 설명</h5>

<p>입출력 예 #1<br>
<code>s</code>="banana"인 경우 ba - na - na와 같이 분해됩니다.</p>

<p>입출력 예 #2<br>
<code>s</code>="abracadabra"인 경우 ab - ra - ca - da - br - a와 같이 분해됩니다.</p>

<p>입출력 예 #3<br>
<code>s</code>="aaabbaccccabba"인 경우 aaabbacc - ccab - ba와 같이 분해됩니다.</p>


> 출처: 프로그래머스 코딩 테스트 연습, https://school.programmers.co.kr/learn/challenges

먼저 받은 문자열의 각 글자를 담은 리스트를 만들었다. 그 후 한글자씩 검사하며 개수를 세고, 검사한 글자는 리스트에서 제거하며 검사를 계속했고, x의 개수와 그 이외 글자들의 개수가 일치하는 순간 검사를 멈추고, answer에 1을 더하고, 다음 x를 설정하고 검사를 이어간다. 이러한 반복을 리스트에 요소가 없을 때까지 반복한다. 단, 반복 종료 시점에서 x의 개수와 그 외 글자의 개수가 일치한다면 answer를 그대로 반환하고, 그렇지 않다면 answer에 1을 더하여 반환한다.  
```cs
using System;
using System.Collections.Generic;
using System.Linq;

public class Solution {
    public int solution(string s) {
        int answer = 0;
        List<char> letters = s.ToCharArray().ToList();
        char x;
        int xCount = 0;
        int elseCount = 0;
        int lettersNum = 0;
        
        while(true)
        {
            xCount = 1;
            elseCount = 0;
            x = letters[0];
            letters.RemoveAt(0);
            lettersNum = letters.Count;
            for(int i = 0; i < lettersNum; i++)
            {
                if(letters[0] == x)
                {
                    xCount++;
                }
                else
                {
                    elseCount++;
                }
                letters.RemoveAt(0);

                if(xCount == elseCount && xCount !=0)
                {
                    answer++;
                    break;
                }
            }    
            
            if(letters.Count == 0 && xCount == elseCount)
                break;
            else if(letters.Count == 0 && xCount != elseCount)
            {
                answer++;
                break;
            }
        }
        return answer;
    }
}
```
