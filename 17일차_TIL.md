# 내일배움캠프 17일차 TIL
오늘도 어김없이 코드카타 후 팀 프로젝트 개발 마무리 및 디버깅을 진행했다. 
​
## **_오늘 푼 문제들 :_** 
​
### **프로그래머스 lv.1 - 12919. 서울에서 김서방 찾기**
​
**문제 설명**  
String형 배열 seoul의 element중 "Kim"의 위치 x를 찾아, "김서방은 x에 있다"는 String을 반환하는 함수, solution을 완성하세요. seoul에 "Kim"은 오직 한 번만 나타나며 잘못된 값이 입력되는 경우는 없습니다.  
  
**제한 사항**  
\- seoul은 길이 1 이상, 1000 이하인 배열입니다.  
\- seoul의 원소는 길이 1 이상, 20 이하인 문자열입니다.  
\- "Kim"은 반드시 seoul 안에 포함되어 있습니다.
​
```cs
public class Solution {
    public string solution(string[] seoul) {
        int i = 0;
        foreach (string str in seoul)
        {
            if(str == "Kim")
            {
                break;
            }
            i++;
        }
        string answer = $"김서방은 {i}에 있다";
        return answer;
    }
}
```
​
### ****프로그래머스 lv.1 - 12910. 나누어 떨어지는 숫자 배열****
​
**문제 설명**  
array의 각 element 중 divisor로 나누어 떨어지는 값을 오름차순으로 정렬한 배열을 반환하는 함수, solution을 작성해주세요. divisor로 나누어 떨어지는 element가 하나도 없다면 배열에 -1을 담아 반환하세요.  
  
**제한 사항**  
\- arr은 자연수를 담은 배열입니다.  
\- 정수 i, j에 대해 i ≠ j 이면 arr\[i\] ≠ arr\[j\] 입니다.  
\- divisor는 자연수입니다.  
\- array는 길이 1 이상인 배열입니다.
​
```cs
using System.Collections.Generic;
public class Solution {
    public int[] solution(int[] arr, int divisor) {
        List<int> answer = new List<int>();
        foreach (int num in arr)
        {
            if (num % divisor == 0)
            {
                answer.Add(num);
            }
        }
        if (answer.Count == 0)
        {
            answer.Add(-1);
        }
        answer.Sort();
        return answer.ToArray();
    }
}
```
​
### **프로그래머스 lv.1 - 76501. 음양 더하기**
​
**문제 설명**  
어떤 정수들이 있습니다. 이 정수들의 절댓값을 차례대로 담은 정수 배열 absolutes와 이 정수들의 부호를 차례대로 담은 불리언 배열 signs가 매개변수로 주어집니다. 실제 정수들의 합을 구하여 return 하록 solution 함수를 완성해주세요.  
  
**제한 사항**  
\- absolutes의 길이는 1 이상 1,000 이하입니다.  
\- absolutes의 모든 수는 각각 1 이상 1,000 이하입니다.  
\- signs의 길이는 absolutes의 길이와 같습니다.  
\- signs\[i\] 가 참이면 absolutes\[i\] 의 실제 정수가 양수임을, 그렇지 않으면 음수임을 의미합니다.
​
```cs
using System;
​
public class Solution {
    public int solution(int[] absolutes, bool[] signs) {
        int answer = 0;
        for(int i = 0; i < absolutes.Length; i++)
        {
            if (signs[i] == true)
            {
                answer += absolutes[i];
            }
            else answer -= absolutes[i];
        }
        return answer;
    }
}
```
​
### ****프로그래머스 lv.1 - 12948. 핸드폰 번호 가리기****
​
**문제 설명**  
프로그래머스 모바일은 개인정보 보호를 위해 고지서를 보낼 때 고객들의 전화번호의 일부를 가립니다.  
전화번호가 문자열 phone\_number로 주어졌을 때, 전화번호의 뒷 4자리를 제외한 나머지 숫자를 전부 \*으로 가린 문자열을 리턴하는 함수, solution을 완성해주세요.  
  
**제한 사항**  
phone\_number는 길이 4 이상, 20이하인 문자열입니다.
​
```cs
public class Solution {
    public string solution(string phone_number) {
        string answer = "";
        char[] arr = phone_number.ToCharArray();
        for (int i = 0; i < arr.Length ; i ++)
        {
            if (i < arr.Length - 4)
            {
                arr[i] = char.Parse("*");
            }            
            answer += arr[i].ToString();
        }
        
        return answer;
    }
}
```
​
### ****프로그래머스 lv.1 - 86051. 없는 숫자 더하기****
​
**문제 설명**  
0부터 9까지의 숫자 중 일부가 들어있는 정수 배열 numbers가 매개변수로 주어집니다. numbers에서 찾을 수 없는 0부터 9까지의 숫자를 모두 찾아 더한 수를 return 하도록 solution 함수를 완성해주세요.  
  
