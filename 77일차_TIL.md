# 내일배움캠프 77일차 TIL  
오늘은 다음 날 첫 빌드 및 테스트 버전 배포를 대비해 대대적인 디버깅 작업을 진행했다.  

## 팀 프로젝트 개발 - 디버깅  
분명 각자 따로 기능을 개발할 때는 별 문제가 없었는데, 모든 기능을 합치고 게임을 처음부터 플레이 해 보니 발견되는 버그가 굉장히 많았다. 대부분 데이터를 저장하고 로드하는 방식과, 씬 간의 이동을 제대로 대응하지 않은 점이 문제였다. 예를 들어 메인 씬에서 사용되던 오브젝트들의 경우, 메인 씬에서 조작된 데이터는 저장하되 오브젝트는 다른 씬으로 이동할 때 파괴해야 하는데, 오브젝트가 파괴되지 않고 다른 씬으로 그대로 넘어오는 버그가 있었다. 대부분 이렇게 사소한 버그들이긴 했지만, 하나를 고치면 또 다른 버그가 발생하고, 버그가 꼬리에 꼬리를 물어 발생하는 것 같았다... 개발의 대부분은 디버깅인 것 같다...
