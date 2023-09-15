# 내일배움캠프 27일차 TIL  
오전에 코드카타 후에 Unity Education Day 온라인 세미나를 대략 5시까지 시청했다. 이후 프로퍼티, 델레게이트, 이벤트, 람다식 등 C# 문법을 복습하는 시간을 가졌다.

## 코드카타 - 기억에 남는 문제  
# [unrated] 가장 가까운 같은 글자 - 142086 

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/142086) 

### 성능 요약

메모리: 32.3 MB, 시간: 2.59 ms

### 구분

코딩테스트 연습 > 연습문제

### 채점결과

Empty

### 문제 설명

<p>문자열 <code>s</code>가&nbsp;주어졌을 때, <code>s</code>의 각 위치마다 자신보다 앞에 나왔으면서, 자신과 가장 가까운 곳에 있는 같은 글자가 어디 있는지 알고 싶습니다.<br>
예를 들어, <code>s</code>="banana"라고 할 때,&nbsp; 각 글자들을 왼쪽부터 오른쪽으로 읽어 나가면서&nbsp;다음과 같이 진행할 수 있습니다.</p>

<ul>
<li>b는 처음 나왔기 때문에 자신의 앞에 같은 글자가 없습니다. 이는 -1로 표현합니다.</li>
<li>a는 처음 나왔기 때문에 자신의 앞에 같은 글자가 없습니다. 이는 -1로 표현합니다.</li>
<li>n은 처음 나왔기 때문에 자신의 앞에 같은 글자가 없습니다. 이는 -1로 표현합니다.</li>
<li>a는 자신보다 두 칸 앞에 a가 있습니다. 이는 2로 표현합니다.</li>
<li>n도&nbsp;자신보다 두 칸 앞에 n이 있습니다. 이는 2로 표현합니다.</li>
<li>a는 자신보다 두 칸, 네 칸 앞에 a가 있습니다. 이 중 가까운 것은 두 칸 앞이고, 이는 2로 표현합니다.</li>
</ul>

<p>따라서 최종 결과물은 [-1, -1, -1, 2, 2, 2]가 됩니다.</p>

<p>문자열 <code>s</code>이 주어질 때, 위와 같이 정의된 연산을 수행하는 함수 solution을 완성해주세요.</p>

<hr>

<h5>제한사항</h5>

<ul>
<li>1 ≤ <code>s</code>의 길이 ≤ 10,000

<ul>
<li><code>s</code>은 영어 소문자로만 이루어져 있습니다.</li>
</ul></li>
</ul>

<hr>

<h5>입출력 예</h5>
<table class="table">
        <thead><tr>
<th>s</th>
<th>result</th>
</tr>
</thead>
        <tbody><tr>
<td>"banana"</td>
<td>[-1, -1, -1, 2, 2, 2]</td>
</tr>
<tr>
<td>"foobar"</td>
<td>[-1, -1, 1, -1, -1, -1]</td>
</tr>
</tbody>
      </table>
<hr>

<h5>입출력 예 설명</h5>

<p>입출력 예 #1<br>
지문과 같습니다.</p>

<p>입출력 예 #2<br>
설명 생략</p>


> 출처: 프로그래머스 코딩 테스트 연습, https://programmers.co.kr/learn/challenges

입력받은 문자열을 한 글자씩 검사하면서, 중복여부 상관없이 순서대로 글자를 받는 리스트 하나와, 중복되는 문자를 추가하려 할 시 false를 반환하는 해시셋을 사용했다. 즉 true 일 시 answer 리스트에 -1을 넣어주고, false일 시 중복여부 상관없이 문자를 받았던 리스트를 맨 뒷순서부터 차례대로
검사하며 가장 가까운 같은 글자와의 인덱스 차이를 answer 리스트에 추가해주는 방식으로 구현했다. 지금 생각해보니 딕셔너리로 구현했다면, ContainsKey를 통해 중복 키가 있는지 여부를 검사하며 문자를 key로, 인덱스를 value로 저장하고, 중복되는 문자가 있을 시 저장하려 했던 인덱스와 원래 
해당 문자의 key로 저장되어 있는 인덱스와의 차를 answer 리스트에 추가하는 방식으로 구현했다면 컬렉션을 하나만 사용하므로 공간 복잡도 측면에서 더 효율적이었을 것 같다.  
```cs
using System;
using System.Collections.Generic;

public class Solution {
    public List<int> solution(string s) {
        List<int> answer = new List<int>();
        char[] array = s.ToCharArray();
        HashSet<string> letters = new HashSet<string>();
        List<string> tempList = new List<string>();
        
        for(int i = 0; i < array.Length; i++)
        {
            tempList.Add(array[i].ToString());
            if(letters.Add(array[i].ToString()))
            {
                answer.Add(-1);
            }
            else
            {
                for(int j = i-1; j >=0; j--)
                {
                    if(array[j]==array[i])
                    {
                        answer.Add(i-j);
                        break;
                    }
                }
            }
        }
        return answer;
    }
}
```