**제한 사항**  
\- 1 ≤ numbers의 길이 ≤ 9  
\- 0 ≤ numbers의 모든 원소 ≤ 9  
\- numbers의 모든 원소는 서로 다릅니다.
​
```cs
using System;
​
public class Solution {
    public int solution(int[] numbers) {
        int answer = 0;
        bool isIn = false;
        for (int i = 0 ; i < 10; i ++)
        {
            isIn = false;
            foreach(int num in numbers)
            {
                if (i == num)
                {
                    isIn = true;
                    break;
                }                   
            }
            if (isIn == false)
                answer += i;
        }
        return answer;
    }
}
```
​
### ****프로그래머스 lv.1 - 12935. 제일 작은 수 제거하기****
​
**문제 설명**  
정수를 저장한 배열, arr 에서 가장 작은 수를 제거한 배열을 리턴하는 함수, solution을 완성해주세요. 단, 리턴하려는 배열이 빈 배열인 경우엔 배열에 -1을 채워 리턴하세요. 예를들어 arr이 \[4,3,2,1\]인 경우는 \[4,3,2\]를 리턴 하고, \[10\]면 \[-1\]을 리턴 합니다.  
  
**제한 사항**  
\- arr은 길이 1 이상인 배열입니다.  
\- 인덱스 i, j에 대해 i ≠ j이면 arr\[i\] ≠ arr\[j\] 입니다.
​
```cs
using System.Collections.Generic;
using System.Linq;
public class Solution {
    public int[] solution(int[] arr) {
        List<int> answer = arr.ToList();
        int minNum = 0;
        for (int i = 0; i < answer.Count; i++)
        {
            if (i == 0)
                minNum = answer[i];
            else
            {
                if(answer[i]<minNum)
                   minNum = answer[i];
                else continue;
            }
        }
        answer.Remove(minNum);
        if (answer.Count == 0)
        {
            answer.Add(-1);
        }
        return answer.ToArray();
    }
}
```
​
### ****프로그래머스 lv.1 - 12903. 가운데 글자 가져오기****
​
**문제 설명**  
단어 s의 가운데 글자를 반환하는 함수, solution을 만들어 보세요. 단어의 길이가 짝수라면 가운데 두글자를 반환하면 됩니다.  
  
