<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/a458479d-5fa7-4b28-9552-fb14ff410312)"/>  

# 내일배움캠프 9일차  
3주차 과제, 개인과제 계획짜기  
## Snake Game  
어제는 뱀의 모든 좌표를 스택에 넣고, 매번 Console.Clear()로 화면을 리셋하고 뱀의 좌표를 업데이트하여 찍는 방식으로 뱀의 움직임을 구현하려 했다. 하지만 뱀의 이동방식과 좌표를 둘로 쪼개어 계속 컬렉션에 보관했다 꺼내는 방식이
매우 번거로워서 오늘은 뱀의 이동 방식을 완전히 뒤엎어버렸다.  
먼저 뱀을 구성하는 점들의 좌표를 보관할 컬렉션으로 Queue를 채택했다. Queue는 선입선출(FIFO)방식으로, 새로운 점을 찍으면 가장 오래된 점은 사라지는 뱀의 특성상 가장 적합하다고 생각했기 때문이다. 어제는 뱀의 좌표를 for문을 이용해서 
컬렉션에 저장하느라 Stack을 사용했는데, 이 때문에 저장 순서와 이동할 때 점이 찍히는 순서가 굉장히 헷갈리기도 하고 방향 전환에 대응이 안된다는 문제가 있었다. 또한 모든 점들을 매 반복마다 다시 찍어줬던 어제와는 다르게, 오늘 구현한
방식은 맨 앞에 점을 하나를 찍으면, 맨 뒤의 점을 지우는 방식을 채택했다. 점을 찍고 지우는 수를 최소화하는 것이 메모리 사용량도 적고 직관적이라고 생각했다. 점의 좌표를 저장하는 방식은 어제처럼 x, y좌표를 컬렉션에 따로 보관하는 것이 아니라,
Point 클래스를 활용한 인스턴스에 매 반복마다 저장해서 출력하는 방식으로 했다. 이 때 Point 객체들을 담는 Queue에 Enqueue하고, 이 Queue에서 Dequeue 한 포인트는 가장 오래된 점의 좌표이기 때문에 빈칸을 출력하는 방식으로 지워주면
뱀의 이동을 깔끔하게 구현할 수 있다.  
알고리즘을 정리하면 다음과 같다.  
방향이 변했는지 파악 및 변경-> 이동 방향에 따라 Head의 다음 좌표 결정 -> Head 점 출력, 해당 좌표 Enqueue, 오래된 점 Dequeue해서 지우기  
### 뱀 이동 구현
```cs
    public class Point
    {
        public int x { get; set; }
        public int y { get; set; }
        public char sym { get; set; }

        // Point 클래스 생성자
        public Point(int _x, int _y, char _sym)
        {
            x = _x;
            y = _y;
            sym = _sym;
        }

        // 점을 그리는 메서드
        public void Draw()
        {
            Console.SetCursorPosition(x, y);
            Console.Write(sym);
        }

        // 점을 지우는 메서드
        public void Clear()
        {
            sym = ' ';
            Draw();
        }

        // 두 점이 같은지 비교하는 메서드
        public bool IsHit(Point p)
        {
            return p.x == x && p.y == y;
        }
    }

    public enum Direction
    {
        LEFT,
        RIGHT,
        UP,
        DOWN
    }

    public class Snake
    {

        public Queue<Point> body;

        public Direction direction { get; set; }

        public Snake(Point tail, int length, Direction _direction)
        {
            direction = _direction;
            body = new Queue<Point>();

            for (int i = 0; i < length; i++)
            {
                Point p = new Point(tail.x, tail.y, tail.sym);
                body.Enqueue(p);
                tail.x += 1;
            }
        }

        public void Draw(Point head)
        {
            Point t = body.Dequeue();
            Point h = new Point(head.x, head.y, '*');
            body.Enqueue(head);
            head.Draw();
            t.Clear();
        }

        public int ChangeDir(int dirNum)
        {
            Direction dir = (Direction)dirNum;
            int num = dirNum;
            if (Console.KeyAvailable == true)
            {
                ConsoleKey keyPressed = Console.ReadKey().Key;
                if (keyPressed == ConsoleKey.RightArrow)
                {
                    dir = Direction.RIGHT;
                }
                else if (keyPressed == ConsoleKey.LeftArrow)
                {
                    dir = Direction.LEFT;
                }

                else if (keyPressed == ConsoleKey.UpArrow)
                {
                    dir = Direction.UP;
                }

                else if (keyPressed == ConsoleKey.DownArrow)
                {
                    dir = Direction.DOWN;
                }
            }

            num = (int)dir;
            return num;
        }

        public void SetHeadPoint(ref int x, ref int y, int dirNum)
        {
            switch (dirNum)
            {
                case 0:
                    x--;
                    break;
                case 1:
                    x++;
                    break;
                case 2:
                    y--;
                    break;
                case 3:
                    y++;
                    break;
            }

        }
    }
static void Main(string[] args)
    {
        // 뱀의 초기 위치와 방향을 설정하고, 그립니다.
        Point p = new Point(9, 5, '*');
        int length = 4;
        Direction dir = Direction.RIGHT;
        Snake snake = new Snake(p, length, Direction.RIGHT);
        Queue<Point> tails = new Queue<Point>();
        for (int i = 0; i < length; i++)
        {
            Point _p = new Point(p.x, p.y, p.sym);
            tails.Enqueue(p);
            _p.x += 1;
        }

        int x = 8;
        int y = 5;
        int dirNum = 1;
        Point head = new Point(x, y, '*');


        // 게임 루프: 이 루프는 게임이 끝날 때까지 계속 실행됩니다.
        while (true)
        {

            Thread.Sleep(100); // 게임 속도 조절 (이 값을 변경하면 게임의 속도가 바뀝니다)

            dirNum = snake.ChangeDir(dirNum); // 키 입력이 있는 경우에만 방향을 변경합니다.


            snake.SetHeadPoint(ref x, ref y, dirNum);

        }
    }
```
### 맵 구현, 벽 또는 본인에게 닿으면 게임 오버  
맵은 간단하게 그리기만 했고, 뱀 머리의 x 또는 y 좌표가 상하좌우 벽 중 하나와 일치하면 벽에 닿은 판정이 되어 게임이 오버되는 방식으로 구현했다.  Snake 클래스 안에 메소드를 선언했다.  
스스로의 몸에 닿으면 게임 오버되는 기능도 벽과 비슷하게 구현했다. foreach 문을 사용해서 body에 있는 점들 중 하나라도 head의 좌표와 일치하면 죽는 방식으로 했는데, 처음에는 게임을 켜자마자 뱀이 죽어버려서
당황스러웠다. F9로 CheckDeath메소드가 사용되는 부분에 브레이크를 걸고 확인해보니 body라는 Queue 안에는 head 도 존재하니까 바로 isDead가 true 가 되버리는 것이었다... head는 검사에서 제외하도록 수정하니
정상적으로 작동하는 것을 확인했다.  
```cs
static public void DrawMap()
    {
        Console.WriteLine();
        Console.WriteLine("______________________________________________________________________________________________");
        for (int i = 0; i < 25; i++)
        {
            Console.WriteLine("|                                                                                             |");
        }
        Console.WriteLine("----------------------------------------------------------------------------------------------");

    }

// Snake 클래스 안에...
public bool CheckDeath(Point head)
        {
            bool isDead = false;
            if(head.x ==0 || head.x == 94 || head.y == 1 || head.y == 27)
                isDead = true;
            foreach (Point item in body)
            {
                if (body.Count >= 5 && item.x == head.x && item.y == head.y && item != head)
                    isDead = true;
            }
            return isDead;
        }
```