## Unity Education Day - Chat GPT를 활용해서 Unity로 게임 만들기  
유명 유튜버이신 골드메탈 님이 오늘 해당 주제로 진행해주신 세션이 가장 기억에 남는다.   
1. 게임 장르를 추천해달라고 물어본다.
2. 장르를 선택하고, 핵심 기능만 담은 게임 기획을 달라고 한다.
3. 필요한 그래픽 에셋 종류를 물어본다.
4. 애셋 준비 과정부터 절차대로 구체적인 행동을 물어본다.
5. 스크립트 적용시 잘못된 부분이 있다면 알려주고, 수정사항을 요구한다.
6. 질문시 "프롬프트"를 적극 활용하자.

## C# 문법 복습  
### 1. 프로퍼티   
  어떤 클래스에 public으로 선언된 필드가 있을 때, 해당 클래스로 생성된 객체들을 통해 자유롭게 이 필드에 접근할 수 있다. 때문에 만약 해당 필드에 잘못된 값이 들어간다면, 어디에서 잘못된 건지 알 방법이 없다. 이러한 문제를 해결하기 위해 프로퍼티를 사용한다. 프로퍼티는 객체지향 
  프로그래밍의 이점 중 하나인 은닉성에 기여한다. get은 프로퍼티로부터 값을 읽어올 때, set은 프로퍼티에 값을 쓸 때 사용된다. 필드는 private으로 선언하고, public 프로퍼티를 선언하고 set에서 아까 선언한 필드에 value를 저장하고 get에서 해당 필드에 저장된 값을 리턴하도록 할 수 있다. 
  이 과정은 필드를 선언하는 과정을 생략하고 자동 프로퍼티를 통해 간단히 구현할 수 있다. 만약 읽기만 가능하고 쓰기는 클래스 내에서만 이루어지게 하고 싶다면 get은 public으로 선언하되 set만 private으로 선언하는 것도 가능하다.   
  ```cs
public class Player
{
    public int Hp { get; private set;}
}
```

### 2. 델레게이트  
델레게이트는 "대리자"라는 뜻이다. 어떤 업체 사장님에게 용건이 있을 때, 우리는 사장님에게 직접 전화하지 않는다. 주로 비서같은 사람에게 우리 연락처나 용건을 남겨두고, 연락을 달라고 한다. 이를 코딩으로 비유하면, 넘겨주는 우리 연락처/용건은 어떤 함수/메서드이고, 그걸 받는 사람이 바로
델레게이트이다. 그리고 어떤 함수를 델레게이트에 전달하고, 델레게이트에서 해당 함수를 호출하는 방식을 콜백 방식이라고 한다. 델리게이트를 따로 선언하지 않고, 미리 사용할 수 있게 만들어진 델리게이트들이 있는데, 바로 Action과 Func이다. 입력받는 인자는 있는데 반환할 필요는 없는 경우 
Action, 입력도 받고 반환값도 필요한 경우 Func를 사용하면 된다. 다만 델레게이트는 그 자체로만 사용하면 언제 어디서나 불러올 수 있다는 단점이 있다. 따라서 외부에서는 호출되지 않고 내부에서만 호출되도록 막을 수 있는 방법이 있는데, 델리게이트 인스턴스를 생성할 때 앞에 event를 붙어주면
된다. 이벤트로 델레게이트를 생성하면 구독 신청 및 취소만 가능하며, 외부에서 이벤트를 직접 호출하는 것은 불가능하다. 또한 이렇게 이벤트를 생성하고 해당 이벤트에 메서드를 구독하는 방식을 옵저버 패턴이라 한다. 
```cs
using System;
namespace MySystem
{
    class MyClass
    {
        // 1. delegate 선언
        private delegate void RunDelegate(int i);

        private void RunThis(int val)
        {
            // 콘솔출력 : 1024
            Console.WriteLine("{0}", val);
        }

        private void RunThat(int value)
        {
            // 콘솔출력 : 0x400
            Console.WriteLine("0x{0:X}", value);
        }

        public void Perform()
        {
            // 2. delegate 인스턴스 생성
            RunDelegate run = new RunDelegate(RunThis);
            // 3. delegate 실행
            run(1024);

            //run = new RunDelegate(RunThat); 을 줄여서 
            //아래와 같이 쓸 수 있다.
            run = RunThat;

            run(1024);
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            MyClass mc = new MyClass();
            mc.Perform();
        }
    }
}

```

### 3. 람다식    
람다식은 무명 함수/메서드와 비슷한데, C# 3.0버전 이후 이를 보다 더 간단히 표현하기 위해 등장한 문법이다. 람다식을 활용하면 이벤트와 델레게이트를 사용할 때 코드를 더 간결히 표현할 수 있다. (입력 파라미) => (실행문장 블럭)의 구조로 이루어져 있다.  
```cs
//버튼 클릭 이벤트에 메서드 추가. 추가한 메서드에 대해 클래스 내부 어디선가 정의해야 한다.
this.button1.Click += button1_Click;
private void button1_Click(object sender, EventArgs e)
{
   ((Button)sender).BackColor = Color.Red;
}

//위 코드를 무명 메서드로 간단히 한 것
this.button1.Click += delegate(object sender, EventArgs e)
{
   ((Button)sender).BackColor = Color.Red;
};

//무명 메서드를 람다식으로 바꿔 보다 더 간단히 표현
this.button1.Click += (sender, e) => ((Button)sender).BackColor = Color.Red;
```
