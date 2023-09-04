# 내일배움캠프 18일차 TIL
오전에는 유니티 입문 주차 발제와 코드카타 시간이 있었다. 이후 유니티 입문 강의를 수강하고 해당 주차 내용을 복습하는 시간을 가졌다.  
기억에 남는 문제는 문자열 내림차순으로 배치하기 다.

# [level 1] 문자열 내림차순으로 배치하기 - 12917 
### 문제 설명

<p>문자열 s에 나타나는 문자를 큰것부터 작은 순으로 정렬해 새로운 문자열을 리턴하는 함수, solution을 완성해주세요.<br>
s는 영문 대소문자로만 구성되어 있으며, 대문자는 소문자보다 작은 것으로 간주합니다.</p>

<h5>제한 사항</h5>

<ul>
<li>str은 길이 1 이상인 문자열입니다.</li>
</ul>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>s</th>
<th>return</th>
</tr>
</thead>
        <tbody><tr>
<td>"Zbcdefg"</td>
<td>"gfedcbZ"</td>
</tr>
</tbody>
      </table>

> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

알파벳을 정렬하는 것 정도는 문자열을 ToCharArray()메서드를 통해 배열로 변환한 후 OrderByDescending 메서드를 이용해서 역순으로 재배치하는 방식으로 쉽게 구현할 수 있다고 생각했다. 일단 이렇게 구현하고 대문자를 어떻게 구분하고 배치할지 고민해보자는 생각으로 일단
코드를 작성하고 실행해보았으나 허무하게 그대로 문제가 풀렸다. 알고 보니 Char 자료형에서 대문자와 소문자를 비교하면 대문자가 더 크기 때문에, OrderByDescending 메서드로 문제의 요구사항대로 대문자가 자연스럽게 뒤쪽으로 배치된 것이었다. 이렇게 하나 알아간다...   
```cs
using System.Linq;

public class Solution {
    public string solution(string s) {
        string answer = "";
        char[] arr = s.ToCharArray();
        arr = arr.OrderByDescending(x => x).ToArray();
        foreach (char letter in arr)
        {
            answer += letter.ToString();
        }
        return answer;
    }
}
```
