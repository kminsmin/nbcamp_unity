# 내일배움캠프 23일차 TIL  
오전에는 코드카타 후, 오후에 팀 프로젝트 개발 및 저녁에는 각자 개발한 부분을 머지하고 리뷰하는 시간을 가졌다.  

## 코드카타 - 기억에 남는 문제  

# [level 1] 시저 암호 - 12926 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/12926) 

### 성능 요약

메모리: 47.5 MB, 시간: 35.24 ms

### 구분

코딩테스트 연습 > 연습문제

### 채점결과

Empty

### 문제 설명

<p>어떤 문장의 각 알파벳을 일정한 거리만큼 밀어서 다른 알파벳으로 바꾸는 암호화 방식을 시저 암호라고 합니다.  예를 들어 "AB"는 1만큼 밀면 "BC"가 되고, 3만큼 밀면 "DE"가 됩니다. "z"는 1만큼 밀면 "a"가 됩니다. 문자열 s와 거리 n을 입력받아 s를 n만큼 민 암호문을 만드는 함수, solution을 완성해 보세요.</p>

<h5>제한 조건</h5>

<ul>
<li>공백은 아무리 밀어도 공백입니다.</li>
<li>s는 알파벳 소문자, 대문자, 공백으로만 이루어져 있습니다.</li>
<li>s의 길이는 8000이하입니다.</li>
<li>n은 1 이상, 25이하인 자연수입니다.</li>
</ul>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>s</th>
<th>n</th>
<th>result</th>
</tr>
</thead>
        <tbody><tr>
<td>"AB"</td>
<td>1</td>
<td>"BC"</td>
</tr>
<tr>
<td>"z"</td>
<td>1</td>
<td>"a"</td>
</tr>
<tr>
<td>"a B z"</td>
<td>4</td>
<td>"e F d"</td>
</tr>
</tbody>
      </table>

> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

처음에는 모든 알파벳이 들어있는 배열을 만들고, 이중 for문으로 입력받은 문자열의 각 알파벳이 몇 번째 알파벳인지 알아내고, 입력받은 정수만큼 뒷 순서에 있는 알파벳을 answer 문자열에 더하는 방식으로 구현했다.  
```cs
public class Solution {
    public string solution(string s, int n) {
        string answer = "";
        string alphabet = "abcdefghijklmnopqrstuvwxyz";
        char[] alphabets = alphabet.ToCharArray();
        char[] letters = s.ToCharArray();
        bool isCapital = false;
        
        for(int i = 0; i < letters.Length; i++)
        {
            if(char.IsLetter(letters[i])==false)
            {
                answer += " ";
                continue;
            }
            if (char.IsLower(letters[i]) == false)
            {
                letters[i] = char.ToLower(letters[i]); 
                isCapital = true;
            }
            

            for(int j = 0; j < 26; j++)
            {         
                if (alphabets[j] == letters[i])
                {
                    letters[i] = (j+n<=25)? alphabets[j+n] : alphabets[j+n-26];
                    if (isCapital)
                        letters[i] = char.ToUpper(letters[i]);
                    answer += letters[i].ToString();
                    break;
                }
                else continue;
            }
            isCapital = false;
        }
        return answer;
    }
}
```
하지만 문제를 푼 후 다른 사람들의 코드를 보니 굳이 알파벳의 배열을 만들 필요가 없다는 것을 알게 되었다. 아래 방식으로 구현할 경우 처음 짠 코드보다 시간복잡도 및 공간복잡도가 현저히 줄어들었다.  
```cs
public class Solution {
    public string solution(string s, int n) {
        char[] temp = s.ToCharArray();
        
        for(int i = 0; i < temp.Length; i++)
        {
            if(char.IsLetter(temp[i]))
            {
                if (char.IsLower(temp[i]))
                {
                    temp[i] = (char)((temp[i] + n - 'a')%26 + 'a');
                    
                }
                else
                {
                   temp[i] = (char)((temp[i] + n - 'A')%26 + 'A');
                }
            }
        }
        return new string(temp);
    }
}
```   
