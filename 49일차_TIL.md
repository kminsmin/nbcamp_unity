## 최종 프로젝트 - 아이디어 회의  
오늘은 최종 프로젝트 발제가 있었다. 발제 후 팀원들과 모여 어떤 게임을 만들지 회의하는 시간을 가졌다. 회의 결과 가장 우리 역량으로 실현할만 하고 포트폴리오로 가치가 있을 만한 경영 + 덱빌딩 게임을 만들기로 했다. 하루 종일 아이디어를 짜내고 MVP(Minimum Value Project) 구현을 위한 와이어프레임 및 플로우차트를 작성했다. 조금 더 계획을 구체화한 다음 내일부터 MVP 개발을 시작할 예정이다.  

<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/adc3174e-8f39-403c-8354-104cb4a94ab8"/>    

## 싱글톤 패턴  
싱글톤(Singleton) 패턴은 말 그대로 객체의 인스턴스가 하나만 생성되도록 하는 패턴이다. 특정 클래스의 인스턴스를 한 개만 생성하고, 해당 인스턴스에 전역적으로 접근할 수 있는 메커니즘을 제공한다.  
```cs
public class Singleton
{
    // 인스턴스를 저장할 정적 변수
    private static Singleton instance;

    // 다른 클래스에서 인스턴스 생성을 막기 위한 private 생성자
    private Singleton() { }

    // 인스턴스에 접근하기 위한 메서드
    public static Singleton GetInstance()
    {
        // 인스턴스가 없을 경우에만 생성
        if (instance == null)
        {
            instance = new Singleton();
        }
        return instance;
    }

    // 싱글턴 클래스의 다른 메서드 및 속성 정의
    public void SomeMethod()
    {
        // 메서드 내용
    }
}
```
처음 호출할 때만 인스턴스가 생성되고, 이후에는 이미 생성된 인스턴스를 반환한다.  
