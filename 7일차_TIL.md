<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/a458479d-5fa7-4b28-9552-fb14ff410312)"/>  

# 내일배움캠프 7일차  
2주차 강의 수강  
## 조건문  
if, else, else if 등 기본적인 조건문에 대해 배웠다. 그동안 이들 모두 중괄호가 필수적인 것으로 알고 있었는데, 조건 충족시 실행할 코드가
한 줄이라면 중괄호가 없어도 된다는 것을  알게 되었다. 이를 이용하여 한 줄로 정리한 것이 else if 라는 것도 알게 되었다.  
```cs
Console.Write("문자를 입력하세요: ");
char input = Console.ReadLine()[0]; //문자열 맨 앞 한글자 [0]

if ((input >= 'a' && input <= 'z')||(input >= 'A' && input <= 'Z'))
{
     Console.WriteLine("알파벳이다");
}
 else
{
     Console.WriteLine("아니네");
}
```
이 외에 switch 문과 3항 연산에 대해 배웠다.  
### switch 문  
swtich 문은 변수나 식에 따라 다른 코드블록을 실행하는 조건문이다. case문을 활용하여 변수나 식의 결과에 따라 다른 코드블록을 실행한다.
```cs
Console.WriteLine("게임을 시작합니다.");
Console.WriteLine("1: 전사 / 2: 마법사 / 3: 궁수");
Console.Write("직업을 선택하세요: ");
string job = Console.ReadLine();

switch (job) // 변수나 식
{
    case "1":
        Console.WriteLine("전사를 선택하셨습니다.");
        break;
    case "2":
        Console.WriteLine("마법사를 선택하셨습니다.");
        break;
    case "3":
        Console.WriteLine("궁수를 선택하셨습니다.");
        break;
    default:
        Console.WriteLine("올바른 값을 입력해주세요.");
        break;
}

Console.WriteLine("게임을 종료합니다.");
```
### 3항 연산자  
3항 연산자는 if문의 간단한 형태이다. 조건의 참 또는 거짓에 따라 두 값을 선택한다.  
(조건식) ? 참일 경우 값 : 거짓일 경우 값;  
```cs
int currentExp = 1200;
int requiredExp = 2000;

# 삼항 연산자
string result = (currentExp >= requiredExp) ? "레벨업 가능" : "레벨업 불가능";
Console.WriteLine(result);


# if else 문
if (currentExp >= requiredExp)
{
    Console.WriteLine("레벨업 가능");
}
else
{
    Console.WriteLine("레벨업 불가능");
}
```
### 디버깅  
f9을 눌러 원하는 줄에 브레이킹 포인트를 만든다. 이를 통해 f5를 눌러 디버깅 실행 시 해당 줄에서 코드를 멈추게 한다. 이후 f10을 눌러 한 줄씩 실행하면서 이상이 없는지 확인한다.  

