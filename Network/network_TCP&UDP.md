## TCP/IP 모델

OSI 참조 모델은 말그대로 참조 모델일 뿐 실제 사용되는 인터넷 프로토콜은 을 7계층 구조를 완전히 따르지는 않는다. 인터넷 프로토콜 스택(Internet Protocol Stack)은 현재 대부분 TCP/IP를 따른다.



TCP/IP는 인터넷 프로토콜 중 가장 중요한 역할을 하는 `TCP`와 `IP`의 합성어로 데이터의 흐름 관리, 정확성 확인, 패킷의 목적지 보장을 담당한다. **데이터의 정확성 확인은 TCP가, 패킷을 목적지까지 전송하는 일은 IP가 담당한다.**



![](https://madplay.github.io/img/post/2018-02-04-network-tcp-udp-tcpip-1.png)

`TCP`와 `UDP`는 `TCP/IP의 전송계층`에서 사용되는 프로토콜이다. **전송계층은 IP에 의해 전달되는 패킷의 오류를 검사하고 재전송 요구 등의 제어를 담당**하는 계층이다.

<br/>

## TCP

TCP는 연결 지향적 프로토콜이다.

연결 지향적 프로토콜은  클라이언트와 서버가 연결된 상태에서 데이터를 주고받는 프로토콜을 의미한다.

장치들 사이에 논리적인 접속을 성립하기 위해 연결을 설정해 신뢰성을 보장하는 연결형 서비스이다.

TCP는 네트워크에 연결된 컴퓨터에서 실행되는 프로그램 간에 일련의 옥텟(데이터, 메시지, 세그먼트라는 블록 단위)을 안정적으로, 순서대로, 에러 없이 교환할 수 있게 한다.

<br/>

## TCP의 특징

1. 연결형 서비스로 가상 회선 방식을 제공

- 3-way handshaking 과정을 통해 연결을 설정하고,

- 4-way handshaking 과정을 통해 연결을 해제한다.
- 가상 회선 방식을 제공한다는 것은 발신지와 수신지를 연결하여 패킷을 전송하기 위한 논리적 경로를 배정한다는 것을 말함

2. 흐름 제어(Flow control)

- 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지

3. 혼잡 제어(Congestion control)

- 네트워크 내의 패킷 수가 과도하게 증가하지 않도록 방지

4. 높은 신뢰성을 보장

- 신뢰성이 높은 전송을 하기 때문에 UDP보다 속도가 느림

5. 전이중(Full-Duplex), 점대점(Point to Point) 방식

- 전이중(Full-Duplex) : 전송이 양방향으로 동시에 일어날 수 있다.
- 점대점(Point to Point) : 각 연결이 정확히 2개의 종단점을 가지고 있다.



<br/>

## TCP의 연결 과정(3-way handshake)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fn67R1%2FbtrsWKQUJ3d%2FwhIVLuEpUWoMXod0fFTfPK%2Fimg.png)

1. 먼저 Open 한 클라이언트가 SYN를 보내고 SYN_SENT 상태로 대기한다.
2. 서버는 SYN-RECEIVED 상태로 바꾸고 SYN과 응답 ACK를 보낸다.
3. SYN과 응답 ACK를 받은 클라이언트는 ESTABLISHED 상태로 변경하고 서버에게 응답 ACK를 보낸다.
4. 응답 ACK를 받은 서버는 ESTABLISHED 상태로 변경한다.

| 상태         | 설명                                                   |
| ------------ | ------------------------------------------------------ |
| CLOSED       | 연결 수립을 시작하기 전의 기본 상태 (연결 없음)        |
| LISTEN       | 포트가 열린 상태로 연결 요청 대기 중                   |
| SYN-SENT     | SYN을 요청한 상태                                      |
| SYN-RECEIVED | SYN 요청을 받고 상대방의 응답을 기다리는 중            |
| ESTABLISHED  | 연결 수립이 완료된 상태, 서로 데이터를 교환할 수 있다. |

<br/>

## TCP의 연결 해제 과정(4-way handshake)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FceLHFJ%2FbtrsWKDsOqv%2FmT0tk87TAnY34Mbec9l920%2Fimg.png)

1. 먼저 close를 실행한 클라이언트가 FIN을 보내고 FIN-WAIT-1 상태로 대기한다.
2. 서버는 CLOSE-WAIT으로 바꾸고 응답 ACK를 전달한다. 동시에 해당 포트에 연결되어 있는 애플리케이션에게 close를 요청한다.
3. ACK를 받은 클라이언트는 상태를 FIN-WAIT-2로 변경한다.
4. close 요청을 받은 서버 애플리케이션은 종료 프로세스를 진행하고 FIN을 클라이언트로 보내 LAST_ACK 상태로 바꾼다.
5. FIN을 받은 클라이언트는 ACK를 서버에 다시 전송하고 TIME-WAIT으로 상태를 바꾼다.
   TIME-WAIT에서 일정 시간이 지나면 CLOSE 된다. ACK를 받은 서버도 포트를 CLOSED로 닫는다.

> ※ TIME-WAIT : 먼저 연결을 끊는 쪽에서 생성되는 소켓으로, 혹시 모를 전송 실패에 대비하기 위해 존재하는 소켓이며,
> TIME-WAIT이 없다면, 패킷의 손실이 발생하거나 통신자 간 연결 해제가 제대로 되지 않을 수 있다.

| 상태        | 설명                                                         |
| ----------- | ------------------------------------------------------------ |
| ESTABLISHED | 연결 수립이 완료된 상태, 서로 데이터를 교환할 수 있다.       |
| FIN-WAIT-1  | 자신이 보낸 FIN에 대한 ACK를 기다리거나 상대방의 FIN을 기다린다. |
| FIN-WAIT-2  | 자신이 보낸 FIN에 대한 ACK를 받았고, 상대방의 FIN을 기다린다. |
| CLOSE-WAIT  | 상대방의 FIN(종료 요청)을 받은 상태. 상대방 FIN에 대한 ACK를 보내고 어플리케이션에 종료를 알린다. |
| LAST-ACK    | CLOSE-WAIT 상태를 처리 후 자신의 FIN 요청을 보낸 후 FIN에 대한 ACK를 기다리는 상태. |
| TIME-WAIT   | 모든 FIN에 대한 ACK를 받고 연결 종료가 완료된 상태. 새 연결과 겹치지 않도록 일정 시간 기다린 후 CLOSED로 전이 한다. |
| CLOSED      | 연결 수립을 시작하기 전의 기본 상태 (연결 없음)              |

<br/>

## UDP의 특징

1. 비연결형 서비스로 데이터그램 방식을 제공한다.

- 데이터의 전송 순서가 바뀔 수 있다.

2. 데이터 수신 여부를 확인하지 않는다.

- TCP의 3-way handshaking과 같은 과정 X

3. 신뢰성이 낮다.

- 흐름 제어(flow control)가 없어서 제대로 전송되었는지, 오류가 없는지 확인할 수 없다.

4. TCP보다 속도가 빠르다.

5. 1:1 & 1:N & N:N 통신이 가능하다.

<br/>

## 정리

### TCP/ UDP 공통점

| TCP(Transfer Control Protocol) \| UDP(User Datagram Protocol) |
| ------------------------------------------------------------ |
| 포트 번호를 이용하여 주소를 지정                             |
| 데이터 오류 검사를 위한 체크섬 존재                          |

### 차이점

| TCP(Transfer Control Protocol)                     | UDP(User Datagram Protocol)                             |
| -------------------------------------------------- | ------------------------------------------------------- |
| 연결이 성공해야 통신 가능(연결형 프로토콜)         | 비연결형 프로토콜(연결 없이 통신이 가능)                |
| 데이터의 경계를 구분하지 않음(Byte-Stream Service) | 데이터의 경계를 구분함(Datagram Service)                |
| 신뢰성 있는 데이터 전송(데이터의 재전송 존재)      | 비신뢰성 있는 데이터 전송(데이터의 재전송 없음)         |
| 일 대 일(Unicast) 통신                             | 일 대 일, 일 대 다(Broadcast), 다 대 다(Multicast) 통신 |
