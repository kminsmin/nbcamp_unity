# 내일배움캠프 33일차 TIL  
오늘은 유니티 숙련주차 팀 프로젝트 발제가 있었다. 이후 팀원들과 개발할 게임의 컨셉 회의를 간략히 하고 코드카타를 진행하고, 다시 만나서 와이어프레임, 패키지 구조도를 작성하고 업무를 분담하는 시간을 가졌다. 저녁에는 전략 패턴에 대한 특강이 있었다.  

## 코드카타 - 기억에 남는 문제  
# [level 1] 모의고사 - 42840 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/42840) 

### 성능 요약

메모리: 31 MB, 시간: 3.19 ms

### 구분

코딩테스트 연습 > 완전탐색

### 채점결과

정확성: 100.0<br/>합계: 100.0 / 100.0

### 문제 설명

<p>수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.</p>

<p>1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...<br>
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...<br>
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...</p>

<p>1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.</p>

<h5>제한 조건</h5>

<ul>
<li>시험은 최대 10,000 문제로 구성되어있습니다.</li>
<li>문제의 정답은 1, 2, 3, 4, 5중 하나입니다.</li>
<li>가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.</li>
</ul>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>answers</th>
<th>return</th>
</tr>
</thead>
        <tbody><tr>
<td>[1,2,3,4,5]</td>
<td>[1]</td>
</tr>
<tr>
<td>[1,3,2,4,2]</td>
<td>[1,2,3]</td>
</tr>
</tbody>
      </table>
<h5>입출력 예 설명</h5>

<p>입출력 예 #1</p>

<ul>
<li>수포자 1은 모든 문제를 맞혔습니다.</li>
<li>수포자 2는 모든 문제를 틀렸습니다.</li>
<li>수포자 3은 모든 문제를 틀렸습니다.</li>
</ul>

<p>따라서 가장 문제를 많이 맞힌 사람은 수포자 1입니다.</p>

<p>입출력 예 #2</p>

<ul>
<li>모든 사람이 2문제씩을 맞췄습니다.</li>
</ul>


> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

수포자의 번호와 점수를 각각 Key와 Value 로 딕셔너리에 저장하여 Value값을 최대값 알고리즘으로 비교하며 answer 리스트에 조건에 따라 Key를 추가하는 방식으로 구현했다.  
```cs
using System;
using System.Collections.Generic;

public class Solution {
    public List<int> solution(int[] answers) {
        int[] answer1 = new int[] { 1, 2, 3, 4, 5};
        int[] answer2 = new int[] {2, 1, 2, 3, 2, 4, 2, 5};
        int[] answer3= new int[] {3, 3, 1, 1, 2, 2, 4, 4, 5, 5};
        Dictionary<int, int> scores = new Dictionary<int, int>();
        scores.Add(1, 0);
        scores.Add(2, 0);
        scores.Add(3, 0);
        List<int> answer = new List<int>();
        
        for (int i = 0; i < answers.Length; i++)
        {
            if (answers[i]==answer1[i%5])
                scores[1]++;
            if (answers[i]==answer2[i%8])
                scores[2]++;
            if (answers[i]==answer3[i%10])
                scores[3]++;
        }
        
        int max = 0;
        foreach(KeyValuePair<int,int> score in scores)
        {
            if (score.Value > max)
            {
                max = score.Value;
                answer.Clear();
                answer.Add(score.Key);
            }
            else if (score.Value == max)
            {
                max = score.Value;
                answer.Add(score.Key);
            }
        }

        return answer;
    }
}
```


  ## 팀 프로젝트 - 와이어프레임 작성, 역할 분담  
  이번 팀 프로젝트는 타워 디펜스, 2D 로그라이크, 3D 퍼즐 플랫폼, 3D 서바이벌 게임 중 하나를 선택하여 개발하는 것이다. 우리 조의 경우, 팀원들이 가장 많이 접해보기도 했고 만들어보고 싶어하는 2D 로그라이크 게임을 개발하기로 했다. 이후 게임의 컨셉, 들어가야 할 필수 기능들을 포함한 와이어프레임을 작성하고, 이를 토대로 간략하게 패키지 구조도를 작성한 뒤 각자 업무를 분담하여 개발을 시작했다.  
  <img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/e2eb3e83-7471-4b7a-bc45-ee8ea5849112"/>   

  <img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/ebfd143f-1631-4869-a4f2-66b5589d99f5"/>   

  ## 디자인 패턴 특강 - 전략 패턴  
  전략패턴은 객체 지향 프로그래밍에서 캡슐화를 극대화하는 패턴이다. 최상위 추상 클래스를 만들고, 이를 상속하는 하위 클래스들을 만들 때, 클래스마다 다르게 동작하는 메서드가 있다면, 이를 인터페이스로 만들고 해당 인터페이스를 구현하는 개별의 클래스들을 만든다. 이후 최상위 추상 클래스에서 인터페이스를 필드로 받아오고, 해당 필드를 각 하위 클래스들에서 개별적으로 인터페이스를 구현한 클래스들 중에서 필요한 클래스의 메서드에 접근한다. 해당 메서드를 인터페이스와 개별적인 클래스들로 구현하지 않았다면, 이를 최상위 클래스에 선언하고 하위 클래스들에서 필요에 따라 오버라이드 하며 사용해야 했을 것이다. 이러한 상황에서 전략 패턴을 사용하면 코드 재사용성을 높여 효율을 극대화할 수 있다.  
