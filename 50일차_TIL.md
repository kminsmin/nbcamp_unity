# 내일배움캠프 50일차 TIL  
오늘부터 아침에 기술면접 대비 면접 질문들을 풀어보기로 했다.  

## 기술면접 대비 - float와 int의 표현 가능한 수의 범위가 다른 이유(부동소수점!)
int 자료형은 고정 소수점 방식으로, 32비트중 하나의 비트는 부호를 나타내고 나머지 31비트로 정수를 나타내어 -2의 31승(-2,147,483,648)부터 2의 31승에서 1을 뺀 값(2,147,483,647)까지 표현할 수 있다. 반면 float 자료형은 부동소수점 방식으로서, 32개의 비트 중 1bit는 부호 비트, 8bit는 지수비트, 그리고 나머지 23bit는 유효자리비트로 사용하여 수를 표현한다. 지수비트로 인해 int형과 같은 32비트임에도 불구하고 훨씬 더 넓은 범위의 수를 표현할 수 있다. 하지만 int는 정수형으로 정확한 수를 저장하는 방면, float는 유효숫자 7자리를 넘어가면 근사값으로밖에 저장할 수 없다.  

## 팀 프로젝트 개발 - 플레이어 조작 및 플레이어 데이터 세팅에 대한 고민  
### 플레이어 조작  
2D 사이드뷰에서 플레이어의 이동을 구현했다. 추후 조작감 개선을 위해 버튼이 눌렸을 때와 때어졌을 때의 이벤트가 발생할 때마다 다양한 처리를 할 예정이기 때문에 기존 인풋 방식이 아닌 Input System을 활용했다. 또한 플레이어가 점프 시 발판에 머리를 부딛히는 현상을 해결하기 위해, 이전 DungeonEleven 프로젝트에서는 점프할 때마다 플레이어의 콜라이더 자체를 꺼버리고 0.5초뒤 켜는 무식한 방식을 사용했었다. 하지만 이번에는 우아하게 LayerMask를 활용하여, 플레이어의 Rigidbody.velocity.y값을 읽어서 해당 값이 양수이면 플레이어의 콜라이더에 Ground Layer를 exclude 처리하고, 해당 값이 음수이면 다시 include하는 방식으로 구현했다. 예전에 사용한 무식한 방법은 시간으로 콜라이더를 켜는 것도 문제지만(이때는 rigidbody나 레이캐스트를 활용할 생각도 못했다...) 플레이어의 콜리이더를 꺼버리면 아예 맵 밖으로 나가버리는 경우도 발생했었기 때문에 문제가 있었다. 하지만 이번 방식은 점프시 뚫어야 하는 발판들과 절대 통과되어선 안되는 지형들의 레이어를 달리 설정하고 뚫고 지나가야 하는 지형들의 레이어들만 점프할 때 exclude 처리하여, 맵 밖으로 나가는 문제도 발생하지 않으며 점프 후 착지도 잘 이루어진다.  
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.InputSystem;

[RequireComponent(typeof(Rigidbody2D), typeof(CapsuleCollider2D))]
public class PlayerController : MonoBehaviour
{
    #region Components
    public PlayerInputActions PlayerInput;
    public PlayerInputActions.PlayerControlActions PlayerControlActions {  get; private set; }
    public Rigidbody2D Rigidbody { get; private set; }
    public CapsuleCollider2D Collider { get; private set; }
    #endregion

    #region Variables
    private Vector2 _moveInput;

    [SerializeField] private float _moveSpeed;
    [SerializeField] private float _jumpForce;
    [SerializeField] private LayerMask _groundLayer;
    [SerializeField] private LayerMask _resetLayer;

    public int MaxJumpCount { get; private set; } = 2;
    private int _jumpCount = 0;
    #endregion

