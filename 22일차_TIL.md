# 내일배움캠프 22일차 TIL  
오늘 오전에는 새로운 팀 프로젝트 발제가 있었다. 발제와 코드카타 후 어떤 게임을 만들지 논의하고, 대략적인 와이어프레임과 클래스 구조를 짜고 업무를 분담하는 시간을 가졌다.  

## 코드카타 - 기억에 남는 문제  

  ### [level 1] 최소직사각형 - 86491 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/86491) 

### 성능 요약

메모리: 32.2 MB, 시간: 0.74 ms

### 구분

코딩테스트 연습 > 완전탐색

### 채점결과

Empty

### 문제 설명

<p>명함 지갑을 만드는 회사에서 지갑의 크기를 정하려고 합니다. 다양한 모양과 크기의 명함들을 모두 수납할 수 있으면서, 작아서 들고 다니기 편한 지갑을 만들어야 합니다. 이러한 요건을 만족하는 지갑을 만들기 위해 디자인팀은 모든 명함의 가로 길이와 세로 길이를 조사했습니다.</p>

<p>아래 표는 4가지 명함의 가로 길이와 세로 길이를 나타냅니다.</p>
<table class="table">
        <thead><tr>
<th>명함 번호</th>
<th>가로 길이</th>
<th>세로 길이</th>
</tr>
</thead>
        <tbody><tr>
<td>1</td>
<td>60</td>
<td>50</td>
</tr>
<tr>
<td>2</td>
<td>30</td>
<td>70</td>
</tr>
<tr>
<td>3</td>
<td>60</td>
<td>30</td>
</tr>
<tr>
<td>4</td>
<td>80</td>
<td>40</td>
</tr>
</tbody>
      </table>
<p>가장 긴 가로 길이와 세로 길이가 각각 80, 70이기 때문에 80(가로) x 70(세로) 크기의 지갑을 만들면 모든 명함들을 수납할 수 있습니다. 하지만 2번 명함을 가로로 눕혀 수납한다면 80(가로) x 50(세로) 크기의 지갑으로 모든 명함들을 수납할 수 있습니다. 이때의 지갑 크기는 4000(=80 x 50)입니다.</p>

<p>모든 명함의 가로 길이와 세로 길이를 나타내는 2차원 배열 sizes가 매개변수로 주어집니다. 모든 명함을 수납할 수 있는 가장 작은 지갑을 만들 때, 지갑의 크기를 return 하도록 solution 함수를 완성해주세요.</p>

<hr>

<h5>제한사항</h5>

<ul>
<li>sizes의 길이는 1 이상 10,000 이하입니다.

<ul>
<li>sizes의 원소는 [w, h] 형식입니다.</li>
<li>w는 명함의 가로 길이를 나타냅니다.</li>
<li>h는 명함의 세로 길이를 나타냅니다.</li>
<li>w와 h는 1 이상 1,000 이하인 자연수입니다.</li>
</ul></li>
</ul>

<hr>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>sizes</th>
<th>result</th>
</tr>
</thead>
        <tbody><tr>
<td>[[60, 50], [30, 70], [60, 30], [80, 40]]</td>
<td>4000</td>
</tr>
<tr>
<td>[[10, 7], [12, 3], [8, 15], [14, 7], [5, 15]]</td>
<td>120</td>
</tr>
<tr>
<td>[[14, 4], [19, 6], [6, 16], [18, 7], [7, 11]]</td>
<td>133</td>
</tr>
</tbody>
      </table>
<hr>

<h5>입출력 예 설명</h5>

<p>입출력 예 #1<br>
문제 예시와 같습니다.</p>

<p>입출력 예 #2<br>
명함들을 적절히 회전시켜 겹쳤을 때, 3번째 명함(가로: 8, 세로: 15)이 다른 모든 명함보다 크기가 큽니다. 따라서 지갑의 크기는 3번째 명함의 크기와 같으며, 120(=8 x 15)을 return 합니다.</p>

<p>입출력 예 #3<br>
명함들을 적절히 회전시켜 겹쳤을 때, 모든 명함을 포함하는 가장 작은 지갑의 크기는 133(=19 x 7)입니다.</p>


> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

  2차원 배열의 각 행을 내림차순으로 배치하고, 각 열의 최대값을 찾아서 둘을 곱함으로써 답을 구하는 방식으로 구현했다. 최댓값 알고리즘, 대입 알고리즘 등 기본적인 알고리즘을 활용하는 데 익숙해지기 위해서 편리한 메소드들을 최대한 사용하지 않고 3번의 for문을 활용해서 구현했다.  
  ```cs
using System;

public class Solution {
    public int solution(int[,] sizes) {
        int answer = 0;
        int maxWidth = int.MinValue;
        int maxHeight = int.MinValue;
        
        for(int i = 0; i < sizes.GetLength(0); i++)
        {
            if (sizes[i,0] < sizes[i,1])
            {
                int temp =sizes[i,1];
                sizes[i,1] = sizes[i,0];
                sizes[i,0] = temp;
            }
            else continue;
        }
        
        for(int i = 0; i < sizes.GetLength(0); i++)
        {
            if (sizes[i,0] > maxWidth)
            {
                maxWidth = sizes[i,0];
            }
            else continue;
        }
        
        for(int i = 0; i < sizes.GetLength(0); i++)
        {
            if (sizes[i,1] > maxHeight)
            {
                maxHeight = sizes[i,1];
            }
            else continue;
        }
        
        answer = maxWidth * maxHeight;
        
        return answer;
    }
}
```

## 게임개발 입문 프로젝트 - 추억의 게임 만들기   
이번 팀 과제는 추억의 게임인 "똥피하기", "닷지", 그리고 "벽돌깨기" 중 하나를 선택해서 현대적인 버전으로 재현하는 과제였다. 우리 팀의 경우 투표를 통해 "닷지"를 리메이크하기로 결정했다. 다만 과제의 기본 요구 사항 중 플레이어의 총알 발사 및 충돌 처리가 있었는데, 투사체를 발사한다는 점에서 이미 원본 닷지와는 거리가 멀다고 판단하여 회의 결과 닷지에 뿌리를 둔 
뱀서라이크류 게임을 만들자고 정했다. 이후 어떤 씬들을 어떤 식으로 보일지 간단한 UI를 포함한 전체적인 와이어프레임을 작성하고, 코드 컨벤션 및 구조의 약속을 정한 뒤 업무를 분담하고 각자 개발을 시작했다. 협업도 많이 하다보니 늘긴 하는 것 같다. 예전에 비해 목표 설정, 구조 설계 및 업무 분담하는 속도가 빨라진 것이 느껴진다.  
<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/51314eb6-640c-4510-9673-868529b12d83"/>  
