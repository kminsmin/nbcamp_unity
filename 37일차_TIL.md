# 내일배움캠프 37일차 TIL  
오늘은 팀 프로젝트 제출일이었다. 팀 프로젝트 개발을 마무리하고, 발표 후 회고를 진행했다.

## 팀 프로젝트 회고  
기획까지는 좋았는데, 아직 실력이 충분히 뒷받침이 되지 않아서 프로젝트르 제대로 마무리하지 못한 것 같다. 우리 조처럼 로그라이크 게임을 개발한 다른 팀들을 보니 BSP 알고리즘을 활용하여 맵을 생성한 팀들이 있었다. 나의 경우 맵을 랜덤으로 생성하는 기능을 구현하기 위해 맵 프리팹을 여러개 만들어 두고, 맵들의 배열의 인덱스를 랜덤으로 섞어서 순서대로 맵을 활성화하는 방식으로 구현했었는데, 맵 생성 알고리즘에 대한 공부가 필요하다고 느꼈다. 몬스터 행동 AI 구현의 경우에도, 길찾기 알고리즘 등을 전혀 도입하지 않아서, 이 부분에 대한 공부도 필요한 것 같다.  
```cs
//맵 생성 시스템
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using static Define;

public class MapManager : MonoBehaviour
{
    public static MapManager Instance;
    [SerializeField] private GameObject _player;
    [SerializeField] private GameObject[] _maps;
    [SerializeField] private GameObject[] _sideMaps;
    [SerializeField] private GameObject _startMap;
    [SerializeField] private GameObject _bossMap;
    [SerializeField] private GameObject _mapTransitionImage;
    private PlayerController _playerController;
    private Transform _monsters;
    private GameObject _currentMap = null;
    private MapTransition _mapTransition;
    private int _mapIndex = 0;
    public bool isFinalMain = false;
    private Vector3 _previousPosition = Vector3.zero;

    private void Awake()
    {
        Instance = this;
    }
    private void Start()
    {
        _startMap.SetActive(true);
        _player.transform.localPosition = Vector3.zero;
        _playerController = _player.GetComponent<PlayerController>();
        _mapTransition = _mapTransitionImage.GetComponent<MapTransition>();
        _currentMap = _startMap;
        _maps = _maps.OrderBy(x => UnityEngine.Random.Range(-6, 6)).ToArray();
        _sideMaps = _sideMaps.OrderBy(x => UnityEngine.Random.Range(-6, 6)).ToArray();

    }

    public void LoadNextMap(PortalType type) 
    {
        try
        {
            switch (type)
            {
                case PortalType.MAIN:
                    _currentMap?.SetActive(false);
                    _mapTransition.LoadingMap();
                    _maps[_mapIndex].SetActive(true);
                    _mapTransition.LoadedMap();
                    _currentMap = _maps[_mapIndex];
                    _previousPosition = _player.transform.localPosition;
                    _player.transform.localPosition = Vector3.zero;
                    _mapIndex++;
                    if (_mapIndex == _maps.Length)
                        isFinalMain = true;
                    break;
                case PortalType.PREVIOUS:
                    if ((_mapIndex - 2) < 0)
                    {
                        //던전 클리어 혹은 탈출장치 발견 전까지 나갈 수 없다는 팝업 표시하면 좋을 것 같습니다.
                        Debug.Log("들어올 땐 마음대로지만 나갈 땐 아니란다...");
                        break;
                    }
                    _currentMap?.SetActive(false);
                    _mapTransition.LoadingMap();
                    _maps[_mapIndex - 2].SetActive(true);
                    _player.transform.localPosition = _previousPosition;
                    _mapTransition.LoadedMap();
                    _currentMap = _maps[_mapIndex - 2];
                    _mapIndex--;
                    break;
                case PortalType.SIDE:
                    _currentMap?.SetActive(false);
                    _mapTransition.LoadingMap();
                    if (_mapIndex == _maps.Length)
                        _sideMaps[_mapIndex - 1].SetActive(true);
                    else
                    {
                        _sideMaps[_mapIndex].SetActive(true);
                        _mapIndex++;
                    }
                    _mapTransition.LoadedMap();
                    _currentMap = _sideMaps[_mapIndex-1];
                    _previousPosition = _player.transform.localPosition;
                    _player.transform.localPosition = Vector3.zero;
                    break;
                case PortalType.BOSS:
                    _currentMap?.SetActive(false);
                    _mapTransition.LoadingMap();
                    _bossMap.SetActive(true);
                    _mapTransition.LoadedMap();
                    _currentMap = _bossMap;
                    _previousPosition = _player.transform.localPosition;
                    _player.transform.localPosition = Vector3.zero;
                    break;
            }
        }
        catch (Exception ex)
        {
            _currentMap?.SetActive(false);
            _mapTransition.LoadingMap();
            _bossMap.SetActive(true);
            _mapTransition.LoadedMap();
            _currentMap = _bossMap;
            _previousPosition = _player.transform.localPosition;
            _player.transform.localPosition = Vector3.zero;
        }
        finally
        {
            _monsters = _currentMap.transform.GetChild(0);
            for(int i = 0; i < _monsters.childCount; i++)
            {
                _monsters.GetChild(i).gameObject.GetComponent<MonsterController>().StartMonsterAction();
            }
        }
       
    }

    public void ReadyNextMap(Define.PortalType type)
    {
        _playerController.isNearPortal = true;
        _playerController.portalType = type;
    }

    public void PortalAway()
    {
        _playerController.isNearPortal = false;
    }

    public void MovedNextMap()
    {
        _playerController.isNearPortal = false;
    }

    public int GetMapIndex()
    {
        return _mapIndex;
    }
}

//몬스터 행동 AI
using DG.Tweening;
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using static Define;



public class MonsterController : MonoBehaviour
{
    public event Action<Vector3> MonsterDeathEvent;
    public event Action<Collider2D> PlayerDetectEvent;
    public Vector3 playerPosition = Vector3.zero;
    public bool isDead = false;
    public bool isAttack = false;
    [SerializeField] public Animator animator;
    [SerializeField][Range(0.5f, 2.0f)] protected float MoveSpeed;
    [SerializeField][Range(1, 4)] protected float MoveRange;
    protected GameObject _player;
    protected const string _isMove = "isMove";
    protected const string _isJump = "isJump";
    protected const string _isAttack = "isAttack";
    protected const string _isStun = "isStun";
    protected const string _isDead = "Death";
    protected int _behaviourSelector = 0;
    protected BehaviourType behaviourType;
    protected bool playerDetected = false;
    protected int positionIndicator = 0;

    private void Awake()
    {
        _player = GameObject.FindGameObjectWithTag("Player");
        MonsterDeathEvent += ItemManager.Instance.CreateItem;
        MonsterDeathEvent += OnMonsterDeath;
    }

    public void StartMonsterAction()
    {
        StartCoroutine(MonsterBehaviour());
    }

    public void CallDeathEvent()
    {
        MonsterDeathEvent?.Invoke(transform.position);
    }

    public void CallDetectEvent(Collider2D player)
    {
        PlayerDetectEvent?.Invoke(player);
    }
    protected virtual IEnumerator MonsterBehaviour()
    {
        while(!isDead)
        {
            animator.SetBool(_isMove, false);
            animator.SetBool(_isAttack, false);
            isAttack = false;
            float moveRate = 1.0f / MoveSpeed;
            _behaviourSelector = UnityEngine.Random.Range(0, 10);
            /*
             * 일정 확률로 행동 선택(IDLE, LEFT, RIGHT)
             * LEFT 으로 특정 횟수 이동했을 시 LEFT 선택 안됨. 중간값이 될 때까찌 IDLE 또는 RIGHT 선택
             * 플레이어 인식 시 플레이어 방향으로 이동 또는 공격
             */
            if (!playerDetected)
            {
                if (_behaviourSelector < 4 && positionIndicator > -MoveRange)
                {
                    behaviourType = BehaviourType.LEFT;
                    positionIndicator--;
                }
                else if (_behaviourSelector < 7 && positionIndicator < MoveRange)
                {
                    behaviourType = BehaviourType.RIGHT;
                    positionIndicator++;
                }
                else
                {
                    behaviourType = BehaviourType.IDLE;
                }
            }
            else
            {
                behaviourType = (_behaviourSelector < 5) ? BehaviourType.MOVE : BehaviourType.ATTACK;
            }

            switch (behaviourType)
            {
                case BehaviourType.LEFT:
                    transform.localScale = new Vector3(-1, 1, 1);
                    animator.SetBool(_isMove, true);
                    transform.DOMoveX(positionIndicator, moveRate);
                    break;
                case BehaviourType.RIGHT:
                    transform.localScale = new Vector3(1, 1, 1);
                    animator.SetBool(_isMove, true);
                    transform.DOMoveX(positionIndicator, moveRate);
                    break;
                case BehaviourType.IDLE:
                    animator.SetBool(_isMove, false);
                    break;
                case BehaviourType.MOVE:
                    animator.SetBool(_isMove, true);
                    MoveTowardsPlayer(moveRate);
                    break;
                case BehaviourType.ATTACK:
                    animator.SetBool(_isAttack, true);
                    isAttack = true;
                    MoveTowardsPlayer(moveRate);
                    break;
            }

            yield return new WaitForSecondsRealtime(moveRate);
        }
    }

    protected virtual void OnMonsterDeath(Vector3 position)
    {
        isDead = true;
        animator.SetTrigger(_isDead);
        Destroy(gameObject);
    }

    protected void MoveTowardsPlayer(float moveRate)
    {
        if (playerPosition.x > transform.localPosition.x)
        {
            transform.localScale = new Vector3(1, 1, 1);
            transform.DOMoveX(playerPosition.x, moveRate * 1.5f);
        }
        else
        {
            transform.localScale = new Vector3(-1, 1, 1);
            transform.DOMoveX(playerPosition.x, moveRate * 1.5f);
        }
    }

    public void DetectPlayer()
    {
        playerDetected = true;
    }
    
    public void AwayPlayer()
    {
        playerDetected = false;
    }
}

```