## food 무작위 생성, 뱀 길이 증가  
FoodCreator 클래스를 생성하고, 생성자는 food의 좌표를 받아오고, 받아온 좌표에 food를 출력하는 메서드를 생성했다. 또 뱀과 닿은 food는 사라저야 하므로 이를 위한 메서드도 생성했다.  
처음에는 생성되는 food들을 모두 리스트에 넣고, foreach 문을 사용해서 게임오버 여부를 판단하는 것과 비슷하게 food와 head의 좌표 일치 여부를 확인하고, 일치하면 해당 food를 리스트에서 Remove메서드로 삭제하는 방식으로 구현하려고 
했는데 컴파일 에러가 발생했다. foreach 문이 돌아가는 중에 리스트의 크기가 변해버리기 때문이었다. 따라서 뱀과 닿은 food들을 저장하는 리스트를 따로 만들고, 이 리스트를 매개변수로 DestroyFood 메서드에 전달하고, 전체 food 리스트는 ref 키워드로 전달해서, 뱀이 먹은 food 리스트를 foreach문으로 돌리고 원본 food 리스트와 비교하며 같으면 원본에서 Remove 하는 방식으로 메서드를 생성했다.  
food 생성 주기는 게임의 재미를 위해 랜덤으로 설정했다.  
```cs
 public class FoodCreator
    {
        Point foodPoint;
        
        public FoodCreator()
        {
            foodPoint = new Point(new Random().Next(2, 92), new Random().Next(2, 24), '$');
        }

        public Point Draw()
        {
            foodPoint.Draw();
            return foodPoint;
        }

        public void DestroyFood(ref List<Point> foods, List<Point> eatedFood)
        {
            foreach (Point item in eatedFood)
            {
                foods.Remove(item);
            }
        }
    }
----------------------------------------------------
while (true)
        {
            FoodCreator food = new FoodCreator();
            Thread.Sleep(100); // 게임 속도 조절 (이 값을 변경하면 게임의 속도가 바뀝니다)
            foodGen++;
            randomNum = new Random().Next(20,50);
            if (foodGen >= randomNum)
            {
                foodPoint = food.Draw();
                foods.Add(foodPoint);
                foodGen = 0;
            }
            dirNum = snake.ChangeDir(dirNum); // 키 입력이 있는 경우에만 방향을 변경합니다.


            snake.SetHeadPoint(ref x, ref y, dirNum);


            // 뱀이 이동하고, 음식을 먹었는지, 벽이나 자신의 몸에 부딪혔는지 등을 확인하고 처리하는 로직을 작성하세요.
            // 이동, 음식 먹기, 충돌 처리 등의 로직을 완성하세요.


            head = new Point(x, y, '*');
            List<Point> eatedFood = snake.CheckFood(head, foods);
            food.DestroyFood(ref foods, eatedFood);
            snake.Draw(head);
            isDead = snake.CheckDeath(head);
            if (isDead)
                break;


            // 뱀의 상태를 출력합니다 (예: 현재 길이, 먹은 음식의 수 등)
        }
        Console.WriteLine("Game Over");
    }
```
## 타이틀, 점수 추가  
보기 좋은 타이틀과 점수를 표시하는 기능을 추가했다. 타이틀과 점수를 출력하는 기능은 하나의 메서드로 묶어서 while문 마지막에 호출하도록 했다.
```cs
 static public void DrawUI(int score)
    {
        Console.SetCursorPosition(91, 3);
        Console.Write("  _____             _        ");
        Console.SetCursorPosition(91, 4);
        Console.Write(" /  ___|           | |       ");
        Console.SetCursorPosition(91, 5);
        Console.Write(" \\ `--. _ __   __ _| | _____ ");
        Console.SetCursorPosition(91, 6);
        Console.Write("  `--. \\ '_ \\ / _` | |/ / _ \\");
        Console.SetCursorPosition(91, 7);
        Console.Write(" /\\__/ / | | | (_| |   <  __/");
        Console.SetCursorPosition(91, 8);
        Console.Write(" \\____/|_| |_|\\__,_|_|\\_\\___|");
        Console.SetCursorPosition(91, 9);
        Console.Write("                             ");
        Console.SetCursorPosition(91, 10);
        Console.Write("                             ");
        Console.SetCursorPosition(91, 11);
        Console.Write(" _____                       ");
        Console.SetCursorPosition(91, 12);
        Console.Write("|  __ \\                      ");
        Console.SetCursorPosition(91, 13);
        Console.Write("| |  \\/ __ _ _ __ ___   ___  ");
        Console.SetCursorPosition(91, 14);
        Console.Write("| | __ / _` | '_ ` _ \\ / _ \\ ");
        Console.SetCursorPosition(91, 15);
        Console.Write("| |_\\ \\ (_| | | | | | |  __/ ");
        Console.SetCursorPosition(91, 16);
        Console.Write(" \\____/\\__,_|_| |_| |_|\\___| ");
        Console.SetCursorPosition(91, 17);
        Console.Write("                            ");
        Console.SetCursorPosition(91, 18);
        Console.Write("                            ");
        Console.SetCursorPosition(91, 19);
        Console.WriteLine("Score : {0}", score);
    }
