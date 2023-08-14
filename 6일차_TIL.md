<img width="80%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/4e72ce85-44aa-460e-907f-0660e5c72d17"/>  

# 내일배움캠프 6일차 TIL  
C# 문법종합반 1주차  
## 기본 코드 구조 및 출력  
```cs
using System;
namespace ConsoleApp1
{
  internal class Program
  {
    static void Main(string[] args)
    {
      Console.WriteLine("Hello, Wolrd!");  // 출력 후 줄 변경
      Console.Write("Hello, "); // 출력 후 줄 변경 안함
      Console.WriteLine("World!");
    }
  }
}
```
System은 C#이 기본적으로 제공하는 네임스페이스이다.
네임스페이스 ConsoleApp1은 클래스와 다른 네임스페이스의 컨테이너 역할을 한다.
그 안에 Program이라는 클래스가 있다. Main 메서드는 프로그램이 시작될 때 자동으로 호출되는 메서드이며, 프로그램 실행 시 필수적이다.  
WriteLine과 Write를 활용하여 콘솔 창에 원하는 문구를 출력할 수 있다. 이스케이프 시퀸스를 활용하여 특수문자를 삽입할 수도 있다.  
### 소소한 꿀팁
코드 전체를 입력하지 않고, 조금만 입력하고 탭을 누르면 자동완성 기능을 활용해 편리하게 입력할 수 있다.  (클래스, 메서드 변수 등. 예제도 볼 수 있다.)
실수로 자동완성 기능을 껐다면 ctrl + space를 눌러 다시 자동완성 창을 열 수 있다.  
코드 템플릿 기능 : for 문 사용시, 탭을 두번 누르면 for 문의 기본적인 코드 템플릿이 완성된다.  

## 변수, 자료형
C#은 다양한 기본 자료형을 제공하는데, 이러한 자료형들을 사용하여 변수를 선언한다. 자료형들은 크기, 부호, 표현 범위 등으로 세분화되어 있는데, 이때 용도에 맞는 자료형을 올바르게
선택하는 것이 좋다. 자료형 각각의 메모리 사용량이 다르기 때문에, 효율적인 메모리 사용 위함이기도 하고, 정확한 데이터 표현과 타입 안정성 때문이다.  


리터럴은 프로그램 내에서 실제로 사용되는 상수값이다. 소스코드에 기록되어 있는 값이다. C#에서는 컴파일러에 의해 상수값으로 처리되며 변수나 상수에 할당되거나 연산에 사용된다.
변수 선언은 다음과 같이 할 수 있다.
```cs
int num;  //변수 선언
int num = 10; // 변수 선언과 동시에 초기화
int num1, num2, num3 = 10; //이러면 num3만 10으로 초기화 된다. 나머지는 초기화 X
num1 = num2 = num3 = 10; //올바른 변수 다중 초기화 방법
float f = 3.14f;
char c = 'A';
string str = "Hello, World!";

int num1 = 0x10;
int num2 = 0b1010;
long num3 = 100000000000000L;
```

### 마법의 키워드 var
```cs
var num = 10;         // int 자료형으로 결정됨
var name = "kero";   // string 자료형으로 결정됨
var pi = 3.141592;    // double 자료형으로 결정됨
```
초기화하는 값에 따라 컴파일러가 변수의 자료형을 자동으로 결정한다. 변수가 초기화될 시점의 자료형을 명확히 알기 힘들 때 사용하면 유용하다.  

## 입력
```cs
Console.Write("Enter your name: ");
string name = Console.ReadLine();
Console.WriteLine("Hello, {0}!", name);
```
ReadLine은 입력값을 문자열로 반환하기 때문에, 숫자나 논리값을 입력받고 싶으면 적절한 형변환을 해주어야 한다.(명시적/ 암시적)  

