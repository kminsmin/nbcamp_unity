# 내일배움캠프 74일차 TIL  
오늘도 오전에 기술면접 예상질문을 풀어보고 팀 프로젝트 개발을 진행했다.    

## 오브젝트 풀링(Object Pooling)이 무엇이며, 어떻게 구현하는지 설명해주세요.  
유니티에서 오브젝트를 생성하는 작업과 파괴하는 작업은 많은 리소스를 필요로 하는 무거운 작업이다. 때문에 이러한 작업을 최소화하기 위해, 오브젝트를 미리 여러개 만들어두고 필요할 때마다 꺼내서 사용하고 사용하지 않을 때 다시 넣어둠으로써 메모리를 절약하는 디자인 패턴이 바로 오브젝트 풀링이다. 유니티에서 오브젝트 풀링을 사용하는 방법은 크게 두 가지이다.  
1. 유니티에서 제공하는 오브젝트 풀 기능 사용
2. 직접 구현

유니티에서 제공하는 오브젝트 풀을 사용하는 방법은 매우 간단하다. IObjectPool 필드를 선언하고, 필수구현부를 구현해주면 된다. 이후 필요 여부에 따라 collectionCheck, defaultCapacity, maxSize 등을 설정해준다. 이후 오브젝트가 필요할 때 .Get() 메서드로 가져오고, 다시 넣어둘 때는 .Release()메서드를 사용하면 된다.  
아래는 현재 진행하고 있는 프로젝트에서 실제로 사용한 예시이다. 무기를 전략패턴으로 구현한 부분들 중 일부분인데, 원거리 무기이기에 총알을 오브젝트 풀로 구현했다. 총알의 경우 프로퍼티를 이용해서 무기 측에 있는 오브젝트 풀을 참조하여 OnDisable에서 스스로 Release할 수 있도록 했다.  
```cs
//무기 측 스크립트. 오브젝트 풀을 구현해두고, Catch()가 실행될 때마다 풀에서 오브젝트를 가져온다.
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AddressableAssets;
using UnityEngine.Pool;

public class RangedCatch : ICatchBehaviour
{
    private AssetReferenceGameObject _projectilePrefabReference;
    private Projectile _projectilePrefab;
    public Queue<GameObject> Projectiles;
    private IObjectPool<Projectile> _projectilePool;

    public async void Catch()
    {
        if(_projectilePrefab == null)
        {
            _projectilePrefabReference = GameObject.FindGameObjectWithTag("Player").GetComponent<PlayerController>().ProjectilePrefab;
            _projectilePrefab = await ObjectManager.Instance.UsePool<Projectile>(_projectilePrefabReference);
            _projectilePrefab.ProjectilePool = _projectilePool;
        }

        if (_projectilePool == null)
        {
            _projectilePool = new ObjectPool<Projectile>(CreateProjectile, OnGetFromPool, OnReleaseToPool, OnDestroyPooledProjectile, true, 15, 50);
        }

        Projectile projectile = _projectilePool.Get();

    }

    private Projectile CreateProjectile()
    {
        Projectile projectileInstance = GameObject.Instantiate(_projectilePrefab);
        projectileInstance.ProjectilePool = _projectilePool;

        return projectileInstance;
    }

    private void OnGetFromPool(Projectile pooledProjectile)
    {
        pooledProjectile.gameObject.SetActive(true);
    }

    private void OnReleaseToPool(Projectile pooledProjectile)
    {
        pooledProjectile.gameObject.SetActive(false);
    }

    private void OnDestroyPooledProjectile(Projectile pooledProjectile)
    {
        GameObject.Destroy(pooledProjectile.gameObject);
    }

    
}

//총알 스크립트. 무기에 있는 오브젝트 풀을 참조하여, OnDisable에서 스스로 Release한다.
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Pool;

public class Projectile : MonoBehaviour
{
    protected Transform _playerTransform;
    protected Camera _camera;
    protected Rigidbody2D _rigidbody;
    protected Player _player;
    protected SpriteRenderer _projectileSprite;

    protected IObjectPool<Projectile> _projectilePool;

    public IObjectPool<Projectile> ProjectilePool { set => _projectilePool = value;  }

    [SerializeField] protected float _flyForce;


    protected virtual void Awake()
    {
        _playerTransform = GameObject.FindGameObjectWithTag("Player").transform.GetChild(0).transform.GetChild(0).transform.GetChild(0).transform;
        _camera = Camera.main;
        _rigidbody = GetComponent<Rigidbody2D>();
        _projectileSprite = GetComponent<SpriteRenderer>();
    }

    protected virtual void Start()
    {
        _player = DataManager.Instance.Player;
    }

    protected virtual void OnEnable()
    {
        FlipSprite();
        transform.position = _playerTransform.position;
        Vector3 point = _camera.ScreenToWorldPoint(new Vector3(Input.mousePosition.x, Input.mousePosition.y, - _camera.transform.position.z));
        Vector3 flyVector = (point.y - transform.position.y < 1.5) ? point - transform.position : new Vector3(point.x - transform.position.x, 0, point.z - transform.position.z);
        Debug.Log(flyVector.y);
        _rigidbody.AddForce(flyVector.normalized *_flyForce, ForceMode2D.Impulse);
        Invoke(nameof(Disappear), 1f);
    }

    protected virtual void OnTriggerEnter2D(Collider2D collision)
    {
        if(collision != null && collision.gameObject.CompareTag("Monster"))
        {
            collision.GetComponent<Monster>().BeAttacked(_player.Attack);
            gameObject.SetActive(false);
        }
        //else if (collision.gameObject.CompareTag("Ground")) Disappear();
    }

    protected void FlipSprite()
    {
        _player = DataManager.Instance.Player;
        _projectileSprite.flipX = (Input.mousePosition.x > 960) ? false : true;
    }

    protected virtual void Disappear()
    {
        gameObject.SetActive(false);
    }

    private void OnDisable()
    {
        CancelInvoke();
        _rigidbody.velocity = Vector3.zero;

        _projectilePool?.Release(this);
    }
}


```

## Object pool을 사용하는 이유는 무엇인가요?    
유니티에서 Object Pooling은 게임이나 애플리케이션의 성능을 향상시키고 메모리 관리를 개선하는 데 사용된다. Object Pooling은 주로 게임에서 많은 수의 오브젝트를 동시에 처리할 때 사용되며, 특히 메모리와 성능을 개선하고자 할 때 유용하게 활용된다. Object Pooling은 다음과 같은 이유로 사용된다.  