```
전체 코드는 아래와 같다.  
```cs
class Program
{
    public class Point
    {
        public int x { get; set; }
        public int y { get; set; }
        public char sym { get; set; }

        // Point 클래스 생성자
        public Point(int _x, int _y, char _sym)
        {
            x = _x;
            y = _y;
            sym = _sym;
        }

        // 점을 그리는 메서드
        public void Draw()
        {
            Console.SetCursorPosition(x, y);
            Console.Write(sym);
        }

        // 점을 지우는 메서드
        public void Clear()
        {
            sym = ' ';
            Draw();
        }

        // 두 점이 같은지 비교하는 메서드
        public bool IsHit(Point p)
        {
            return p.x == x && p.y == y;
        }
    }

    public enum Direction
    {
        LEFT,
        RIGHT,
        UP,
        DOWN
    }

    public class Snake
    {

        public Queue<Point> body;

        public Direction direction { get; set; }



        public Snake(Point tail, int length, Direction _direction)
        {
            direction = _direction;
            body = new Queue<Point>();

            for (int i = 0; i < length; i++)
            {
                Point p = new Point(tail.x, tail.y, tail.sym);
                body.Enqueue(p);
                tail.x += 1;
            }
        }

        public void Draw(Point head)
        {
            Point t = body.Dequeue();
            Point h = new Point(head.x, head.y, '*');
            body.Enqueue(head);
            head.Draw();
            t.Clear();
        }

