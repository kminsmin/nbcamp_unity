# 내일배움캠프 47일차 TIL  
오전에 코드카타 후,  팀 프로젝트 개발을 진행했다.   

## 코드카타 - 기억에 남는 문제    
# [level 1] 대충 만든 자판 - 160586 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/160586) 

### 성능 요약

메모리: 31.5 MB, 시간: 4.26 ms

### 구분

코딩테스트 연습 > 연습문제

### 채점결과

정확성: 100.0<br/>합계: 100.0 / 100.0

### 제출 일자

2023년 10월 1일 0:51:54

### 문제 설명

<p>휴대폰의 자판은 컴퓨터 키보드 자판과는 다르게 하나의 키에 여러 개의 문자가 할당될 수 있습니다. 키 하나에 여러 문자가 할당된 경우, 동일한 키를 연속해서 빠르게 누르면 할당된 순서대로 문자가 바뀝니다. </p>

<p>예를 들어, 1번 키에 "A", "B", "C" 순서대로 문자가 할당되어 있다면 1번 키를 한 번 누르면 "A", 두 번 누르면 "B", 세 번 누르면 "C"가 되는 식입니다. </p>

<p>같은 규칙을 적용해 아무렇게나 만든 휴대폰 자판이 있습니다. 이 휴대폰 자판은 키의 개수가 1개부터 최대 100개까지 있을 수 있으며, 특정 키를 눌렀을 때 입력되는 문자들도 무작위로 배열되어 있습니다. 또, 같은 문자가 자판 전체에 여러 번 할당된 경우도 있고, 키 하나에 같은 문자가 여러 번 할당된 경우도 있습니다. 심지어 아예 할당되지 않은 경우도 있습니다. 따라서 몇몇 문자열은 작성할 수 없을 수도 있습니다. </p>

<p>이 휴대폰 자판을 이용해 특정 문자열을 작성할 때, 키를 최소 몇 번 눌러야 그 문자열을 작성할 수 있는지 알아보고자 합니다. </p>

<p>1번 키부터 차례대로 할당된 문자들이 순서대로 담긴 문자열배열 <code>keymap</code>과 입력하려는 문자열들이 담긴 문자열 배열 <code>targets</code>가 주어질 때, 각 문자열을 작성하기 위해 키를 최소 몇 번씩 눌러야 하는지 순서대로 배열에 담아 return 하는 solution 함수를 완성해 주세요. </p>

<p>단, 목표 문자열을 작성할 수 없을 때는 -1을 저장합니다.</p>

<hr>

<h5>제한사항</h5>

<ul>
<li>1 ≤ <code>keymap</code>의 길이 ≤ 100

<ul>
<li>1 ≤ <code>keymap</code>의 원소의 길이 ≤ 100</li>
<li><code>keymap[i]</code>는 i + 1번 키를 눌렀을 때 순서대로 바뀌는 문자를 의미합니다.

<ul>
<li>예를 들어 <code>keymap[0]</code> = "ABACD" 인 경우 1번 키를 한 번 누르면 A, 두 번 누르면 B, 세 번 누르면 A 가 됩니다.</li>
</ul></li>
<li><code>keymap</code>의 원소의 길이는 서로 다를 수 있습니다.</li>
<li><code>keymap</code>의 원소는 알파벳 대문자로만 이루어져 있습니다.</li>
</ul></li>
<li>1 ≤ <code>targets</code>의 길이 ≤ 100

<ul>
<li>1 ≤ <code>targets</code>의 원소의 길이 ≤ 100</li>
<li><code>targets</code>의 원소는 알파벳 대문자로만 이루어져 있습니다.</li>
</ul></li>
</ul>

<hr>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>keymap</th>
<th>targets</th>
<th>result</th>
</tr>
</thead>
        <tbody><tr>
<td>["ABACD", "BCEFD"]</td>
<td>["ABCD","AABB"]</td>
<td>[9, 4]</td>
</tr>
<tr>
<td>["AA"]</td>
<td>["B"]</td>
<td>[-1]</td>
</tr>
<tr>
<td>["AGZ", "BSSS"]</td>
<td>["ASA","BGZ"]</td>
<td>[4, 6]</td>
</tr>
</tbody>
      </table>
