# 내일배움캠프 66일차 TIL    
오늘도 오전에 기술면접 예상질문을 풀어보고 팀 프로젝트 개발을 진행했다.     

## 선택정렬과 버블 정렬  
- 선택 정렬 : 주어진 배열에서 최솟값을 찾고, 그 최솟값을 맨 앞값과 바꾸는 과정을 반복하는 정렬 알고리즘이다.   
- 버블 정렬 : 인접한 두 개읜 원소를 비교해서 자리를 교환하는 것을 반복하는 방식으로, 해당 과정이 끝나면 가장 크거나 작은 원소가 마지막 자리로 위치한다.  
- 둘다 시간복잡도는 최악과 최선 상관없이 O(n^2)이어서 대중적으로 잘 쓰이는 정렬 방식은 아니다.
```cs
public static void BubbleSort(int[] arr)
{
    int n = arr.Length;
    for (int i = 0; i < n - 1; i++)
    {
        for (int j = 0; j < n - i - 1; j++)
        {
            if (arr[j] > arr[j + 1])
            {
                // 인접한 요소를 교환
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

public static void SelectionSort(int[] arr)
{
    int n = arr.Length;
    for (int i = 0; i < n - 1; i++)
    {
        int minIndex = i;
        for (int j = i + 1; j < n; j++)
        {
            if (arr[j] < arr[minIndex])
            {
                // 최소값을 찾음
                minIndex = j;
            }
        }
        // 최소값과 현재 요소를 교환
        int temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
}
```
## 스택 메모리와 힙 메모리  
- 스택 메모리 : 프로그램 실행 과정에 호출되는 함수와 값 형식의 지역 변수가 저장되는 공간  
- 힙 메모리 : 프로그램 실행 과정에서 동적으로 할당되는 변수가 저장되는 공간
- 힙은 스택보다 큰 메모리를 할당하기 위해 구분한 장소이며, 스택 메모리는 스코프를 벗어나면 자동적으로 메모리가 해제되지만, 힙은 가비지 컬렉터를 통해 해제된다.

## 값 형식과 참조 형식  
- 값 형식은 스택 공간에 데이터를 할당한다. 구조체, 문자열, 열거형을 제외한 모든 기본 데이터 유형은 값 유형이다.
- 참조형식은 힙 공간에 데이터를 할당한다. 대표적으로 클래스가 있다.  
- 값 유형을 대입 시 값이 복사되고 참조 유형을 대입 시 주소가 복사된다.

  ## 팀 프로젝트 개발 - Inventory
  오늘은 어제 개발하던 Inventory 기능을 이어서 개발했다. GameObject째로 아이템을 저장하면 추후 아이템에 접근할 때 원본 아이템이 파괴되었을 시 MissingReference 에러가 발생할 우려도 있고, 어차피 인벤토리 UI에 아이템 목록을 출력할 때 필요한 정보들만 보관하는 것이 효율적이기 때문에 Inventory는 Item 클래스를 저장하는 구조로 구현했다. 추후 선택한 아이템을 장착하거나 생성할 때에는 아이템의 ScriptableObject에 저장되어 있는 아이템 ID에 접근하여 필요한 작업을 수행한다.  
  
```cs
 public void EquipItem()
    {
        switch(_selectedItem.ItemType)
        {
            case ItemType.Weapon:
                if(_currentWeapon != null) _player.Attack -= _currentWeapon.ItemParameter;
                _currentWeapon = _selectedItem;
                _player.ChangeWeapon(_selectedItem.WeaponType);
                _player.Attack += _selectedItem.ItemParameter;
                _playerController.ChangeWeapon();
                _currentWeaponImage.sprite = _selectedItem.ItemSprite;
                break;
            case ItemType.Item:
                _currentOther = _selectedItem;
                //투척용 아이템 오브젝트를 아이템 ID를 통해 가져온다.
                _player.SetThrowableItem(ResourceManager.Instance.LoadPrefab<GameObject>($"Items/{_selectedItem.ItemID}", _selectedItem.ItemID.ToString())); 
                _currentItemImage.sprite = _selectedItem.ItemSprite;
                break;
        }
        _equipBtnText.text = "장착중";
        _equipBtnText.color = Color.green;
    }
```
