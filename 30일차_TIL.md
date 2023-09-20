# 내일배움캠프 30일차 TIL  
오전에는 코드카타를 진행하고 이후 개인과제 와이어프레임을 작성하고 개발을 시작했다.  

## 코드카타 - 기억에 남는 문제    
# [level 1] 2016년 - 12901 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/12901) 

### 성능 요약

메모리: 31.3 MB, 시간: 2.51 ms

### 구분

코딩테스트 연습 > 연습문제

### 채점결과

Empty

### 문제 설명

<p>2016년 1월 1일은 금요일입니다. 2016년 a월 b일은 무슨 요일일까요? 두 수 a ,b를 입력받아 2016년 a월 b일이 무슨 요일인지 리턴하는 함수, solution을 완성하세요. 요일의 이름은 일요일부터  토요일까지 각각 <code>SUN,MON,TUE,WED,THU,FRI,SAT</code></p>

<p>입니다. 예를 들어 a=5, b=24라면 5월 24일은 화요일이므로 문자열 "TUE"를 반환하세요.</p>

<h5>제한 조건</h5>

<ul>
<li>2016년은 윤년입니다.</li>
<li>2016년 a월 b일은 실제로 있는 날입니다. (13월 26일이나 2월 45일같은 날짜는 주어지지 않습니다)</li>
</ul>

<h4>입출력 예</h4>
<table class="table">
        <thead><tr>
<th>a</th>
<th>b</th>
<th>result</th>
</tr>
</thead>
        <tbody><tr>
<td>5</td>
<td>24</td>
<td>"TUE"</td>
</tr>
</tbody>
      </table>

> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

요일들을 열거형으로 저장하고, 입력받는 a값에 따라 케이스를 나눠 스위치문을 작성했다. 이후 각 월의 1일에 해당하는 요일들을 기반으로 b일에 무슨 요일인지 계산하고, 해당하는 열거형 변수를 문자열로 변환하여 반환한다. b를 0부터 6 사이의 숫자로 맞춰주기 위해 while문을 통해 7식 빼는 방식을 사용했는데, 지금 생각해보니 7로 나눈 나머지를 구하면 더 간단했을 것 같다...그것보다 더 간단한 방법은 그냥 Datetime 구조체를 사용했으면 됐을텐데 하는 생각이 든다...   
```cs
public class Solution {
    enum DayType {
            SUN,
            MON,
            TUE,
            WED,
            THU,
            FRI,
            SAT
        }
    public string solution(int a, int b) {
        string answer = "";
        b--;
        while(b>=0)
        {
            b -= 7;
        }
        
        switch(a)
        {
            case 1:
                b =((5+b)>=0)? 5+b : 12 + b;
                break;
            case 2:
                b =((1+b)>=0)? 1+b : 8 + b;
                break;
            case 3:
                b =((2+b)>=0)? 2+b : 9 + b;
                break;
            case 4:
                b =((5+b)>=0)? 5+b : 12 + b;
                break;
            case 5:
                b =(b>=0)? b : 7 + b;
                break;
            case 6:
                b =((3+b)>=0)? 3+b : 10 + b;
                break;
            case 7:
                b =((5+b)>=0)? 5+b : 12 + b;
                break;
            case 8:
                b =((1+b)>=0)? 1+b : 8 + b;
                break;
            case 9:
                b =((4+b)>=0)? 4+b : 11 + b;
                break;
            case 10:
                b =((6+b)>=0)? 6+b : 13 + b;
                break;
            case 11:
                b =((2+b)>=0)? 2+b : 9 + b;
                break;
            case 12:
                b =((4+b)>=0)? 4+b : 11 + b;
                break;
        }
        answer += ((DayType)b).ToString();
        return answer;
    }
}
```
## Unity 숙련주차 개인과제 - 와이어프레임 작성  
이번 과제는 ATM 시스템 또는 RPG 기본 시스템 만들기 중 선택하는 것이었는데, 보다 도전적인 경험을 위해 RPG 시스템 만들기를 선택했다. 일단 필수 요구사항부터 완성해두기 위해, 상점부분 와이어프레임은 대략적으로만 짜두었다.    
<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/4a839aac-cc84-4cbc-925d-5e498508474b"/>     

플레이어 이름, 레벨, 스탯 등의 정보를 어떻게 저장해야 할 지에 대해 고민이 많았다. 기존에 해왔던 방식은 스탯을 저장하는 클래스를 만들고, 모든 정보를 클래스에 저장하는 것이었는데, 개발하다가 어떤 부분에서는 참조를 하고 어떤 부분에서는 값 복사를 하기도 해서 스탯을 제대로 불러오거나 업데이트하지 못하는 경우가 있곤 했다. 때문에 최근에 새롭게 공부한 스크립터블 오브젝트를 사용해보기로 했다. 또한 스탯을 외부에서 변경되면 혼동이 일어날 가능성이 크므로, 스탯 프로퍼티들을 private set으로 설정하여 스탯을 다루는 클래스 내부에서만 스탯을 변경하도록 개발할 예정이다.  