**제한 사항**  
\- s는 길이가 1 이상, 100이하인 스트링입니다.
​
```cs
public class Solution {
    public string solution(string s) {
        string answer = "";
        char[] arr = s.ToCharArray();
        if (arr.Length % 2 == 0)
        {
            answer += arr[(arr.Length-2)/2];
            answer += arr[(arr.Length-2)/2 + 1];
        }
        else
        {
            answer += arr[(arr.Length +1)/2 - 1];
        }
        return answer;
    }
}
```
​
### **Text RPG 버그 : 세이브 파일 로드/상점에서 아이템 구매 후, 인벤토리 진입시 예외 발생**
​
오늘 인벤토리 씬 개발을 완료 후 메인에 머지하고, 편안한 마음으로 기능 테스트를 하다가 슬프게도 발견한 버그다. 예외 메시지를 확인해보니, Item 클래스의 객체를 Item을 상속받은 하위 클래스인 Weapon 클래스로 다운캐스팅할 수 없다고 했다. 재미있게도 캐릭터 생성시 기본적으로 인벤토리에 추가되도록 한 기본 아이템들은 이러한 문제가 발생하지 않았다. 그래서 인벤토리 씬 클래스의 문제는 아니라고 판단했다. 아래는 플레이어 인벤토리의 아이템들을 검사하여 각각 맞는 형식으로 다운캐스팅한 뒤 하위 클래스의 프로퍼티에 접근하는 코드이다.
​
```cs
 private string PrintItemInfo(Item item)
        {
            int nameLength;
            byte[] data;
            int blank;
            string message = "";
            if (item == null) return message; 
            switch (item.Type)
            {
                case Define.GameEnum.eItemType.Weapon:
                    data = Encoding.Unicode.GetBytes($"ATK +{((Weapon)item).ATK}");
                    nameLength = data.Length;
                    blank = (30 - nameLength) / 2;
​
                    for (int i = 0; i < 2; i++)
                    {
                        for (int j = 0; j < blank; j++)
                        {
                            message += " ";
                        }
                        if (i == 1)
                            break;
                        message += $"ATK +{((Weapon)item).ATK}  CRT +{((Weapon)item).CRT}";
                    }
                    break;
                case Define.GameEnum.eItemType.Armor:
                    data = Encoding.Unicode.GetBytes($"ATK +{((Armor)item).DEF}");
                    nameLength = data.Length;
                    blank = (30 - nameLength) / 2;
​
                    for (int i = 0; i < 2; i++)
                    {
                        for (int j = 0; j < blank; j++)
                        {
                            message += " ";
                        }
                        if (i == 1)
                            break;
                        message += $"DEF +{((Armor)item).DEF}  AVD +{((Armor)item).AVD}";
                    }
                    break;
                case Define.GameEnum.eItemType.Potion:
                    data = Encoding.Unicode.GetBytes($"회복량  30");
                    nameLength = data.Length;
                    blank = (38 - nameLength) / 2;
​
                    for (int i = 0; i < 2; i++)
                    {
                        for (int j = 0; j < blank; j++)
                        {
                            message += " ";
                        }
                        if (i == 1)
                            break;
                        message += $"회복량 +30";
                    }
                    break;
​
            }
​
            message += $"|\t{item.Description}\n";
        
            return message;
        }
```
​
다운캐스팅을 한 이유는 Item 클래스에는 ATK나 DEF 프로퍼티가 없기 때문이다. ATK는 Item의 하위 클래스인 Weapon에, DEF는 Armor에만 있다. 이렇게 설정한 이유는 각각의 하위 클래스에서 다른 하위 클래스에 있는 프로퍼티를 굳이 필요로 하지 않기 때문이다. 하지만 이 경우 아이템을 각각의 하위 클래스 생성자가 아닌, Item의 생성자를 사용해서 생성하게 되면 다운 캐스팅이 불가능하다는 사실을 알게 되었다. Weapon과 Armor 생성자를 이용해 만들어진 기본 아이템들을 제외하고, 상점에서 생성된 아이템들이나 세이브 로드 시 만들어지는 아이템들은 Item의 생성자를 사용해서 만들어지기 때문에 다운캐스팅이 불가능했던 것이었다. 다운캐스팅을 가능하게 하기 위해서 Item 생성자를 사용해 만들어졌던 아이템들을 각각 Weapon과 Armor 생성자를 사용해서 생성하려는 시도를 해보았으나,  이 경우 열거형으로 저장되는 아이템의 Type 프로퍼티를 검사하여 각각의 생성자로 새로운 객체를 생성하는 방법밖에 떠오르지 않았고, 이 경우 Store 클래스와 SaveLoad 클래스의 코드가 굉장히 복잡해졌다. 그래서 과감히 다운캐스팅 자체를 하지 않는 것이 낫다고 판단했다. 그러하기 위해서는 Item클래스 자체에 ATK와 DEF 프로퍼티가 필요했기 때문에, 두 프로퍼티를 추가해 주었고, 모든 아이템들을 생성할 때 Item  생성자를 사용하도록 했다. 이 방법으로 버그를 고칠 수 있었다.
​
### **Text RPG 개발 - 팀 프로젝트 회고**
​
기능별로 패키지 구조를 미리 짜고 개발하는 방식이 처음이라서 초반에 난항을 많이 겪었다. 하지만 익숙해지고 나니 확실히 개발을 시작하기 전에 다이어그램과 와이어프레임을 미리 만들어 두는 것이 나중에 개발하기 수월하다는 것을 알게 되었다. 또한 파일을 나눠서 개발하는 것이 추후에 디버깅할 때도 편리하게 작용했다. 초반에 발생했던 버그 중에, 다른 맵으로 이동했는데도 불구하고 맵 이동을 담당하는 SceneManager 객체가 현재 맵을 기본 시작 맵으로 가지고 있는 버그가 있었다. 현재 맵은 열거형으로 맵 이동시마다 SceneManager 안에서 변경하는 방식으로 구현되어 있었는데, 다른 맵으로 이동하기 위해 SceneManager에 있는 메서드를 호출하면, 엉뚱하게도 현재 맵을 저장하는 변수에 기본 맵에 해당하는 값이 들어있었다. 지금의 나는 CS지식이 부족하여 아직까지도 정확한 원인 파악을 하지 못했으나, 예상하기로는 호출 스택의 문제인 것 같았다. 그래서 현재 맵을 저장하는 변수를 SceneManager 안에서 조작하는 것이 아니라, 이를 다른 맵들을 호출할 때 참조로 넘겨서 각 맵에서 현재 맵으로 설정하는 방식으로 변경했더니 정상적으로 작동하는 것을 확인했다. 이러한 방식으로 디버깅을 성공할 수 있었던 것은, 처음 개발할 때 각 맵들과 GameManager, SceneManager 등을 각각 다른 스크립트에 작성해서 어떤 곳에서 문제가 발생했는지 한 눈에 파악 할 수 있었기 때문이 아닌가 싶다. 앞으로도 개발할 때 패키지 구조를 효율적으로 나눠 설계하고 개발을 시작할 수 있도록 노력해야겠다.
