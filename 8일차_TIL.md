<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/793639b2-e1bb-478c-9958-c21f3342314f"/>  

# 내일배움캠프 8일차  
3주차 강의, 과제, 학습법 특강  
## 객체지향 프로그래밍 (Object-Oriented Programming, OOP) 특징  
1. 캡슐화 (Encapsulation) : 관련된 데이터와 기능을 하나의 단위로 묶는 것. 정보 은닉, 안정성, 보수
2. 상속 (Inheritance) : 기존의 클래스를 확장하여 새로운 클래스를 만드는 메커니즘
3. 다형성 (Polymorphism) : 하나의 메서드 이름이 오버로딩, 오버라이딩을 통해 다양한 객체에서 다르게 동작할 수 있다.
4. 추상화 (Abstraction) : 복잡한 시스템이나 개념을 단순화하여 필요한 기능에 집중하는 것
5. 객체 (Object) : 클래스로부터 생성된 실체. 각 객체는 독랍적인 상태를 가지고 있다. 즉 객체마다 고유한 데이터를 가질 수 있다.  

## 클래스  
데이터와 메서드를 하나로 묶은 사용자 정의 타입. 객체를 생성하기 위한 설계도 역할을 한다. 속성과 동작을 가지며, 속성은 필드, 동작은 메서드로 표현된다. 객체를 생성하기 위해서는 클래스를 사용하여 인스턴스를 생성해야 한다.  
클래스의 구성 요소 :  
1. 필드 (Fields) : 클래스에서 사용되는 변수
2. 메서드 (Methods) : 클래스에서 수행되는 동작을 정의
3. 생성자 (Constructors) : 객체를 초기화하는 역할. 객체가 생성될 때 자동으로 호출되며, 필드를 초기화하는 등의 작업을 수행한다.
4. 소멸자 (Destructors) : 객체가 소멸될 때 호출되는 메서드로, 메모리나 리소스 해제 등의 작업을 수행한다.
### 구조체 vs 클래스  
둘 다 사용자 정의 형식을 만드는 데 사용될 수 있다.  
구조체는 값 형식, 클래스는 참조 형식으로, 성능 측면에서 다소 차이가 있다. 구조체는 상속 받을 수 없지만, 클래스는 단일 및 다중 상속이 가능하다. 구조체는 작은 크기의 데이터, 클래스는 더 복잡한 객체를 표현하는 데 적합하다.  

### 프로퍼티  
클래스 멤버로서, 객체의 필드 값을 읽거나 설정하는데 사용되는 접근자(Accessor) 메서드의 조합이다. 객체의 필드에 직접 접근하지 않고, 간접적으로 값을 설정하거나 읽을 수 있도록 한다. 또한 필드에 대한 접근 제어와 데이터 유효성 검사를 수행할 수 있다. 프로퍼티는 필드와 마찬가지로
객체의 상태를 나타내는 데이터 역할을 하지만, 외부에서 접근할 때 추가적인 로직을 수행할 수 있다는 점에서 차이가 있다.  
```cs
class Person
{
    private string name;
    private int age;

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public int Age
    {
        get { return age; }
        set { age = value; }
    }
}
------------------------------------------
Person person = new Person();
person.Name = "John";   // Name 프로퍼티에 값 설정
person.Age = 25;        // Age 프로퍼티에 값 설정

Console.WriteLine($"Name: {person.Name}, Age: {person.Age}");  // Name과 Age 프로
```

