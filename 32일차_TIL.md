# 내일배움캠프 32일차 TIL  
오늘도 코드카타를 진행하고 개인과제 개발에 몰두했다. 과제 제출 직전에 필수 구현 기능들을 완성했고, 제출 후 선택 구현 기능을 개발하는 데 집중했다.  

## 코드카타 - 기억에 남는 문제  
# [unrated] 카드 뭉치 - 159994 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/159994) 

### 성능 요약

메모리: 31.2 MB, 시간: 0.28 ms

### 구분

코딩테스트 연습 > 연습문제

### 채점결과

정확성: 100.0<br/>합계: 100.0 / 100.0

### 문제 설명

<p>코니는 영어 단어가 적힌 카드 뭉치 두 개를 선물로 받았습니다. 코니는 다음과 같은 규칙으로 카드에 적힌 단어들을 사용해 원하는 순서의 단어 배열을 만들 수 있는지 알고 싶습니다.</p>

<ul>
<li>원하는 카드 뭉치에서 카드를 순서대로 한 장씩 사용합니다.</li>
<li>한 번 사용한 카드는 다시 사용할 수 없습니다.</li>
<li>카드를 사용하지 않고 다음 카드로 넘어갈 수 없습니다.</li>
<li>기존에 주어진 카드 뭉치의 단어 순서는 바꿀 수 없습니다.</li>
</ul>

<p>예를 들어 첫 번째 카드 뭉치에 순서대로 ["i", "drink", "water"], 두 번째 카드 뭉치에 순서대로 ["want", "to"]가 적혀있을 때 ["i", "want", "to", "drink", "water"] 순서의 단어 배열을 만들려고 한다면 첫 번째 카드 뭉치에서 "i"를 사용한 후 두 번째 카드 뭉치에서 "want"와 "to"를 사용하고 첫 번째 카드뭉치에 "drink"와 "water"를 차례대로 사용하면 원하는 순서의 단어 배열을 만들 수 있습니다.</p>

<p>문자열로 이루어진 배열 <code>cards1</code>, <code>cards2</code>와 원하는 단어 배열&nbsp;<code>goal</code>이 매개변수로 주어질 때, <code>cards1</code>과 <code>cards2</code>에 적힌 단어들로 <code>goal</code>를 만들 있다면 "Yes"를, 만들 수 없다면 "No"를 return하는 solution 함수를 완성해주세요.</p>

<hr>

<h5>제한사항</h5>

<ul>
<li>1 ≤ <code>cards1</code>의 길이, <code>cards2</code>의 길이 ≤ 10

<ul>
<li>1 ≤ <code>cards1[i]</code>의 길이, <code>cards2[i]</code>의 길이 ≤ 10</li>
<li><code>cards1</code>과 <code>cards2</code>에는 서로 다른 단어만 존재합니다.</li>
</ul></li>
<li>2 ≤ <code>goal</code>의 길이 ≤ <code>cards1</code>의 길이 + <code>cards2</code>의 길이

<ul>
<li>1 ≤ <code>goal[i]</code>의 길이 ≤ 10</li>
<li><code>goal</code>의 원소는 <code>cards1</code>과 <code>cards2</code>의 원소들로만 이루어져 있습니다.</li>
</ul></li>
<li><code>cards1</code>, <code>cards2</code>, <code>goal</code>의 문자열들은 모두 알파벳 소문자로만 이루어져 있습니다.</li>
</ul>

<hr>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>cards1</th>
<th>cards2</th>
<th>goal</th>
<th>result</th>
</tr>
</thead>
        <tbody><tr>
<td>["i", "drink", "water"]</td>
<td>["want", "to"]</td>
<td>["i", "want", "to", "drink", "water"]</td>
<td>"Yes"</td>
</tr>
<tr>
<td>["i", "water", "drink"]</td>
<td>["want", "to"]</td>
<td>["i", "want", "to", "drink", "water"]</td>
<td>"No"</td>
</tr>
</tbody>
      </table>
<hr>

<h5>입출력 예 설명</h5>

<p>입출력 예 #1</p>