<hr>

<h5>입출력 예 설명</h5>

<p>입출력 예 #1 </p>

<ul>
<li>"ABCD"의 경우, </li>
<li>1번 키 한 번 → A </li>
<li>2번 키 한 번 → B </li>
<li>2번 키 두 번 → C </li>
<li>1번 키 다섯 번 → D </li>
<li>따라서 총합인 9를 첫 번째 인덱스에 저장합니다. </li>
<li>"AABB"의 경우, </li>
<li>1번 키 한 번 → A </li>
<li>1번 키 한 번 → A </li>
<li>2번 키 한 번 → B </li>
<li>2번 키 한 번 → B </li>
<li>따라서 총합인 4를 두 번째 인덱스에 저장합니다. </li>
<li>결과적으로 [9,4]를 return 합니다. </li>
</ul>

<p>입출력 예 #2 </p>

<ul>
<li>"B"의 경우, 'B'가 어디에도 존재하지 않기 때문에 -1을 첫 번째 인덱스에 저장합니다. </li>
<li>결과적으로 [-1]을 return 합니다. </li>
</ul>

<p>입출력 예 #3 </p>

<ul>
<li>"ASA"의 경우, </li>
<li>1번 키 한 번 → A </li>
<li>2번 키 두 번 → S </li>
<li>1번 키 한 번 → A </li>
<li>따라서 총합인 4를 첫 번째 인덱스에 저장합니다. </li>
<li>"BGZ"의 경우, </li>
<li>2번 키 한 번 → B </li>
<li>1번 키 두 번 → G </li>
<li>1번 키 세 번 → Z </li>
<li>따라서 총합인 6을 두 번째 인덱스에 저장합니다. </li>
<li>결과적으로 [4, 6]을 return 합니다.</li>
</ul>


> 출처: 프로그래머스 코딩 테스트 연습, https://school.programmers.co.kr/learn/challenges


  타깃 배열 내의 문자열들을 차례대로 검사한다. 먼저 타깃 문자열을 ToCharArray 메서드를 통해 문자의 배열로 변환한다. 그리고 각 문자들을 차례대로 검사한다. 키맵 문자열에서 각 문자와 일치하는 가장 빠른 인덱스를 최솟값 알고리즘으로 비교하여 키를 누르는 최솟값을 구한다. count라는 변수에 최솟값을 더하고 다음 문자의 인덱스 최솟값을 구한다. 이때 IndexOf 메서드를 이용해서 구하기 때문에 일치하는 문자가 없으면 -1을 반환하여, 이런 경우는 즉시 현재 타깃 문자열 검사를 중단하고 다음 타깃 문자열을 검사한다. 타깃 문자열 검사를 끝낼 때마다 같은 인덱스의 answer 배열 요소에 현재 count값을 저장하고, 다음 타깃 문자열을 검사하는 방식으로 구현했다.  
  ```cs
using System;

public class Solution {
    public int[] solution(string[] keymap, string[] targets) {
        int[] answer = new int[targets.Length];
        int minPress = 100;
        int count = 0;
        int targetIndex = 0;
        foreach(string word in targets)
        {
            char[] letters = word.ToCharArray();
            count = 0;
            foreach(char letter in letters)
            {
                minPress = 100;
                for(int i = 0; i < keymap.Length; i++)
                {
                    int index = keymap[i].IndexOf(letter);
                    if(index != -1 && index+1 < minPress)
                    {
                        minPress = index + 1;
                    }
                }
                 if(minPress == 100)
                {
                    count = -1;
                    break;
                }
                else
                {
                    count += minPress;
                }
            }
            answer[targetIndex] = count;
            targetIndex++;
        }
        return answer;
    }
}
```

