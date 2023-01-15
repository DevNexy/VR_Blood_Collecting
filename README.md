# VR 핵심간호술기 - 채혈 및 검사

Developed with Unreal Engine 4

<img src="https://user-images.githubusercontent.com/92451281/210620714-b3157cb7-3061-47bc-96f0-5da89ed4fb72.png" width="50%" height="50%"><img src="https://user-images.githubusercontent.com/92451281/210620314-319ded32-1d77-43ab-8d66-5d5f9235869a.png" width="50%" height="50%"><img src="https://user-images.githubusercontent.com/92451281/210620384-b558b844-4b3b-44ff-a4fe-e8ad51175df0.png" width="50%" height="50%"><img src="https://user-images.githubusercontent.com/92451281/210620872-26aa430d-e094-441f-8eca-558dc3e84f53.png" width="50%" height="50%">

### [시연동영상](https://youtu.be/9srFcM9b2T0)

---
<핵심간호술기란?>   
한국간호교육평가원에서 제시한 간호사 직무수행 중 빈도와 중요도가 높은 간호술로서 간호사 양성 교육과정 중에 필수적으로 학습되고 성취되어야 할 기술을 의미합니다.

<채혈 및 검사>   
핵심기본간호술 평가항목 중 채혈 및 검사 컨텐츠를 Unreal Engine 4 로 개발하였습니다.   
Oculus Quest2를 사용하여 VR 컨텐츠로 학습할 수 있습니다.

---
<오류 해결>   

**1. 미 구현한 경고 스크립트 구현(2가지)**
- 채혈이 끝나기 전까지 잡고 있어야 합니다.   
버그 현상 : 채혈이 끝나지 않고 손을 떼면 경고 스크립트가 안뜨는 현상   
수정 방법 : 팔을 떼었을 때 이벤트를 바인딩해서 경고 스크립트를 구현했습니다. (그림 1 참고)   
- 잘못된 각도 입니다. 위쪽에서 삽입해주세요.   
버그 현상 : Tube에 주사기를 다른 각도로 삽입을 시도 했을 때 경고 스크립트가 안뜨는 현상   
수정 방법 : 각도가 틀렸을 때 이벤트를 바인딩해서 경고 스크립트를 구현했습니다. (그림 1 참고)   

![image](https://user-images.githubusercontent.com/92451281/212527603-f6bee934-14fb-4e1c-801b-22d187e37748.png)   
(그림 1)   

**2. 미구현한 Tube와 주사기 각도 체크**   
버그 현상 : 아무 각도에서 모두 Attach(주사기 삽입)가 되는 현상   
수정 방법 : Tube와 주사기를 내적해서 각도를 Tick에서 적당한 각도로 체크했습니다. 각도가 false이면 on wrong angle 이벤트 디스패처를 호출하고, Tube에 Attach 되지 않게 했습니다. (그림 2 참고)   

**3. 미구현한 팔과 주사기 각도 체크**   
버그 현상 : 팔과 주사기가 15도일 때만, Attach를 해서 삽입 해야 하는데, 바로 Attach가 되어버리는 현상   
수정 현상 : 팔과 주사기를 내적해서 각도를 Tick에서 적당한 각도로 체크, 각도가 false이면 주사기가 팔에 Attach 되지 않게 했습니다.(그림 2 참고)   

![image](https://user-images.githubusercontent.com/92451281/212527730-592e3290-4c98-49f7-86af-bead1724e360.png)   
(그림 2)   

**4. Tick에서 Delay 사용했던 부분 없애고, Tube 섞는 부분 재구현 (시간 관계없이 횟수에 따라 카운트)**   
버그 현상 : Tick에서 1초 간격으로 Delay를 사용해서 체크하다보니, 기울이고 가만히 있어도 섞이는 문제가 발생   
수정 방법 : 바닥과 튜브를 내적해서 각도를 Tick에서 적당한 각도로 체크했습니다. (그림 3 참고)   
처음엔, 튜브 섞기 카운트를 Tick에서 구현하다 보니 인티저 값이 한 번만 기울여도 급증하였습니다. 그래서 제가 생각한 방법으로 (그림 4, 5 참고)   
- Tube 내적 값이 -0.7 보다 작거나 같으면 인티저 값 1로 지정(첫 번째 섞임)
- 인티저 값 1이 Ture이고, Tube 내적 값이 -0.7보다 크면 한 번 섞였다는 것을 bool변수로 체크
- 한 번 섞인 것이 True이면, Tube 내적 값이 -0.7보다 작거나 같으면 인티저 값을 2로 지정 (두 번째 섞임)
- 인티저 값 2가 True이고, Tube 내적 값이 -0.7보다 크면 두 번 섞였다는 것을 bool변수로 체크
- 두 번 섞인 것이 True이면, Tube 내적 값이 -0.7보다 작거나 같으면 인티저 값 3으로 지정 (세 번째 섞임)
- 인티저 값이 3이 True이면, 이벤트 디스패처 호출
-> 바닥과 튜브 각도를 체크하여 구현   
![image](https://user-images.githubusercontent.com/92451281/212528004-d93fb2ca-63e5-4c6b-9cbf-1273529f0cb0.png)   
(그림 3)   
![image](https://user-images.githubusercontent.com/92451281/212528012-b5db95f8-ef12-44ee-ad62-7d46d52b9701.png)   
(그림 4)   
![image](https://user-images.githubusercontent.com/92451281/212528017-1516e169-6740-48bc-9971-19a12fc37610.png)   
(그림 5)   

**5. 글러브 박스가 인터랙션 됨**   
버그 현상 : 글러브 박스는 인터랙션 되면 안되고, 글러브만 spawn 해야 하는데, 인터랙션 되는 현상 발생   
수정 방법 : begin play에서 Ready To Interact를 체크했었습니다. 다시 Ready To Interact의 체크를 해제해주었습니다.(그림 6 참고)   
![image](https://user-images.githubusercontent.com/92451281/212528077-3f943c76-d679-4c7b-b3d7-c6a51e8a5a7a.png)   
(그림 6)   

**6. 팔 소독 부분 붉은색 영역 표시가 안됨**   
버그 현상 : 팔을 소독하려면 붉은 색 영역이 표시되어야 하는데, 구현하지 못함   
수정 방법 : 정맥 주사 콘텐츠를 참고하여, 채혈 콘텐츠 레벨에 BrushDataManager를 배치해 주었으며, Gauze Scale과 UVModifier 값을 바꾸고, 팔 소독 파트 스크립트에서 Start Dis Infection Part를 호출해주었습니다.(그림 7, 그림 8 참고)   
![image](https://user-images.githubusercontent.com/92451281/212528150-4a169fed-e31f-4183-b815-718e70ad882c.png)   
(그림 7)   
![image](https://user-images.githubusercontent.com/92451281/212528154-bd4e46b8-f02c-4a62-a286-274f47d8c6ac.png)   
(그림 8)
