# 내일배움캠프 24일차 TIL  
오전에는 코드카타 후, 오후에 팀 프로젝트 개발 및 저녁에는 각자 개발한 부분을 머지하고 리뷰하는 시간을 가졌다.  

## 코드카타 - 기억에 남는 문제  
# [level 1] 숫자 문자열과 영단어 - 81301 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/81301) 

### 성능 요약

메모리: 31.7 MB, 시간: 1.21 ms

### 구분

코딩테스트 연습 > 2021 카카오 채용연계형 인턴십

### 채점결과

Empty

### 문제 설명

<p><img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d31cb063-4025-4412-8cbc-6ac6909cf93e/img1.png" title="" alt="img1.png"></p>

<p>네오와 프로도가 숫자놀이를 하고 있습니다. 네오가 프로도에게 숫자를 건넬 때 일부 자릿수를 영단어로 바꾼 카드를 건네주면 프로도는 원래 숫자를 찾는 게임입니다.<br><br>
다음은 숫자의 일부 자릿수를 영단어로 바꾸는 예시입니다.</p>

<ul>
<li>1478 → "one4seveneight"</li>
<li>234567 → "23four5six7"</li>
<li>10203 → "1zerotwozero3"</li>
</ul>

<p>이렇게 숫자의 일부 자릿수가 영단어로 바뀌어졌거나, 혹은 바뀌지 않고 그대로인 문자열 <code>s</code>가 매개변수로 주어집니다. <code>s</code>가 의미하는 원래 숫자를 return 하도록 solution 함수를 완성해주세요.</p>

<p>참고로 각 숫자에 대응되는 영단어는 다음 표와 같습니다.</p>
<table class="table">
        <thead><tr>
<th>숫자</th>
<th>영단어</th>
</tr>
</thead>
        <tbody><tr>
<td>0</td>
<td>zero</td>
</tr>
<tr>
<td>1</td>
<td>one</td>
</tr>
<tr>
<td>2</td>
<td>two</td>
</tr>
<tr>
<td>3</td>
<td>three</td>
</tr>
<tr>
<td>4</td>
<td>four</td>
</tr>
<tr>
<td>5</td>
<td>five</td>
</tr>
<tr>
<td>6</td>
<td>six</td>
</tr>
<tr>
<td>7</td>
<td>seven</td>
</tr>
<tr>
<td>8</td>
<td>eight</td>
</tr>
<tr>
<td>9</td>
<td>nine</td>
</tr>
</tbody>
      </table>
<hr>

<h5>제한사항</h5>

<ul>
<li>1 ≤ <code>s</code>의 길이 ≤ 50</li>
<li><code>s</code>가 "zero" 또는 "0"으로 시작하는 경우는 주어지지 않습니다.</li>
<li>return 값이 1 이상 2,000,000,000 이하의 정수가 되는 올바른 입력만 <code>s</code>로 주어집니다.</li>
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
<td><code>"one4seveneight"</code></td>
<td>1478</td>
</tr>
<tr>
<td><code>"23four5six7"</code></td>
<td>234567</td>
</tr>
<tr>
<td><code>"2three45sixseven"</code></td>
<td>234567</td>
</tr>
<tr>
<td><code>"123"</code></td>
<td>123</td>
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
<li>문제 예시와 같습니다.</li>
</ul>

<p><strong>입출력 예 #3</strong></p>

<ul>
<li>"three"는 3, "six"는 6, "seven"은 7에 대응되기 때문에 정답은 입출력 예 #2와 같은 234567이 됩니다.</li>
<li>입출력 예 #2와 #3과 같이 같은 정답을 가리키는 문자열이 여러 가지가 나올 수 있습니다.</li>
</ul>

<p><strong>입출력 예 #4</strong></p>

<ul>
<li><code>s</code>에는 영단어로 바뀐 부분이 없습니다.</li>
</ul>

<hr>

<h5>제한시간 안내</h5>

<ul>
<li>정확성 테스트 : 10초</li>
</ul>


> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

