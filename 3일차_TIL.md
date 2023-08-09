# 내일배움캠프 3일차
게임개발종합반 4강 + 5강 수강
## 나머지 연산자
%
## for 문
```cs
 void Start()
    {
        for (int i = 0; i < 16; i++)
        {
            GameObject newCard = Instantiate(card);
            newCard.transform.parent = GameObject.Find("Cards").transform;
            newCard.transform.position = new Vector3((i%4) * 1.4f - 2.1f, (i/4) * 1.4f - 3.0f, 0);
        }
    }
```
i 는 0부터 시작, 15가 될 때까지 총 16회 반복.
카드 한 장의 너비가 1.3 이므로 1.4의 간격으로 배치하면 된다.
x좌표는 i를 4로 나눈 나머지를 곱하고, y좌표는 i를 4로 나눈 몫을 곱하면
4x4 바둑판으로 배치할 수 있다.  
## 리스트 랜덤으로 섞기
```cs
using System.Linq;
    .
    .
    .
            int[] rtans = { 0, 0, 1, 1, 2, 2, 3, 3, 4, 4, 5, 5, 6, 6, 7, 7 };
            rtans = rtans.OrderBy(item => Random.Range(-1.0f, 1.0f)).ToArray();
    .
    .
    .
```
OrderBy : 출력 시퀀스를 기준에 따라 정렬하는 연산자이다.
## Parent 아래 Instantiate, Child 수 세기
```cs
        GameObject newCard = Instantiate(card);
        newCard.transform.parent = GameObject.Find("cards").transform;
    .
    .
    .
        int cardsLeft = GameObject.Find("cards").transform.childCount;

```
카드를 Instantiate 할 때, Cards라는 Empty  Game Object 아래에 넣고, 이후  Cards의 child 수를 카운트하여 게임 오버 조건 달성 여부를 확인할 수 있다.

## 스플래시 이미지 추가
Project Settings -> Player -> Splash Image

## 미니프로젝트 개발일지
기본 기능 구현은 어제 완료해서 오늘은 각자 추가기능을 한 가지씩 맡아 구현하기로 했다.
### 뒤집은 카드 색깔 변경
CloseCardInvoke안에 색을 변경하는 코드를 추가하면 간단히 카드 색 변경 기능을 구현할 수 있다.
```cs
public void CloseCardInvoke() 
    {
        cardAnim.SetBool("isOpen", false);
        transform.Find("Back").gameObject.SetActive(true);
        transform.Find("Front").gameObject.SetActive(false);
        transform.Find("Back").gameObject.GetComponent<SpriteRenderer>().color = new Color(
            150f / 255f,
            150f / 255f,
            150f / 255f,
            1
        );
    }
```

### 첫 번째 카드 선택 후 5초 이내 선택하지 않으면 카드 다시 뒤집기
아래와 같이 카드 한 장을 뒤집으면, 그것이 첫번째 카드일 때, 그리고 2번째 카드가 선택되지 않았을 때, 5초를 세고 CloseCardInvoke 메소드를 부른다. 타이머는 리셋된다.
```cs
if (timeLimit >5.0f && firstCard != null && secondCard == null) //5초 후 카드 뒤집기
        {
            firstCard.GetComponent<Card>().CloseCardInvoke();
            firstCard = null;
            timeLimit = 0.0f;
        }
```

