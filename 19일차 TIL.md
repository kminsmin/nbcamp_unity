# 내일배움캠프 19일차 TIL
오늘은 코드카타 후 어제 배웠던 내용을 정리하며 복습하는 시간을 가졌다. 이후 개인과제 와이어프레임 및 패키지 구조도를 작성하고 개발을 시작했다.  

## 코드카타 - 기억에 남는 문제  
### [level 1] 행렬의 덧셈 - 12950 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/12950) 

### 성능 요약

메모리: 52.1 MB, 시간: 9.90 ms

### 구분

코딩테스트 연습 > 연습문제

### 채점결과

Empty

### 문제 설명

<p>행렬의 덧셈은 행과 열의 크기가 같은 두 행렬의 같은 행, 같은 열의 값을 서로 더한 결과가 됩니다. 2개의 행렬 arr1과 arr2를 입력받아, 행렬 덧셈의 결과를 반환하는 함수, solution을 완성해주세요.</p>

<h5>제한 조건</h5>

<ul>
<li>행렬 arr1, arr2의 행과 열의 길이는 500을 넘지 않습니다.</li>
</ul>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>arr1</th>
<th>arr2</th>
<th>return</th>
</tr>
</thead>
        <tbody><tr>
<td>[[1,2],[2,3]]</td>
<td>[[3,4],[5,6]]</td>
<td>[[4,6],[7,9]]</td>
</tr>
<tr>
<td>[[1],[2]]</td>
<td>[[3],[4]]</td>
<td>[[4],[6]]</td>
</tr>
</tbody>
      </table>

> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

  이 문제를 풀기 위해 그동안 어렴풋이 알고 있었던 다차원 배열에 대해 자세히 공부해야 했다.  
  ## 다차원배열  
  다차원 배열이란 일차원 배열 여러개로 이루어진 배열을 말한다. 즉 2차원 이상의 배열을 의미한다. 2차원 배열의 경우 그 요소들을 행렬로 나타낼 수 있다. 예를 들면 2차원 배열 arr에서 arr.GetLength(0) 은 행의 수, arr.GetLength(1)은 열의 수를 반환한다.  
  ```cs
public class Solution {
    public int[,] solution(int[,] arr1, int[,] arr2) {
        int[,] answer = new int[,] {{}};
        for (int i = 0; i<arr1.GetLength(0); i++)
        {
            for (int j = 0; j<arr1.GetLength(1); j++)
            {
                arr1[i,j] += arr2[i,j];
            }
        }
        answer = arr1;
        
        
        return answer;
    }
}
```

## 개인과제 -게더타운 클론 만들기  
본격적으로 개발을 시작하기에 앞서 원활한 개발을 위해 와이어프레임 및 다이어그램과 패키지 구조도 등을 작성해보았다.  
### 와이어프레임  
먼저 구현하고자 하는 기능들을 정리하고, 해당 기능들을 어떤 모습으로 구현하고 싶은지 간단히 나타내보았다.  
<img width="100%" src="https://github.com/kminsmin/Algorithm_Test_Practice/assets/114645806/e7a1784c-3815-461c-aed1-ac7677ba8b72"/>    

아직도 미리 구조를 설계하고 플로우차트를 그리는 것이 어렵게 느껴진다. 익숙해지도록 노력해야겠다.