## 반복문  
반복문은 for 문과 while 문이 있다. for 문은 반복횟수를 직관적으로 알 수 있다. while 문은 조건을 만족하는 한 계속 반복하기 때문에 반복횟수가 유동적이다. 상황에 따라 적합한 반복문을 사용하는 것이 좋다.
for 문을 활용하여 구구단을 출력해보았다.  
```cs
for (int i = 1; i < 10; i++)
{
     for (int j = 2; j < 10; j++)
    {
         Console.Write("{0} X {1} = {2}\t", j, i, j * i);

    }
     Console.WriteLine();
}
```
이 외에 do while 문과 foreach 문이 있다. do while 문은 while 문과 비슷하지만 조건식을 검사하기 전 코드블럭을 먼저 한번 실행한다는 점에서 다르다.  
```cs
int sum = 0;
int num;

do
{
    Console.Write("숫자를 입력하세요 (0 입력 시 종료): ");
    num = int.Parse(Console.ReadLine());
    sum += num;
} while (num != 0);

Console.WriteLine("합계: " + sum);
```
foreach 문은 배열이나 컬렉션에서 각 요소마다 반복작업을 할 때 사용한다.  
```cs
string[] inventory = { "검", "방패", "활", "화살", "물약" };

foreach (string item in inventory)
{
    Console.WriteLine(item);
}
```
break : 반복문에서 나올 때 사용한다.  
continue : 현재 반복을 중단하고 다음 반복을 시작한다.  
```cs
int sum = 0;

            while (true)
            {
                Console.Write("숫자를 입력하세요: ");
                int input = int.Parse(Console.ReadLine());

                if (input == 0)
                {
                    Console.WriteLine("프로그램을 종료합니다.");
                    break;
                }

                if (input < 0)
                {
                    Console.WriteLine("음수는 무시합니다.");
                    continue;
                }

                sum += input;
                Console.WriteLine("현재까지의 합: " + sum);
            }

            Console.WriteLine("합계: " + sum);
```
### 조건문, 반복문 사용해서 숫자 맞추기 게임  
```cs
            int targetNumber = new Random().Next(1, 101);
            int guess = 0;
            int count = 0;
            Console.WriteLine("1부터 100 사이의 숫자를 맞춰보세요.");

            while (guess != targetNumber)
            {
                Console.Write("자! 말해봐요 : ");
                guess = int.Parse(Console.ReadLine());
                count++;
                if (guess > targetNumber) 
                {
                    Console.WriteLine("다운!");
                    
                }
                else if(guess < targetNumber) 
                {
                    Console.WriteLine(("업!"));
                   
                }
                else
                {
                
                    Console.WriteLine("정답! 시도 횟수 {0}번",count);
                }
            }
```

## 배열  
배열은 동일한 자료형의 값들을 모아놓은 자료구조이다.  
1차원 배열은 동일한 데이터 유형을 가지는 데이터 요소들을 한 번에 모아서 다룰 수 있는 구조이다. 인덱스를 통해 각 요소에 접근할 수 있고 선언된 크기만큼의 공간을 메모리에 할당받는다.  
다차원 배열은 기본적으로 행렬의 구조를 가지고 있다. 다차원 배열을 활용하면 복잡한 데이터 구조를 효율적으로 관리할 수 있다.  
(2차원 : 행/열, 3차원 : 면/행/열)  
### 2차원 배열을 활용한 게임 맵 구현  
```cs
int[,] map = new int[5, 5] 
{ 
    { 1, 1, 1, 1, 1 }, 
    { 1, 0, 0, 0, 1 }, 
    { 1, 0, 1, 0, 1 }, 
    { 1, 0, 0, 0, 1 }, 
    { 1, 1, 1, 1, 1 } 
};

for (int i = 0; i < 5; i++)
{
    for (int j = 0; j < 5; j++)
    {
        if (map[i, j] == 1)
        {
            Console.Write("■ ");
        }
        else
        {
            Console.Write("□ ");
        }
    }
    Console.WriteLine();
}
```
## 컬렉션  
컬렉션은 자료를 모아 놓은 데이터 구조이다. 배열과 비슷하지만 배열과는 다르게 크기가 가변적이다. 사용하기 위해서는 System.Collections.Generic 네임스페이스를 추가해야 한다. 배열보다 사용하기 편리할 수도 있지만, 메모리 사용량 증가, 데이터 접근 시간 증가, 코드 복잡도 증가 등의 이유로 인해 무분별한 사용은 좋지 않다. 
C#에서는 다양한 종류의 컬렉션을 제공한다.  
1. List : 가변적인 크기를 갖는 배열
2. Dictionary : 키와 값으로 구성된 데이터를 저장
3. Stack : 후입선출(LIFO) 구조를 가진 자료구조
4. Queue : 선입선출(FIFO) 구조를 가진 자료구조
5. HashSet : 중복되지 않은 요소들로 이루어진 집합

