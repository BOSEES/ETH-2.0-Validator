# 이더리움 2.0 메달라 테스트넷 구축


## 1. ETH 2 런치패드를 이용한 Prysm 가입하기 
- 첫번째로 이더리움 2.0 벨리데이터가 되기위한 기본적인    개념과 안내사항 미 경고사항 에 대해서 설명해준다.
- 두번째로는 구동시킬 벨리데이터를 몇개를 설정할껀지에 대한
 설정과 그에 따른 GoETH(test ETH) 32x n개의 이더가 들어간다는 말인듯하다. 그리고 운영체제를 선택. 나는  맥os를 사용하고있기때문에 맥을 선택. 그리고서 이더리움 깃허브 에서 제공하고있는 이더리움을 예치시켜주는 deposit-cli 바이너리 실행파일은 다운로드 하자!
 <br>그다음은 deposit.exe를 이용해 키생성을 하자 이때
 이모닉 키를 알려주는게 잊어버리않도록 관리하자
 

## 2. Ethereum 2.0 prysm 설치
    일단 나는 docker 위에 설치를 하였다.
    docker 마저 공부할 겸 겸사겸사 진행하였다.

## 3. TEST 32 ETH 받기
    디스코드를 통해 테스트넷에서 사용 할수 있는 이더리움을 요청할수있다. 
    그나저나 32개만 주는줄 알았는데 165개나 줬다. 여러번 해봐야 겠다.

## 4. 밸리데이터 계정을 prysm으로 가져오기
    밸리데이터 계정을 prysm 으로 가져오는방법이 조금 어려웠다.
    ui 설명이 조금은 부족한듯 하다.
    뭐 결국은 처음에 런치패드에서 만든 키를 복사해서 ethe2.0키로 다시 복사를 해서 쓰라는 말이었다.
    뭐 일단은 성공

## 5. 비콘체인 ,밸리데이터 실행
### Prysm 이미지를 도커로 가져오는법
터미널에 
docker pull gcr.io/prysmaticlabs/prysm/validator:latest
docker pull gcr.io/prysmaticlabs/prysm/beacon-chain:latest
---
---
### 공개 테스트넷에 연결하는 법!
터미널에 
docker run -it -v $HOME/.eth2:/data -p 4000:4000 -p 13000:13000 -p 12000:12000/udp --name beacon-node \
  gcr.io/prysmaticlabs/prysm/beacon-chain:latest \
  --datadir=/data \
  --rpc-host=0.0.0.0 \
  --monitoring-host=0.0.0.0
---
---
    나는 도커 위에 비콘체인을 수행하기 위해 이미지 를 만들어놨다. 그레서 이 비콘체인을 컨트롤하기 위한 명령어는
    - docker stop beacon-node(비콘체인 정지)
    - docker start -ai beacon-node(비콘체인 시작)
    - docker rm beacon-node (비콘체인 삭제)

만약 도커위에 비콘체인을 삭제했을때 다시 연결하고 싶다면
    
    docker run -it -v $HOME/.eth2:/data -p 4000:4000 -p 13000:13000 -p 12000:12000/udp --name beacon-node \
    gcr.io/prysmaticlabs/prysm/beacon-chain:latest \
    --datadir=/data \
    --clear-db \
    --rpc-host=0.0.0.0 \
    --monitoring-host=0.0.0.0

## 6. 벨리데이트
    일단 터미널 2개를 연다
    1개는 비콘체인 을 연결시키고 
    1개는 밸리데이트 지갑과 블록헤더를 정보를 연결시키기 위해 구동하자
    일단 처음이라면 헤더에 정보를 추가시키기까지 시간이 5~12시간이 걸릴수도 있다고 한다... 내가 막 시작했을때
    블록이 생성이 벌써 5287개나 생성된 후였다. 따라잡는데는 그리 시간이 걸리진 않을거같다.