        public int ChangeDir(int dirNum)
        {
            Direction dir = (Direction)dirNum;
            int num = dirNum;
            if (Console.KeyAvailable == true)
            {
                ConsoleKey keyPressed = Console.ReadKey().Key;
                if (keyPressed == ConsoleKey.RightArrow)
                {

                    dir = Direction.RIGHT;

                }
                else if (keyPressed == ConsoleKey.LeftArrow)
                {

                    dir = Direction.LEFT;


                }

                else if (keyPressed == ConsoleKey.UpArrow)
                {

                    dir = Direction.UP;


                }

                else if (keyPressed == ConsoleKey.DownArrow)
                {
                    dir = Direction.DOWN;

                }


            }

            num = (int)dir;
            return num;
        }

        public void SetHeadPoint(ref int x, ref int y, int dirNum)
        {
            switch (dirNum)
            {
                case 0:
                    x--;
                    break;
                case 1:
                    x++;
                    break;
                case 2:
                    y--;
                    break;
                case 3:
                    y++;
                    break;
            }

        }

        public bool CheckDeath(Point head)
        {
            bool isDead = false;
            if(head.x ==0 || head.x == 94 || head.y == 1 || head.y == 27)
                isDead = true;
            foreach (Point item in body)
            {
                if (body.Count >= 5 && item.x == head.x && item.y == head.y && item != head)
                    isDead = true;
            }
            return isDead;
        }

        public List<Point> CheckFood(Point head, List<Point>foods)
        {
            List<Point> eatedFood = new List<Point>();
            foreach (Point item in foods)
            {
                if (item.x == head.x && item.y == head.y)
                {
                    //Console.WriteLine("옴뇸뇸");
                    body.Enqueue(head);
                    eatedFood.Add(item);
                }
            }
            return eatedFood;
        }
    }

    public class FoodCreator
    {
        Point foodPoint;
        
        public FoodCreator()
        {
            foodPoint = new Point(new Random().Next(2, 92), new Random().Next(2, 24), '$');
        }

        public Point Draw()
        {
            foodPoint.Draw();
            return foodPoint;
        }

        public void DestroyFood(ref List<Point> foods, List<Point> eatedFood)
        {
            foreach (Point item in eatedFood)
            {
                foods.Remove(item);
            }
        }
    }
    static public void DrawMap()
    {
        Console.WriteLine();
        Console.WriteLine("______________________________________________________________________________________________");
        for (int i = 0; i < 25; i++)
        {
            Console.WriteLine("|                                                                                             |");
        }
        Console.WriteLine("----------------------------------------------------------------------------------------------");

    }

