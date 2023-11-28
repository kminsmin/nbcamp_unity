# 내일배움캠프 75일차 TIL  
오늘도 오전에 기술면접 예상질문을 풀어보고 팀 프로젝트 개발을 진행했다.     

## 팀 프로젝트 - 트러블 슈팅  
오늘은 곧 있을 유저 테스트를 위해 대부분 디버깅과 트러블 슈팅 위주의 작업을 진행했다.  

### 간헐적으로 플레이어가 타일맵 사이에 끼이는 현상  
https://community.gamedev.tv/t/player-gets-randomly-stuck-on-tiles/205142  
해외 사이트를 참고하여 해당 문제를 해결하는 방법을 알아냈다. TilemapCollider와 함께 CompositeCollider를 사용하는 것이다. CompositeCollider를 사용하면 인접한 콜라이더들을 하나의 콜라이더로 취급하여, 기존에 콜라이더들 사이에 존재했던 작은 틈새들을 메울 수 있다. 여기서 주의할 점이 있는데 CompositeCollider는 Rigidbody 컴포넌트를 반드시 필요로 한다. 이 점이 이후 잠시 문제가 되었다.   

### 최적화 - Profiler  
위에서 CompositeCollider를 사용하기 위해 모든 타일맵에 Rigidbody를 추가해주었더니, 프레임 드랍이 눈에 띄게 증가하는 현상이 있었다. 그래서 Profiler를 활용해서 무엇이 문제인지 알아보았다.  
<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/0cfcc518-40b8-4aba-8694-05698110497f"/>    
2D 게임이 평균 30프레임 이하로 나오는 말도 안되는 현상이다.  
확인해보니 물리연산(주황색)이 차지하는 분량이 다른 부분에 비해 확연히 많은 것을 알 수 있다. 모든 맵마다 Rigidbody를 추가하였기에 발생한 현상으로 보였다. 생각해보니 맵의 Rigidbody를 디폴트인 Dynamic으로 두는 것은, 맵은 어차피 움직이지 않을 것이기 때문에 심각한 자원 낭비였다. 그래서 맵마다 Rigidbody를 Dynamic에서 Static으로 변경해주었더니, 눈에 띄게 프레임이 증가한 모습을 볼 수 있었다.  
<img width="100%" src="https://github.com/kminsmin/nbcamp_unity/assets/114645806/0e4b870e-0c17-4fdb-a506-182efa1c8e1c"/>  
