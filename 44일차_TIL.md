# 내일배움캠프 44일차 TIL  
오전에 코드카타 후,  팀 프로젝트 개발을 진행했다. 저녁에는 디자인 패턴 중 빌더 패턴에 대한 특강이 있었다.  

## 코드카타 - 기억에 남는 문제    
# [level 1] 체육복 - 42862 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/42862) 

### 성능 요약

메모리: 31.6 MB, 시간: 3.32 ms

### 구분

코딩테스트 연습 > 탐욕법（Greedy）

### 채점결과

정확성: 100.0<br/>합계: 100.0 / 100.0

### 제출 일자

2023년 10월 1일 18:13:27

### 문제 설명

<p>점심시간에 도둑이 들어, 일부 학생이 체육복을 도난당했습니다. 다행히 여벌 체육복이 있는 학생이 이들에게 체육복을 빌려주려 합니다. 학생들의 번호는 체격 순으로 매겨져 있어, 바로 앞번호의 학생이나 바로 뒷번호의 학생에게만 체육복을 빌려줄 수 있습니다. 예를 들어, 4번 학생은 3번 학생이나 5번 학생에게만 체육복을 빌려줄 수 있습니다. 체육복이 없으면 수업을 들을 수 없기 때문에 체육복을 적절히 빌려 최대한 많은 학생이 체육수업을 들어야 합니다.</p>

<p>전체 학생의 수 n, 체육복을 도난당한 학생들의 번호가 담긴 배열 lost, 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열 reserve가 매개변수로 주어질 때, 체육수업을 들을 수 있는 학생의 최댓값을 return 하도록 solution 함수를 작성해주세요.</p>

<h5>제한사항</h5>

<ul>
<li>전체 학생의 수는 2명 이상 30명 이하입니다.</li>
<li>체육복을 도난당한 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.</li>
<li>여벌의 체육복을 가져온 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.</li>
<li>여벌 체육복이 있는 학생만 다른 학생에게 체육복을 빌려줄 수 있습니다.</li>
<li>여벌 체육복을 가져온 학생이 체육복을 도난당했을 수 있습니다. 이때 이 학생은 체육복을 하나만 도난당했다고 가정하며, 남은 체육복이 하나이기에 다른 학생에게는 체육복을 빌려줄 수 없습니다.</li>
</ul>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>n</th>
<th>lost</th>
<th>reserve</th>
<th>return</th>
</tr>
</thead>
        <tbody><tr>
<td>5</td>
<td>[2, 4]</td>
<td>[1, 3, 5]</td>
<td>5</td>
</tr>
<tr>
<td>5</td>
<td>[2, 4]</td>
<td>[3]</td>
<td>4</td>
</tr>
<tr>
<td>3</td>
<td>[3]</td>
<td>[1]</td>
<td>2</td>
</tr>
</tbody>
      </table>
<h5>입출력 예 설명</h5>

<p>예제 #1<br>
1번 학생이 2번 학생에게 체육복을 빌려주고, 3번 학생이나 5번 학생이 4번 학생에게 체육복을 빌려주면 학생 5명이 체육수업을 들을 수 있습니다.</p>

<p>예제 #2<br>
3번 학생이 2번 학생이나 4번 학생에게 체육복을 빌려주면 학생 4명이 체육수업을 들을 수 있습니다.</p>

> 출처: 프로그래머스 코딩 테스트 연습, https://school.programmers.co.kr/learn/challenges  
  
  
      
  
먼저 전체 학생들의 체육복 지참 여부를 담은 배열과, 전체 학생들 중 여벌의 체육복 보유 여부를 담은 배열을 만들었다. 그리고 체육복을 잃어버린 학생이 앞 또는 뒤에있는 학생에게 체육복을 빌려서 해당 정보를 두 배열에 반영하는 방식으로 구현했다.  
```cs
using System;
using System.Linq;

public class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int answer = 0;
        bool[] isGood = new bool[n];
        bool[] isRich = new bool[n];

        for(int i = 0; i < n; i++)
        {
            isGood[i] = true;
        }
        for(int i = 0;i < lost.Length; i++)
        {
            isGood[lost[i]-1] = false;
        }
        foreach(int heroNum in reserve)
        {
            if(!isGood[heroNum-1])
            {
                isGood[heroNum-1] = true;
                continue;
            }            
            isRich[heroNum-1] = true;     
        }
        
        lost = lost.OrderBy(x => x).ToArray();
        foreach(int lostStudent in lost)
        {
            if(isGood[lostStudent-1])
                continue;
            else if(lostStudent - 2 >= 0 && isRich[lostStudent-2])
            {
                isGood[lostStudent-1] = true;
                isRich[lostStudent-2] = false;
                continue;
            }  
            else if(lostStudent < n && isRich[lostStudent])
            {
                isGood[lostStudent-1] = true;
                isRich[lostStudent] = false;
                continue;
            }      
        }
        
        foreach(bool studentReady in isGood)
        {
            if(studentReady)
            {
                answer++;
            }
        }
        return answer;
    }
}
```  