## 팀 프로젝트 개발 - 포톤
이번 팀 프로젝트를 개발하며 멀티플레이를 구현하기 위해 처음으로 포톤을 사용했다. 포톤은 멀티플레이 게임용 유니티 패키지인데, 포톤이 제공해주는 서버와 API들을 통해 네트워크에 대해 자세히 알지 못해도 쉽게 멀티플레이 게임을 개발할 수 있다. 이번 프로젝트에는 PUN2를 사용했다.  
### 문제의 시작  
팀원들 모두 포톤을 사용하는 것이 처음이다 보니, 어떤 식으로 개발을 해야 하는지 몰랐다. 그래서 와이어프레임을 작성하고 업무를 분담하고 각자 파트를 개발할 때, 일단 로컬에서의 작동만 고려하며 개발하고, 나중에 포톤을 연결했을 때의 경우는 고려하지 않아서 대참사가 발생했다. 나의 경우는 컷씬 제작과 레벨 디자인을 맡았었다. 레밸 내에 구현된 기능들로는 컷씬 재생 후 플레이어 오브젝트 생성, 플레이어들의 부활, 점프시 플레이어가 윗 발판에 머리를 부딛히지 않게 하는 기능, 체크포인트 업데이트, 그리고 그 외 움직이는 장애물들이나 기믹 들이 있었다. 플레이어의 부활의 경우 플레이어 사망을 이벤트로 처리하여, 플레이어 오브젝트를 비활성화했다가 플레이어 오브젝트의 위치를 체크포인트의 위치로 이동하고 다시 재활성화하는 메서드를 구독해둠으로써 구현했었다. 점프시 플레이어가 윗 발판에 머리를 부딛히지 않게 하는 기능은, 플레이어의 RigidBody의 Velocity의 y값을 읽어서, 해당 값이 양수이면 점프를 하고 있다는 의미이므로 지형의 콜라이더를 비활성화하고, 음수이면 낙하중이라는 뜻이므로 지형의 콜라이더를 활성화하고, 이 과정을 코루틴으로 구현했었다. 이 두 기능들 모두 로컬에서 혼자 테스트할 때는 아무 문제가 없었다. 하지만 포톤을 연결하고 온라인으로 테스트를 해보니 전혀 작동하지 않았다. 조사 결과 동기화의 문제였다.   
### 동기화  
Transform, Animator, Rigidbody의 경우 해당 PhotonView 컴포넌트를 추가해주면 손쉽게 동기화를 구현할 수 있다. 하지만 함수나 이벤트의 경우, 이렇게 간단한 방식으로는 동기화를 구현할 수 없었다. 함수나 메서드의 경우, Remote Procedure Calls(RPCs)를 통해 동기화와 비슷한 효과를 낼 수 있다. RPC를 사용하면 해당 메서드를 클라이언트에서 실행하는 것이 아니라 서버에서 호출한다. 포톤의 경우 ```photonView.RPC()```메서드를 통해 호출하는데, 타깃을 정해서 어떤 클라이언트들을 타깃으로 호출할지 정할 수 있다.  
```cs
//메서드를 원격호출하려면 [PunRPC] 속성을 적용해야 한다.
[PunRPC]
void ChatMessage(string a, string b)
{
    Debug.Log("ChatMessage " + a + " " + b);
}

//호출하는 방식은 다음과 같다.
PhotonView photonView = PhotonView.Get(this);
photonView.RPC("ChatMessage", PhotonTargets.All, "jup", "and jup!");
```
이벤트를 동기화하고 싶을 경우, ```PhotonNetwork.RaiseEvent```를 통해 네트워크 객체에 관계 없이 이벤트들을 전송할 수 있다. 이렇게 동기화에 대해 공부하며, 작동하지 않는 기능들을 고쳐나갔다. 이렇게 동기화를 구현하며 새삼 느낀 것은, 개발할 때 미리 서버에서 실행되어야 할 메서드와 클라이언트에서만 실행해야 할 메서드를 미리 구분해두며 개발하면 편할 것 같다는 생각이 들었다. 다음에 포톤을 사용한 프로젝트를 개발할 때는 꼭 그래야겠다...
