# PUBG_dataset_project
![dddd](https://user-images.githubusercontent.com/37128004/168798843-74807401-9c7a-4a40-905b-fc4d4da1a0a0.png)
## 개요
학교에서 신청하는 코멘토 빅데이터 프로젝트에 참여했다가 운영과정의 문제(?)가 생겼다. 이에 추가로 받은 쿠폰으로 등록한 빅데이터 분석 자율과정에서 진행한 조그마한 프로젝트이다. 
완전 초창기라 데이터셋 찾는 것도 빡쎘던 기억이 있으나 좋아하던 게임, 배틀그라운드 데이터가 Kaggle에 있기에 신나게 다운받아 주제로 선정하였다. 주제는 '배틀그라운드 게임 데이터를 이용한 순위 예측'으로 게임 내의 유저 정보를 통해 최종 순위를 예측해보았다. 

## 데이터셋
데이터는 Kaggle의 [PUBG_Datset](https://www.kaggle.com/datasets/razamh/pubg-dataset)를 활용하였다. 유저들의 matchID, 킬 수, 어시스트 수, 데미지, 힐링 아이템, 킬 수 순위, 시간, 매치 타입, 부활 횟수, 이동 거리 등의 정보로 최종 순위(winPlacePerc, 0에서 1사이 실수, 0이면 꼴등, 1이면 1등)을 맞추는 구조로 진행하였다. 

## 
EDA는 [EDA is Fun!](https://www.kaggle.com/code/deffro/eda-is-fun)이라는 competition 공유 코드를 참고하였다. 총 6단계로 나눠서 알아보고 싶은 것들을 구현해봤다. 좋아하는 게임이라 도메인 지식들이 있어 수월했다.
1) Kills : 사람들은 몇 킬을 하고 얼만큼의 데미지를 넣었는가?