처음에는 입력받은 문자열을 ToCharArray()로 배열로 바꾸고, 해당 배열의 모든 요소를 사용해서 숫자이면 정답 문자열에 더하고, 숫자가 아니면 tempString 문자열에 더해서 스위치문으로 숫자에 해당하는 영단어가 완성되었는지 검사하여 정답 문자열을 완성하고 정수로 바꿔 리턴하는 방식으로 구현했다.  
```cs
using System;

public class Solution {
    public int solution(string s) {
        string answer = "";
        string tempString = "";
        char[] temp = s.ToCharArray();
        for(int i = 0; i < temp.Length; i++)
        {
            if(char.IsDigit(temp[i]))
             {
                 answer += temp[i].ToString();
                 continue;
             }
            else
               {
                   tempString += temp[i].ToString();
                   switch(tempString)
                   {
                       case "zero":
                           answer += "0";
                           tempString = "";
                           break;
                       case "one":
                           answer += "1";
                           tempString = "";
                           break;
                       case "two":
                           answer += "2";
                           tempString = "";
                           break;
                       case "three":
                           answer += "3";
                           tempString = "";
                           break;
                       case "four":
                           answer += "4";
                           tempString = "";
                           break;
                       case "five":
                           answer += "5";
                           tempString = "";
                           break;
                       case "six":
                           answer += "6";
                           tempString = "";
                           break;
                       case "seven":
                           answer += "7";
                           tempString = "";
                           break;
                       case "eight":
                           answer += "8";
                           tempString = "";
                           break;
                       case "nine":
                           answer += "9";
                           tempString = "";
                           break;
                       default:
                           break;
                   }
               }
        }
        return int.Parse(answer);
    }
               
}
```
문제를 풀고 다른 사람들의 풀이를 보니 string.Replace()메서드를 활용해서 쉽게 풀 수 있다는 것을 깨닫고 아래와 같이 코드를 간결하게 수정할 수 있었다.  
```cs
using System;

public class Solution {
    public int solution(string s) {
        s = s.Replace("zero", "0");
        s = s.Replace("one", "1");
        s = s.Replace("two", "2");
        s = s.Replace("three", "3");
        s = s.Replace("four", "4");
        s = s.Replace("five", "5");
        s = s.Replace("six", "6");
        s = s.Replace("seven", "7");
        s = s.Replace("eight", "8");
        s = s.Replace("nine", "9");
        return int.Parse(s);
    }
               
}
```

## 팀 프로젝트 개발 - 플레이어  
팀 프로젝트에서 플레이어의 움직임 구현, 공격, 피격, 직업에 따른 외형 변경, 애니메이션을 맡았다. 전체적인 구조는 옵저버 패턴을 활용해서 만들었다. 인풋에 따라 이벤트를 호출하고 해당 이벤트에 구독한 메서드들이 실행된다. 옵저버 패턴을 
사용하니 특정 이벤트가 발생했을 때 실행해야 하는 다양한 행동들을 따로 개발할 수 있어서 편리했다. 예를 들자면 키보드의 WASD가 눌리면 Vector2값으로 변환해서 OnMove 액션을 호출한다. 캐릭터를 움직이라는 인풋을 받으면, 일단 캐릭터의 움직임 자체를 구현해야 하고, 또  idle 애니메이션과 움직일 때의 애니매이션이 다르다면, Animator에 있는 Parameter를 변경해서 애니메이션 전환이 이루어질 수 있도록 해야 한다. 이 외에도 캐릭터가 움직일 때 수행해야 하는 작업이 늘어날 수 
있고, 이 모든 작업을 한 스크립트 또는 클래스 안에서 일어난다면 나중에 문제가 발생했을 때 문제가 발생한 원인을 추적하기 쉽지 않다. 하지만 옵저버 패턴을 구독해서 특정 이벤트를 구독하게 하고 기능들을 나눠놓고 개발한다면, 처음 개발할
때는 한 곳에 몰아서 개발하는 것보다 양이 많을 수는 있어도 추후 디버깅 및 유지보수가 수월해진다는 것을 알게 되었다.  