    static public void DrawUI(int score)
    {
        Console.SetCursorPosition(91, 3);
        Console.Write("  _____             _        ");
        Console.SetCursorPosition(91, 4);
        Console.Write(" /  ___|           | |       ");
        Console.SetCursorPosition(91, 5);
        Console.Write(" \\ `--. _ __   __ _| | _____ ");
        Console.SetCursorPosition(91, 6);
        Console.Write("  `--. \\ '_ \\ / _` | |/ / _ \\");
        Console.SetCursorPosition(91, 7);
        Console.Write(" /\\__/ / | | | (_| |   <  __/");
        Console.SetCursorPosition(91, 8);
        Console.Write(" \\____/|_| |_|\\__,_|_|\\_\\___|");
        Console.SetCursorPosition(91, 9);
        Console.Write("                             ");
        Console.SetCursorPosition(91, 10);
        Console.Write("                             ");
        Console.SetCursorPosition(91, 11);
        Console.Write(" _____                       ");
        Console.SetCursorPosition(91, 12);
        Console.Write("|  __ \\                      ");
        Console.SetCursorPosition(91, 13);
        Console.Write("| |  \\/ __ _ _ __ ___   ___  ");
        Console.SetCursorPosition(91, 14);
        Console.Write("| | __ / _` | '_ ` _ \\ / _ \\ ");
        Console.SetCursorPosition(91, 15);
        Console.Write("| |_\\ \\ (_| | | | | | |  __/ ");
        Console.SetCursorPosition(91, 16);
        Console.Write(" \\____/\\__,_|_| |_| |_|\\___| ");
        Console.SetCursorPosition(91, 17);
        Console.Write("                            ");
        Console.SetCursorPosition(91, 18);
        Console.Write("                            ");
        Console.SetCursorPosition(91, 19);
        Console.WriteLine("Score : {0}", score);
    }

    static void Main(string[] args)
    {
        DrawMap();
        // 뱀의 초기 위치와 방향을 설정하고, 그립니다.
        Point p = new Point(4, 5, '*');
        int length = 4;
        Direction dir = Direction.RIGHT;
        Snake snake = new Snake(p, length, Direction.RIGHT);
        Queue<Point> tails = new Queue<Point>();
        for (int i = 0; i < length; i++)
        {
            Point _p = new Point(p.x, p.y, p.sym);
            tails.Enqueue(p);
            _p.x += 1;
        }

        int x = 8;
        int y = 5;
        int foodGen = 0;
        int dirNum = 1;
        Point head = new Point(x, y, '*');
        bool isDead = false;
        int randomNum = 5;
        List<Point> foods = new List<Point>();
        int score = 0;



        // 음식의 위치를 무작위로 생성하고, 그립니다.
        //FoodCreator foodCreator = new FoodCreator(80, 20, '$');
        //Point food = foodCreator.CreateFood();
        //food.Draw();
        Point foodPoint = new Point(5, 5, '$');
        // 게임 루프: 이 루프는 게임이 끝날 때까지 계속 실행됩니다.
        while (true)
        {
            FoodCreator food = new FoodCreator();
            Thread.Sleep(100); // 게임 속도 조절 (이 값을 변경하면 게임의 속도가 바뀝니다)
            foodGen++;
            randomNum = new Random().Next(20,50);
            if (foodGen >= randomNum)
            {
                foodPoint = food.Draw();
                foods.Add(foodPoint);
                foodGen = 0;
            }
            dirNum = snake.ChangeDir(dirNum); // 키 입력이 있는 경우에만 방향을 변경합니다.


            snake.SetHeadPoint(ref x, ref y, dirNum);


            // 뱀이 이동하고, 음식을 먹었는지, 벽이나 자신의 몸에 부딪혔는지 등을 확인하고 처리하는 로직을 작성하세요.
            // 이동, 음식 먹기, 충돌 처리 등의 로직을 완성하세요.

            score = snake.body.Count;
            head = new Point(x, y, '*');
            List<Point> eatedFood = snake.CheckFood(head, foods);
            food.DestroyFood(ref foods, eatedFood);
            snake.Draw(head);
            isDead = snake.CheckDeath(head);
            DrawUI(score);
            if (isDead)
                break;


            // 뱀의 상태를 출력합니다 (예: 현재 길이, 먹은 음식의 수 등)
        }
        Console.WriteLine("Game Over");
    }
}



```
