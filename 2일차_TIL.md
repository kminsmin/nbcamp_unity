# 내일배움캠프 2일차

게임개발 종합반 3주차 강의를 수강했다.  

StartScene을 만들고 Unity의 SceneManager를 통해 MainScene으로 이동
```cs
using UnityEngine.SceneManagement;

public void GameStart()
{
    SceneManager.LoadScene("MainScene");
}
```
위와 같은 스크립트를 버튼의 OnClick에 등록해주면 된다.

마우스의 x좌표를 가져오고, 자신의 y 좌표는 유지하여, 마우스의 움직임에 따라 x좌표를 변화한다.
```cs
void Update()
{
    Vector3 mousePos = Camera.main.ScreenToWorldPoint(Input.mousePosition);
    transform.position = new Vector3(mousePos.x, transform.position.y, 0);
}
```
## 애니메이션 설정하는 과정
1. Animations 폴더 생성
2. Animation 만들기
3. Loop time 체크
4. 오브젝트의 부착
5. 애니매이션 더블 클릭, 녹화 버튼 누르고 원하는 시간대에 원하는 스프라이트 넣기

## 애니메이션 전환
1. Animation Controller에서 전환하고자 하는 애니메이션 우클릭, Make Transition 추가
2. 해당 애니메이션 좌클릭, Has Exit Time 체크 해제
3. Parameter 추가 (ex. bool , isOnAir)
4. 전환하기 전 애니메이션 클릭, Condition 추가, 아까 만든 parameter 추가 (ex. isOnAir true)

## 미니프로젝트
올바른 카드 매치 시 팀원의 이름 출력, 다른 카드 매치 시 "실패" 메시지를 출력하는 기능을 구현했다.
  ```cs
    public void isMatched()
    {
        string firstCardImage = firstCard.transform
            .Find("Front")
            .GetComponent<SpriteRenderer>()
            .sprite.name;

        string secondCardImage = secondCard.transform
            .Find("Front")
            .GetComponent<SpriteRenderer>()
            .sprite.name;

        if (firstCardImage == secondCardImage)
        {
            firstCard.GetComponent<Card>().DestroyCard();
            secondCard.GetComponent<Card>().DestroyCard();

            int cardsLeft = GameObject.Find("Cards").transform.childCount;

            if (cardsLeft == 2)
            {
                Invoke("GameEnd", 1f);
            }
            else
            {
                if (firstCardImage == "ourpic0" || firstCardImage == "ourpic1")
                {
                    notificationText.SetActive(true);
                    notificationText.GetComponent<TextMeshProUGUI>().text = "배인호";
                }
                else if (firstCardImage == "ourpic2" || firstCardImage == "ourpic3")
                {
                    notificationText.SetActive(true);
                    notificationText.GetComponent<TextMeshProUGUI>().text = "이경민";
                }
                else if (firstCardImage == "ourpic4" || firstCardImage == "ourpic5")
                {
                    notificationText.SetActive(true);
                    notificationText.GetComponent<TextMeshProUGUI>().text = "장성민";
                }
                else if (firstCardImage == "ourpic6" || firstCardImage == "ourpic7")
                {
                    notificationText.SetActive(true);
                    notificationText.GetComponent<TextMeshProUGUI>().text = "염종인";
                }
            }
        }
        else
        {
            firstCard.GetComponent<Card>().CloseCard();
            secondCard.GetComponent<Card>().CloseCard();
            notificationText.SetActive(true);
            notificationText.GetComponent<TextMeshProUGUI>().text = "실패!";
        }

        firstCard = null;
        secondCard = null;
    }
```
이름을 출력하는 부분은 처음 누른 카드와 나중에 누른 카드가 같은 카드일 때만 작동하도록 하는 if 문 안에 넣었고,  
둘이 다를 경우 실패 텍스트를 출력하도록 했다. 평소에 SetActive가 false였다가 카드가 눌렸을 때 true로 바뀌도록 했는데,  
내일은 시간이 지나면 다시 false로 바꾸는 메소드를 추가해야겠다.

 #내일배움캠프
 #스파르타내일배움캠프
 #스파르타내일배움캠프TIL


