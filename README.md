# PUBG_dataset_project
![dddd](https://user-images.githubusercontent.com/37128004/168798843-74807401-9c7a-4a40-905b-fc4d4da1a0a0.png)
## Abstract
  코멘토의 빅데이터 분석 자율과정에서 진행한 조그마한 프로젝트이다. 완전 초창기라 데이터셋 찾는 것도 빡쎘던 기억이 있으나 좋아하던 게임, 배틀그라운드 데이터가 Kaggle에 있기에 신나게 다운받아 주제로 선정하였다. 주제는 '배틀그라운드 게임 데이터를 이용한 순위 예측'으로 게임 내의 유저 정보를 통해 최종 순위(winPlacePerc, 0~1)를 예측해보았다. 전처리 과정을 거치고 이번엔 DNN을 활용했다.

## Data
데이터는 Kaggle의 [PUBG_Datset](https://www.kaggle.com/datasets/razamh/pubg-dataset)를 활용하였다. 유저들의 matchID, 킬 수, 어시스트 수, 데미지, 힐링 아이템, 킬 수 순위, 시간, 매치 타입, 부활 횟수, 이동 거리 등의 정보로 최종 순위(winPlacePerc, 0에서 1사이 실수, 0이면 꼴등, 1이면 1등)을 맞추는 구조로 진행하였다. 

## EDA
EDA는 [EDA is Fun!](https://www.kaggle.com/code/deffro/eda-is-fun)이라는 competition 공유 코드를 참고하였다. 총 6단계로 나눠서 알아보고 싶은 것들을 구현해봤다. winner winner chicken dinner! 과연 어떤 유저가 치킨을 더 잘 먹을까?? 
### 1. Kills : 사람들은 몇 킬을 하고 데미지를 얼마나 넣었는가?
반 이상의 사람들이 1킬도 하지 못한 채 죽었으며 예상대로 킬을 못한 사람들은 데미지 역시 많이 넣지 못했으나 높은 등수까지 살아남은 비중이 꽤 높았다. 킬 수는 높을수록 더 높은 순위를 차지한 것으로 보인다.

<img src="https://user-images.githubusercontent.com/37128004/197382143-5b53357f-fa7f-4c77-a07c-8fa7886293cf.png" width="600" height="400"/>

### 2. Groups : 매치 내 총 그룹 수가 적을수록 치킨을 잘 먹는가?
숫자 별로 구분하여 비교해본 결과 솔로 -> 듀오 -> 스쿼드로 갈수록 상위권 성적의 유저가 적었다. 하지만 솔로와는 다르게 듀오와 스쿼드는 게임을 나가지 않는다면 마지막까지 살아남는 팀원의 순위가 자신의 순위가 됨을 인지하자!

<img src="https://user-images.githubusercontent.com/37128004/197382282-70230c0e-bbf1-4326-8438-d6eeb58fc1e7.png"  width="600" height="400"/>

### 3. Moving Distance : 많이 이동할수록 치킨을 잘 먹는가?
도보 이동거리와 순위는 높은 상관관계를 가진다. 당연하다. 순위가 높을수록 살아있는 시간이 길고, 많이 움직인다!

<img src="https://user-images.githubusercontent.com/37128004/197382351-5b732ffa-b393-408a-a22e-05dbc78ca9c3.png" width="600" height="400"/>

### 4. Match Duration : 매치시간이 길수록 치킨을 잘 먹는가?
모든 순위와의 관계를 보기에 무리가 있어 1등만 봤다. 1200에서 1500초 사이와 1700에서 2000초 사이에 대부분 밀집되어 있는 것은 블루존이 5에서 6단계, 8에서 9단계 사이에 게임이 끝났음을 의미한다. 맵의 크기에 따라 다르게 나타나는 것이므로 타겟 예측에 큰 영향을 끼치진 않을 것 같다.

<img src="https://user-images.githubusercontent.com/37128004/197382478-e4e19258-d98c-4d24-8085-f16b1e60c175.png" width="600" height="400"/>

### 5. Plyaers in Group : 같은 팀이 많을수록 치킨을 잘 먹는가?
솔로, 듀오, 스쿼드 모두 비슷하게 나타나고 있다.

<img src="https://user-images.githubusercontent.com/37128004/197382498-ced66f99-14f1-43f4-9381-7ba4200afd0b.png" width="800" height="400"/>

## Preprocess
1) 이상치 제거 : Unnamed:0 와 Id 특성을 제거하고 이상한 데이터(ex) 매치 킬 1등인데 순위 꼴등인 유저,..)를 삭제했다. 
2) 파생변수 생성 : 전투포인트, 메디컬 포인트, 팀워크, 초당 킬수, 거리 당 포인트 등의 파생변수를 생성했다. 
3) 정규화 및 매핑 : 딥러닝 모델을 사용할 것이기에 변수들을 정규화해주었는데 팀 단위로 순위에 영향을 끼치는 변수를 따로 묶고 개인 능력치 변수를 따로 묶어서 적용시켰다. 또한 matchId, groupId 별 평균치로 바꿔 일반화 성능을 높였다.

## Modeling
256 X 2, 128 X 2, 64 X 2, 32, 16 층만 사용하였고 overfitting 방지를 위한 batch normalization, dropout, initializer, regularizer와 earlystoppin을 적용하였다. ReduceLRonPlateau를 통해 loss를 줄이는 최소한의 노력을 들였다. 너무 간단한 구조라 딱히 설명할 것도 없는 것 같다! 학습 곡선을 그려본 결과 train loss와 val loss가 같이 잘 내려가는 것처럼 보인다. NMAE는 0.1273 정도 나왔다.

![1](https://user-images.githubusercontent.com/37128004/168826013-b0453a1a-7e9c-47df-a8cd-23a0bc955cb5.png)
![2](https://user-images.githubusercontent.com/37128004/168826063-168c1e9d-446a-4269-ba90-1a1f7f9df5cb.png)

## Conclusion
요즘은 잘 사용하는 것 같진 않지만 전체적으로 딥러닝이라는 과정이 어떻게 돌아가는지 맛보기로 해볼 수 있던 경험이었다. 생각보다 디테일하게 신경써야 할 부분들이 많다는 점도 깨달았다.(마지막 test 셋으로 평가해볼때 reshape를 해주지 않아서 ram용량 초과로 터져버리는 일도 생기고..) 이젠 최신 논문들 리뷰도 하면서 트렌드를 따라가봐야겠다. 





