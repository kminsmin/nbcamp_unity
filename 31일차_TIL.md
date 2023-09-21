# 내일배움캠프 31일차 TIL  
오전에는 코드카타를 진행하고 개인과제를 진행했다. 오후에는 선발대 특강이 있었고, 이후 개인과제에 집중했다.  

## 선발대 특강 - UI 프로그래밍  
게임 매니저와는 별도로 UI매니저를 두는 것이 좋다. 프로젝트 규모가 커질수록 게임 매니저가 하는 일들이 많아지는데, 그만큼 UI의 종류 및 동작도 다양해지기 때문에 미리 구분해두는 것이 편리하다. UI매니저를 싱글톤화하여 필요할 때마다 호출하여 UI 관련 동작을 실행하도록 한다.  

## 개인과제 개발  
 필수 요구사항인 상태창과 인벤토리 창 UI를 만들고, 각 UI를 활성화 및 비활성화 할 수 있는 버튼들을 만들고 연결해주었다. 상태창의 스탯들과, 인벤토리의 아이템 칸들은 최근에 새로 알게된 Layout 컴포턴트를 이용해서 정리했다. 일일이 앵커나 포지션을 조작하여 정리하는 것 보다는 확실히 훨신 편했다.  
 그리고 요구사항에는 없지만, 플레이어의 이름과 직업을 설정하는 시작 씬을 만들었다. 시작 씬에서의 안내 문구들은 많은 게임들에서 하듯이 한 글자씩 천천히 뜨는 효과를 주고 싶어서 어떻게 구현할지 고민해보았다. Update 메서드에서는 이미 플레이어의 인풋을 감지하고 있기 때문에
 Update메서드를 사용하는 것 보다는, 코루틴을 사용해서 한 글자씩 출력하도록 구현했다.   

 ```cs
private void ChangeText(string line)
    {
        string _line = line;
        char[] dialogueTexts = _line.ToCharArray();
        StartCoroutine(SSUIManager.instance.UpdateText(dialogueTexts));
    }


 public IEnumerator UpdateText(char[] letters)
    {
        string textBuffer = null;
        int i = 0;
        float time = 0;
        
        while (i < letters.Length)
        {
            if (time <= 0)
            {
                textBuffer += letters[i];
                _dialogueText.text = textBuffer;
                time += textInterval;
                i++;
            }
            else
            {
                time -= Time.deltaTime;
                yield return null;
            }
                
        }
        _dialogueManager.NextTextReady();
    }

```
