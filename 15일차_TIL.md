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
문제를 풀고 다른 사람들의 풀이를 보니, Int.Parse 단 한줄로 문제를 푼 코드를 보았다...  그래도 위에서 뻘짓을 한 덕에 해당 char가 0부터 9사이의 숫자인지 검사하고 bool 값을 반환하는 char.IsDigit()메서드를 알게 되었다. 문자열 맨 앞이 부호인지 숫자인지 판단하기 위해 해당 
메서드를 사용했었다.  
```cs
public class Solution {
    public int solution(string s) {
        int answer = int.Parse(s);
        return answer;
    }
}
```
## 팀프로젝트 개발 - Recovery Scene  
이번 팀프로젝트는 저번 개인과제와 굉장히 유사하지만, 개인과제때 짰던 코드와 이번에 개발하기로 한 코드의 구조가 많이 달라서 초반에 난항을 많이 겪었다. 개인과제 때는 기능별로 스크립트를 나눌 생각도 못하고 하나의 스크립트로 모든 기능을 구현했었는데, 이번에는 다양한 분류로 스크립트를
구분해두고 개발했다. 내가 맡은 부분은 플레이어의 체력을 회복하는 Recovery Scene, 캐릭터와 몬스터 클래스 및 그 상위 클래스인 캐릭터와 몬스터 컨트롤러 클래스였다.  
우리 코드의 기본적인 작동방식은 프로그램이 시작하면 게임 매니저를 시작하고, 게임 매니저 안에서 루프를 돌며 인풋에 따른 행동을 게임 매니저 안에서 객체로 만들어진 씬 매니저가 처리하는 방식이다. 초기화면에서는 받은 인풋에 해당하는 씬을 불러오고, 이후 해당 씬 내부의 인풋 처리 함수를 
호출하여 씬 내부에서 인풋에 따른 행동을 수행한다. 즉, 각 씬에서는 스위치문을 활용해 다양한 상황에서 보여야 할 화면을 출력하는 부분, 그리고 인풋을 처리하는 부분이 필요하다. 이 두가지가 공통적으로 필요하므로 MainLoop(), ActByInput 메서드라고 칭하여 IScene이라는 인터페이스에 선언해주고, 각 씬은 이 인터페이스를 상속받고 메서드를 정의한다. 이 원리를 이해하기 정말 힘들었으나 팀원들의 도움으로 겨우 겨우 이해할 수 있었다. Recovery Scene의 경우 초기에는 씬에 대한 설명과 사용 가능한 포션 수 등을 보여준다. 이후 플레이어가 체력을 회복하면 이전에 출력했던 내용에 추가로
얼마를 회복했는지를 출력한다. 다른 씬으로 이동하는 인풋을 받으면 이동할 수 있는 씬들의 리스트를 씬 매니저로부터 받아서 출력한다.  
```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using TextRPGing.Define.Interface;
using TextRPGing.Manager;
using TextRPGing.Model;
using TextRPGing.Utils;

namespace TextRPGing.Scene
{
    public class Recovery : IScene
    {
        Define.GameEnum.eSceneType type = Define.GameEnum.eSceneType.Recovery;
        private int tempHP;
        private enum HealingState
        {
            isHeal,
            isOut
        }
        private int heal = 0;
        HealingState healingState = HealingState.isHeal;
        public void MainLoop()
        {
            GameManager.UIManager.ConsoleClear();
            DisplayRecoveryScene();        
            switch(healingState)
            {
                case HealingState.isHeal:
                    DisplayRecoveryScene();
                    DisplayHeal(heal);
                    break;
                case HealingState.isOut:
                    DisplayRecoveryScene();
                    DisplayRoute();
                    break;

            }

        }
        public bool ActByInput(int input)
        {
            if (input == 1 && healingState == HealingState.isHeal)
            {
                tempHP = Character.Player.HP;
                Character.Player.TakeHeal(30);
                heal = Character.Player.HP - tempHP;
                return true;
            }
            else if (input == 0 && healingState == HealingState.isHeal)
            {
                healingState = HealingState.isOut;
                return true;
            }
            else if (healingState == HealingState.isOut)
            {
                GameManager.SceneManager.ChangeScene((Define.GameEnum.eSceneType)input);
                return true;
            }
            return false;
        }

        private void DisplayRecoveryScene()
        {
            GameManager.UIManager.ConsoleClear();
            string message = "회복\n아이템을 사용해 체력을 회복할 수 있습니다.\r\n";

            MessageToUIManager(message);
        }
        
        private void DisplayHeal(int heal)
        {
            string message = "";
            int count = 0;
            foreach (Item item in Character.Player.Inven.Items)
            {
                if (item.Type == Define.GameEnum.eItemType.Potion)
                {
                    count++;
                }                
            }

            if (heal > 0)
                message += $"체력을 +{heal} 회복하였습니다.\r\n";
            else message += "이미 체력이 가득 찼습니다.\r\n";
            message += $"소지개수 : {count}\n\n체력: {Character.Player.HP}/{Character.Player.MaxHP}\n\n\n\n\n1. 회복\r\n0. 나가기";
            MessageToUIManager(message);
        }

        private void DisplayRoute()
        {
            Define.GameEnum.eSceneType[] routeList = GameManager.SceneManager.GetEnableScene();
            string message = $"소지개수 : [개수]\n\n체력: {Character.Player.HP}/{Character.Player.MaxHP}\n\n\n\n\n";
            int index = 0;
            foreach (var route in routeList)
            {
                message += $"{index}. " + routeList[index] + "\t";
                index++;
            }
            message += "\r\n" +
                       "원하는 행동을 입력해주세요.\r\n" +
                       ">> ";
            MessageToUIManager(message);

        }


        private void MessageToUIManager(string message)
        {
            MessageToUI messageToUi = new MessageToUI(
                Define.GameEnum.eSceneType.Recovery,
                new string[] { message }
            );

            GameManager.UIManager.PutToOutQueue(messageToUi);
            GameManager.UIManager.DisplayUpdate();
            GameManager.UIManager.ClearMessageQueue();
        }

    }
}

```
