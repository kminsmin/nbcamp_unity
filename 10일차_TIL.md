<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/a458479d-5fa7-4b28-9552-fb14ff410312)"/>  

# 내일배움캠프 10일차  
개인과제 - Text RPG 개발  
## 필수 요구사항  
필수 요구사항은 다음 세 가지였다.  
### 1. 시작화면  
간단한 소개말과 함께 마을에서 할 수 있는 행동들을 알려준다. 0과 1 이외의 키를 누르면 잘못된 입력이라는 메시지를 출력해야 한다. 처음에는 Int.Parse()메소드를 사용해서 0, 1, 2 이외의 숫자를 판단했었는데 숫자 이외의 입력을 받으면 에러가
발생했다. 대안을 찾아본 결과 Int.TryParse()메소드를 알게 되었고, 이를 통해 True 값이 반환되면 결과값으로 나온 정수값을 위의 방법으로 판단하여 메시지를 출력하는 방식으로 마무리했다. 또한 다른 장면으로 전환될 때 플레이어와 인벤토리의
데이터를 ref 키워드로 전달하도록 했다. 이를 위해 플레이어의 구조체를 생성했고, 메인 메서드에서 구조체의 변수들을 초기화하였고 인벤토리의 리스트 선언 및 기본 아이템들을 추가해주었다. 아이템의 경우 추후 인벤토리 기능 구현을 위해 
미리 클래스로 구성해주었다. 각각의 아이템은 Item이라는 클래스를 상속받아서 그 안에 있는 다양한 필드들을 각자 초기화한 값을 가지고 있다. 
```cs
public static void StartScene(ref int playerChoice, ref Player player, ref List<Item> playerInv)
        {
            LoadPlayerStat(ref player, ref playerInv); // 장비 착용, 상점 구매 등, 플레이어의 스탯과 인벤토리의 변화를 반영해주는 메서드
            Console.Clear();
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("스파르타 마을에 오신 여러분 환영합니다. \n이곳에서 던전으로 들어가기 전 활동을 할 수 있습니다.\n\n1. 상태보기\n2. 인벤토리\n\n");
            Console.WriteLine("원하시는 행동을 입력해주세요.");
            Console.ForegroundColor = ConsoleColor.White;
            while (playerChoice == 0)
            {
                if (int.TryParse(Console.ReadLine(),out playerChoice))
                {
                    if (playerChoice == 1)
                    {
                        StatusScene(ref playerChoice, ref player, ref playerInv);
                        break;
                    }
                    else if (playerChoice == 2)
                    {
                        InventoryScene(ref playerChoice, ref player, ref playerInv);
                        break;
                    }
                    else
                    {
                        Console.WriteLine("잘못된 입력입니다!");
                        playerChoice = 0;
                    }
                }
                else
                {
                    Console.WriteLine("잘못된 입력입니다!");
                    playerChoice = 0;
                }
            }
        }

public struct Player
        {
            public string name;
            public string job;
            public int level;
            public int atk;
            public int def;
            public int xAtk; //장비 착용으로 인해 상승하는 공격력
            public int xDef; //장비 착용으로 인해 상승하는 방어력
            public int maxHp;
            public int gold;

            public void PrintStatus() //플레이어 스탯 확인 메서드에서 호출될 스탯 출력 메서드
            {
                Console.WriteLine($"Lv. {level.ToString("D2")}\n{name} ( {job} )\n공격력 : {atk} (+{xAtk})\n방어력 : {def} (+{xDef})\n체 력 : {maxHp}\nGold : {gold} G\n\n0. 나가기");
            }
        }

        static void Main(string[] args)
        {
            int playerChoice = 0;
            List<Item> playerInv = new List<Item>();
            playerInv.Add(new OldSword());
            playerInv.Add(new IronArmor());
            playerInv.Add(new TestItem());
            Player player;
            player.level = 1;
            player.job = "전사";
            player.atk = 10;
            player.def = 5;
            player.xAtk = 0;
            player.xDef = 0;
            player.maxHp = 100;
            player.gold = 1500;
            Console.WriteLine("당신의 이름은?");
            player.name = Console.ReadLine();


            StartScene(ref playerChoice, ref player, ref playerInv);
        }
```
### 2. 상태보기  
플레이어의 스탯 등의 정보를 출력하는 기능이다.  
위에서 만든 시작화면에서 playerChoice에 2가 입력되면, StatusScene 메서드가 호출되고 StartScene 메서드의 while문은 종료된다. playerChoice는 ref로 받아와서 다시 입력을 받으며, 0이 입력되면 해당 메서드의 while문을 종료하고,
StartScene 호출한다. StatusScene에서 ref로 매개변수들을 받고, 따로 변경하지 않고 그대로 StartScene 호출할 때 전달한다.  
```cs
 public static void StatusScene(ref int playerChoice, ref Player player, ref List<Item> playerInv) //플레이어의 현재 스탯을 보여줍니다.
        {
            Console.Clear();
            player.PrintStatus();
            while (true)
            {
                if (int.TryParse(Console.ReadLine(), out playerChoice))
                {
                    if (playerChoice == 0)
                    {
                        break;
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
### 3. 인벤토리   
StatusScene 메서드와 마찬가지로 StartScene으로부터 행동을 결정할 playerChoice, 플레이어 구조체, 그리고 아이템들이 들어있는 리스트를 매개변수로 받아온다. 호출되자마자 일단 인벤토리 내에 있는 아이템들을 출력한다. 
인벤토리 메서드는 아이템의 정보를 자주 출력하기 때문에 이 부분을 PrintItemInfo 메서드로 따로 생성하여 코드 반복성을 개선했다. PrintItemInfo 메서드는 아이템의 isOn변수 값에 따라 색을 다르게 출력하도록 하였고, 보다 간결한 
화면 구성을 위해 만약 아이템의 addAtk 또는 addDef값이 0이라면 해당 부분은 출력하지 않도록 구성하였다. 착용중인 아이템은 이름 앞에 "[E]"라는 문구를 출력하기 위해, 아이템 클래스에 isOn 이라는 boolean 변수를 선언해주었다. 
wasOn 변수는 추후 착용한 아이템을 플레이어의 스탯에 반영하기 위한 연산 과정에서 사용된다. 해당 연산은 StartScene 메서드가 호출될 때 가장 먼저 호출되는 LoadPlayerStat 메서드에서 이루어진다. 
```cs
        /* 인벤토리에 있는 장비들을 보여줍니다.
         * 장비 관리 칸에서는 장비의 인덱스 번호를 입력하면 착용 여부를 변경할 수 있으며, 0을 입력하면
         * 인벤토리 기본 화면을 다시 출력합니다. 이 상태에서 다시 0을 입력하면 시작화면으로 돌아갑니다.
         */
        public class Item 
        {
            public string name;
            public int addAtk;
            public int addDef;
            public string description;
            public bool isOn;
            public bool wasOn;
        }

         public class IronArmor : Item
        {
            public IronArmor()
            {
                name = "무쇠 갑옷";
                addAtk = 0;
                addDef = 5;
                isOn = false;
                wasOn = false;
                description = "무쇠로 만들어져 튼튼한 갑옷입니다.";
            }
        }

         public static void InventoryScene(ref int playerChoice, ref Player player, ref List<Item> playerInv)
        {
            int i = 1;

            while (true)
            {
                Console.Clear();

                foreach (Item item in playerInv)
                {
                    Console.Write("- ");
                    PrintItemInfo(item);
                }

                Console.WriteLine("\n\n1. 장착 관리\n0.나가기\n\n원하시는 행동을 입력해주세요.");
                if (int.TryParse(Console.ReadLine(), out playerChoice))
                {
                    if (playerChoice == 0)
                    {
                        break;
                    }
                    else if (playerChoice == 1)
                    {
                        Console.Clear();

                        while (true)
                        {
                            Console.Clear();
                            foreach (Item item in playerInv)
                            {
                                Console.Write("- {0} ", i);
                                PrintItemInfo(item);
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

        public static void PrintItemInfo(Item item) // 장비의 착용여부, 증가 스탯, 상세 설명 등을 출력합니다.
        {
            string itemName;
            int nameLength;
            byte[] data; 
            int blank;
            if (item.isOn)
            {
                Console.ForegroundColor = ConsoleColor.Yellow;
                itemName = " " + item.name;
                data = Encoding.Unicode.GetBytes(itemName);
                nameLength = data.Length;
                blank = (30 - nameLength) / 2;

                for (int i = 0; i < 2; i++)
                {
                    for (int j = 0; j < blank; j++)
                    {
                        Console.Write(" ");
                    }
                    if (i == 1)
                        break;
                    Console.Write($"[E]{item.name}");
                }
                if (item.addDef != 0 && item.addAtk == 0)
                    Console.WriteLine($"|            방어력 +{item.addDef}           | {item.description}");
                else if (item.addDef == 0 && item.addAtk != 0)
                    Console.WriteLine($"|            공격력 +{item.addAtk}           | {item.description}");

                else
                {
                    Console.WriteLine($"|      공격력 +{item.addAtk} 방어력 +{item.addDef}     | {item.description}");
                }
                Console.ForegroundColor = ConsoleColor.White;
            }
            else
            {
                itemName = item.name;
                data = Encoding.Unicode.GetBytes(itemName);
                nameLength = data.Length;
                blank = (30 - nameLength) / 2;

                for (int i = 0; i < 2; i++)
                {
                    for (int j = 0; j < blank; j++)
                    {
                        Console.Write(" ");
                    }
                    if (i == 1)
                        break;
                    Console.Write($"{item.name}");
                }
                    
                if (item.addDef != 0 && item.addAtk == 0)
                    Console.WriteLine($"|            방어력 +{item.addDef}           | {item.description}");               
                else if (item.addDef == 0 && item.addAtk != 0)
                    Console.WriteLine($"|            공격력 +{item.addAtk}           | {item.description}");
                else
                {
                    Console.WriteLine($"|      공격력 +{item.addAtk} 방어력 +{item.addDef}     | {item.description}");
                }
            }
        }

```
### LoadPlayerStat 메서드  
시작화면으로 돌아갈 때 가장 먼저 호출된다. 장비 변경 여부를 아이템의 isOn 변수와 wasOn 변수로 판단한다. 아이템을 착용하고 있으면 isOn은 True이다. 착용 후 스탯 상승이 반영이 안된 상태라면 wasOn이 False이다. 
아이템을 현재는 착용하고 있지 않으면 isOn은 False이며, 이 아이템이 이전에 착용된 상태였으나 착용 해제했다는 정보가 반영이 되지 않은 상태라면 wasOn이 True이다. 착용하고 있지 않고, 착용 해제되었다는 정보가 
플레이어 스탯에 반영이 된 상태라면, isOn과 wasOn이 모두 false이다.  
```cs
        public static void LoadPlayerStat(ref Player player, ref List<Item> playerInv) //시작화면으로 돌아갈 때마다 호출되어, 장비 변경 등으로 플레이어의 스탯에 변화가 있다면 이를 플레이어 구조체에 반영합니다.
        {
            foreach (Item item in playerInv)
            {
                if (item.isOn && item.wasOn == false) //아이템을 방금 착용하여 아직 스탯에 반영이 되지 않았으므로, 이를 반영하고 wasOn을 True로 초기화해줍니다.
                {
                    player.atk += item.addAtk;
                    player.xAtk += item.addAtk;
                    player.def += item.addDef;
                    player.xDef += item.addDef;
                    item.wasOn = true;
                }
                else if (item.isOn && item.wasOn) // 아이템을 착용중이나 해당 정보는 이미 반영이 된 상태이므로 다음 아이템을 검사합니다.
                {
                    continue;
                }
                else if (item.isOn == false && item.wasOn) // 아이템을 착용 해제했으나 이 정보가 반영이 안된 상태이므로 반영해주고 wasOn을 False로 초기화해줍니다.  
                {
                    player.atk -= item.addAtk;
                    player.xAtk -= item.addAtk;
                    player.def -= item.addDef;
                    player.xDef -= item.addDef;
                    item.wasOn = false;
                }
                else  //남는 경우의 수는 아이템이 해제된 상태이고, 해제되었다는 정보가 반영되었거나, 한 번도 착용하지 않은 경우이므로 다음 아이템을 검사합니다.
                {
                    continue;
                }
            }
        }
```
### 추가 기능 - 인벤토리 사이즈 균일화  
아이템의 이름 수에 따라 출력되는 크기가 다르기 때문에, 이를 균일화하기 위해 아이템 이름 스트링을 Byte수로 변환하고, 이름이 들어갈 칸의 크기를 미리 할당하고 빈칸을 계산하여 채워넣는 방식으로 인벤토리의 사이즈를 균일하게 했다.  
```cs
            string itemName;
            int nameLength;
            byte[] data; 
            int blank;
            if (item.isOn)
            {
                Console.ForegroundColor = ConsoleColor.Yellow;
                itemName = " " + item.name;
                data = Encoding.Unicode.GetBytes(itemName);
                nameLength = data.Length;
                blank = (30 - nameLength) / 2;

                for (int i = 0; i < 2; i++)
                {
                    for (int j = 0; j < blank; j++)
                    {
                        Console.Write(" ");
                    }
                    if (i == 1)//아이템의 이름은 한 번만 출력하면 되므로 위의 for문에서 두 번째 빈칸들을 출력하면 그 뒤에 아이템 이름은 출력하지 않고 상위 for문을 종료하기 위함이다.
                        break;
                    Console.Write($"[E]{item.name}");
                }
            ...
```
<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/3c4cc32d-2f52-404a-a589-cf905415fb5b"/>    
<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/f96f0053-dd28-42ed-90d8-f4405437ed1b"/>  

