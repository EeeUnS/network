# 5. TCP

# **이해하면 인생이 바뀌는 TCP 송/수신 원리**

![Untitled](/network/images/network5/Untitled.png)

TCP/IP 연결을 3way handshake 한다고 치자.

이를 깊이있게 한번 따져보자

서버의 상황을 한번 보자

![Untitled](/network/images/network5/Untitled%201.png)

누누이 얘기하지만 Socket의 본질은 파일임 

서버는 프로세스

Process는 파일을 RWX를 할 수 있다.

read / write/execute

근데 소켓일때는 의미가 조금 달라짐 

- R : Receive
- W : Send

![Untitled](/network/images/network5/Untitled%202.png)

File a.bmp가 있다고 하고 크기가 1.4mb 정도라고 하자.

그럼 보통 서버 개발자는 이 파일을 위한 메모리 크기를 담당하는데

이는 메모리 크기는 1.4mb보다 보통 작다.

64kb정도 라고하고, 이 파일을 64kb만큼 쪼개서 받아온다(read)고 보자.

![Untitled](/network/images/network5/Untitled%203.png)

그럼 이 들고있는 메모리를 다시 socket file에 쓸거고 TCP단으로 넘어갈때 분해가 날거다.

이 분해가 넘어가는 과정에도 또 메모리(Buffer)가 존재해 다시 Copy가 일어난다.

Buffered I/O라 부른다.

IP 단으로 넘어갈때 다시 잘게 쪼개는데 Segment로 쪼개짐

![Untitled](/network/images/network5/Untitled%204.png)

