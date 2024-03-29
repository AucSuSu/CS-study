# 네트워크의 기초

## 처리량과 지연 시간

### 처리량
: 링크(네트워크 장치를 연결하는 유무선의 연결) 내에서 성공적으로 전달된 데이터의 양 <b>== 얼만큼 트래픽을 처리했는지</b>
- 트래픽: 특정 시점에 링크 내에 흐르는 <b>데이터의 양</b>
  - 트래픽이 많아졌다 == 흐르는 데이터가 많아졌다
  - 처리량이 많아졌다 == 처리되는 트래픽이 많아졌다.
 
### 지연 시간
: 요청이 처리되는 시간 == 메세지가 두 장치 사이를 왕복하는데 걸린 시간

## 네트워크 토폴로지
: 네트워크의 연결형태/구조

### 트리 토폴로지
![image](https://github.com/AucSuSu/CS-study/assets/75782242/ce0e5013-ac10-4f80-a15e-45b080fe3ee2)
- 장점: 노드의 추가, 삭제가 쉽다
- 단점: 특정 노드에 트래픽이 집중 -> 하위 노드에 영향

### 버스 토폴로지
: 중앙 통신 회선 하나에 여러 개의 노드가 연결된 네트워크(근거리 통신망 LAN에서 사용)
![image](https://github.com/AucSuSu/CS-study/assets/75782242/10819ab8-e050-42c5-ac5d-469bfcbcddaf)
- 장점: 설치 비용 저렴, 신뢰성 우수, 중앙 통신 회선에 노드 추가/삭제 쉬움
- 단점: 스푸핑
  - 스푸핑: 스위칭 기능 마비/속이기 -> 패킷(네트워크를 통해 전송되는 형식화된 데이터 덩어리)을 이상한 곳으로 전달
 
### 스타 토폴로지
: 중앙에 있는 노드에 모두 연결된 네트워크 구성
![image](https://github.com/AucSuSu/CS-study/assets/75782242/c24bb252-9eda-4411-84f1-6207cd9c44aa)
- 장점: 노드 추가가 쉬움, 에러 탐지 쉬움, 패킷 충돌 발생 가능성이 적음, 에러 쉽게 발견 가능, 중앙 노드에 발생한 에러가 아니면 다른 노드에 영향 X
- 단점: 중앙 노드에 장애 발생 시 전체 네트워크 사용 불가, 설치 비용 고가

### 링형 토폴로지
: 전체적으로 고리로 망 구성
![image](https://github.com/AucSuSu/CS-study/assets/75782242/399574a4-dd34-438b-8a00-6c60248363e9)
- 장점: 네트워크 상의 손실 없, 충돌 발생 가능성 적음, 에러 발견 쉬움
- 단점: 네트워크 구성 변경이 어렵, 회선에 장애 시 전체 네트워크에 영향

### 메시 토폴로지
: 그물망처럼 연결된 구조
![image](https://github.com/AucSuSu/CS-study/assets/75782242/78eda0bb-ba67-44f4-8b88-9587a6611af2)
- 장점: 여러 개의 경로 존재, 트래픽 분산 처리 가능
- 단점: 노드 추가 어렵, 구축 비용과 운용 비용 고가

## 병목 현상
: 서비스에서 이벤트를 열었을 때 트래픽을 잘 관리하지 못해서 생기는 현상
▶️ 적절한 네트워크 토폴로지 구성으로 병목 현상을 해결할 수 있다.

### 병목 현상의 주된 원인
- 네트워크 대역폭
- 네트워크 토폴로지
- 서버 CPU, 메모리 사용량
- 비효율적인 네트워크 구성

## 네트워크 프로토콜
: 다른 장치들끼리 데이터를 주고받기 위해 설정된 공통된 인터페이스
- HTTP: 웹 접속 시 사용하는 프로토콜 -> 웹 서비스 기반으로 데이터를 주고받을 수 있다.
