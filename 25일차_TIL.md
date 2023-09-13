# 내일배움캠프 24일차 TIL  
오전에는 코드카타 후, 오후에 팀 프로젝트 개발 및 저녁에는 각자 개발한 부분을 머지하고 리뷰하는 시간을 가졌다.  

## 코드카타 - 기억에 남는 문제  
# [level 1] K번째수 - 42748 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/42748) 

### 성능 요약

메모리: 31.3 MB, 시간: 4.30 ms

### 구분

코딩테스트 연습 > 정렬

### 채점결과

Empty

### 문제 설명

<p>배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.</p>

<p>예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면</p>

<ol>
<li>array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.</li>
<li>1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.</li>
<li>2에서 나온 배열의 3번째 숫자는 5입니다.</li>
</ol>

<p>배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.</p>

<h5>제한사항</h5>

<ul>
<li>array의 길이는 1 이상 100 이하입니다.</li>
<li>array의 각 원소는 1 이상 100 이하입니다.</li>
<li>commands의 길이는 1 이상 50 이하입니다.</li>
<li>commands의 각 원소는 길이가 3입니다.</li>
</ul>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>array</th>
<th>commands</th>
<th>return</th>
</tr>
</thead>
        <tbody><tr>
<td>[1, 5, 2, 6, 3, 7, 4]</td>
<td>[[2, 5, 3], [4, 4, 1], [1, 7, 3]]</td>
<td>[5, 6, 3]</td>
</tr>
</tbody>
      </table>
<h5>입출력 예 설명</h5>

<p>[1, 5, 2, 6, 3, 7, 4]를 2번째부터 5번째까지 자른 후 정렬합니다. [2, 3, 5, 6]의 세 번째 숫자는 5입니다.<br>
[1, 5, 2, 6, 3, 7, 4]를 4번째부터 4번째까지 자른 후 정렬합니다. [6]의 첫 번째 숫자는 6입니다.<br>
[1, 5, 2, 6, 3, 7, 4]를 1번째부터 7번째까지 자릅니다. [1, 2, 3, 4, 5, 6, 7]의 세 번째 숫자는 3입니다.</p>


> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

  앞에서 주어진 수만큼의 요소를 제거하는 Skip, 그리고 시작지점으로부터 주어진 수만큼의 요소를 가져오는 Take 함수를 사용해서 i부터j까지의 요소를 가진 리스트를 만들고, 이를 정렬하여 k번째 숫자를 answerList에 추가하는 방식으로 구현했다.  
  ```cs
using System;
using System.Linq;
using System.Collections.Generic;

public class Solution {
    public List<int> solution(int[] array, int[,] commands) {       
        List<int> temp = new List<int>();
        List<int> answerList = new List<int>();
        
        for(int i = 0; i < commands.GetLength(0); i++)
        {
            temp.Clear();
            temp = array.Skip(commands[i,0]-1).Take(commands[i,1] - commands[i,0] + 1).ToList();
            temp.Sort();
            answerList.Add(temp[commands[i,2]-1]);
        }
        return answerList;
    }
}
```

## 팀 프로젝트 개발 - 디버깅  
 플레이어의 기능들은 옵저버 패턴을 활용해서 구현했다. 플레이어의 사망도 마찬가지로 플레이어의 체력이 0 이하가 되면 OnDeathEvent를 구독한 메서드를 실행하도록 해두었는데, 게임 매니저에서 해당 이벤트를 구독했는데도 불구하고 게임 오버시 실행해야 하는 메서드가 호출되지 않는 현상이 있었다. 플레이어 게임 오브젝트 내부에 있는 클래스들 사이에서는 같은 객체에 들어있기 때문에 이러한 문제가
 없었던 것 같다. 아무래도 플레이어 오브젝트를 참조하고 오브젝트 내부에 있는 스크립트의 이벤트를 구독하다 보니, 전혀 다른 객체의 이벤트를 구독해서 이러한 현상이 벌어진 것으로 추정되어서, 이벤트들이 정의되어 있는 CharacterEventController 스크립트를 싱글톤화 하였다. 이후 해당 버그는 해결되었다.  
 ```cs
//CharacterEventController
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CharacterEventController : MonoBehaviour
{
    public event Action<Vector2> OnMoveEvent;
    public event Action<Vector2> OnLookEvent;
    public event Action OnDeathEvent;
    public event Action OnShootEvent;
    public float skillTime = 2.0f;
    public static CharacterEventController instance = null;

    [SerializeField] private GameObject skill;

    private void Awake()
    {
        instance = this;
    }
    public void CallMoveEvent(Vector2 direction)
    {
        OnMoveEvent?.Invoke(direction);
    }

    public void CallLookEvent(Vector2 direction) 
    {  
        OnLookEvent?.Invoke(direction);
    }

    public void CallShootEvent(bool isShooting)
    {
        OnShootEvent?.Invoke();
    }

    public void CallSkill(bool isShooting)
    {
        skill.SetActive(isShooting);
        Invoke("DeactivateSkill", skillTime);
    }

    public void CallDeathEvent()
    {
        Debug.Log("Died!");
        OnDeathEvent?.Invoke();
    }

    private void DeactivateSkill()
    {
        GameObject.Find("Skill")?.SetActive(false);
    }
}

//GameManager
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GameManager : MonoBehaviour
{
    [SerializeField] GameManager gm;
    [SerializeField] GameObject gameOverImg;
    [SerializeField] GameObject gameWinImg;
    [SerializeField] AudioManager am;
    [SerializeField] UIManager um;
    AudioSource audioS;

    void Awake()
    {
        audioS = GetComponent<AudioSource>();
    }

    void Start()
    {
        audioS.clip = null;
        CharacterEventController.instance.OnDeathEvent += TurnOnGameOver; //플레이어 사망 이벤트 구독
        MonsterManager.Instance.AddMonsterDieEvent(DefeatBoss);
    }

    void TurnOnGameOver()                                                 //플레이어 사망 시 실드
    {
        um.GetComponent<AudioSource>().Stop();
        Time.timeScale = 0;
        audioS.clip = am.gameOver;
        audioS.Play();
        gameOverImg.SetActive(true);
    }

    void DefeatBoss(int identifier, Vector3 position)
    {
        if (identifier == 3)
        {
            um.GetComponent<AudioSource>().Stop();
            Time.timeScale = 0;
            audioS.clip = am.gameWin;
            audioS.Play();
            audioS.Play();
            gameWinImg.SetActive(true);
        }
    }
}