## 빌더 패턴  
객체를 생성할 때, 클래스의 생성자에 매개변수가 다수 존재한다면, 우리는 어떤 순서에 어떤 매개변수를 넣어줘야 하는지 일일이 기억해서 넣어주어야 할 것이다. 이런 방식이라면 매개변수 개수가 늘어날 때마다 개발 난이도가 상승할 것이다. 이를 해결하기 위한 방법으로 생성자를 오버로딩하거나, 필수 초기화 변수 외 변수들을 위한 Setter 메서드들을 일일이 만들어주는 방법이 있을 것이다. 하지만 이러한 방법들도 좋은 방법은 아니다. 생성자를 오버로딩하는 방법은 매개변수가 증가함에 따라 생성자의 종류도 늘어나기 때문에 가독성이나 유지보수 측면에서 좋지 않고, Setter 메서드를 만드는 경우는 객체 생성 시점에 모든 값들을 주입하지 않아 일관성 문제와 불변성 문제가 발생할 수 있다. 객체가 생성되는 도중 오류가 발생한다면 일부 변수는 초기화가 되지 않은 상태로 불안정한 상태의 객체가 만들어지는 것이다. 또한 객체가 만들어진 이후에도 여전히 외부적으로 Setter 메서드를 노출하고 있으므로 외부에서 함부로 객체를 조작할 수 있다. 이러한 문제들의 해결 방안으로 빌더 패턴이 등장했다.   

빌더 패턴은 별도의 빌더 클래스를 만들어 필수 값들의 경우는 생성자를 통해, 선택적인 값들의 경우는 메서드를 통해 값을 입력받은 후 최종적으로 빌더 메서드를 호출하여 하나의 인스턴스를 반환하는 방식이다.  
### 1. 자동차 클래스 작성  
```cs
using System;

public class Car
{
    private string steeringWheel;
    private int tires;
    private string engine;
    private bool sunroof;
    private bool navigation;
    private bool blackbox;

    public Car(string steeringWheel, int tires, string engine)
    {
        this.steeringWheel = steeringWheel;
        this.tires = tires;
        this.engine = engine;
    }

    public override string ToString()
    {
        return $"Steering Wheel: {steeringWheel}, Tires: {tires}, Engine: {engine}, Sunroof: {sunroof}, Navigation: {navigation}, Blackbox: {blackbox}";
    }
}
```
### 2. 빌더 클래스 작성   
```cs
public class Builder
{
    private string steeringWheel;
    private int tires;
    private string engine;

    private bool sunroof = false;
    private bool navigation = false;
    private bool blackbox = false;

    public Builder(string steeringWheel, int tires, string engine)
    {
        this.steeringWheel = steeringWheel;
        this.tires = tires;
        this.engine = engine;
    }

    public Builder WithSunroof(bool sunroof)
    {
        this.sunroof = sunroof;
        return this;
    }

    public Builder WithNavigation(bool navigation)
    {
        this.navigation = navigation;
        return this;
    }

    public Builder WithBlackbox(bool blackbox)
    {
        this.blackbox = blackbox;
        return this;
    }

    public Car Build()
    {
        return new Car(steeringWheel, tires, engine)
        {
            sunroof = this.sunroof,
            navigation = this.navigation,
            blackbox = this.blackbox
        };
    }
}
```
### 3. 프로그램 실행  
```cs
class Program
{
    static void Main()
    {
        Car car1 = new Car.Builder("Leather", 4, "V6")
            .WithSunroof(true)
            .WithNavigation(true)
            .WithBlackbox(true)
            .Build();

        Car car2 = new Car.Builder("Standard", 4, "Inline-4")
            .WithSunroof(true)
            .WithNavigation(false)
            .WithBlackbox(false)
            .Build();

        Console.WriteLine("Car 1 Details:");
        Console.WriteLine(car1);

        Console.WriteLine("Car 2 Details:");
        Console.WriteLine(car2);
    }
}
```

프로그램 실행 시 객체 생성은 여러가지 메서드들을 체이닝하여 이루어지기 때문에, 객체 생성 시점에 모든 작업이 이루어지므로 앞서 언급된 일관성, 불변성 문제가 발생하지 않게 된다. 초기화가 필수인 멤버는 생성자로, 선택적인 멤버는 빌더의 메서드로 받기 때문에, 사용자로 하여금 필수 멤버와 선택 멤버를 구분하여 객체를 생성하도록 유도할 수 있다. 하지만 빌더 패턴을 적용하려면 각 클래서마다 새로운 빌더 클래스를 만들어야 해서 클래스 수가 늘어날수록 관리해야할 클래서가 많아지고 구조가 복잡해질 수 있다는 단점이 있다.  
