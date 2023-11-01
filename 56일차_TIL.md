# 내일배움캠프 56일차 TIL    
오늘도 오전에 기술면접 예상질문을 풀어보고 팀 프로젝트 개발을 진행했다.  

## 박싱과 언박싱  
- 박싱은 값 형식을 object 형식 또는 이 값 형식에서 구현된 임의의 인터페이스 형식으로 암시적으로 변환하는 프로세스이다. 언박싱은 object 형식에서 값 형식으로, 또는 인터페이스 형태에서 해당 인터페이스를 구현한 값 형식으로 명시적으로 변환하는 프로세스이다.
- 값 형식을 박싱하면 힙에 새로운 개체 인스턴스가 할당되고 값이 새 개체에 복사된다. 언박싱을 진행할 때는 개체 인스턴스가 지정한 값 형식을 박싱한 값인지 확인하고, 인스턴스의 값을 값 형식 변수에 복사한다. 해당 프로세스들은 수행하는 데 많은 계산 과정이 필요해서 성능 저하의 우려가 있기 때문에 주의해야 한다.  
```cs
int i = 123;      // a value type
object o = i;     // boxing
int j = (int)o;   // unboxing
```

## 팀 프로젝트 개발  
### 맵 생성 알고리즘 개선  
기존 방식은 한 방을 생성하고 다음 방을 생성할 때 방향 상관 없이 연결 가능 여부만을 판단하여 무작위로 방을 생성했기 때문에, 방들이 겹쳐서 생성되는 경우가 종종 발생했다. 그래서 방이 생성될 때마다 좌표를 저장하고, 새로 방을 만들 때에는 기존 좌표와의 거리를 계산하여 일정 거리가 확보되었을 때에만 생성이 가능하도록 알고리즘을 개선했다.  
```cs
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using static Enums;

public class DungeonGenerator : MonoBehaviour
{
    #region Keys
    private const string _horizontalCorridorKey = "HorizontalCorridor";
    private const string _verticalCorridorKey = "VerticalCorridor";
    private const string _mapPrefabsPath = "Maps/Test/";
    #endregion

    #region Room Prefabs
    private List<GameObject> _rooms = new List<GameObject>();
    [SerializeField] private GameObject _spawnRoom;
    private GameObject _horizontalCorridor;
    private GameObject _verticalCorridor;
    #endregion

    #region Variables
    private int _roomCount = 1;
    private int _maxRoom = 30;
    private int _corridorCount = 0;
    private int _line = 0;
    private int _failCount = 0;
    private bool _isBuilding = true;
    private List<Vector3> _roomPositions = new List<Vector3>();
    [SerializeField] private int _roomDistance;
    #endregion

    private void Awake()
    {
        _horizontalCorridor = (GameObject)Resources.Load(_mapPrefabsPath + _horizontalCorridorKey);
        _verticalCorridor = (GameObject)Resources.Load(_mapPrefabsPath +  _verticalCorridorKey);
        for (int i = 1; i <= 7; i++)
        {
            _rooms.Add((GameObject)Resources.Load(_mapPrefabsPath + i));
        }

        ConnectRoom(_spawnRoom.GetComponent<RoomTemplate>().Doors[0].GetComponent<DoorData>(), _horizontalCorridor);
    }
    private void Start()
    {
        _isBuilding = false;
        Invoke("CloseHoles", 1f);
    }

    private void ConnectRoom(DoorData exitDoor, GameObject to)
    {
        if (_isBuilding && exitDoor.DoorType == DoorType.Left) return;
        if (_roomCount > _maxRoom) return;
        if(_line > 2)
        {
            _line = 0;
            return;
        }
        DoorType target = DoorType.Bottom;
        switch(exitDoor.DoorType)
        {
            case DoorType.Left:
                target = DoorType.Right; break;
            case DoorType.Right:
                target = DoorType.Left; break;
            case DoorType.Top:
                target = DoorType.Bottom; break;
            case DoorType.Bottom:
                target = DoorType.Top; break;
        }
        DoorData entranceDoor = SelectEntrance(target, to.GetComponent<RoomTemplate>().Doors);
        if (entranceDoor == null)
        {
            TryCreateRoom(exitDoor);
            return;
        }

        GameObject currentRoom = CreateRoom(ref entranceDoor, ref exitDoor, to);

        if (currentRoom != null)
        {
            foreach (GameObject door in currentRoom.GetComponent<RoomTemplate>().Doors)
            {
                DoorData data = door.GetComponent<DoorData>();
                if (data.DoorType == target)
                    data.IsConnected = true;
            }


            if (currentRoom.GetComponent<RoomTemplate>().Type != RoomType.HorizontalCorridor && currentRoom.GetComponent<RoomTemplate>().Type != RoomType.VerticalCorridor)
            {
                _roomCount++;
                _line++;
            }

            if (currentRoom.GetComponent<RoomTemplate>().Doors.Length > 1)
            {
                GameObject[] doors = currentRoom.GetComponent<RoomTemplate>().Doors.OrderBy(x => Random.Range(-4, 4)).ToArray();
                foreach (GameObject door in doors)
                {
                    TryCreateRoom(door);
                }
            }
        }
        else if (_failCount < 50)
        {
            _failCount++;
            TryCreateRoom(exitDoor);
            return;
        }
        else return;
        
    }

    private DoorData SelectEntrance(DoorType target, GameObject[] doors)
    {
        foreach (GameObject door in doors)
        {
            DoorData currentDoor = door.GetComponent<DoorData>();
            if (currentDoor.DoorType == target)
            {
                return currentDoor;
            }   
        }
        return null;
    }

    private void TryCreateRoom(GameObject door)
    {
        int roomIndex = (_roomCount > 3)? Random.Range(0, 8) : 1;
        if (!door.GetComponent<DoorData>().IsConnected && roomIndex < _rooms.Count && door.GetComponent<DoorData>().IsCorridor && _corridorCount > 10)
        {
            ConnectRoom(door.GetComponent<DoorData>(), _rooms[roomIndex]);
        }
        else if (!door.GetComponent<DoorData>().IsConnected)
        {
            _corridorCount++;
            if (door.GetComponent<DoorData>().DoorType == DoorType.Top || door.GetComponent<DoorData>().DoorType == DoorType.Bottom)
                ConnectRoom(door.GetComponent<DoorData>(), _verticalCorridor);
            else if (door.GetComponent<DoorData>().DoorType == DoorType.Left || door.GetComponent<DoorData>().DoorType == DoorType.Right)
                ConnectRoom(door.GetComponent<DoorData>(), _horizontalCorridor);  
        }
    }

    private void TryCreateRoom(DoorData door)
    {
        int roomIndex = (_roomCount > 3) ? Random.Range(0, 8) : 1;
        if (!door.IsConnected && roomIndex < _rooms.Count && door.IsCorridor && _corridorCount > 10)
        {
            ConnectRoom(door, _rooms[roomIndex]);
        }
        else if (!door.IsConnected)
        {
            if(door.DoorType == DoorType.Top || door.DoorType == DoorType.Bottom)
                ConnectRoom(door, _verticalCorridor);
            else ConnectRoom(door, _horizontalCorridor);
        }
    }

    private void CloseHoles()
    {
        
        GameObject[] emptyDoors = GameObject.FindGameObjectsWithTag("Door");
        _roomCount = 0;
        foreach (GameObject door in emptyDoors)
        {
            DoorData data = door.GetComponent<DoorData>();
            if (data.IsConnected) continue;
            switch (data.DoorType)
            {
                case DoorType.Left:
                    ConnectRoom(data, _rooms[4]); break;
                case DoorType.Right:
                    ConnectRoom(data, _rooms[3]); break;
                case DoorType.Top:
                    ConnectRoom(data, _rooms[5]); break;
                case DoorType.Bottom:
                    ConnectRoom(data, _rooms[6]); break;
            }
        }
    }

    private GameObject CreateRoom(ref DoorData entranceDoor, ref DoorData exitDoor, GameObject to)
    {
        Vector3 spawnPos = exitDoor.transform.position - entranceDoor.PivotDiff;
        GameObject room = Instantiate(to, spawnPos, Quaternion.identity);
        if (room != null && CheckSpawnableRoom(spawnPos))
        {
            exitDoor.IsConnected = true;
            if (!entranceDoor.IsCorridor)
            {
                _roomPositions.Add(spawnPos);
            }
        }
        else return null;
        
        return room;
    }

    private bool CheckSpawnableRoom(Vector3 spawnPos)
    {
        foreach(Vector3 position in _roomPositions)
        {
            if ((spawnPos - position).sqrMagnitude < _roomDistance) return false;
        }
        return true;
    }
}

```
### ```MonoBehaviour.Invoke``` 개선  
플레이어가 발판 아래로 점프할 수 있는 기능을 구현하기 위해, 아래로 점프 시 플레이어의 콜라이더에서 잠시 발판의 레이어를 exclude하고, 특정 시간 뒤어 다시 include하고 싶어서, 특정 시간 뒤 레이어를 포함해주는 메서드를 Invoke로 구현하고 싶었다. 그런데 해당 메서드는 여기에서만 쓰이고 다른 곳에서는 쓰이지 않을 예정이기 때문에, 람다식으로 표현하고 싶었다. 하지만 유니티의 MonoBehaviour 클래서에서 제공되는 Invoke 메서드는 별다른 오버로드 없이 메서드 이름과 시간만 매개변수로 받을 수 있고, 때문에 파라미터를 필요로 하는 메서드는 애초에 사용할 수도 없으며, 람다도 사용할 수 없다. 그래서 다른 방법을 찾아보다가, Invoke 함수로 매개변수를 포함한 메서드, 람다식 등도 호출할 수 있는 방법을 알아냈다.  
출처 : https://forum.unity.com/threads/tip-invoke-any-function-with-delay-also-with-parameters.978273/  
```cs
using UnityEngine;
using System;
using System.Collections;

public static class Utility
{
    public static void Invoke(this MonoBehaviour mb, Action f, float delay)
    {
        mb.StartCoroutine(InvokeRoutine(f, delay));
    }

    private static IEnumerator InvokeRoutine(System.Action f, float delay)
    {
        yield return new WaitForSeconds(delay);
        f();
    }
}

```
위의 코드와 같이 정적인 클래스를 만들고 그 안에 다음과 같은 세 개의 파라미터를 받는 Invoke 메서드를 정의하고, 받은 파라미터를 토대로 일정 시간 뒤에 받은 함수를 실행하는 코루틴를 실행하도록 구현하면, 다음과 같이 Invoke 메서드를 기존보다 다양하게 활용할 수 있다. 단, 원래의 Invoke를 사용하는 것처럼 바로 ```Invoke(str methodName, float time)``` 이렇게 사용할 수는 없고 앞에 this 키워드를 붙어야만 한다.    
```cs
private void Jump()
    {
        if (Rigidbody != null && _jumpCount > 0)
        {
            Rigidbody.gravityScale = _initialGravity;
            Rigidbody.velocity = Vector2.zero;
            if (_moveInput.y >= 0)
            {
                Rigidbody.AddForce(Vector2.up * _jumpForce, ForceMode2D.Impulse);
            }
            else
            {
                _isDownJumping = true;
                this.Invoke(() => _isDownJumping = false, 0.3f);  //새롭게 구현한 Invoke 메서드를 사용하여 안에 람다식을 0.3초 뒤에 실행한다.
            }

            Debug.Log(_moveInput.y);
            _jumpCount--;

        }
    }
```


