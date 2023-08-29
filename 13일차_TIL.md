<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/a458479d-5fa7-4b28-9552-fb14ff410312)"/>  

# 내일배움캠프 13일차   
코드카타, BlackJack, C# 문법종합반 4주차  

오늘은 새로운 팀 편성과 함께 팀 프로젝트 발제가 있었다. 팀원들 모두 C# 문법종합반 강의 수강을 완료하지 못했던 터라 오늘까지는 각자 강의 수강에 집중하고 내일 본격적인 계획 수립 및 개발을 시작하기로 했다.  
## 코드카타 - 자릿수 더하기  
받은 정수형을 문자열로 변환하고, String.Substring 메서드를 활용하여 한 자리씩 정수로 바꿔서 새로운 정수형 배열에 추가해주고, 이 배열 내의 숫자들의 합을 구하는 방식으로 구현했다. Substring 메서드가 아닌 ToCharArray 메서드를 활용했다면 더 간결했을 것 같다.  
```cs
using System;

public class Solution {
    public int solution(int n) {
        string numString = n.ToString();
        string[] stringList = new string[numString.Length];
        for (int i = 0; i < numString.Length; i ++)
        {
            stringList[i] = numString.Substring(i,1);
        }
        /*
        n을 스트링으로 변환하고 String.Substring 메서드를 활용하여 스트링 배열에 한 글자씩 넣어주는 방식을 사용했는데,  
        String.ToCharArray 메서드를 활용했다면 더 깔끔했을 것 같다.
        */
        int[] numList = new int[stringList.Length];
        for (int i = 0; i < stringList.Length; i++)
        {
            numList[i] = int.Parse(stringList[i]);
        }
        int sum= 0;
        foreach (int num in numList)
        {
            sum += num;
        }
        int answer = sum;
        return answer;
    }
}
```
## BlackJack 게임 만들기  
저번 특강에서 배웠던 것처럼 본격적인 개발을 시작하기에 앞서 와이어프레임을 먼저 만들었다(만들고 나서 보니 엉성한 다이어그램이나 구조도에 가깝지만...). 만들어야 할 클래스들과 각 클래스 안에 들어갈 변수들과 메서드 등을 정리해보았다.  
<img width="100%" src="https://github.com/kminsmin/Algorithm_Test_Practice/assets/114645806/709dd8c7-ef41-4a2d-8489-af881ba46603"/>   
게임이 시작되면 Deck 클래스의 인스턴스를 만들어주고, 이 인스턴스의 빈 리스트 안에 Deck의 LoadDeck 메서드를 활용하여 52장의 카드를 넣어준다. 이후 플레이어와 딜러가 차례대로 해당 카드의 리스트에서 랜덤으로 2장씩 카드를 뽑는다. 이 과정은 Hand 클래스의 
DrawCard 메서드를 사용한다. DrawCard 메서드는 Hand 클래스 내에서 virtual로 선언되어 있다. 기본적으로 뽑는 모든 카드를 출력하게 되어 있지만, 딜러의 카드는 한장만 공개해야 하므로 Hand를 상속받은 딜러 클래서에서 DrawCard 메서드를 Override 해서 사용한다.   
```cs
using System;
using System.Threading;
using static BlackJack.Program;

namespace BlackJack
{
    internal class Program
    {
        public class Card
        {
            public string cardSuit;
            public int cardValue;
            public string cardName;

            public Card(int value, string suit)
            {
                cardSuit = suit;
                cardValue = value;
                cardName = value.ToString() + " of " + cardSuit;
            }
            public Card(string value, string suit)
            {
                cardSuit = suit;
                if (value == "A")
                {
                    cardValue = 11;
                }
                else
                {
                    cardValue = 10;
                }
                cardName = value + " of " + cardSuit;
            }
        }

        public class Deck
        {
            public List<Card> cards;

            public Deck()
            {
                cards = new List<Card>();
            }

            public static List<Card> LoadDeck(List<Card> _deck, string suit)
            {
                List<Card> deck = _deck;
                for (int i = 2; i <= 10; i++)
                {
                    deck.Add(new Card(i, suit));
                }
                deck.Add(new Card("J", suit));
                deck.Add(new Card("Q", suit));
                deck.Add(new Card("K", suit));
                deck.Add(new Card("A", suit));
                return deck;
            }
        }

        public abstract class Hand
        {
            public List<Card> handCards;
            public virtual Card DrawCard(ref List<Card> deck)
            {
                int cardIndex = new Random().Next(deck.Count);
                Card card = deck[cardIndex];
                deck.Remove(deck[cardIndex]);
                Console.WriteLine($"뽑은 카드는 {card.cardName} 입니다.");
                return card;
            }
        }

        public class Player : Hand
        {
            public int score;
            public Player()
            {
                handCards = new List<Card>();
                score = 0;
            }
        }

        public class Dealer : Hand
        {
            public int score;

            public override Card DrawCard(ref List<Card> deck)
            {
                int cardIndex = new Random().Next(deck.Count);
                Card card = deck[cardIndex];
                deck.Remove(deck[cardIndex]);
                return card;
            }

            public Dealer()
            {
                handCards = new List<Card>();
                score = 0;
            }
        }

        static void Main(string[] args)
        {
            Console.WriteLine("블랙잭에 오신 걸 환영합니다.");
            Thread.Sleep(1000);
            Console.WriteLine("블랙잭은 점수의 합을 21점 가까이 맞추면 이기는 게임입니다.");
            Thread.Sleep(1000);
            Console.WriteLine("게임을 준비합니다.");
            //덱에 카드 넣기
            Deck deck = new Deck();
            deck.cards = Deck.LoadDeck(deck.cards, "Spade");
            deck.cards = Deck.LoadDeck(deck.cards, "Diamond");
            deck.cards = Deck.LoadDeck(deck.cards, "Club");
            deck.cards = Deck.LoadDeck(deck.cards, "Heart");

            //플레이어 패 뽑기
            Console.WriteLine("플레이어의 카드를 덱에서 뽑습니다.");
            Player player = new Player();
            player.handCards.Add(new Player().DrawCard(ref deck.cards));
            player.handCards.Add(new Player().DrawCard(ref deck.cards));
            Console.WriteLine($"당신의 카드는 {player.handCards[0].cardName}, {player.handCards[1].cardName} 입니다.");
            foreach (Card card in player.handCards)
            {
                player.score += card.cardValue;
            }
            Console.WriteLine($"현재 플레이어의 점수는 {player.score}점 입니다.\n");
            Thread.Sleep(1000);

            //딜러 패 뽑기
            Console.WriteLine("딜러의 카드를 덱에서 뽑습니다.");
            Dealer dealer = new Dealer();
            dealer.handCards.Add(new Dealer().DrawCard(ref deck.cards));
            dealer.handCards.Add(new Dealer().DrawCard(ref deck.cards));
            Console.WriteLine($"딜러의 카드 중 하나는 {dealer.handCards[0].cardName} 입니다.");
            foreach (Card card in dealer.handCards)
            {
                dealer.score += card.cardValue;
            }
            Thread.Sleep(1000);

            while (true)
            {
                if (player.score < 21)
                {
                    Console.Write("Hit 하시겠습니까 (y/n) ? : ");
                    string playerChoice = Console.ReadLine();
                    if (playerChoice == "y")
                    {
                        Console.WriteLine("플레이어의 카드를 덱에서 뽑습니다.");
                        Thread.Sleep(1000);
                        player.handCards.Add(new Player().DrawCard(ref deck.cards));
                        player.score += player.handCards[player.handCards.Count - 1].cardValue;
                        if (player.score > 21)
                        {
                            foreach (Card card in player.handCards)
                            {
                                if (card.cardValue == 11)
                                {
                                    card.cardValue = 1;
                                    player.score -= 10;
                                    break;
                                }
                            }
                            if (player.score > 21)
                            {
                                Console.WriteLine("Busted!");
                                break;
                            }
                        }
                        else if (player.score == 21)
                        {
                            Console.WriteLine("BlackJack!");
                            break;
                        }
                        Console.WriteLine($"현재 플레이어의 점수는 {player.score}점 입니다.\n");
                    }
                    else if (playerChoice == "n")
                    {
                        Console.WriteLine("플레이어가 Stay 합니다.");
                        break;
                    }
                    else
                    {
                        Console.WriteLine("잘못된 입력입니다!");
                    }
                }                
            }
            Console.WriteLine($"딜러의 나머지 카드는 {dealer.handCards[1].cardName} 입니다.");
            Console.WriteLine($"딜러의 점수는 {dealer.score} 입니다.\n");
            Thread.Sleep(1000);
            while (dealer.score < 17)
            {
                Console.WriteLine("딜러의 점수가 17보다 낮으므로 딜러가 Hit 합니다.");
                dealer.handCards.Add(new Player().DrawCard(ref deck.cards));
                dealer.score += dealer.handCards[dealer.handCards.Count - 1].cardValue;
                if (dealer.score > 21)
                {
                    foreach (Card card in dealer.handCards)
                    {
                        if (card.cardValue == 11)
                        {
                            card.cardValue = 1;
                            dealer.score -= 10;
                            break;
                        }
                    }
                    if (dealer.score > 21)
                    {
                        Console.WriteLine("Busted!");
                        break;
                    }
                }
                else if (dealer.score == 21)
                {
                    Console.WriteLine("BlackJack!");
                    break;
                }
                Console.WriteLine($"딜러의 점수는 {dealer.score} 입니다.\n");
            }

            if ((dealer.score > player.score)&&dealer.score <= 21 || player.score > 21)
            {
                Console.WriteLine("패배했습니다...");
            }
            else if (dealer.score < player.score || dealer.score > 21)
            {
                Console.WriteLine("승리했습니다!");
            }
            else Console.WriteLine("무승부입니다.");
        }
    }
}
```
## 팀프로젝트 회의 - 와이어프레임 제작  
내일부터 본격적인 계획 수립 및 개발에 돌입하기에 앞서, 오늘 와이어프레임을 먼저 제작하기로 했다. 우리가 구현하고자 하는 기능들을 어떻게 나타낼지를 생각하며 전체적인 흐름도를 작성해보았다. 과제의 필수 요구사항과 
선택 요구사항도 알아보기 쉽게 선을 그어놓았다. 원래 와이어프레임어 어떤 내용을 담아야 할지 잘 몰랐었는데, 팀원분들로부터 이상적인 와이어프레임이 무엇인지를 배울 수 있었다.  
<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/ebb1ddad-44dc-4ecd-9f97-2519e73f415b)"/>  