    private void Awake()
    {
        Rigidbody = GetComponent<Rigidbody2D>();
        Collider = GetComponent<CapsuleCollider2D>();
        PlayerInput = new PlayerInputActions();
        PlayerControlActions = PlayerInput.PlayerControl;
    }
    private void Start()
    {
        _jumpCount = MaxJumpCount;

        PlayerControlActions.Movement.started += (context) =>
        {
            _moveInput = context.ReadValue<Vector2>();
            //TODO : 이동 방향에 따라 플레이어 스프라이트 좌우반전
        };
        PlayerControlActions.Movement.canceled += _ =>
        {
            _moveInput = Vector2.zero;
            //Rigidbody.velocity = _moveInput;
        };
        //PlayerControlActions.Shoot.started += _ => 
        PlayerControlActions.Jump.started += _ => Jump();
        StartCoroutine(CheckJump(new WaitForSeconds(0.05f)));
    }

    private void OnEnable()
    {
        PlayerInput.Enable();
    }

    private void OnDisable()
    {
        PlayerInput.Disable();
    }

    private void FixedUpdate()
    {
        ApplyMovement();
    }

    private void ApplyMovement()
    {
        if (Rigidbody != null && _moveInput != Vector2.zero)
        {
            Rigidbody.AddForce(_moveInput.normalized.x * Vector2.right * _moveSpeed, ForceMode2D.Force);
        }
    }

    private void Jump()
    {
        if (Rigidbody != null && _jumpCount > 0)
        {
            Rigidbody.velocity = Vector2.zero;
            Rigidbody.AddForce(Vector2.up * _jumpForce, ForceMode2D.Impulse);
            _jumpCount--;
        }
    }

    private void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
        {
            _jumpCount = MaxJumpCount;
        }
    }

    private IEnumerator CheckJump(WaitForSeconds interval)
    {
        while (true)
        {
            if(Rigidbody != null && Rigidbody.velocity.y > 0)
            {
                Collider.excludeLayers = _groundLayer;
            }
            else
            {
                Collider.excludeLayers = _resetLayer;
            }

            yield return interval;
        }
    }
}

```
### 플레이어 데이터  
이번 프로젝트에서는 플레이어의 스탯으로 강인함, 체력, 스트레스를 저장해야 한다. 모든 스탯은 UI로 표시할 예정이다. 해당 데이터들을 어떻게 저장해야 할지 고민해본 결과, 어차피 기본적으로 싱글 플레이어 게임이기 때문에 다양한 플레이어 오브젝트들을 생성할 것도 아니어서, 상속을 사용할 일이 없다고 생각해서 데이터들은 구조체로 저장하기로 했다. 또한 빌더 패턴과 유사한 방식으로 필드들을 초기화하는 메서드들을 체이닝하여 한꺼번에 초기화하는 방식으로 구현했으나, 어차피 생성 타이밍을 따로 만들 필요는 없어서 빌더 클래스를 별도로 만들지는 않고 플레이어 구조체에 한꺼번에 구현했다.  
```cs
//플레이어 구조
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public struct Player
{
    #region Player Data
    public int MaxTenacity { get; private set; }
    public int CurrentTenacity { get; private set; }
    public int MaxHP { get; private set; }
    public int CurrentHP { get; private set;}
    public int Stress { get; private set; }
    public int StressLimit { get; private set; }
    #endregion

    public Player SetTenacity(int tenacity)
    {
        MaxTenacity = tenacity;
        CurrentTenacity = tenacity;
        return this;
    }
    public Player SetHP(int hp)
    {
        MaxHP = hp;
        CurrentHP = hp;
        return this;
    }
    public Player SetStress(int stress)
    {
        StressLimit = stress;
        Stress = 0;
        return this;
    }
}




```
```cs
//구조체 필드 선언 및 초기화
public class PlayerStatus : MonoBehaviour
{
    public Player Player { get; private set; }

    [SerializeField] private int _tenacity;
    [SerializeField] private int _hp;
    [SerializeField] private int _stress;

    private void Awake()
    {
        Player = Player.SetTenacity(_tenacity).SetHP(_hp).SetHP(_stress);
    }
...
```

