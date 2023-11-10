# 내일배움캠프 63일차 TIL    
오늘도 오전에 기술면접 예상질문을 풀어보고 팀 프로젝트 개발을 진행했다.     

## 팀 프로젝트 개발 - 금주 계획 회고  
이번주는 대부분 MVP 구현을 위해 시간을 많이 할애했다. 새로 추가한 기능은, 이벤트, 옵저버 패턴, 람다를 이용해서 플레이어의 움직임과 애니메이션 변경을 인풋 시스템을 사용하여 특정 인풋을 받았을 때 콜백을 받아서 움직임과 애니메이션 변경이 일어나도록 구현했다. 콜백을 받아서 실행되는 메서드는 해당 부분밖에 쓰이지 않아서 람다식으로 구성하여 가독성을 높였다.  
```cs
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.InputSystem;
using static Enums;

[RequireComponent(typeof(Rigidbody2D), typeof(CapsuleCollider2D))]
public class PlayerController : MonoBehaviour
{
    #region Components
    public Player Player { get; private set; }
    public PlayerInputActions PlayerInput;
    public PlayerInputActions.PlayerControlActions PlayerControlActions {  get; private set; }
    public Rigidbody2D Rigidbody { get; private set; }
    public CapsuleCollider2D Collider { get; private set; }
    public Weapon Weapon { get; private set; }
    [SerializeField] private Transform _playerSprite;
    [SerializeField] private Transform _weaponSprite;
    [SerializeField] private Animator _animator;
    #endregion

    #region Animation Parameters
    private const string _jumpTrigger = "jump";
    private const string _hitTrigger = "hit";
    private const string _fallTrigger = "fall";
    private const string _isMove = "isMove";
    private const string _isDuck = "isDuck";
    private const string _shootTrigger = "shoot";
    private const string _isGround = "isGround";
    #endregion

    #region Variables
    private Vector2 _moveInput;

    [SerializeField] private float _moveAccel;
    [SerializeField] private float _maxSpeed;
    [SerializeField] private float _jumpForce;
    [SerializeField] private float _knockBack;
    [SerializeField] private LayerMask _groundLayer;
    [SerializeField] private LayerMask _resetLayer;
    private Vector3 _leftVector = new Vector3(-1, 1, 1);
    private float _initialGravity = 0f;
    private bool _isDownJumping;

    private int _maxJumpCount = 1;
    private int _jumpCount = 0;

    private WeaponType _weaponType;
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
        if (DataManager.Instance.Player != null)
        {
            Player = DataManager.Instance.Player;
            _maxJumpCount = Player.MaxJump;
        }
        _jumpCount = _maxJumpCount;
        _initialGravity = Rigidbody.gravityScale;
        
        PlayerControlActions.Movement.started += (context) =>
        {
            _moveInput = context.ReadValue<Vector2>();
            if(!Player.IsSwinging)
            {
                _animator.SetBool(_isMove, true);
                if(_moveInput.y == 0)
                {
                    if (_moveInput.x < 0)
                    {
                        _playerSprite.localScale = _leftVector;
                        Player.IsRight = false;
                    }
                    else
                    {
                        _playerSprite.localScale = Vector3.one;
                        Player.IsRight = true;
                    }
                }
                else if (_moveInput.y < 0) _animator.SetBool(_isDuck, true);
            }
            
        };
        PlayerControlActions.Movement.canceled += _ =>
        {
            _moveInput = Vector2.zero;
            _animator.SetBool(_isMove, false);
            _animator.SetBool(_isDuck, false);
            //Rigidbody.velocity = _moveInput;
        };
        PlayerControlActions.Shoot.started += _ =>
        {
            _animator.SetTrigger(_shootTrigger);
            Weapon.PerformCatch();
        };
        PlayerControlActions.Skill.started += _ =>
        {
            _animator.SetTrigger(_shootTrigger);
            Weapon.PerformSpecial();
        };
        PlayerControlActions.Jump.started += _ =>
        {
            _animator.SetBool(_isGround, false);
            Jump();
        };

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
            if(Math.Abs(Rigidbody.velocity.x) < _maxSpeed)
                Rigidbody.AddForce(_moveInput.normalized.x * Vector2.right * _moveAccel, ForceMode2D.Force);
        }

        if(Rigidbody.velocity.magnitude > _maxSpeed * 2.5f)
        {
            Rigidbody.gravityScale = 0f;
        }
    }

    private void Jump()
    {
        if (Rigidbody != null && _jumpCount > 0)
        {
            Rigidbody.gravityScale = _initialGravity;
            Rigidbody.velocity = Vector2.zero;
            if (_moveInput.y >= 0)
            {
                _animator.SetTrigger(_jumpTrigger);
                StartCoroutine(CheckFall(new WaitForSeconds(0.1f)));
                Rigidbody.AddForce(Vector2.up * _jumpForce, ForceMode2D.Impulse);
            }
            else
            {
                _animator.SetTrigger(_fallTrigger);
                _isDownJumping = true;
                this.Invoke(() => _isDownJumping = false, 0.3f);
                //Rigidbody.AddForce(Vector2.down * _jumpForce, ForceMode2D.Impulse); 공중 아래공격 구현시 사용
            }

            _jumpCount--;

        }
    }

    private IEnumerator CheckFall(WaitForSeconds interval)
    {
        while(_jumpCount < _maxJumpCount)
        {
            if(Rigidbody.velocity.y < 0)
            {
                _animator.SetTrigger(_fallTrigger);
                break;
            }
            yield return interval;
        }
    }

    public void ApplyKnockBack(bool isRight)
    {
        _animator.SetTrigger(_hitTrigger);
        var dir = (isRight) ? Vector2.right : Vector2.left;
        Rigidbody.AddForce(dir * _knockBack, ForceMode2D.Impulse);        
    }

    public void ChangeWeapon()
    {
        _weaponType = (Player != null) ? Player.WeaponType : WeaponType.BasicNet;
        //_weaponType = GetComponent<PlayerStatus>()._weaponType; //테스트용 임시 코드
        switch (_weaponType)
        {
            case WeaponType.BasicNet:
                Weapon = gameObject.AddComponent<BasicNet>();
                break;
            case WeaponType.CloseNet:
                Weapon = gameObject.AddComponent<CloseNet>();
                break;
            case WeaponType.Harpoon :
                Weapon = gameObject.AddComponent<HarpoonThrow>();
                break;

        }
    }

    private void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
        {
            _animator.SetBool(_isGround, true);
            _jumpCount = _maxJumpCount;
            Rigidbody.gravityScale = _initialGravity;
        }
    }

    private IEnumerator CheckJump(WaitForSeconds interval)
    {
        while (true)
        {
            if(Rigidbody != null && Rigidbody.velocity.y > 0 || _isDownJumping)
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
