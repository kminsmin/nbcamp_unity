# 내일배움캠프 79일차 TIL  
이번주는 주말동안 진행한 유저 테스트 피드백의 내용을 토대로 디버깅 및 기능 수정에 집중할 예정이다. 오늘은 스토리 컷씬이 너무 길고 지루하다는 의견을 해결하기 위해 컷씬을 짧은 여러개의 컷씬으로 쪼개는 작업을 진행했다.  

## 컷쎈 제작 - Playable Director, Timeline  
예전 팀 프로젝트에서 컷씬을 제작할 때 했던 것 처럼, PlayableDirector 컴포넌트와 Timeline을 활용하여 컷씬을 제작했다. 대신 하나의 컷씬을 하나의 타임라인으로 만드는 것이 아니라, 장면을 여러개의 타임라인으로 쪼개 만들어서, 추후 스킵 기능을 도입한다면 스킵 버튼이 눌렸을 때 다음 타임라인을 불러와서 재생할 수 있는 구조로 설계했다. 현재 타임라인 재생이 끝나면 배열에 있는 다음 타임라인을 재생하고, 마지막 인덱스라면 현재 재생하고 있는 컷씬의 종류에 따라 이동해야할 씬을 로드한다.  

```cs
using UnityEngine;
using UnityEngine.Playables;
using UnityEngine.Timeline;

public class Director : MonoBehaviour
{
    private PlayableDirector _director;
    [SerializeField] private TimelineAsset[] _timelines;
    [SerializeField] private bool _isEndScene;
    [SerializeField] private bool _isBoss;

    private int _index = 0;

    private void Awake()
    {
        Time.timeScale = 1.0f;
        _director = GetComponent<PlayableDirector>();
        LoadNextCut(_director);
    }

    private void Start()
    {
        _director.stopped += context => LoadNextCut(context);
    }

    private void LoadNextCut(PlayableDirector director)
    {
        if(_index < _timelines.Length)
        {
            director.playableAsset = _timelines[_index];
            _index++;
        }
        else if (_isBoss) LoadingSceneController.LoadScene("BossScene");
        else if(_isEndScene) LoadingSceneController.LoadScene("IntroScene");
        else LoadingSceneController.LoadScene("MainScene");
        director.Play();
    }
}

```

컷씬에서 재생해야 하는 대사들의 경우, 스크립터블 오브젝트를 통해 관리하도록 했고, DialogueManager를 만들어서 언어마다 컷씬들에서 사용될 SO의 배열을 가지고 있도록 했고, 컷씬의 인덱스와 현재 언어 설정에 따라 알맞은 대사 SO에 있는 대사 리스트를 로드하여 출력하도록 했다. 대사 출력은 DOTween을 통해 한 글자씩 출력되는 효과를 연출했다.  
```cs
using System.Collections.Generic;
using UnityEngine;
using TMPro;
using DG.Tweening;

public class DialogueManager : Singleton<DialogueManager>
{
    [SerializeField] private DialogueSO[] _koDialogueSO;
    [SerializeField] private DialogueSO[] _engDialogueSO;
    [SerializeField] private float _textTypeDuration = 1.0f;
    private TextMeshProUGUI _dialogueText;

    private List<string> _dialogueList;
    private int _dialogueIndex;
    private int _currentSceneIndex;

    private const string key = "Dialogue";
    private const string langKey = "en-US";


    public void LoadNextDialogue(int sceneIndex)
    {
        if ((_currentSceneIndex != sceneIndex) || _dialogueText == null)
        {
            _dialogueText = GameObject.FindGameObjectWithTag(key).GetComponent<TextMeshProUGUI>();
            _currentSceneIndex = sceneIndex;
            _dialogueIndex = 0;
        }

        _dialogueList = GlobalSettings.CurrentLocale != langKey? _koDialogueSO[sceneIndex].dialogue : _engDialogueSO[sceneIndex].dialogue;
        _dialogueText.text = _dialogueList[_dialogueIndex];
        ShowText(_dialogueText, _textTypeDuration);


        if (_dialogueIndex < _dialogueList.Count - 1)
            _dialogueIndex++;
        else _dialogueIndex = 0;
    }

    private static void ShowText(TextMeshProUGUI text, float duration)
    {
        text.maxVisibleCharacters = 0;
        DOTween.To(x => text.maxVisibleCharacters = (int)x, 0f, text.text.Length, duration);
    }

}
```
