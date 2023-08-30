<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/a458479d-5fa7-4b28-9552-fb14ff410312)"/>   

# 내일배움캠프 15일차    
코드카타, 팀프로젝트 개발, 4주차 강의 복습  


## 코드카타 - 문자열을 정수로 바꾸기  
### 문제 설명  

<p>문자열 s를 숫자로 변환한 결과를 반환하는 함수, solution을 완성하세요.</p>

<h5>제한 조건</h5>

<ul>
<li>s의 길이는 1 이상 5이하입니다.</li>
<li>s의 맨앞에는 부호(+, -)가 올 수 있습니다.</li>
<li>s는 부호와 숫자로만 이루어져있습니다.</li>
<li>s는 "0"으로 시작하지 않습니다.</li>
</ul>

<h5>입출력 예</h5>

<p>예를들어 str이 "1234"이면 1234를 반환하고, "-1234"이면 -1234를 반환하면 됩니다.<br>
str은 부호(+,-)와 숫자로만 구성되어 있고, 잘못된 값이 입력되는 경우는 없습니다.</p>


> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

int.Parse 메서드를 사용해야 되는 줄은 알았지만, int.Parse가 부호도 처리할 수 있다는 사실을 몰라서, 처음에는 받은 문자열을 ToCharArray 메서드를 통해 배열로 바꾸고, 배열의 맨 앞 요소를 검사해서 케이스를 나눠 Int.Parse 메서드를 사용하는 방식으로 구현했다.  
```cs
public class Solution {
    public int solution(string s) {
        int answer = 0;
        char[] arr = s.ToCharArray();
        if (char.IsDigit(arr[0]))
        {
            answer = int.Parse(s);
        }
        else
        {
            if (arr[0].ToString() == "+")
            {
                s = s.Substring(1, s.Length-1);
                answer = int.Parse(s);
            }
            else
            {
                s = s.Substring(1, s.Length-1);
                answer = -int.Parse(s);
            }
            
        }
        return answer;
    }
}
```
문제를 풀고 다른 사람들의 풀이를 보니, Int.Parse 단 한줄로 문제를 푼 코드를 보았다...  
```cs
public class Solution {
    public int solution(string s) {
        int answer = int.Parse(s);
        return answer;
    }
}
```
