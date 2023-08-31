<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/a458479d-5fa7-4b28-9552-fb14ff410312)"/>   

# 내일배움캠프 16일차    
코드카타, 팀프로젝트 개발, 4주차 강의 복습   

## 추상 클래스 vs 인터페이스  
비슷한 동작을 수행하는데, 각각의 클래스에서 세부 사항은 다르게 구현하고 싶을 때는 추상 클래스를 사용하면 된다. 하지만 C#은 다이아몬드 문제로 인해 다중상속을 지원하지 않으므로, 여러 클래스를 상속받아서 메소드를 구현하고 싶어도 할 수가 없다. 이럴 때 유용하게 사용 가능한 것이 바로 인터페이스이다. 인터페이스는 다중상속이 가능하고, 인터페이스에서 선언된 메서드는 
상속받은 클래스들 안에서 구현되어야 한다. 이번 팀 프로젝트를 진행하면서, 각 씬에서 공통적으로 MainLoop()와 ActByInput()을 사용하므로 이를 IScene 인터페이스에 선언하여, 각 씬 클래스가 상속받도록 했다.  
```cs
//IScene 인터페이스
namespace TextRPGing.Define.Interface
{
    public interface IScene
    {
        public void MainLoop();
        public bool ActByInput(int input);
    }
}

//Recovery Scene 클래스
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

## 열거형 [Enums]  
열거형을 사용하면 의미 있는 이름으로 상수를 명명할 수 있다. 이로써 코드의 가독성을 높이고 잘못된 상수가 사용되는 경우도 방지할 수 있다. 또한 열거형은 스위치문과 함께 사용하기에 매우 유용하다. 위의 Recovery Scene 스크립트에서도 열거형과 스위치문을 함께 사용했다.  

  ## 예외 처리  
예외란 말 그대로 예기치 않은 상황이다. 예외는 프로그램의 정상적인 진행을 방해하고 오류를 야기할 수 있다. 때문에 예외 상황에 대비하여 예외 처리를 해 둘 필요가 있다. 예외 처리는 예외 상황에 대비하여 프로그램의 안전성을 향상시킬 수 있고, 디버깅을 용이하게 해준다.  
