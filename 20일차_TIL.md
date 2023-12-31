# 내일배움캠프 20일차 TIL
오늘도 코드카타 후 개인과제 개발을 진행했다.  

## 코드카타 - 기억에 남는 문제  
# [level 1] 3진법 뒤집기 - 68935 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/68935) 

### 성능 요약

메모리: 31.6 MB, 시간: 0.67 ms

### 구분

코딩테스트 연습 > 월간 코드 챌린지 시즌1

### 채점결과

Empty

### 문제 설명

<p>자연수 n이 매개변수로 주어집니다. n을 3진법 상에서 앞뒤로 뒤집은 후, 이를 다시 10진법으로 표현한 수를 return 하도록 solution 함수를 완성해주세요.</p>

<hr>

<h5>제한사항</h5>

<ul>
<li>n은 1 이상 100,000,000 이하인 자연수입니다.</li>
</ul>

<hr>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>n</th>
<th>result</th>
</tr>
</thead>
        <tbody><tr>
<td>45</td>
<td>7</td>
</tr>
<tr>
<td>125</td>
<td>229</td>
</tr>
</tbody>
      </table>
<hr>

<h5>입출력 예 설명</h5>

<p>입출력 예 #1</p>

<ul>
<li>답을 도출하는 과정은 다음과 같습니다.</li>
</ul>
<table class="table">
        <thead><tr>
<th>n (10진법)</th>
<th>n (3진법)</th>
<th>앞뒤 반전(3진법)</th>
<th>10진법으로 표현</th>
</tr>
</thead>
        <tbody><tr>
<td>45</td>
<td>1200</td>
<td>0021</td>
<td>7</td>
</tr>
</tbody>
      </table>
<ul>
<li>따라서 7을 return 해야 합니다.</li>
</ul>

<p>입출력 예 #2</p>

<ul>
<li>답을 도출하는 과정은 다음과 같습니다.</li>
</ul>
<table class="table">
        <thead><tr>
<th>n (10진법)</th>
<th>n (3진법)</th>
<th>앞뒤 반전(3진법)</th>
<th>10진법으로 표현</th>
</tr>
</thead>
        <tbody><tr>
<td>125</td>
<td>11122</td>
<td>22111</td>
<td>229</td>
</tr>
</tbody>
      </table>
<ul>
<li>따라서 229를 return 해야 합니다.</li>
</ul>


> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

## 개인과제 개발 회고  
나름 구체적으로 설계했다고 생각했는데, 막상 개발을 진행해보니 막히거나 고민해야 하는 부분이 여럿 있었다. 가장 오래걸렸던 문제는 캐릭터 이름 변경시, 전체 참석자 목록에서 이전 이름이 지워지지 않고 새 이름이 추가되는 버그였다.
UI매니저를 만들어서 UI패널들의 On/Off를 담당하고, 텍스트 출력을 담당하게 만들고, 게임 매니저를 만들어서 모든 데이터를 한 곳에서 관리할 수 있도록 만들었다면 디버깅하기 편했을 것 같다. 이번에는 데이터 관리나 UI 출력 모두 각 기능을 담당하는 스크립트에서 모두 
처리하도록 개발했기 때문에, 플레이어의 이름을 변경하고 참석자 이름을 저장하는 리스트에 Add 하는 스크립트와, 해당 스크립트에서 플레이어의 옛 이름을 Remove하는 스크립트가 따로 있었기 때문에 어디가 문제인지 찾기가 쉽지 않았다. 개발을 공부하면 할수록 
알고리즘과 디자인 패턴의 중요성을 느끼는 것 같다.  
## 새로 알게 된 점 - DateTime 구조체  
이번에 개인과제를 진행하면서 현재 시간을 출력하는 선택 요구사항이 있었다. 이 기능을 구현하기 위해 조금 구글링을 해본 결과 유니티에 DateTime 이라는 ReadOnly Struct가 있다는 것을 알게 되었다. DateTime은 현재 시간에 관련된 다양한 프로퍼티와 메서드들을 제공한다.   
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using TMPro;
using System;

public class DisplayTime : MonoBehaviour
{
    private TextMeshProUGUI timeText;

    private void Awake()
    {
        timeText = GetComponent<TextMeshProUGUI>();
    }
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        timeText.text = DateTime.Now.ToShortTimeString();
    }
}
```

## 인풋 시스템, 델리게이트(액션), 옵저버 패턴  
기존의 인풋 방식이 아닌, 유니티에 새롭게 추가된 인풋 시스템을 활용하여 플레이어의 움직임을 구현해보았다. Input Actions에서 control scheme, 어떤 인풋으로 어떤 값을 얻어올 건지 등을 정의한다. 이번 과제의 경우 플레이어의 움직임을 구현하기 위해 Move 액션을 추가하고, 플레이어 오브젝트에 PlayerInput 컴포넌트를 추가하여 방금 만든 Input Actions를 넣어준다. 이후 PlayerInputController라는 스크립트를 만들어서 OnMove라는 이름으로 메서드를 만들어주면, 인풋값을 받을 때마다 해당 메서드를 호출한다. OnMove는 받은 인풋값을 노멀라이즈 한 후 Vector2값으로 변환하여 이벤트를 호출하는 메서드로 전달한다. 이벤트를 사용한 이유는 외부에서의 호출을 막기 위함이다. 액션은 매개변수를 받지만 반환값은 없는 델리게이트이다. OnMoveEvent 를 event Action<Vector2>로 선언하고, CallMoveEvent라는 메서드에서 OnMoveEvent가 null이 아닐 시 받은 Vector2값을 넣어 Invoke하도록 한다. 즉 해당 델리게이트에 어떤 메서드가 등록되어 있다면 그 메서드를 실행하는 것이다. 이러한 구조를 옵저버 패턴이라 하고, 이 이벤트에 메서드를 등록하는 것을 "구독"한다고 표현한다.  
<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/2c04e560-8202-4c2e-b2e6-c0b65a760a0f"/>    


