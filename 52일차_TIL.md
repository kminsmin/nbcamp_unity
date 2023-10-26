# 내일배움캠프 52일차 TIL  
오늘도 오전에 기술면집 질문들을 풀어보고, 이후 팀 프로젝트 개발을 진행했다.    

## struct와 class  
struct와 class 모두 변수 및 메서드를 선언할 수 있다. 하지만 struct는 값 형식으로, 메모리의 스택 영역에 저장되며, 상속이 불가능하다. 단, 멤버로 클래스를 가지거나 코드가 너무 길 경우 힙 영역에 저장되기도 한다. 반면 클래스는 참조형식으로, 힙 영역에 저장되며, 상속이 가능하다.  

## 팀 프로젝트 - 전략 패턴을 이용한 Weapon 개발 : Weapon 하위 클래스  
어제 전략 패턴으로 Weapon 클래스를 구현했었는데, 오늘은 Weapon을 상속하는 다양한 하위 클래스들을 제작하려고 했다. 금방하고 다른 작업으로 넘어갈 줄 알았으나 생각보다 예기치 못한 문제들을 많이 겪어서 오래 걸렸다...  
근접 무기를 휘두르는 모션을 구현하기 위해 코루틴을 사용했는데, 코루틴을 사용하려면 유니티의 MonoBehaviour클래스를 상속해야 했다. 하지만 MonoBehaviour를 사용하면 new 키워드를 통해 인스턴스르 만들 수 없게 된다. 따라서 AddComponent메서드를 사용해야 한다.  
```cs
    public void ChangeWeapon()
    {
        _weaponType = (GameManager.Instance.Status != null) ? GameManager.Instance.Status.Player.WeaponType : WeaponType.BasicNet;

        switch (_weaponType)
        {
            case WeaponType.BasicNet:
                Weapon = gameObject.AddComponent<BasicNet>();
                break;
            case WeaponType.CloseNet:
                Weapon = gameObject.AddComponent<CloseNet>();
                break;

        }
    }
```
Player는 데이터랑 메서드 몇개만 저장하면 되서 구조체로 만들어 놓았더니 접근할 때마다 GameManager를 통해 접근해야 한다. 캐싱하면 참조가 아닌 값만 복사되므로 캐생을 할 수도 없어 여간 불편한 것이 아니다. 앞으로 구조가 간단해도 하나의 객체를 여러 곳에서 참조할 필요가 있으면 그냥 클래스로 선언하는 것이 나을 것 같다.  
