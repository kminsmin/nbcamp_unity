# 내일배움캠프 46일차 TIL  
오전에 코드카타 후,  팀 프로젝트 개발을 진행했다.  

## Cinemachine을 활용한 컷씬 구현  
1. Director 오브젝트 생성 및 Playable Director 컴포넌트 추가
   - Playable Director 컴포넌트는 타임라인 인스턴스와 타임라인 에셋들 사이의 링크를 저장하는 컴포넌트이다. 타임라인 인스턴스 재생과 관련된 다양한 파라미터를 조작할 수 있다.  
   <img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/dcc9b598-97c5-49ff-b701-6103e050919d"/>
2. 타임라인 생성
   - 다양한 트랙을 추가하여 타암라인을 만들고, Playable Director 컴포넌트에 연결해준다.
     <img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/77ef24fd-87dd-4b7d-b6dc-9cbe7106ade3"/>
     - Activation Track : 참조한 게임 오브젝트의 활성화 여부를 결정한다. 이번에 만든 컷씬의 경우 플레이어의 TextMeshPro 컴포넌트를 비활성화 헀다가 활성화함으로써 OnEnable에서 다음 대사를 불러오도록 하여 컷씬에서의 대화를 구현했다.  
     - Animation Track : Animation 자체를 만드는 것과 비슷하게 특정 시간에서 게임 오브젝트가 어떤 위치에서 어떤 방향으로 어떤 모습으로 있을지 등을 결정할 수 있다.
     - Cinemachine Track : 특정 장면을 담을 Cinemachine Virtual Camera들을 미리 세팅해두고, 해당 카메라들을 트랙으로 추가할 수 있다. 두 개의 서로 다른 장면을 촬영하고 있는 카메라들을 타임라인 상에서 겹치게 하면 부드러운 화면 전환을 구현할 수도 있다.
     - 그 외에도 Audio, Control, Playable, Signal Track 등 다양한 종류의 트랙이 존재한다.

        <img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/41489df2-f617-42c4-bc0f-f0c1e2bad64a"/>
