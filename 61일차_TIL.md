# 내일배움캠프 61일차 TIL    
오늘도 오전에 기술면접 예상질문을 풀어보고 팀 프로젝트 개발을 진행했다.   

## 람다식 
- 일종의 무명 메서드이다. => 기호의 왼쪽에는 매개변수를, 오른쪽에는 실행할 코드블럭을 쓴다.

## 콜백  
- 피호출자가 호출자를 다시 호출하는 것.
- 일상 생활에서의 예시  
      - 어떤 사람이 한 회사에 사장님을 만나러 갔다. 그런데 사장님이 부재중이어서, 사장님의 비서에게 '연락처를 남겨두고 갈 테니 사장님 오시면 연락 달라고 해주세요'라고 해두고 간다. 사장님이 돌아왔을 때, 해당 연락처로 연락을 하면 이것이 바로 콜백이다.
- 실제 사용 예시 :
```cs
    public event Action DesynchronizeEvent;

    private void Start()
    {
        DesynchronizeEvent += OnDesychronizeEvent;
    }

    private void OnDesychronizeEvent()
    {
        Time.timeScale = 0f;
        _resultUI.gameObject.SetActive(true);
        _timeText.text = (_time >= 60)? (_time/60).ToString("F0")+"분 " +  (_time%60).ToString("F0")+"초" : _time.ToString("F2")+"초";
        _roomText.text = _checkedRoom.ToString() + "개";
    }
  // DesynchronizeEvent?.Invoke(); 콜백
```
