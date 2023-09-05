# 내일배움캠프 18일차 TIL
오전에는 유니티 입문 주차 발제와 코드카타 시간이 있었다. 이후 유니티 입문 강의를 수강하고 해당 주차 내용을 복습하는 시간을 가졌다.  
기억에 남는 문제는 문자열 내림차순으로 배치하기 이다.

## [level 1] 문자열 내림차순으로 배치하기 - 12917 
### 문제 설명

<p>문자열 s에 나타나는 문자를 큰것부터 작은 순으로 정렬해 새로운 문자열을 리턴하는 함수, solution을 완성해주세요.<br>
s는 영문 대소문자로만 구성되어 있으며, 대문자는 소문자보다 작은 것으로 간주합니다.</p>

<h5>제한 사항</h5>

<ul>
<li>str은 길이 1 이상인 문자열입니다.</li>
</ul>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>s</th>
<th>return</th>
</tr>
</thead>
        <tbody><tr>
<td>"Zbcdefg"</td>
<td>"gfedcbZ"</td>
</tr>
</tbody>
      </table>

> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

알파벳을 정렬하는 것 정도는 문자열을 ToCharArray()메서드를 통해 배열로 변환한 후 OrderByDescending 메서드를 이용해서 역순으로 재배치하는 방식으로 쉽게 구현할 수 있다고 생각했다. 일단 이렇게 구현하고 대문자를 어떻게 구분하고 배치할지 고민해보자는 생각으로 일단
코드를 작성하고 실행해보았으나 허무하게 그대로 문제가 풀렸다. 알고 보니 Char 자료형에서 대문자와 소문자를 비교하면 대문자가 더 크기 때문에, OrderByDescending 메서드로 문제의 요구사항대로 대문자가 자연스럽게 뒤쪽으로 배치된 것이었다. 이렇게 하나 알아간다...   
```cs
using System.Linq;

public class Solution {
    public string solution(string s) {
        string answer = "";
        char[] arr = s.ToCharArray();
        arr = arr.OrderByDescending(x => x).ToArray();
        foreach (char letter in arr)
        {
            answer += letter.ToString();
        }
        return answer;
    }
}
```

## Unity 게임 개발 입문  
사전캠프 때 들었던 게임개발 종합반 수업에서 웬만한 기본기는 다 익혔기 때문에, 이번 입문 강의는 별로 어려운 내용이 없을 줄 알았는데, 생각보다 이해하기 어렵고 새로운 내용들이 있었다.  

### Input Manager  
Input.GetAxis 등 메서드를 통해 인풋을 받아오던 방식과는 다르게, 패키지 매니저에서 Input Manager라는 패키지를 받아서 사용해보았다. 먼저 이벤트를 정의하고 , 이벤트를 부르는 메서드를 정의하는 CharacterController 스크립트를 작성한다.  
```cs
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;


public class TopDownCharacterController : MonoBehaviour
{
    public event Action<Vector2> OnMoveEvent;
    public event Action<Vector2> OnLookEvent;


    public void CallMoveEvent(Vector2 direction)
    {
        OnMoveEvent?.Invoke(direction);
    }

    public void CallLookEvent(Vector2 direction)
    {
        OnLookEvent?.Invoke(direction);
    }

}
```
방금 작성한 캐릭터 컨트롤러를 상속받고, 인풋에 따라 캐릭어 컨트롤러의 메서드를 호출하는 인풋 컨트롤러 스크립트를 작성한다.  
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerInputController : TopDownCharacterController
{
    private Camera _camera;
    private void Awake()
    {
        _camera = Camera.main;
    }

    public void OnMove(InputValue value)
    {
        // Debug.Log("OnMove" + value.ToString());
        Vector2 moveInput = value.Get<Vector2>().normalized;
        CallMoveEvent(moveInput);
    }

    public void OnLook(InputValue value)
    {
        // Debug.Log("OnLook" + value.ToString());
        Vector2 newAim = value.Get<Vector2>();
        Vector2 worldPos = _camera.ScreenToWorldPoint(newAim);
        newAim = (worldPos - (Vector2)transform.position).normalized;

        if (newAim.magnitude >= .9f)
        {
            CallLookEvent(newAim);
        }
    }

    public void OnFire(InputValue value)
    {
        Debug.Log("OnFire" + value.ToString());
    }
}
```
이후 별도의 스크립트에서 CharacterController 스크립트의 이벤트들에 구독하고, 이동 및 시선처리 등 이벤트를 통해 전달되는 값을 통해 다양한 기능을 구현할 수 있다. 아래는 이동 구현의 예시다.  
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class TopDownMovement : MonoBehaviour
{
    private TopDownCharacterController _controller;

    private Vector2 _movementDirection = Vector2.zero;
    private Rigidbody2D _rigidbody;

    private void Awake()
    {
        _controller = GetComponent<TopDownCharacterController>();
        _rigidbody = GetComponent<Rigidbody2D>();
    }

    private void Start()
    {
        _controller.OnMoveEvent += Move;
    }

    private void FixedUpdate()
    {
        ApplyMovment(_movementDirection);
    }

    private void Move(Vector2 direction)
    {
        _movementDirection = direction;
    }

    private void ApplyMovment(Vector2 direction)
    {
        direction = direction * 5;

        _rigidbody.velocity = direction;
    }
}
```
