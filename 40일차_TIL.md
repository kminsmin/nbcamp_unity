# 내일배움캠프 40일차 TIL  
오전에 코드카타 후, 유니티 심화주차 강의를 수강하고 개인과제로 무엇을 만들지 고민하는 시간을 가졌다.  

## 코드카타 - 기억에 남는 문제    
# [level 1] 옹알이 (2) - 133499 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/133499) 

### 성능 요약

메모리: 31.3 MB, 시간: 0.85 ms

### 구분

코딩테스트 연습 > 연습문제

### 채점결과

정확성: 100.0<br/>합계: 100.0 / 100.0

### 제출 일자

2023년 10월 2일 10:16:39

### 문제 설명

<p>머쓱이는 태어난 지 11개월 된 조카를 돌보고 있습니다. 조카는 아직 "aya", "ye", "woo", "ma" 네 가지 발음과 네 가지 발음을 조합해서 만들 수 있는 발음밖에 하지 못하고 연속해서 같은 발음을 하는 것을 어려워합니다. 문자열 배열 <code>babbling</code>이 매개변수로 주어질 때, 머쓱이의 조카가 발음할 수 있는 단어의 개수를 return하도록 solution 함수를 완성해주세요.</p>

<hr>

<h5>제한사항</h5>

<ul>
<li>1 ≤ <code>babbling</code>의 길이 ≤ 100</li>
<li>1 ≤ <code>babbling[i]</code>의 길이 ≤ 30</li>
<li>문자열은 알파벳 소문자로만 이루어져 있습니다.</li>
</ul>

<hr>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>babbling</th>
<th>result</th>
</tr>
</thead>
        <tbody><tr>
<td>["aya", "yee", "u", "maa"]</td>
<td>1</td>
</tr>
<tr>
<td>["ayaye", "uuu", "yeye", "yemawoo", "ayaayaa"]</td>
<td>2</td>
</tr>
</tbody>
      </table>
<hr>

<h5>입출력 예 설명</h5>

<p>입출력 예 #1</p>

<ul>
<li>["aya", "yee", "u", "maa"]에서 발음할 수 있는 것은 "aya"뿐입니다. 따라서 1을 return합니다.</li>
</ul>

<p>입출력 예 #2</p>

<ul>
<li>["ayaye", "uuuma", "yeye", "yemawoo", "ayaayaa"]에서 발음할 수 있는 것은 "aya" + "ye" = "ayaye", "ye" + "ma" + "woo" = "yemawoo"로 2개입니다. "yeye"는 같은 발음이 연속되므로 발음할 수 없습니다. 따라서 2를 return합니다.</li>
</ul>

<hr>

<h5>유의사항</h5>

<ul>
<li>네 가지를 붙여 만들 수 있는 발음 이외에는 어떤 발음도 할 수 없는 것으로 규정합니다. 예를 들어 "woowo"는 "woo"는 발음할 수 있지만 "wo"를 발음할 수 없기 때문에 할 수 없는 발음입니다.</li>
</ul>


> 출처: 프로그래머스 코딩 테스트 연습, https://school.programmers.co.kr/learn/challenges

주어진 babbling 배열에 있는 각 문자열마다 answer와 함께 CheckValidBabble이라는 메서드에 매개변수로 전달하고, 가능한 발음인지 검사했다. 해당 메서드는 받은 문자열 배열의 맨 앞 글자에 따라 스위치 문으로 케이스를 나누고, 이후 나머지 문자들이 올바른 발음 문자열과 일치하는지 검사한다. 검사 후 뒤에 글자들이 남아있으면 나머지 글자들로 문자열을 만들어 메서드를 재귀호출한다. 검사 결과 올바른 발음이고 뒤에 남은 글자가 없으면 answer에 1을 더한다.  
```cs
using System;

public class Solution {
    public bool isBabble = false;
    
    public int solution(string[] babbling) {
        int answer = 0;

         foreach(string babble in babbling)
         {
             CheckValidBabble(babble, ref answer);
         }
        return answer;
    }
    
    public void CheckValidBabble(string babble, ref int answer)
    {
        char[] letters = babble.ToCharArray();
             switch(letters[0].ToString())
             {
                 case "a":
                     try
                     {
                         if (letters[1].ToString() != "y") {break;}
                         if (letters[2].ToString() != "a") {break;}
                     }
                     catch(Exception E)
                     {
                         break;
                     }
                     
                     isBabble = true;
                     if(letters.Length > 3)
                     {
                         if (letters[3].ToString() == "a") {break;}
                         string remainingLetters = "";
                         for(int i = 3; i < letters.Length; i ++)
                         {
                             remainingLetters += letters[i];
                         }
                         CheckValidBabble(remainingLetters, ref answer);
                     }
                     else if (isBabble)
                     {
                         answer++;
                     }
                     break;
                 case "y":
                     try
                     {
                         if (letters[1].ToString() != "e") {break;}
                     }
                    catch(Exception E)
                     {
                         break;
                     }
                     isBabble = true;
                     if(letters.Length > 2)
                     {
                         if (letters[2].ToString() == "y") {break;}
                         string remainingLetters = "";
                         for(int i = 2; i < letters.Length; i ++)
                         {
                             remainingLetters += letters[i];
                         }
                         CheckValidBabble(remainingLetters, ref answer);
                     }
                     else if (isBabble)
                     {
                         answer++;
                     }
                     break;
                case "w":
                     try
                     {
                         if (letters[1].ToString() != "o") {break;}
                         if (letters[2].ToString() != "o") {break;}
                     }
                     catch(Exception E)
                     {
                         break;
                     }
                     isBabble = true;
                     if(letters.Length > 3)
                     {
                         if (letters[3].ToString() == "w") {break;}
                         string remainingLetters = "";
                         for(int i = 3; i < letters.Length; i ++)
                         {
                             remainingLetters += letters[i];
                         }
                         CheckValidBabble(remainingLetters, ref answer);
                     }
                     else if (isBabble)
                     {
                         answer++;
                     }
                     break;
                 case "m":
                    try
                     {
                         if (letters[1].ToString() != "a") {break;}
                     }
                    catch(Exception E)
                     {
                         break;
                     }
                     isBabble = true;
                     if(letters.Length > 2)
                     {
                         if (letters[2].ToString() == "m") {break;}
                         string remainingLetters = "";
                         for(int i = 2; i < letters.Length; i ++)
                         {
                             remainingLetters += letters[i];
                         }
                         CheckValidBabble(remainingLetters, ref answer);
                     }
                     else if (isBabble)
                     {
                         answer++;
                     }
                     break;
                 default:
                     break;
                
             }
        isBabble = false;
    }
}
```