## 상속  
기존의 클래스를 확장하거나 재사용하여 새로운 클래스를 생성하는 것.  
자식 클래스는 부모 클래스의 멤버를 상속받아 사용할 수 있다. 상속을 통해 코드를 재사용해 반복적인 코드 작성을 줄일 수 있고, 클래스 간의 계층 구조를 표현하여 코드 구조를 명확하게 표현할 수 있고, 기존 클래스의 수정이 필요한 경우 해당 클래스만 수정하면 되기 때문에 코드의 유지보수성이 향상된다.  
상속의 종류에는 단일 상속, 다중 상속, 인터페이스 상속이 있는데, C#의 경우 다중 상속은 지원하지 않는다. 하나의 자식 클래스가 하나의 부모 클래스만 상속받는 단일 상속만 가능하다. 하지만 인터페이스 상속의 경우 다중 상속이 가능하다.  
### 다형성 - 가상(Virtual) 메서드  
가상 메서드는 부모 클래스에서 정의되고 자식 클래스에서 재정의할 수 있는 메서드이다. 이를 통해 자식 클래스에서 부모 클래스의 메서드를 변경하거나 확장할 수 있다.  
```cs
public class Unit
{
    public virtual void Move()
    {
        Console.WriteLine("두발로 걷기");
    }

    public void Attack()
    {
        Console.WriteLine("Unit 공격");
    }
}

public class Marine : Unit
{

}

public class Zergling : Unit
{
    public override void Move()
    {
        Console.WriteLine("네발로 걷기");
    }
}
----------------------------------------------
// 사용 예시
// #1 참조형태와 실형태가 같을때
Marine marine = new Marine();
marine.Move();
marine.Attack();

Zergling zergling = new Zergling();
zergling.Move();
zergling.Attack();

// #2 참조형태와 실형태가 다를때
List<Unit> list = new List<Unit>();
list.Add(new Marine());
list.Add(new Zergling());

foreach (Unit unit in list)
{
    unit.Move();
}
```
### 추상(Abstract) 클래스와 메서드  
추상 클래스는 직접적으로 인스턴스를 생성할 수 없으며, 주로 상속을 위한 베이스 클래스로 사용된다.  
추상 메서드는 구현부가 없는 메서드로, 자식 클래스에서 반드시 구현되어야 한다.  
```cs
abstract class Shape
{
    public abstract void Draw();
}

class Circle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a circle");
    }
}

class Square : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a square");
    }
}

class Triangle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a triangle");
    }
}
-----------------------------------------
List<Shape> list = new List<Shape>();
list.Add(new Circle());
list.Add(new Square());
list.Add(new Triangle());

foreach (Shape shape in list )
{
    shape.Draw();
}
```
### 오버라이딩과 오버로딩  
오버라이딩(Overriding) : 부모 클래스에서 이미 정의된 메소드를 자식 클래스에서 재정의하는 것
    1. 상속 관계에 있는 클래스 간에 발생
    2. 메서드 이름, 매개변수, 반환타입이 동일해야 함
    3. 오버라이딩을 통해 자식 클래스는 부모 클래스의 메서드를 재정의하여 자신에게 맞는 동작을 구현할 수 있다.  