이 쪼개지는거에 순서를 붙임 (편의상 1,2,3…

![Untitled](/network/images/network5/Untitled%205.png)

Packet은 택배박스와 굉장히 유사함

이 박스에 택배를 집어넣는다고 보자.

![Untitled](/network/images/network5/Untitled%206.png)

이 Packet이 이제 Client로 최종적으로 날라간다. 

![Untitled](/network/images/network5/Untitled%207.png)

Frame으로 최종적으로 간다

이제 클라단 컴퓨터 상황을 보자

서버와 마찬가지로 

소켓과 연결된 File I/O Buffer도 존재하고

TCP Buffer도 존재하는 상황 

![Untitled](/network/images/network5/Untitled%208.png)

![Untitled](/network/images/network5/Untitled%209.png)

세그먼트가 대략 두번 오면

택배가 두개 왔으니 

잘 받았다고 ACK 3을 보낸다.

**서버 단에서도 사실 세그먼트 1,2를 보내고 3을 보내지않고 Ack를 기다리기 위해 Wait한다** 

![Untitled](/network/images/network5/Untitled%2010.png)

이로 인해 속도 지연이 발생한다.

TCP가 UDP느리다고 하는 이유가 이 때문

![Untitled](/network/images/network5/Untitled%2011.png)

그리고 이 TCP Buffer의 크기를 Window Size라 함 

**수신측**에서 세그먼트를 조립해서 넣는 공간 

ACK를 보낼때 Window Size를 포함해서 보냄 

![Untitled](/network/images/network5/Untitled%2012.png)

**3번을 보내는데 전송 할지 말지를 전송 window size를 보고 3번을 보내도 받을 공간이 없으면 안 보냄** 

수신측의 

- Windows size >  mss ? send
- **반대일시 Wait**

그럼 이제 수신측에서는 받지 못하는 상황을 피하기위해선 TCP Buffer의 Segment를 빨리 퍼 올려야한다.

그러니 Receive 속도가 중요

Read 속도가 네트워크 수신 속도보다 빨라야함 

![Untitled](/network/images/network5/Untitled%2013.png)

이 Windows Size를 잘 체크하면 네트워크 어플리케이션 문제를 찾을 수 있음

읽는 속도가 너무 느리고 Window Size가 계속 줄어들더라 

네트워크에서 장애원인을 찾으면 안됨 

클라이언트 프로그램에서 찾아야함 

수신이 느리다 : 결과적 현상 

송신이 느린게 서버가 느려서 그런지, 받을준비가 안되서 느린지

체크 필요 

# **TCP 연결이라는 착각에 대해**

연결지향

![Untitled](/network/images/network5/Untitled%2014.png)

port가 두개 존재

16bit으니 2^16 가능 근데 0, 1,65535도 안써서 그 뺀수 만큼 포트 사용가능

Sequence Number : 직소퍼즐의 번호 순서

32bit → 2^32 = 4GB

연결의 근거 ? 

3 way handshake

목적

- 시퀀스 번호를 교환
- MSS도 교환함 작은 쪽에 맞춤.
- 정책 교환 : 혼잡제어 SACK

![Untitled](/network/images/network5/Untitled%2015.png)

보안성이 없기에 속이려면 언제든지 가능함

# **TCP 연결, LAN선 뽑기 그리고 게임해킹!**

![Untitled](/network/images/network5/Untitled%2016.png)

여기서 랜 케이블을 뽑으면 TCP연결은 어떻게 될까?

뭔가 다운로드 하는 중에 랜 케이블을 뽑아보자 

5초 뽑았다 다시 꽂으면 보통 연결이 다시 된다.

10초… 20초.. 늘리고 몇 초 이상 지나버리면 연결자체가 끊겼다고 얘기하고 닫혀버림

랜섭이 꼽혀 있는거 Link up이라함 (<> Link Down)

이 TCP 레이어는 밑의 모든 계층 연결이 된다는 전제하에 이뤄지는데 선이 뽑히면..

Link Down을 네트워크의 충격이라함

유선에선 잘 안나지만 무선에서는 허구한날 발생 함

지하철타면 기지국을 계속 갈아탐

RFC표준에는 몇시간 단위로 굉장히 긴데 

대부분 아무리 오래잡아도 75초 이상 연결안되면 끊겼다고봄

만약 게임서버의 상황이라 생각해보자.

로그인하고 게임하는데 랜선을 75초이상 뽑았다 꽂았는데 제대로 연결됐다고 한다면 게임서버가 정상 작동을 안할 가능성이 높다.

이 사람이 하는 행동을 다른 사람 컴퓨터에도 업데이트 해줘야하는데 정상 작동이 안됐을 가능성이 높고 동기화가 깨질 가능성이 높음

그래서 이런 서버 개발할때 연결 세션 관리를 하는데 물리적인 충돌이있는데 이를 인지못하는지 아니면   신경쓰지 않아도 되는건지 지속적으로 확인하도록 프로그램을 짬.

이를 보통 keep alive라함 살아있는지를 계속 체크

랜선을 뽑아도 일정시간동안 유지된다.예민하면 5초만에.

# **Unicast, Broadcast, Multicast**

![Untitled](/network/images/network5/Untitled%2017.png)

라우터라는 애가 (NAT network address translation 공유기 느낌) 해당 라우터가 커버하는 영역 192.168.0.X ~  192.168.0.255 

- 192.168.0 c클래스 ip주소 Network id
- X Host id

라우터를 기준으로  내부 외부가 갈라짐 

## Unicast

![Untitled](/network/images/network5/Untitled%2018.png)

내부 통신

## Broadcast

L3 L2 모두 사용

Broadcast주소 : Host ID가 2진수로 모두 11111로 되어있을떄 

IP address의 broadcast 주소 : FF

MAC address 의 broadcast 주소 48비트 : FFFFFFFFFF

![Untitled](/network/images/network5/Untitled%2019.png)

전체 보냄

이더넷 : CSMA/CD 방식 사용

주황불 

네트워크 효율을 떨어뜨리는 주요원인

해당 broadcast 중에는 엔드포인트 중 누구도 신호를 보낼 수 없다 

## Multicast

IPGF 방송하는데  연결 tv가 2000대면 2000대 전체에 송신하는건아니고 송신은 1회만하고 송신자쪽에 등록하고 송신받을 Group 등록된 사용자보고 골라보냄

 L2수준에서는 Broadcast함’

SDN 에서 해당 부분 컨트롤하는게 중요 

# **IP주소의 종류와 특징**

![Untitled](/network/images/network5/Untitled%2020.png)

Global IP : Internet 이라는건 Public, Global  

→ 인터넷은 라우터의 집합체,  구성요소 라우터, DNS

라우터의 역할을 라우팅을 하는데 Global IP일때만 라우팅을 함 (강제로 세팅은가능)

기본적으로 인터넷에서 Global IP가 똑같은 놈은 없음 

Private : 작은 소규모 인터넷을 구축 

![Untitled](/network/images/network5/Untitled%2021.png)

ABCD 클래스가 존재

D클래스는 멀티캐스트 어드레스 형  

A클래스는 네트워크 id 8비트,  호스트 id 24비트

B클래스 : 16/16 : 큰 대학교 

C 클래스 24/8 일반기업에서 많이 사용  

![Untitled](/network/images/network5/Untitled%2022.png)

클래스별로 사설 ip주소가 정해져있음

A 클래스 10.xxx

B 클래스 172.16..xxx

C 클래스 192.168.xxx

공유기에서 많이 사용 함

이 사설 아이피로는 Global IP를 잘 못쓰는데 

이 공유기가 Global IP를 공유해줌

Loopback address 127.0.0.1 Host 자신을 의미

![Untitled](/network/images/network5/Untitled%2023.png)

Routing 되지않고 IP단에서 process B로 넘어감

# **전세계 인터넷을 멈추는 방법과 DNS**

이 DNS는 분산형 DB구조로 되어있음 

계층적으로 되어있어 Root DNS가 전세계에 13대있음 얘네를 다 멈추면 멈춰짐 

![Untitled](/network/images/network5/Untitled%2024.png)

실제 시도

2002/10/22 루트 DNS해킹 DDOS공격 사건 9대가 실제로 장애가 남

03.1.25일 인터넷 대란 발생

![Untitled](/network/images/network5/Untitled%2025.png)

[www.naver.com](http://www.naver.com) 을 도메인 네임이라 함 

www : 호스트 네임 

[naver.com](http://naver.com) : 도메인 네임

[www.naver.com](http://www.naver.com) 를 쳤을때

1. pc메모리의 dns cache를 뒤짐 
2. hosts file 검색
3. DNS에 물어봄

공유기가 DNS forwarding을 제공 그래서 얘가 DNS인거마냥 응답해주기도 함

서비스 하고있는 ISP에서 응답을 제공함 (kt 168.126.63.1 , skt …)

받으면 그걸 cache로 들고있음

8.8.8.8 Google DNS

![Untitled](/network/images/network5/Untitled%2026.png)

- cache dns에 질의 .com의 경우 RootDNS에 질의하면
- *.com 을 처리하는 DNS목록을 여러개 줌
- 그럼 다시 .com을 처리하는 DNS에 naver에 대해 질의
- naver를 다루는 DNS 목록을 줌
- www. 을 질의 그때 ip주소 전달

![Untitled](/network/images/network5/Untitled%2027.png)

모든 응답에 유효기간을 설정 expire되면 다시 물음 (캐싱 유지하던거 날림)

![Untitled](/network/images/network5/Untitled%2028.png)

![Untitled](/network/images/network5/Untitled%2029.png)

nslookup 을 통해 질의 가능

# **TCP/IP통신과 MAC주소의 변화**

![Untitled](/network/images/network5/Untitled%2030.png)

IP Packet 헤더는 바뀌지않지만

Frame 에서 목적지는 PC → R#1으로 향함.

R#1에서는 Frame 해더가 R#1 → R#2 로 됨 

MAC 어드레스는 패킷이 건너갈때마다 바뀐다.

MAC 어드레스가 유니크하긴하지만 L2에서만 쓰기에 다른 단에서 쓰는것 / IP 와 체계가 전혀다름 

![Untitled](/network/images/network5/Untitled%2031.png)

# ARP(address resolution protocol) 프로토콜

IP 주소로 MAC 어드레스 추적

L2구간에서는 MAC 우선 

![Untitled](/network/images/network5/Untitled%2032.png)

포트마다 MAC을 저장

Switch가 하는일 MAC을 저장시킴 

스위치 전원을 처음 켜면 학습모드로 들어감 

L2 Ethernet Frame이 왔다갔다하는데 MAC어드레스를 서로 기억함 

![Untitled](/network/images/network5/Untitled%2033.png)

연결된 MAC을 서로 기억함 

![Untitled](/network/images/network5/Untitled%2034.png)

C에서 보낼때 MAC을 기억하고 있으니 바로 A로 보냄

![Untitled](/network/images/network5/Untitled%2035.png)

10번 포트에서는 한 포트에 여러개가 있다고 생각 할 수도있음 

![Untitled](/network/images/network5/Untitled%2036.png)

만약 D, E랜선을 뽑아서 포트를 바꾼다면

재설정 or 다시 학습을 하던가 해야함 

ARP의 작동 

![Untitled](/network/images/network5/Untitled%2037.png)

3번 PC가  꺼졌다가 켜졌다고 하자 

설정상 IP주소는 알고있는데 GateWay의 MAC어드레스를 모르는 상황

네트워크 전체에 Broadcast 발송 쿼리를 날림 

![Untitled](/network/images/network5/Untitled%2038.png)

이중에 본인이 아니면 응답을 안함

Router가 응답을 하는데 이때 응답은 Unicast로 옴 

![Untitled](/network/images/network5/Untitled%2039.png)

![Untitled](/network/images/network5/Untitled%2040.png)

그 정보를 이제 메모리에 캐싱함 ARP CAche

ARP Spoofing 같은것도 가능 MITM

# **길 잃은 Packet의 소멸과 TTL**

![Untitled](/network/images/network5/Untitled%2041.png)

IP에 TTL값이 같이 들어가는데 

![Untitled](/network/images/network5/Untitled%2042.png)

라우터 사이의 간격 Hop

Hop을 한번 건널때마다 TTL값을 1씩 감소시킴

TTL을 8비트라 0~255값가짐 

값자체는 세팅, OS마다 다름 

TTL이 0일때 소멸함 

라우팅 세팅이 잘못되서 라우팅사이에 돌아버리는 경우 

![Untitled](/network/images/network5/Untitled%2043.png)

이때 라우터 cpu가 튀면서 상황이 지속되면 장비 다 죽음 

네트워크 사고중에 가장 무식하고 파워풀한 케이스가 Looping임 

Routing 이 버릴때 IP를 확인하고 ICMP 프로토콜을 이용해 시작 주소로 패킷 버려진것을 알려줌 (설정상 가능)

# **MTU와 Packet 단편화**

![Untitled](/network/images/network5/Untitled%2044.png)

단편화에 관한 얘기는 두번째 줄

![Untitled](/network/images/network5/Untitled%2045.png)

Router1이 2로 보낼때 2의 MTU가 1400이라 무조건 단편화가 발생할 수 밖에없음 

![Untitled](/network/images/network5/Untitled%2046.png)

분할되었을때 재조립을 한다면 재조립은 보통 수식측에서 담당을 한다 

현재에는 단편화가 거의 발생하지 않음 

VPN 사용시 단편화가 날 수 있음 

# **가래떡과 Stream**

![Untitled](/network/images/network5/Untitled%2047.png)

Header/Payload는 상대적인것

# **퇴근시간을 결정하는 TCP 장애유형 5가지**

![Untitled](/network/images/network5/Untitled%2048.png)

**장애 대응의 대원칙 : 밑에서 위로 감**

Layered 구조는 밑에가 성립해야 올라가는 것 

1. L1 L2 수준에서 Loss가 난것  (서버에서는 보냈는데 클라가 못 받은것) 
    1. 인프라사이에서 문제가 될 수도있음

TCP 상

1. Duplicate ACK  
    1. Seg먼트를 보내고  wait를 하는데, 속도 높이려고 미리 보내는 케이스가 잇음 
2. Retransmission 

→ 네트워크 지연을 의심 + 커널단에서 문제가 발생했을 수도 있음 

⇒ 네트워크가 **혼잡 상태로 인지** (TCP 혼잡 제어 )

네트워크 통신 속도를 낮춰버림 

1. Windows Size가 제로가 되는 경우 

⇒ 수신측 버퍼가 꽉 찬 경우 / 어플리케이션을 의심해야함 

1. TCP 통신이 리셋 

예를들어 네트워크에서 데이터를 받다가 강제로 킬하는 경우 

OS에서는 서버쪽으로 RST를 보냄 

스레드가 비정상적이거나 프로세스가 비정상 상태.

![Untitled](/network/images/network5/Untitled%2049.png)

서버에 연결된 PC가 100명이 있는데 서버가 죽으면 100명에게 리셋 100개가 확 가버림

MSA multi segment analysis : 구간마다 선을 연결해서 Out of pass로 포트 미러링 하듯이 캡쳐 구간단위 속도를 계측해서 어디서 패킷이 loss되는지 추적 

# **네트워크를 다시 또 내부로 자르는 서브넷팅**

서브넷 왜하는걸까? 

192.168.0.10

![Untitled](/network/images/network5/Untitled%2050.png)

각 필드 기준으로 경우의수 256개

0 ~ 255까지

![Untitled](/network/images/network5/Untitled%2051.png)

통상적으로 C class 의 경우 24bite를 네트워크 ID로 두고 Host 로 8비트를 둠 

전체가 0인경우, 전체가 1인 경우(Broadcast) 사용 X

그래서 총 254개만 사용 가능

C class 주소 하나에 할당하면 254개 사용가능 

만약 특정 회사에서는 C calss를 할당받고 ip가 100개만 필요한 상황이면 다른 154개의 주소가 낭비되는 상황 그래서 이를 방지하기 위해 서브넷팅을 이용 

![Untitled](/network/images/network5/Untitled%2052.png)

네트워크 ID를 25비트를 사용 그럼 할당가능 IP는 126개가됨 

나눠진 class를 더 잘게 쓰기위해 

서브넷을 나눌수록 2개 주소가 모자라게되기에 나눌수록 사용할수있는  ip가 줄어들어 나빠짐