<p>본문과 같습니다.</p>

<p>입출력 예 #2</p>

<p><code>cards1</code>에서 "i"를 사용하고 <code>cards2</code>에서 "want"와 "to"를 사용하여 "i want to"까지는 만들 수 있지만 "water"가 "drink"보다 먼저 사용되어야 하기 때문에 해당 문장을 완성시킬 수 없습니다. 따라서 "No"를 반환합니다.</p>


> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

문제를 제대로 읽지 않아 한동안 헤맸다. 앞 순서의 카드를 사용하지 않으면 뒷 카드에 접근하지 못한다는 사실을 간과하고 순서와 상관 없는 탐색 알고리즘으로 구현했었다. 중요한 조건을 놓쳤음을 깨닫고 처음부터 코드를 갈아엎었다. 각각의 카드 뭉치에서 맨 앞 카드가 목표 배열의 검사중인 문자열과 일치하는지 검사하고, 일치한다면 각각의 인덱스를 증가시킨다. 이러한 검사 시도 횟수가 일정 횟수 이상으로 넘어갈 경우, 실패로 판단하는 방식으로 구현했다. 또한 인덱스가 배열의 크기를 초과하는 경우는 목표 배열을 끝까지 검사한 경우이므로 예외처리하여 참을 반환하도록 했다.  
```cs
using System;

public class Solution {
    public string solution(string[] cards1, string[] cards2, string[] goal) {
        string answer = "";
        int goalIndex = 0;
        int firstIndex = 0;
        int secondIndex = 0;
        int tryNum = 0;
        string[] trial = new string[goal.Length];

        while(goalIndex < goal.Length)
        {
            try
            {
                for(int i = firstIndex; i < cards1.Length; i++)
                {
                    if(cards1[firstIndex]==goal[goalIndex])
                    {
                        trial[goalIndex] = cards1[firstIndex];
                        goalIndex++;
                        firstIndex++;
                    }
                }
                for(int i = secondIndex; i < cards2.Length; i++)
                {
                    if(cards2[secondIndex]==goal[goalIndex])
                    {
                        trial[goalIndex] = cards2[secondIndex];
                        goalIndex++;
                        secondIndex++;
                    }
                }

                if(tryNum > 5)
                {
                    return "No";
                }
                tryNum++;
            }
            catch(Exception e)
            {
                return "Yes";
            }
            
        }
        answer = "Yes";
        return answer;
    }
}
```
지금 돌이켜 생각해보니 인덱스를 일일이 증가시키는 방법 대신 배열의 요소들을 Queue에 집어넣어 하나씩 Dequeue하며 검사하는 방식으로 구현했다면 코드가 더 간결해졌을 것 같다. 그렇게 되면 인덱스 초과 예외도 발생하지 않아서 예외처리를 할 필요도 없었을텐데...  

## 개인과제 : 스파르타 던전 - 개인과제 개발 회고  
유니티를 사용하면 할수록 와이어프레임이나 다이어그램을 작성하는 실력이 확실히 늘어나는 것 같다. 하지만 아직도 구조를 효율적이고 구체적으로 짜는 것은 여전히 어렵게 느껴진다. 이번 과제와 같은 경우에도, 인벤토리에서 아이템 장칙 여부를 변경하는 팝업 창에 아이템 관련 정보를 출력해야 했는데, 개발을 시작하고 나서도 아이템의 정보를 클래스로 저장할 지, 스크립터블 오브젝트로 저장할 지, 또 만들어진 아이템 객체들은 어디에 저장할지 조차 정해두지 않은 상태여서 어려움을 많이 겪었다. 이번에는 아직 낯설게 느껴지는 스크립터블 오브젝트를 적극 사용하느라 개발 속도가 이전 과제에 비해 느렸던 것 같다. 선택과제를 끝까지 해내지 못해서 아쉬울 따름이다. 이제는 스크립터블 오브젝트에 조금은 더 익숙해졌으므로, 다음 과제때는 이번보다 더 효율적이고 구체적으로 구조를 미리 짜 두고 개발을 시작할 수 있을 것 같다.