오버로딩(Overloading) : 동일한 메서드 이름을 가지고 있지만, 매개변수의 개수, 타입, 또는 순서가 다른 여러 개의 메소드를 정의하는 것  
    -> 이를 통해 동일한 이름을 가진 메서드를 다양한 매개변수 조합으로 호출할 수 있다. Ex) Console.WriteLine(),Console.WriteLine(string, int. ...

## 고급 문법 및 기능  
제너릭, out, ref  
### 제너릭  
클래스나 메서드를 일반화시켜 다양한 자료형에 대응할 수 있게 하는 기능이다.  코드의 재사용성을 높일 수 있다. 제너릭 클래스나 메서드에서 사용될 자료형은 선언 시점이 아니라 사용 시점에 결정된다.
제러릭 클래스나 메서드를 사용할 때는 <T>가 아니라 구체적인 자료형을 넣어준다.  
```cs
// 제너릭 클래스 선언 예시
class Stack<T>
{
    private T[] elements;
    private int top;

    public Stack()
    {
        elements = new T[100];
        top = 0;
    }

    public void Push(T item)
    {
        elements[top++] = item;
    }

    public T Pop()
    {
        return elements[--top];
    }
}

// 제너릭 클래스 사용 예시
Stack<int> intStack = new Stack<int>();
intStack.Push(1);
intStack.Push(2);
intStack.Push(3);
Console.WriteLine(intStack.Pop()); // 출력 결과: 3
```
### out, ref 키워드  
둘 다 메서드에서 매개변수를 전달할 때 사용한다.  
out 키워드는 메서드에서 반환값이 매개변수로 사용될 때, ref 키워드는 매개변수 값을 메서드 내에서 수정하여 원래 값에 영향을 줄 때 사용한다.   
#### out 매개변수는 이전의 값을 전부 무시하기 때문에 초기화가 필요없다. ref 매개변수는 사용하기 전에 초기화가 필요하다. 아님 할당되지 않았다는 컴파일 에러가 발생한다. 또한 너무 많이 사용하면 코드의 가독성이 떨어지고 유지보수가 어려워지니 주의하자.  
```cs
// out 키워드 사용 예시
void Divide(int a, int b, out int quotient, out int remainder)
{
    quotient = a / b;
    remainder = a % b;
}

int quotient, remainder;
Divide(7, 3, out quotient, out remainder);
Console.WriteLine($"{quotient}, {remainder}"); // 출력 결과: 2, 1

// ref 키워드 사용 예시
void Swap(ref int a, ref int b)
{
    int temp = a;
    a = b;
    b = temp;
}

int x = 1, y = 2;
Swap(ref x, ref y);
Console.WriteLine($"{x}, {y}"); // 출력 결과: 2, 1


```

## 학습법 특강  
1. 주특기를 가진 개발자
2. 레거시 코드 개선, 현업 실무 능력, 성장 잠재력, 라이브러리 활용력, 사고력
3. 원활하게 소통하며 헙업가능한 개발자  
프로젝트 하면서...문제 가설 대안 결정 : 기술적 의사결정 -> 이러이러하게 해결이 되더라
### 코더가 아닌 개발자
### 기술적 고민을 잘 하려면  
로직과 코드에 대한 의도 생각하기  
구현하는 기술, 스택에 목적과 근거 가지기  
더 좋은 방법이 있는지 고민하기   
### 협업을 잘 하려면  
예쁘게 말하기  
전달하고자 하는 바를 명확하게 말하기  
데이터 또는 기술적인 근거를 바탕으로 소통하기  
"옳은 말을 기분 좋게 하라. 당해낼 자가 없다"  
셀프 고민 2시간 넘어가면 그냥 물어보자  

## 실습 과제 - Snake Game   
요구 조건은 다음과 같다.  
1. 뱀은 매 턴마다 자신의 앞으로 이동합니다.  
2. 사용자는 방향키를 이용하여 뱀의 이동 방향을 제어할 수 있습니다.  
3. 뱀은 맵에 무작위로 생성되는 음식을 먹을 수 있습니다. 뱀이 음식을 먹으면 점수가 올라가고, 뱀의 길이가 늘어납니다.  
4. 뱀이 벽이나 자신의 몸에 부딪히면 게임이 끝나고 'Game Over' 메시지를 출력합니다.
     
처음에는 Stack 클래스를 활용하여 점의 좌표를 반복적으로 넣었다가 빼는 방식을 구현하려 했다. 하지만 커서에서 뱀을 출력하는 방향과 뱀이 이동하려는 방향이 늘 일치하는 것이 아니기 때문에, 뱀의 이동경로가 오른쪽이 아닌 경우
뱀의 모양이 늘 흐트러지는 버그가 발생했고, 이를 바로잡기가 쉽지 않았다. 그래서 커서 위치를 옮겨야겠다는 필요성을 느꼈다.
```cs
using System;
using System.Threading;

namespace SnakeGame
{
    internal class Program
    {
        public class Snake
        {
            private int yPos = 5;
            private int xPos = 5;
            private bool yMove = false;
            private bool xMove = false;
            
            bool IsHit {get;set;}
            int SnakeSize {get;set;}
            private Stack<int> xPoints = new Stack<int>();
            private Stack<int> yPoints = new Stack<int>();

            public Snake ( bool isHit, int snakeSize)
            {
                IsHit = isHit;
                SnakeSize = snakeSize;
            }

            public void FirstMove()
            {
                for (int y = 0; y < yPos; y++)
                {
                    Console.WriteLine();
                    yPoints.Push(yPos);
                }

                for (int i = 0; i < SnakeSize; i++) 
                { 
                    Console.Write(" *");
                    xPoints.Push(xPos +1 + i);
                    Thread.Sleep(500);
                }

            }

            public void SnakeMove()
            {
                Console.Clear();
                for (int i =0; i < SnakeSize;i++)
                {
                    yPos = yPoints.Pop();
                    if (yMove == false)
                    {
                        for (int y = 0; y < yPos; y++)
                        {

                            Console.WriteLine();
                            
                        }
                    }
                    yMove = true;
                    xPos = xPoints.Pop();
                    if (xMove == false)
                    {
                        for (int x = 0; x < xPos; x++)
                        {
                            Console.Write(" ");
                        }
                        
                    }
                    xMove = true;
                    Console.Write(" *");
                  

                }

                for (int i = 0; i < SnakeSize; i++)
                {
                    xPoints.Push(xPos  + i + 1);
                    yPoints.Push(yPos);
                }
                yMove = false;
                xMove = false;
                
            }



        }
        
        static void Main(string[] args)
        {
            bool isHit = false;
            int snakeSize = 4;
            Snake snake = new Snake(isHit, snakeSize);
            snake.FirstMove();
            while (true)
            {
                snake.SnakeMove();
                Thread.Sleep(500);
            }

        }
    }
}
```
내일 계속 매달려봐야겠다...


