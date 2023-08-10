# 내일배움캠프 4일차
미니프로젝트 추가 기능 구현(feat. 태초마을 파티)
## 버그 수정 및 최고점수 구현
타이머 시간이 게임 종료 시 음수로 출력되는 버그가 있어서 수정했고, 최고점수를 출력하는 기능을 구현했다.
```cs
inGamePanel.SetActive(false);
resultPanel.SetActive(true);
time = 0.00f;
if (PlayerPrefs.HasKey("bestScore") == false)
            {
                PlayerPrefs.SetFloat("bestScore", score);
            }
            else
            {
                if (PlayerPrefs.GetFloat("bestScore") < score)
                {
                    PlayerPrefs.SetFloat("bestScore", score);
                }
            }
```
### 태초마을...
자잘한 버그 수정 및 기능 구현을 한꺼번에 하려다 merge conflict가 발생했다. 폰트가 깨지는 것은 물론 스크립트 위치도 뒤죽박죽이고, MainScene은 아예 새까맣게 변하고 no cameras rendering
이라는 메시지만 써져 있었다. 다른 conflict들은 잘 해결했지만, 새까맣게 변한 MainScene은 씬 파일을 직접 텍스트 에디터로 고치려고 시도해 보았으나, 실패했다. 메인씬을 켤 수 있게 만드는 데는 성공했으나,
다른 분히 구현한 기능이 온전하지 못해서 결국 다 밀어버리고 새로 클론했다. 결국 내 노력은 태초마을로...  
https://github.com/anacat/unity-mergetool => 팀원분이 알려주신 Unity Smart Merge Tool. 사용법을 알아보아야겠다.

### 버그 수정, 타이틀 화면으로 돌아가는 버튼 추가
또 태초마을에 가긴 싫어서 이전에 한꺼번에 푸시하려다 난리났던 것들을 하나하나씩 조심스럽게 푸시했다. 다행히 큰 문제는 발생하지 않았다.

## 난이도 선택 씬, 어려운 난이도 추가
난이도를 선택하는 씬을 추가했다. EasyMode 클리어 시 PlayerPrefs.SetInt로 숫자를 저장하고, 다시 난이도 선택 씬으로 가면 PlayerPrefs.HasKey가 true일 때 어려운 난이도 버튼이 활성화된다.(사실 색만 변한다.
외형을 바꿀 생각만 하고 정작 기능을 비활성화했다가 다시 켤 생각은 안했다...). 버튼들은 눌렸을 때 각각 다른 Int값을 PlayerPrefs로 저장하고 이 숫자가 총 제한시간에 영향을 줘서 난이도를 조절하는 방식이다.

```cs
//EasyMode Button
public void ChooseStage()
    {
        PlayerPrefs.SetInt("diff", 1); //난이도 상수
        SceneManager.LoadScene("MainStage");
    }

//GameManager
 private void Awake()
    {
        Time.timeScale = 1.0f;
        gameManager = this;
        time /= PlayerPrefs.GetInt("diff"); // 난이도 조절
    }
if (cardsLeft == 2) //게임클리어 조건
            {
                Invoke("GameOver", 0.0f);
                PlayerPrefs.SetInt("clear", 0); // 게임 클리어시 상수 부여
            }

//HardMode Button
    public Button button;
    void Start()
    {
        if (PlayerPrefs.HasKey("clear") == true) // EasyMode 클리어 한 상태라면 색 변화
        {
            ColorBlock colorBlock = button.colors;
            colorBlock.normalColor = Color.white;
            button.colors = colorBlock;
        }
    }
     public void HardGame()
    {
        PlayerPrefs.SetInt("diff", 2); // 난이도 상수
        SceneManager.LoadScene("MainStage");
    }
```

## 난이도에 따른 최고점수 저장 방식 변경
기존 방식대로 하면 Easy와 Hard 난이도의 Best Score가 동일하게 사용되어서, 각각 따로 저장하는 방식으로 변경했다. 난이도 상수를 통해 구분했다.
``` cs
 // 최고점수 업데이트
        if (PlayerPrefs.GetInt("diff")==1) // Easy Mode 최고점수 업데이트
        {
            if (PlayerPrefs.HasKey("bestScore") == false)
            {
                PlayerPrefs.SetFloat("bestScore", score);
            }
            else
            {
                if (PlayerPrefs.GetFloat("bestScore") < score)
                {
                    PlayerPrefs.SetFloat("bestScore", score);
                }
            }
            bestScoreText.text = PlayerPrefs.GetFloat("bestScore").ToString();
        }
        if (PlayerPrefs.GetInt("diff") == 2) // Hard Mode 최고점수 업데이트
        {
            if (PlayerPrefs.HasKey("bestScore1") == false)
            {
                PlayerPrefs.SetFloat("bestScore1", score);
            }
            else
            {
                if (PlayerPrefs.GetFloat("bestScore1") < score)
                {
                    PlayerPrefs.SetFloat("bestScore1", score);
                }
            }
            bestScoreText.text = PlayerPrefs.GetFloat("bestScore1").ToString();
        }
```
뭔가 크게 어려운걸 한건 아닌데 태초마을 한번으로 멘탈 와장창 됐던 하루...
