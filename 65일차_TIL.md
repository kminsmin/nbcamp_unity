# 내일배움캠프 65일차 TIL  
오늘은 플레이어의 인벤토리를 구현하고자 했다.  

## Inventory의 구조에 대한 고민  
인벤토리의 구조를 어떻게 할지에 대한 고민이 많았다. 궁극적으로는 무기, 장비, 기타 아이템들의 세부 정보를 확인하고 장착하는 기능과, 기타 아이템의 경우 장착한 아이템을 던질 수 있어야 하기 때문에, 아이템 GameObject 자체를 가지고 있도록 하려고 했었다. 하지만 맵에 있는 아이템 오브젝트를 플레이어와의 상호작용을 통해 Inventory에 추가하는 테스트를 해보았을 때, 맵에 존재하는 아이템 오브젝트를 파괴할 경우 Missing Reference 에러가 발생했다. 이 때의 인벤토리는 ItemType을 Key로, 그리고 각 ItemType의 아이템 오브젝트를 담는 List를 Value로 저장하는 Dictionary로 되어 있었다. 맵에 존재하는 아이템 오브젝트와 상호작용하여 Inventory에 추가할 때, GameObject를 복사하여 새로운 개체를 넣은 것이 아니라, 참조 복사만이 일어나서 맵에 존재하는 아이템 오브젝트의 주소만 복사된 것이다. 때문에 해당 아이템 오브젝트가 사라지면 제대로 아이템에 접근하지 못하는 현상이었다. 이를 해결하기 위해 얕은 복사와 깊은 복사의 차이를 공부하고, 깊은 복사를 통해 개체를 복사하려고 했다. 하지만 깊은 복사의 경우, 클래스 내 모든 필드에 대해 값 복사를 해줘야 하기 때문에, GameObject형식의 경우 복사해야할 값이 한두 가지가 아니어서 GameObect를 통째로 저장하는 방식 대신 Item 클래스만을 복사하는 방식으로 변경했다. 추후 아이템을 던지는 등 GameObject가 필요한 때에는, 아이템 관련 데이터를 저장해둔 스크립터블 오브젝트에 접근하여, 해당 아이템의 ID값을 가져와서 ID에 해당하는 아이템 프리팹을 로드해서 생성하는 방식으로 구현할 예정이다.  
```cs
public Dictionary<ItemType, Dictionary<int, Item>> Inventory { get; private set; }

public void AddToInventory(Item item)
    {
        ItemType itemType = item.ItemSO.ItemType;
        int itemID = item.ItemSO.ItemID;


        if (!Inventory[itemType].ContainsKey(itemID))
        {
            Inventory[itemType].Add(itemID, item);
            Inventory[itemType][itemID].ItemCount++;
        }

        else Inventory[itemType][itemID].ItemCount++;
        Debug.Log("아이템 획득 완료");
    }

//Inventory UI
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UIElements;
using UnityEngine.UI;
using TMPro;
using static Enums;
using System;

public class InventoryUI : MonoBehaviour
{
    #region Keys
    private const string _playerTag = "Player";
    #endregion

    #region Components
    [SerializeField] private UnityEngine.UI.Image[] _itemSlots;

    [SerializeField] private TextMeshProUGUI _weaponText;
    [SerializeField] private TextMeshProUGUI _equipmentText;
    [SerializeField] private TextMeshProUGUI _otherText;

    [SerializeField] private UnityEngine.UI.Button _weaponTab;
    [SerializeField] private UnityEngine.UI.Button _equipmentTab;
    [SerializeField] private UnityEngine.UI.Button _itemTab;
    [SerializeField] private UnityEngine.UI.Button _equipBtn;

    [SerializeField] private UnityEngine.UI.Image _itemImage;
    [SerializeField] private TextMeshProUGUI _itemName;
    [SerializeField] private TextMeshProUGUI _itemDescription;
    [SerializeField] private TextMeshProUGUI _itemType;
    [SerializeField] private TextMeshProUGUI _itemRarity;


    private Player _player;
    private PlayerController _playerController;
    #endregion

    #region Variables
    private ItemType _currentTab;
    private ItemSO _selectedItem;
    private ItemSO _currentWeapon;
    private ItemSO _currentEquipment;
    private ItemSO _currentOther;
    #endregion

    private void Start()
    {
        _weaponTab.onClick.AddListener(ShowWeapon);
        _equipmentTab.onClick.AddListener(ShowEquipment);
        _itemTab.onClick.AddListener(ShowOther);
        _equipBtn.onClick.AddListener(EquipItem);
    }

    private void OnEnable()
    {
        if (_player == null) _player = DataManager.Instance.Player;
        if(_playerController == null) _playerController = GameObject.FindGameObjectWithTag(_playerTag).GetComponent<PlayerController>();
        ShowItems(_currentTab);
    }


    private void ShowItems(ItemType itemType)
    {
        Dictionary<int, Item> currentInventory = _player.Inventory[itemType];
        int i = 0;
        foreach(Item item in currentInventory.Values)
        {
            _itemSlots[i].sprite = item.ItemSO.ItemSprite;
            _itemSlots[i].transform.GetChild(0).GetComponent<UnityEngine.UI.Button>().onClick.AddListener(() =>
            {
                _selectedItem = item.ItemSO;
                _itemName.text = _selectedItem.ItemName;
                _itemImage.sprite = _selectedItem.ItemSprite;
                _itemDescription.text = _selectedItem.ItemDescription;

                 switch(_selectedItem.ItemType)
                {
                    case ItemType.Weapon:
                        _itemType.text = "무기";
                        break;
                    case ItemType.Equipment:
                        _itemType.text = "장비";
                        break;
                    case ItemType.Item:
                        _itemType.text = "기타";
                        break;
                }

                switch(_selectedItem.Rarity)
                {
                    case CardRarity.Normal:
                        _itemRarity.text = "노멀";
                        _itemRarity.color = Color.white;
                        break;
                    case CardRarity.Rare:
                        _itemRarity.text = "레어";
                        _itemRarity.color = Color.blue;
                        break;
                    case CardRarity.Epic:
                        _itemRarity.text = "에픽";
                        _itemRarity.color = Color.magenta;
                        break;
                    case CardRarity.Legend:
                        _itemRarity.text = "레전더리";
                        _itemRarity.color = Color.red;
                        break;
                    case CardRarity.EndPoint:
                        _itemRarity.text = "신화";
                        _itemRarity.color = Color.yellow;
                        break;
                }

            });
            i++;
        }
    }

    public void ShowWeapon()
    {
        ClearItemSlots();
        _currentTab = ItemType.Weapon;
        ShowItems(ItemType.Weapon);
        _weaponText.color = Color.white;
        _equipmentText.color = new Color(1, 1, 1, 50f / 255);
        _otherText.color = new Color(1, 1, 1, 50f / 255);
    }

    public void ShowEquipment()
    {
        ClearItemSlots();
        _currentTab = ItemType.Equipment;
        ShowItems(ItemType.Equipment);
        _weaponText.color = new Color(1, 1, 1, 50f / 255);
        _equipmentText.color = Color.white;
        _otherText.color = new Color(1, 1, 1, 50f / 255);
    }

    public void ShowOther()
    {
        ClearItemSlots();
        _currentTab = ItemType.Item;
        ShowItems(ItemType.Item);
        _weaponText.color = new Color(1, 1, 1, 50f / 255);
        _equipmentText.color = new Color(1, 1, 1, 50f / 255);
        _otherText.color = Color.white;
    }

    public void ClearItemSlots()
    {
        foreach(var slot in _itemSlots)
        {
            slot.sprite = slot.gameObject.transform.GetChild(0).GetComponent<UnityEngine.UI.Image>().sprite;
        }
    }

    public void EquipItem()
    {
        switch(_selectedItem.ItemType)
        {
            case ItemType.Weapon:
                _currentWeapon = _selectedItem;
                _player.ChangeWeapon(_selectedItem.WeaponType);
                _playerController.ChangeWeapon();
                break;
        }
    }
}

```

<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/62f720af-45cd-4ae8-839e-e7b426a168bf"/>  