## 메서드
특정 작업 수행을 위한 독립적인 코드블록이다. 함수와 혼동하기 쉬우나, 클래스 안에 포함된 함수라고 생각하면 될 것 같다. 즉 함수가 상위 개념이다.  
매서드를 사용하면 코드가 깔끔해지고 관리하기 쉬워진다.  
### 메서드 선언과 호출  
```
[접근 제한자] [리턴 타입] [메서드 이름]([매개변수])
{
    // 메서드 실행 코드
}
```
접근 제한자(Access Modifier) : 메서드에 접근할 수 있는 범위(public, private, protected)  
리턴 타입(Return Type)  : 메서드가 반환하는 값의 데이터 타입. 반환값이 없을 경우 void  
메서드 이름(Method Name) : 메서드를 호출하기 위해 사용하는 이름  
매개변수(Parameters) : 메서드에 전달되는 입력 값  
메서드 실행 코드(Method Body) : 중괄호 안에 메서드가 수행하는 작업을 구현하는 코드를 작성  
```cs
// 예시 1: 반환 값이 없는 메서드
public void SayHello()
{
    Console.WriteLine("안녕하세요!");
}

// 예시 2: 매개변수가 있는 메서드
public void GreetPerson(string name)
{
    Console.WriteLine("안녕하세요, " + name + "님!");
}

// 예시 3: 반환 값이 있는 메서드
public int AddNumbers(int a, int b)
{
    int sum = a + b;
    return sum;
}
```
## 게임 과제 : 틱택토 게임 만들기  
콘솔에서 마우스 커서 위치를 가져올 수 있는 기능이 있는 줄 모르고 행렬을 각각 입력하는 방식으로 구현했다.  개인적인 욕심으로 인해 게임판 사이즈도 변경 가능하게 구현했다.  
```cs
using System.Drawing;

namespace ConsoleApp1
{
    internal class Program
    {
        static void StartGame(int size)
        {
            for (int i = 0; i < size; i++)
            {   
                for (int j = 0; j < size; j++)
                {
                    Console.Write("□ ");
                }
                Console.WriteLine();
            }
        }

        static int PlayerInputR()
        {
            Console.Write("표시할 곳을 고르세요(행) :  ");
           int row = int.Parse(Console.ReadLine());
            return row;
            
        }

        static int PlayerInputC()
        {
            Console.Write("표시할 곳을 고르세요(열) :  ");
            int col = int.Parse(Console.ReadLine());
            Console.WriteLine();
            return col;

        }
        static void Main(string[] args)
        {
            Console.Write("게임의 난이도를 정해보세요 : ");
            int gameSize = int.Parse(Console.ReadLine());
            StartGame(gameSize);
            string[,] blocks = new string [gameSize, gameSize];
            for (int i = 0;i < blocks.GetLength(0);i++)
            {
                for (int j=0;j < blocks.GetLength(1);j++)
                {
                    blocks[i, j] = "□ ";
                }
            }
            
            int count = 0;
            while (true)
            {
                int row = PlayerInputR();
                int col = PlayerInputC();
                blocks[row-1 , col-1] = "● ";
                
                int comRow = new Random().Next(0,gameSize);
                int comCol = new Random().Next(0, gameSize);
                while (row == comRow && col == comCol)
                {
                    comRow = new Random().Next(0, gameSize);
                    comRow = new Random().Next(0, gameSize);
                }
                blocks[comRow, comCol] = "※ ";
                Console.Clear();
                for (int i = 0; i < gameSize; i++)
                {
                    for (int j = 0; j < gameSize; j++)
                    {
                        Console.Write(blocks[i, j]);
                    }
                    Console.WriteLine();
                }
                count++;
                if (count == gameSize * gameSize)
                {
                    Console.WriteLine("게임 끝!");
                    break;
                }



            }
            
        }


    }
}
```
## 구조체  
구조체는 여러 데이터를 한 데 모아둔 것이다. struct 로 선언할 수 있다. 변수와 메소드로 구성할 수 있다.  
```cs
struct Person
{
    public string Name;
    public int Age;

    public void PrintInfo()
    {
        Console.WriteLine($"Name: {Name}, Age: {Age}");
    }
}

Person person1;
person1.Name = "John";
person1.Age = 25;
person1.PrintInfo();

```

