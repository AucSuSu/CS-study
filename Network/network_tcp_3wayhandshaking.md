[링크](https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake)


TCP 통신 과정에서 데이터를 전송하기 위해서는 먼저 "연결" 상태 (가상의 통신로)를 확보해야 한다.
이 과정이 3-way handshake이다.
## 3-Way Handshake
컴퓨터 네트워크에서 TCP/IP 프로토콜을 사용하여 두 대의 컴퓨터가 안정적인 **연결을 설정**하는 과정.

총 3번의 메시지 교환을 통해 연결이 확립됨.

![image](https://github.com/AucSuSu/CS-study/assets/109134365/83a7ea52-3cf0-462f-8bcd-b1fed0ca7ed5)

TCP 헤더의 플래그(코드 비트)가 3-way handshake 과정에서 사용이 된다.

![image](https://github.com/AucSuSu/CS-study/assets/109134365/eb9baeca-73d0-48df-abda-c1c2084471ec)


확대해보면 아래와 같다.

![image](https://github.com/AucSuSu/CS-study/assets/109134365/59e2d010-8d19-436c-b1d7-01fa08074cbc)

6개의 비트들이 각각 1비트씩 구성되는데, ACK, SYN이 사용된다.

이제 3-way handshake가 진행되는 과정을 알아보자.


### 1. 통신을 하기 위해서 A가 B에게 통신을 하기 위한 "연결"확립을 해달라고 요청(SYN)한다. 이때 Synchronize 비트가 활성화되어 1로 바뀐 세그먼트를 전송한다.

   
![image](https://github.com/AucSuSu/CS-study/assets/109134365/50fa1b92-83af-4716-b2ef-2333219f0af3)



### 2. B는 A가 보낸 요청을 받은 후 **응답할게-** 라고 응답을 회신하기 위해 연결 확립에 대한 "응답(ACK)"를 보낸다. 동시에 B도 A에게 데이터 전송에 대한 허가를 받기 위해 "연결"을 확립 해달라고 요청(SYN)한다. 이때, Acknowledgement와 Synchronize의 비트가 1로 활성이 된 세그먼트를 전송하게 된다.

![image](https://github.com/AucSuSu/CS-study/assets/109134365/05f31016-c4f0-451e-be60-69ba87a679e4)


### 3. B의 요청을 받은 A도 연결 확립에 대한 응답(ACK)를 보낸다. Ack 비트가 1로 활성이 된 세그먼트를 전송하게 된다.
![image](https://github.com/AucSuSu/CS-study/assets/109134365/ddd76040-30ca-4d65-99e2-e541898bac64)

## 4-Way Handshake

연결을 설정하는 3-way handshake 말고 **연결을 종료** 하는 4-way handshake도 알아보자

### 1. A가 B에게 연결 종료(FIN)을 요청한다. 이때 FIN이 1로 활성화된 세그먼트를 전송한다.

![image](https://github.com/AucSuSu/CS-study/assets/109134365/85dc22f1-5e5e-4674-b48a-338f7ca8886e)


### 2. B는 A에게 연결 종료에 대한 응답(ACK)를 반환한다. ACK가 1인 세그먼트를 전송한다.

![image](https://github.com/AucSuSu/CS-study/assets/109134365/f1cc1255-ecd3-4765-98e8-3f5b6edf457f)

### 3. B는 자신도 A에게 연결 종료(FIN)을 요청한다. 이때 FIN이 1로 활성화된 세그먼트를 전송한다. 
![image](https://github.com/AucSuSu/CS-study/assets/109134365/0cc119bb-4675-4df2-91e1-34c757a23e31)

### 4. A는 자신도 B에게 연결 종료에 대한 응답(ACK)를 반환한다. ACK가 1인 세그먼트를 전송한다.

![image](https://github.com/AucSuSu/CS-study/assets/109134365/2cd79aa7-b7a5-4246-9236-4d971e90e533)


이와 같이 TCP는 세션 연결 확립 후 종료하기 위해 4번의 패킷 교환을 거친 후 마무리하는 4-way-handshake 과정을 거친다.


## 의문점
>**엥 3-way 에서는 B가 A로 ACK + SYN을 한번에 패킷 교환을 진행하는데 4-way에서는 ACK, FIN을 따로 진행하네?**

### ????????


그래서 G쌤한테 물어봤슴다
>3-Way Handshake와는 다르게, TCP 연결을 종료할 때 사용되는 4-Way Handshake 과정에서는 FIN (Finish)과 ACK (Acknowledgement) 메시지가 나눠서 전송되는데, 이는 TCP 연결을 종료하는 과정에서 양방향으로 각각 독립적으로 연결을 종료할 수 있도록 하기 위함입니다.

1. 연결을 종료하고자 하는 클라이언트가 서버에게 FIN 패킷을 보내 연결 종료 의사를 전달합니다.
2. 서버는 클라이언트로부터 받은 FIN 패킷에 대한 확인 응답으로 ACK 패킷을 보냅니다. **이 때, 서버는 아직 보낼 데이터가 있을 수 있으므로 데이터 전송을 마칠 때까지 연결을 유지**합니다.
3. 서버가 모든 데이터를 전송하고 연결을 종료할 준비가 되면, 서버는 클라이언트에게 FIN 패킷을 보냅니다.
4. 클라이언트는 서버로부터 받은 FIN 패킷에 대한 응답으로 ACK 패킷을 서버에게 보내 연결을 종료합니다.

>이렇게 FIN과 ACK가 나눠서 전송되는 이유는 TCP 연결이 전이중 통신(두 대의 단말기가 데이터를 송수신하기 위해 동시에 각각 독립된 회선을 사용하는 통신 방식)을 지원하기 때문이다.
>즉, 각 방향으로 독립적인 종료 과정을 거쳐야 하며 한 쪽 방향의 연결 종료가 다른 방향의 데이터 전송에 영향을 주지 않도록 설계되어 있다.
