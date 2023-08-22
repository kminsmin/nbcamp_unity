<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/a458479d-5fa7-4b28-9552-fb14ff410312)"/>  

# 내일배움캠프 11일차  
개인과제 - Text RPG 개발  
과제의 필수 요구사항은 어제 개발을 마쳤기 때문에, 오늘은 추가 기능 개발에 집중했다. 
인벤토리에 들어있는 아이템을 정렬하는 기능, 그리고 상점에서 아이템을 사고 파는 기능을 추가했다.  

## 인벤토리 정렬  
다양한 정렬 기준을 선택할 수 있도록 switch문을 사용해서 경우의 수를 나눠놓았다. 그리고 OrderBy메서드와 ToList 메서드를 활용해 playerInv 내의 인스턴스들을 재정렬하는 방식으로 구현했다.  
```cs
        public static void OrderItem(ref List<Item> playerInv)
        {
            Console.Clear();

            foreach (Item item in playerInv)
            {
                Console.Write("- ");
                PrintItemInfo(item, false);
            }

            Console.WriteLine("\n\n1. 이름\n2. 이름 길이\n3. 공력력\n4. 방어력\n\n\n\n0.나가기\n\n정렬 기준을 입력해주세요."); // 정렬 기준 선택

            if (int.TryParse(Console.ReadLine(), out int orderChoice)&& orderChoice<5)
            {
                Console.WriteLine("\n\n1. 오름차순\n2. 내림차순\n\n정렬 기준을 입력해주세요.");
                if (int.TryParse(Console.ReadLine(), out int isAscending)&& isAscending <3)    //선택한 기준으로 오름차순/내림차순으로 정렬할지 선택
                {
                    switch (orderChoice)                                                          //위에서 선택한 기준들에 따라 4가지 경우의 수 나눠서 정렬
                    {
                        case 1:
                            if (isAscending == 1)
                                playerInv = playerInv.OrderBy(item => item.name).ToList();
                            else playerInv = playerInv.OrderByDescending(item => item.name).ToList();
                            break;
                        case 2:
                            if (isAscending == 1)
                                playerInv = playerInv.OrderBy(item => item.name.Length).ToList();
                            else playerInv = playerInv.OrderByDescending(item => item.name.Length).ToList();
                            break;
                        case 3:
                            if (isAscending == 1)
                                playerInv = playerInv.OrderBy(item => item.addAtk).ToList();
                            else playerInv = playerInv.OrderByDescending(item => item.addAtk).ToList();
                            break;
                        case 4:
                            if (isAscending == 1)
                                playerInv = playerInv.OrderBy(item => item.addDef).ToList();
                            else playerInv = playerInv.OrderByDescending(item => item.addDef).ToList();
                            break;
                        default:
                            Console.WriteLine("잘못된 입력입니다!");
                            Thread.Sleep(1000);
                            break;
                    }
                }
                else
                {
                    Console.WriteLine("잘못된 입력입니다!");
                    Thread.Sleep(1000);
                }
            }
            else
            {
                Console.WriteLine("잘못된 입력입니다!");
                Thread.Sleep(1000);
            }
        }
```
## 상점
StoreScene이라는 메서드를 만들고, 그 안에서 아이템을 사고 파는 메서드들을 호출하는 방식이다. 상점에서 판매하는 아이템의 리스트는 StoreScene 메서드가 호출될 때마다 새로 선언되고 초기화된다. 메인 메서드에서 미리 선언할까 고민했었는데, 어차피 다른 메서드에서는 전혀 사용하지 않기 때문에 이 메서드 안에서만 사용하도록 여기서 선언하는 것이
덜 번거로울 것이라고 판단했다. 이렇게 StoreScene 메서드가 호출되지마자 storeItems라는 리스트가 생성되고, 미리 만들어둔 Item의 클래스를 상속받은 다양한 하위 클래스들의 인스턴스들을 생성하고 리스트에 추가한다. 
그리고 이중 foreach문을 사용해서 storeItems리스트와 playerInv 리스트를 비교하여, 만약 플레이어 인벤토리에 상점에서 판매하는 아이템이 이미 있다면, "판매완료"라는 문구를 출력하기 위해 boolean 변수인 isBuy를 True로 변경하도록 했다.
이후 플레이어의 선택에 따라 아이템을 사거나 파는 메서드를 호출한다.  
```cs
        public static void StoreScene(ref int playerChoice, ref Player player, ref List<Item> playerInv)
        {
            //상점아이템 리스트 생성
            List<Item> storeItems = new List<Item>();
            storeItems.Add(new OldSword());
            storeItems.Add(new TrainingArmor());
            storeItems.Add(new IronArmor());
            storeItems.Add(new SpartanArmor());
            storeItems.Add(new BronzeAxe());
            storeItems.Add(new SpartanSpear());
            
            while (true)
            {
                Console.Clear();
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine($"어서오세요! 각종 장비 판매 및 구매 가능하신 이 마을 최고의 상점입니다!\n\n[내 돈]\n{player.gold}\n\n[아이템 목록]\n");
                Console.ForegroundColor = ConsoleColor.White;
                //아이템 목록 보여주기, 플레이어 인벤토리와 비교하여 같은 아이템이 있으면 "구매완료" 표시
                foreach (Item item in storeItems)
                {
                    item.isBuy = false; //플레이어가 아이템을 팔았을 때, 해당 정보를 상점 아이템 리스트의 요소들에게 반영해주기 위함입니다.
                    foreach (Item myItem in playerInv)
                    {
                        if (myItem.name == item.name)
                        {
                            item.isBuy = true;
                            break;
                        }   
                    }
                    ShowStoreItem(item);
                }

                Console.WriteLine("\n\n1. 아이템 구매\n2. 아이템 판매\n\n\n\n0.나가기\n\n원하시는 행동을 입력해주세요.");
                if (int.TryParse(Console.ReadLine(), out playerChoice))
                {
                    if (playerChoice == 0)
                    {
                        break;
                    }
                    else if (playerChoice == 1)
                    {
                        BuyItem(ref storeItems, ref player,ref playerInv);
                    }
                    else if (playerChoice == 2)
                    {
                        SellItem(ref storeItems, ref player, ref playerInv);
                    }
                    else
                    {
                        Console.WriteLine("잘못된 입력입니다!");
                        Thread.Sleep(1000);
                    }
                }
                else
                {
                    Console.WriteLine("잘못된 입력입니다!");
                    Thread.Sleep(1000);
                }

            }
            StartScene(ref playerChoice, ref player, ref playerInv);
        }
```
## 상점 - 아이템 구매  
StoreScene 메서드로부터 ref 키워드로 상점의 아이템이 담겨있는 리스트, 플레이어 구조체, 그리고 플레이어의 인벤토리 리스트를 매개변수로 받아온다. 그리고 어제 만든 장비 변경 메서드와 비슷한 방식으로 
아이템마다 인덱스를 달아주고, 해당 인덱스의 숫자를 플레이어가 입력하면 이미 구매한 아이템인지, 플레이어가 충분한 돈이 있는지를 판단하여 구매 여부를 결정한다.  
```cs
        public static void BuyItem(ref List<Item> storeItems, ref Player player, ref List<Item> playerInv)
        {
            Console.Clear();
            int i = 1;
            while (true)
            {
                Console.Clear();
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine($"어서오세요! 각종 장비 판매 및 구매 가능하신 이 마을 최고의 상점입니다!\n\n[내 돈]\n{player.gold}\n\n[아이템 목록]\n");
                Console.ForegroundColor = ConsoleColor.White;

                foreach (Item item in storeItems)
                {
                    Console.Write("- {0} ", i);
                    ShowStoreItem(item);
                    i++;
                }
                i = 1;
                Console.WriteLine("\n\n0. 나가기\n\n구매할 장비를 선택해주세요.");
                if (int.TryParse(Console.ReadLine(), out int itemChoice)&& itemChoice < storeItems.Count+1)
                {
                    for (int j = 0; j < storeItems.Count; j++)
                    {
                        if (itemChoice == j + 1)
                        {
                            if (storeItems[j].isBuy == false)
                            {
                                if (player.gold >= storeItems[j].price)
                                {
                                    storeItems[j].isBuy = true;
                                    playerInv.Add(storeItems[j]);
                                    player.gold -= storeItems[j].price ;
                                    Console.ForegroundColor = ConsoleColor.DarkCyan;
                                    Console.WriteLine("구매를 완료했습니다.");
                                    Console.ForegroundColor = ConsoleColor.White;
                                    Thread.Sleep(1000);
                                }
                                else
                                {
                                    Console.ForegroundColor = ConsoleColor.Red;
                                    Console.WriteLine("돈이 부족합니다...");
                                    Console.ForegroundColor = ConsoleColor.White;
                                    Thread.Sleep(1000);
                                }
                            }
                            else
                            {
                                Console.ForegroundColor = ConsoleColor.Cyan;
                                Console.WriteLine("이미 구매한 아이템입니다.");
                                Console.ForegroundColor = ConsoleColor.White;
                                Thread.Sleep(1000);
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
## 상점 - 아이템 판매  
아이템 구매 메서드와 비슷한 방식으로 구현했다. 이미 판매한 아이템인지, 귀속 아이템인지 판단하고 판매 여부를 결정한다. 또한 판매시 착용 중인 아이템이었다면 착용 해제되고 이를 플레이어 스탯에 반영한다.  
```cs
public static void SellItem(ref List<Item> storeItems, ref Player player, ref List<Item> playerInv)
        {
            Console.Clear();
            int i = 1;
            while (true)
            {
                Console.Clear();
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine($"어서오세요! 각종 장비 판매 및 구매 가능하신 이 마을 최고의 상점입니다!\n\n[내 돈]\n{player.gold}\n\n[아이템 목록]\n");
                Console.ForegroundColor = ConsoleColor.White;

                foreach (Item item in playerInv)
                {
                    Console.Write("- {0} ", i);
                    PrintItemInfo(item, true);
                    i++;
                }
                i = 1;
                Console.WriteLine("\n\n0. 나가기\n\n판매할 장비를 선택해주세요.");
                if (int.TryParse(Console.ReadLine(), out int itemChoice) && itemChoice < playerInv.Count + 1)
                {
                    for (int j = 0; j < playerInv.Count; j++)
                    {
                        if (itemChoice == j + 1)
                        {
                            if (playerInv[j].isBuy == false && playerInv[j].bound == false)
                            {
                                Console.ForegroundColor = ConsoleColor.Cyan;
                                Console.WriteLine("이미 판매한 아이템입니다.");
                                Console.ForegroundColor = ConsoleColor.White;
                                Thread.Sleep(1000);
                            }
                            else if (playerInv[j].isBuy == false && playerInv[j].bound == true)
                            {
                                Console.ForegroundColor = ConsoleColor.Magenta;
                                Console.WriteLine("아무리 돈이 없어도 이건 팔 수 없지.");
                                Console.ForegroundColor = ConsoleColor.White;
                                Thread.Sleep(1000);
                            }
                            else
                            {
                                player.gold += (playerInv[j].price/ 100) * 85;
                                playerInv[j].isBuy = false;
                                Console.ForegroundColor = ConsoleColor.DarkCyan;
                                Console.WriteLine("판매 완료했습니다.");
                                Console.ForegroundColor = ConsoleColor.White;
                                if (playerInv[j].isOn == true)
                                {
                                    playerInv[j].isOn = false;
                                    player.atk -= playerInv[j].addAtk;
                                    player.xAtk -= playerInv[j].addAtk;
                                    player.def -= playerInv[j].addDef;
                                    player.xDef -= playerInv[j].addDef;
                                }
                                playerInv.Remove(playerInv[j]);
                                Thread.Sleep(1000);
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
## 새로 배운 것들  
1. 리스트의 요소들을 재배열하기 위한 OrderBy, ToList 메서드
2. 콘솔창의 크기를 수정하는 Console.SetWindowSize(너비,높이) 메서드, 폰트색을 수정하려면 Console.ForegroundColor = ConsoleColor.색
3. 입력받은 값을 정수로 변환 시도하는 Int.TryParse()메서드
4. 단순 문자열의 길이를 구하는 String.Length, 문자열의 바이트수를 구하는 Encoding.Default/Unicode.GetBytes() 메서드  