### 다중 입력, 스플릿
```cs
Console.Write("Enter two numbers: ");
 string input = Console.ReadLine();    // "10 20"과 같은 문자열을 입력받음

string[] numbers = input.Split(' ');  // 문자열을 공백으로 구분하여 배열로 만듦
int num1 = int.Parse(numbers[0]);     // 첫 번째 값을 정수로 변환하여 저장
int num2 = int.Parse(numbers[1]);     // 두 번째 값을 정수로 변환하여 저장

int sum = num1 + num2;                // 두 수를 더하여 결과를 계산

Console.WriteLine("The sum of {0} and {1} is {2}.", num1, num2, sum);
```

### 소소한 꿀팁
원하는 부분 드래그 +  CTRL + K + C => 주석 처리  
원하는 부분 드래그 +  CTRL + K + U => 주석 해제

### 간단한 계산기 만들며 실습해보기
```cs
// 이름, 나이 입력받고 출력하기
            Console.Write("이름을 입력해주세요 : ");
            string name = Console.ReadLine();
            Console.Write("나이를 입력해주세요 : ");
            string age = Console.ReadLine();
            Console.WriteLine("당신은 {0}, {1}세 입니다.",name,age);

// 두 숫자를 받고 합을 출력하기
            Console.Write("첫 번째 숫자를 입력해주세요 : ");
            string num1 = Console.ReadLine();
            Console.Write("두 번째 숫자를 입력해주세요 : ");
            string num2 = Console.ReadLine();
            int num3 = int.Parse(num1) + int.Parse(num2);
            Console.WriteLine("두 숫자 {0}, {1}의 합은 {2} 입니다.",num1,num2,num3);

//섭씨에서 화씨로 변환하기
            Console.Write("현재 온도를 섭씨로 입력해주세요 : ");
            string tempC = Console.ReadLine();
            float tempF = (int.Parse(tempC) * 9f / 5f) + 32f;
            Console.WriteLine("현재 온도는 화씨 {0}도 입니다.",tempF);


//BMI 계산기
            Console.Write("당신의 몸무게를 입력해주세요 : ");
            float weight = int.Parse(Console.ReadLine());
            Console.Write("당신의 키를 입력해주세요 : ");
            float height = int.Parse(Console.ReadLine()) / 100f;
            float bmi = weight / (height*height);
            Console.WriteLine("당신의 BMI 지수는{0} 입니다.",bmi);
```

## 코딩 테스트  
1주차 강의 수강 후 배운 내용도 복습할 겸 코딩테스트를 풀어보았다.  
### 최빈값 구하기
<img width="80%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/5f7baa6b-00f5-487e-9288-40d0a08a465e"/>  

Array에 대해 자세히 배우지 않아서 상당히 많은 구글링이 필요했다.  

기본적인 알고리즘은 먼저 주어진 array의 요소가 가질 수 있는 원소의 최대 크기만큼의 새로운 index라는 배열을 선언한다. 이후 for 문을 활용하여 array의 각 원소를 확인하고 해당 원소에 해당하는 index 배열의 원소에 1씩 더한다.
이 단계가 끝나면, 또 다른 for 문에서 index 배열의 최대값을 찾는다. 최대값은 array 배열속 최빈값 숫자의 개수이고, 이때의 i값이 최빈값이 된다. 하지만 최빈값이 다수 존재할 경우 -1을 반환해야 하기 때문에, else if 로 구분하고 bool
변수를 활용하여 판단하도록 했다.  
```cs
using System;

public class Solution {
    public int solution(int[] array) {
        int[] index = new int[1001];
        int max = int.MinValue;
        int mode = 0;
        bool multiMode = false;
        for (int i=0; i < array.Length; i++)
        {
            index[array[i]]++;
        }
        for (int i = 0; i < index.Length; i++)
        {
            if (index[i] > max )
            {
                max = index[i];
                mode = i;
                multiMode = false;
            }
            else if (index[i] == max)
            {
                multiMode = true;
            }
        }
        if (multiMode == true)
        {
            mode = -1;
        }
        int answer = mode;
        
        return answer;
    }
}
```
