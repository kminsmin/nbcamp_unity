# 내일배움캠프 51일차 TIL  
오늘도 오전에 기술면집 질문들을 풀어보고, 이후 팀 프로젝트 개발을 진행했다.  

## ref와 out의 사용 시 차이  
메서드를 호출할 때, 기본 자료형인 매개변수는 값 형식으로 전달되기 때문에 메서드 내부에서 변화가 일어나도 바깥의 변수에 해당 수정사항이 반영되지 않는다. 메서드 내부의 변화를 바깥의 변수에도 반영하고 싶을 때 ref 또는 out 키워드를 사용한다. 두 키워드 모두 매개변수를 참조 형식으로 전달한다는 점에서 비슷하지만, 가장 큰 차이점은 바로 초기화이다. ref 키워드로 매개변수를 전달하는 경우, 전달하기 전 변수의 초기화가 필수적이다. 반면 out 키워드로 전달하는 경우 매개변수를 전달하기 전 명시적으로 초기화를 해줄 필요가 없으며, 대신 이전 값을 아예 무시하기 때문에 메서드 내부에서 반드시 초기화를 진행해주어야 한다. 따라서 ref 키워드는 기존에 있는 변수를 메서드 내부에서 수정하고 싶을 때 사용하고, out 키워드는 메서드에서 생성된 값을 반환하고 싶을 때 사용한다.   

## 접근제한자란 무엇이며, 각각 어떤 차이가 있는가?  
접근제한자란 외부에서 타입(클래스, 구조체, 인터페이스, 델리게이트 등) 또는 타입 멤버들(프로퍼티, 필드, 메서드, 이벤트 등)로의 접근을 제한하는 키워드이다. public, internal, protected, private이 있다. public으로 선언될 경우 어디서든 자유롭게 사용될 수 있다. internal로 선언될 경우 해당 프로젝트 내부에서 public처럼 사용되고, 외부 프로젝트에서는 접근하지 못한다. 클래스를 생성할 경우 아무 접근 제한자를 선언하지 않는다면 기본값은 internal이다. 타입 멤버를 private으로 선언할 경우, 해당 타입 내부에서만 접근이 가능하다. protected로 클래스 내부 멤버를 선언할 경우, 해당 클래스와 클래스를 상속받은 하위 클래스들에서만 접근이 가능하다.   

## 팀 프로젝트 개발 - 전략 패턴을 이용한 Weapon 클래스 구현  
이번 프로젝트에는 플레이어가 댜양한 무기를 활용하여 몬스터를 포획하는 컨텐츠가 있다. 무기들은 근거리 혹은 원거리, 그리고 몬스터 포획 외에도 또다른 특별한 기능이 있는 무기들도 있다. 공통된 Weapon 추상 클래스를 상속하고, 일일이 메서드를 오버라이드하지 않고, 다르게 동작해야하는 메서드를 담은 인터페이스를 만들고, 인터페이스를 상속해서 필수 구현부를 각각 다르게 구현한 클래스들을 만든다. Weapon 추상 클래스는 인터페이스를 필드로 가지며, Weapon을 상속한 하위 클래스들에서 필드로 받은 인터페이스를 미리 구현해둔 클래스의 인스턴스로 초기화해주면, 전략 패턴이 완성된다.  
```cs
//Weapon 추상 클래스
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public abstract class Weapon
{
    protected ICatchBehaviour CatchBehaviour;
    protected ISpecialBehaviour SpecialBehaviour;

    public void PerformCatch()
    {
        CatchBehaviour.Catch();
    }

    public void PerformSpecial()
    {
        SpecialBehaviour.Special();
    }
}

```
```cs
// 인터페이스
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public interface ICatchBehaviour
{
    void Catch();
}

```
```cs
//인터페이스를 구현한 클래스
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RangedCatch : ICatchBehaviour
{
    private GameObject projectile;
    public void Catch()
    {
        if (projectile == null)
        {
            projectile = Resources.Load<GameObject>("TestProjectile");
            projectile = GameObject.Instantiate(projectile);
        }
        projectile.SetActive(true);
        Debug.Log("Shoot!");
    }
}

```
```cs
//Weapon을 상속받은 하위 클래스
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BasicNet : Weapon
{
    public BasicNet()
    {
        CatchBehaviour = new RangedCatch();
    }
}

```
```cs
//사용법
private void Start()
    {
        Weapon = new BasicNet();
        PlayerControlActions.Shoot.started += _ => Weapon.PerformCatch();
...        
```
