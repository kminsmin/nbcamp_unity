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
