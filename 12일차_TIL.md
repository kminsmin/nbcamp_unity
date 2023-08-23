<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/a458479d-5fa7-4b28-9552-fb14ff410312)"/>  

# 내일배움캠프 12일차   
개인과제 추가기능 개발 : 장비 장착 개선, 던전 입장/전투, 레벨업, 휴식 추가  

## 개인과제 해설 특강  
오픈 클로즈 원칙  
와이어프레임(노트, PPT, figma) : 흐름 도식화 
전역변수 활용 - 내 경우 ref 엄청 썼음  
아이템 장착 : 나는 bool 변수 활용. EquippedItem 배열을 따로 만드는 게 확장성 면에서 유리함.   
메모리 측면에서 클래스/ 구조체, 객체 지향의 원칙이 무엇인지...  
알고리즘(배열 다루는 법 등), 디자인 패턴(객체지향) 공부하기  
### 개발 프로세스  
1. 만드려고 하는 기능 정리
2. 와이어프레임 정리
3. 화면 먼저 만들어보기 (UI)
4. 화면들끼리 연결해보기 (버튼, 화살표)  
5. 데이터 클래스/구조체 만들어보기 (데이터 모델링)  
6. 필요한 상황에 맞춰서 직접 개발
   
## 장비 장착 개선  
장비를 부위마다 하나씩 장착 가능하도록 개선했다. 각각의 장비 클래스마다 스트링 변수를 하나씩 선언해주고, 이를 비교하여 해당 부위 장착 여부를 확인한다. 
새로 착용할 아이템이 원래 착용중인 아이템과 같은 부위의 아이템이라면, 원래 착용하고 있던 아이템을 해제하고 새로운 아아템을 착용한다.  

```cs
 public class Item
 {
     public string name;
     public int addAtk;
     public int addDef;
     public int price;
     public string description;
     public bool isOn;
     public bool wasOn;
     public bool isBuy;
     public bool bound;
     public string part;
 }
 public class IronArmor : Item
 {
     public IronArmor()
     {
         name = "무쇠 갑옷";
         addAtk = 0;
         addDef = 9;
         price = 1500;
         isOn = false;
         wasOn = false;
         isBuy = false;
         bound = false;
         part = "Armor";
         description = "무쇠로 만들어져 튼튼한 갑옷입니다.";
     }
 }

        public static void ChangeItem(ref List<Item> playerInv) // 착용중인 아이템을 변경합니다. 착용중인 아이템을 선택하면 착용 해제하고, 착용하지 않은 아이템을 선택하면 장착합니다. 0이 입력되면 인벤토리 초기 화면으로 돌아갑니다.
        {
            Console.Clear();
            int i = 1;
            while (true)
            {
                Console.Clear();
                foreach (Item item in playerInv)
                {
                    Console.Write("- {0} ", i);
                    PrintItemInfo(item, false);
                    i++;
                }
                i = 1;

                Console.Write("\n\n\n0. 나가기\n\n장착 여부를 결정할 장비를 선택해주세요.");
                if (int.TryParse(Console.ReadLine(), out int itemChoice))
                {
                    for (int j = 0; j < playerInv.Count; j++)
                    {
                        if (itemChoice == j + 1)
                        {
                            if (playerInv[j].isOn == false)
                            {
                                playerInv[j].isOn = true;
                                foreach (Item item in playerInv)
                                {
                                    if (playerInv[j].name != item.name && playerInv[j].part == item.part && item.isOn == true)
                                    {
                                        item.isOn = false; //새로 착용한 아이템과 원래 착용중인 아이템의 부위가 같다면,
                                    }                      // 원래 착용중이던 아이템을 해제
                                }
                            }
                            else
                            {
                                playerInv[j].isOn = false;
                            }
                        }
                    }
                    if (itemChoice == 0)
                    {
                        i = 1;
                        break;
                    }
                }
                else
                {
                    Console.WriteLine("잘못된 입력입니다!");
                    Thread.Sleep(1000);
                }
            }
        }
```

## 던전 입장  
EnterDungeon 메서드에서는 시작화면 메서드와 비슷하게 다양한 던전들 중 하나를 선택할 수 있다. 선택한 던전의 메서드를 호출하여 탐험을 진행한다.  
```cs
        public static void EnterDungeon(ref int playerChoice, ref Player player, ref List<Item> playerInv)
        {
            while (true)
            {
                Console.Clear();
                Console.ForegroundColor = ConsoleColor.DarkMagenta;
                Console.WriteLine("이곳은 던전의 입구입니다. 온갖 위험이 도사리는 던전에서 꼭 살아남으시길. 건투를 빕니다.\n\n1. 동네 뒷산 [권장 공력력 : 15, 권장 방어력 : 5]\n2. 지하철 [권장 공력력 : 17, 권장 방어력 : 10]\n3. 악의 둥지[권장 공력력 : ???, 권장 방어력 : ???]\n\n0. 마을로 돌아가기\n\n");
                Console.WriteLine("도전하실 던전을 선택해주세요.");
                Console.ForegroundColor = ConsoleColor.White;
                if (player.hp <= 0)
                {
                    player.hp = 10;
                }
                if (int.TryParse(Console.ReadLine(), out playerChoice))
                {
                    if (playerChoice == 0)
                    {
                        break;
                    }
                    if (playerChoice == 1)
                    {
                        Forest(ref playerChoice, ref player, ref playerInv);
                    }
                    else if (playerChoice == 2)
                    {
                        Metro(ref playerChoice, ref player, ref playerInv);
                    }
                    else if (playerChoice == 3)
                    {
                        Evil(ref playerChoice, ref player, ref playerInv);
                    }
                    else
                    {
                        Console.WriteLine("잘못된 입력입니다!");
                    }
                }
                else
                {
                    Console.WriteLine("잘못된 입력입니다!");
                }
            }
            StartScene(ref playerChoice, ref player, ref playerInv);
        }

```
