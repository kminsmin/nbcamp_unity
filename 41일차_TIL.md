# 내일배움캠프 41일차 TIL  
오전에 코드카타 후, 유니티 심화주차 강의를 수강하고 개인과제로 무엇을 만들지 고민하는 시간을 가졌다.  

## 코드카타 - 기억에 남는 문제    
# [level 1] 숫자 짝꿍 - 131128 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/131128) 

### 성능 요약

메모리: 31.4 MB, 시간: 1.44 ms

### 구분

코딩테스트 연습 > 연습문제

### 채점결과

정확성: 100.0<br/>합계: 100.0 / 100.0

### 제출 일자

2023년 10월 4일 10:44:41

### 문제 설명

<p>두 정수 <code>X</code>, <code>Y</code>의 임의의 자리에서 공통으로 나타나는 정수 k(0 ≤ k ≤ 9)들을 이용하여 만들 수 있는 가장 큰 정수를 두 수의 짝꿍이라 합니다(단, 공통으로 나타나는 정수 중 서로 짝지을 수 있는 숫자만 사용합니다). <code>X</code>, <code>Y</code>의 짝꿍이 존재하지 않으면, 짝꿍은 -1입니다. <code>X</code>, <code>Y</code>의 짝꿍이 0으로만 구성되어 있다면, 짝꿍은 0입니다.</p>

<p>예를 들어, <code>X</code> = 3403이고 <code>Y</code> = 13203이라면, <code>X</code>와 <code>Y</code>의 짝꿍은 <code>X</code>와 <code>Y</code>에서 공통으로 나타나는 3, 0, 3으로 만들 수 있는 가장 큰 정수인 330입니다. 다른 예시로 <code>X</code> = 5525이고 <code>Y</code> = 1255이면 <code>X</code>와 <code>Y</code>의 짝꿍은 <code>X</code>와 <code>Y</code>에서 공통으로 나타나는 2, 5, 5로 만들 수 있는 가장 큰 정수인 552입니다(<code>X</code>에는 5가 3개, <code>Y</code>에는 5가 2개 나타나므로 남는 5 한 개는 짝 지을 수 없습니다.)<br>
두 정수 <code>X</code>, <code>Y</code>가 주어졌을 때, <code>X</code>, <code>Y</code>의 짝꿍을 return하는 solution 함수를 완성해주세요.</p>

<h5>제한사항</h5>

<ul>
<li>3 ≤ <code>X</code>, <code>Y</code>의 길이(자릿수) ≤ 3,000,000입니다.</li>
<li><code>X</code>, <code>Y</code>는 0으로 시작하지 않습니다.</li>
<li><code>X</code>, <code>Y</code>의 짝꿍은 상당히 큰 정수일 수 있으므로, 문자열로 반환합니다.</li>
</ul>

<hr>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>X</th>
<th>Y</th>
<th>result</th>
</tr>
</thead>
        <tbody><tr>
<td>"100"</td>
<td>"2345"</td>
<td>"-1"</td>
</tr>
<tr>
<td>"100"</td>
<td>"203045"</td>
<td>"0"</td>
</tr>
<tr>
<td>"100"</td>
<td>"123450"</td>
<td>"10"</td>
</tr>
<tr>
<td>"12321"</td>
<td>"42531"</td>
<td>"321"</td>
</tr>
<tr>
<td>"5525"</td>
<td>"1255"</td>
<td>"552"</td>
</tr>
</tbody>
      </table>
<hr>

<h5>입출력 예 설명</h5>

<p><strong>입출력 예 #1</strong></p>

<ul>
<li><code>X</code>, <code>Y</code>의 짝꿍은 존재하지 않습니다. 따라서 "-1"을 return합니다.</li>
</ul>

<p><strong>입출력 예 #2</strong></p>

<ul>
<li><code>X</code>, <code>Y</code>의 공통된 숫자는 0으로만 구성되어 있기 때문에, 두 수의 짝꿍은 정수 0입니다. 따라서 "0"을 return합니다.</li>
</ul>

<p><strong>입출력 예 #3</strong></p>

<ul>
<li><code>X</code>, <code>Y</code>의 짝꿍은 10이므로, "10"을 return합니다.</li>
</ul>

<p><strong>입출력 예 #4</strong></p>

<ul>
<li><code>X</code>, <code>Y</code>의 짝꿍은 321입니다. 따라서 "321"을 return합니다.</li>
</ul>

<p><strong>입출력 예 #5</strong></p>

<ul>
<li>지문에 설명된 예시와 같습니다.</li>
</ul>


> 출처: 프로그래머스 코딩 테스트 연습, https://school.programmers.co.kr/learn/challenges

처음에는 foreach문으로 X 배열의 요소를 각각 검사하여 Y 리스트에 동일한 요소가 있는지 Contains 메서드로 검사하고, 있다면 해당 요소를 Y 리스트에서 Remove하고 짝꿍 리스트에 Add하는 방식으로 구현했다. 하지만 이런 방식으로는 시간 복잡도가 O(n^2)이 되어서, 일부 테스트 케이스에서 실행 시간이 너무 길게 나오는 현상이 발생했다. 그래서 각 배열을 한번씩만 순회하고, 각 숫자의 총 개수를 담은 배열을 만들어서 해당 배열을 비교하여 짝꿍 리스트에 요소를 추가하는 방식으로 구현했다.  
```cs
using System;
using System.Collections.Generic;
using System.Linq;

public class Solution {
    public string solution(string X, string Y) {
        string answer = "";
        char[] xDigits = X.ToCharArray();
        int[] xIntDigits = new int[10];
        char[] yDigits = Y.ToCharArray();
        int[] yIntDigits = new int[10];
        List<int> coupleDigits = new List<int>();
        
        //효율적으로 바꾸기
        for(int i = 0; i < xDigits.Length; i++)
        {
            xIntDigits[int.Parse(xDigits[i].ToString())]++;
        }
        for(int i = 0; i < yDigits.Length; i++)
        {
            yIntDigits[int.Parse(yDigits[i].ToString())]++;
        }
        
        for(int i = 0; i < 10; i++)
        {
            int digitCount = Math.Min(xIntDigits[i],yIntDigits[i]);
            for(int j = 0; j < digitCount; j++)
            {
                coupleDigits.Add(i);
            }
        }
        
        coupleDigits.Sort();
        coupleDigits.Reverse();
        if(coupleDigits.Count == 0)
        {
            answer = "-1";
        }
        else if(coupleDigits[0].ToString() == "0")
        {
            answer = "0";
        }
        else
        {
            answer = String.Join("", coupleDigits.ToArray());
        }
        return answer;
    }
}
```
