# 네트워크 응용

# **알아두면 개발자 인생 업그레이드되는 공유기 작동원리**

![Untitled](/network/images/network6/Untitled.png)

공유기는 기본적으로 L2 스위치를 내장하고 있음

= 라우터 + L2 스위치 

인터넷 공유기는 **IP주소 공유기**라 봐야함.

**Global IP 를 공유한다**

또한 내부에서 IP를 변환한다 192.168.0.1 로

![Untitled](/network/images/network6/Untitled%201.png)

IP 주소를 변환하는거니 기본적으로 L3레이어에서 작동함

192.168.0.10 인 PC가 네이버에 접속을 한다고 보자.

그러면 게이트웨이인 Router로 감

그때 공유기 내부의 NAT-Table을 만들게된다.

![Untitled](/network/images/network6/Untitled%202.png)

현재 테이블 상황이 이러한데 이 로컬 IP를 글로벌 IP, 자체 Port로 변환해버린다 

패킷 해더를 바꿔 보낸다.

![Untitled](/network/images/network6/Untitled%203.png)

![Untitled](/network/images/network6/Untitled%204.png)

그렇게 네이버 입장에서는 논리적으로 TCP/IP연결이 3.3.3.3 20000 port로 연결이 되어있다고 착각함

응답 패킷이 왔을때도 패킷 헤더를까서 변환함 

![Untitled](/network/images/network6/Untitled%205.png)

만약에 추가로 노트북이 네이버에 접속해 연결했다고 생각하자.

네이버 입장에서는 3.3.3.3이 두번 접속했다고 생각하게된다.

공유기는 자동으로 FireWall 기능이 자동으로 작동해서 보안성이 상승한다.

해커 4.4.4.4가 3,3,3,3 80 으로 만약 패킷을 보낸다면 

공유기가 NAT-TAble에 따로없으니 패킷을 버린다.

공유기가 들어오면서 웜 바이러스 피해가 많이 죽음

# **공유기를 뒤집으면(?) 부하분산 장치**

![Untitled](/network/images/network6/Untitled%206.png)

그냥 반대로…

로드밸런싱

제일 간단하게는 

철수가 왔을때 Web#1에 연결시켜주고

영희가 왔을떄 Web#2에 연결시켜주는 식으로 분산시켜버리면 됨..

조금 더 들어가면

Health Check를 통해서 

- cpu
- network 대영
- RAM 사용량

등의 정보를 가지고 처리 

LB는 L4스위치로 보통 구현을 함

로드 밸런서가 죽으면 통신이 그냥 막혀버리니

두대를 설치해서 2중화를 해버림

# **VPN?? 그럼 PN(Private Network)이 무엇인지는 알고 있는 거죠?**

![Untitled](/network/images/network6/Untitled%207.png)

Virtual = Logical ⇒ SW 상에서 구현 

**PN(Private Network) 부터 알아보자.**

PN은 LAN임

![Untitled](/network/images/network6/Untitled%208.png)

Router를 입구로 외부 접근은 차단하고 내부에서만 왔다갔다 할 수 있게 해놨는데

재택근무로 바뀌었다고 보자 PC#5가 생겼다.

9.9.9.10 

![Untitled](/network/images/network6/Untitled%209.png)

가장 무식하게 생각하면 유선 떼서 집까지 연결하면 된다.

![Untitled](/network/images/network6/Untitled%2010.png)

만약 서울 본사에서 부산까지 확장한다면?

KT같은데 연락해서 연결선 몇개 빌린다고 빌려버림 

문제는 Private Network 구축에 가격이 너무 비싼 문제가 생김

이를 해결하기위해 VPN 등장

# **개발자는 알아야 할 VPN 작동원리**

![Untitled](/network/images/network6/Untitled%2011.png)

VPN 지원하는 Router가 필요

VPN 지원하는 Router = SG / Secure GateWay

외부 PC#3에서 VPN Client 소프트웨어를 설치해야함

연결이되면 터널링이 됐다고 표현함.

![Untitled](/network/images/network6/Untitled%2012.png)

이때 회사 IP를 하나 더 받음

![Untitled](/network/images/network6/Untitled%2013.png)

IP주소가 두개가 됨 

9.9.9.9 는 ISP로 받은 IP

![Untitled](/network/images/network6/Untitled%2014.png)

![Untitled](/network/images/network6/Untitled%2015.png)

Secure Gateway

![Untitled](/network/images/network6/Untitled%2016.png)

그 후 내부를 싹다 암호화 시켜버림 

SG는 이 패킷을 받고나서 터널링 된 컴퓨터를 인식하고 Server로 응담 

![Untitled](/network/images/network6/Untitled%2017.png)

![Untitled](/network/images/network6/Untitled%2018.png)

SG는 이 터널링을 통해 전달

![Untitled](/network/images/network6/Untitled%2019.png)

![Untitled](/network/images/network6/Untitled%2020.png)

단편화 문제로인해, VPN으로는 MTU를 기본적으로 줄여버린다.

# **IT전문가라면 반드시 알아야 할 네트워크 보안 종류#1**

![Untitled](/network/images/network6/Untitled%2021.png)

Segment Size 1460 byte 

Packet MTU 1500

Ethernet 대략 14Byte

Frame 1514 byte

Access 계층, IP계층까지 같이보는 보안 솔루션

NAC Network Access Control 제품

Probe  + 관리서버 

⇒ IP주소 MAC 주소 보는 솔루션

![Untitled](/network/images/network6/Untitled%2022.png)

1. L2Port (Interface ) Down 시켜버림
2. HTTP 통신 Redirect 
3. DHCP 통제
4. ARP Spoofing  ≤ 장애를 꽤 많이 일으킴

진짜 NAC

![Untitled](/network/images/network6/Untitled%2023.png)

(기간계)인증 담당 서버 존재

RADIUS

TACACS

무선보안

WIPS : 무선네트워크 못쓰게 차단

여기까진 물리적 네트워크 담당 

![Untitled](/network/images/network6/Untitled%2024.png)

F/W (Packet Filter )

비슷하게 할 수 있는것

Screering Router 

L3 수준에서 통제가능 

Packet을 보겠다 (Segment도 ) stream은 안봄

---

Application 단에서 작동하는 F/W  Application Firewall

Web관련 **WAF**

Proxy형태

이를 묶어서 Hybrid F/W

F/W : TCP Stateful Inspection 기능 제공

상태 이전(TCP는 연결지향이니 상태존재) 감지

VPN(IPSec)

- GtoG(gate)
- GtoE (endpoint)

![Untitled](/network/images/network6/Untitled%2025.png)

터널링을 통해 망접속

![Untitled](/network/images/network6/Untitled%2026.png)

SSL VPN도 존재 (HTTP 통신 한해서 Proxy 구조)

![Untitled](/network/images/network6/Untitled%2027.png)

다 합치면 UTM Unified three management system  ⇒ 성능 떨어짐 (중 소 용)

Access 단에서 망분리 망연계도 존재 

![Untitled](/network/images/network6/Untitled%2028.png)

여기까지 네트워크 보안 인프라라고 함  (철조망 같은 수동적임)

설치 + 설정(정책)이 중요

![Untitled](/network/images/network6/Untitled%2029.png)

IP TCP 헤더를 보다가 그 안쪽 stream 영역 부분을 봐야할때가 있는데  Deep Packet Inspection 라 함 

이 전체를 하겠다고 하는게 NIDS, IPS라함 

- NIDS (Out of Path)
- IPS(Inline)

악성코드 분석기술중에 SandBox라고 악성코드를 집어넣고 감시해서 악성여부를 판단하는데 이 SandBox를 합쳐버림 

![Untitled](/network/images/network6/Untitled%2030.png)

MPS  malware  Prevention system 

뭐 결과적으론 잘안됨

혼자 작동하지않고 Cloud가 되어버리는데 이중 **평판 시스템(**사실상 입소문**)** 이라함 

백신과 비교했을때 평판시스템이 더 좋기에 현재의 백신은 평판시스템과 연동이되어있음

이 능동형의 경우 엄청난 로그 데이터를 생성함

이에 대한 통합관리 시스템이 등장  ESM Enterprised Security Management system 

![Untitled](/network/images/network6/Untitled%2031.png)

이 로그를 분석하는 시스템은 SIEM Security information event management  → 여기에 AI가 많이 사용

보안쪽 취업해서 보안 관제센터가면 ESM SIEM 관리해서 로그나온걸로 보고서 사용

# **ARP Spoofing 공격과 NAC(△)의 원리**

ARP 프로토콜

![Untitled](/network/images/network6/Untitled%2032.png)

L2 구간에서는 MAC주소가 중요

![Untitled](/network/images/network6/Untitled%2033.png)

ETHERNET은 L2구간마다 바뀜

GW 세팅하고

ARP Req 3.3.3.1 M< Reply

![Untitled](/network/images/network6/Untitled%2034.png)

속였을거라 생각하지 않고 믿어버림

![Untitled](/network/images/network6/Untitled%2035.png)

PC#3이 arp req를 보내지도않았는데 PC#1과 Router에 Reply를 보내버림 

GateWay에게  본인이 3.3.3.10이고 내 MAC address 는 PC#3이라고 속임

이러면 Router가 PC#1에게 올 패킷을 PC#3에 보내버림 

![Untitled](/network/images/network6/Untitled%2036.png)

그러면 이를 다시보고 PC#1에 보내버림

모조리 도청이 가능해짐.

- 위/변조까지 가능해버림.

MITM 어택이 가능한 수준

같은 LAN이라 내부자가 내부자를 해킹하는 상황.

게임방의 경우 서로가 남일 수 있음..

L2 스위치가 진작에 SEcure L2 스위치로 바뀌어버림

![Untitled](/network/images/network6/Untitled%2037.png)

NAC 장비의 Out of path 구조로 L2 스위치 Probe라 불리는 놈이 설치해서 Copy떠서 감시를하는데 

PC#1 외부의 허가되지않는 PC가 접속해서 사용하려고할때 

![Untitled](/network/images/network6/Untitled%2038.png)

각각에 ARP spoofing을 해서 Gateway를 Probe라 속여버리면 PC#1은 인터넷이 안될것.

![Untitled](/network/images/network6/Untitled%2039.png)

그런데 스텝이 엉키면서 다운될 수도있음

스위치 오동작을 일으킬수도있어 이를 통해 장애가 발생할 수 있음 

# **고급 개발자가 되려면 반드시 알아야 하는 TCP 혼잡제어**

![Untitled](/network/images/network6/Untitled%2040.png)

![Untitled](/network/images/network6/Untitled%2041.png)

연결 절차 3Way  : 주로 정책교환 , MSS SACK 

기본적으로 양쪽 다 Buffered IO를 함

이중에 착각하면 안되는것 중 하나가 

Buffer크기 보통 64kb로 잡음

64kb마다 끊어서 보냄

![Untitled](/network/images/network6/Untitled%2042.png)

**즉시 패킷을 보내지않음  Wait가 걸림**

TCP는 정책에 따라서 기다렸다 보내기도하고 생각한것보다 빨리 보내기도함

Recv는 항상 끊어낸다라고 생각해야함

3Way중에 SRTT 값이 있는데 서버-클라까지 갔다가 돌아오는 시간

이 값에 따라 Wait정책이 바뀜

장애의 대표적인 Loss, Out of order 

![Untitled](/network/images/network6/Untitled%2043.png)

패킷 순서가 반대로 오는 경우

 

![Untitled](/network/images/network6/Untitled%2044.png)

즉각적 반응을 Req해버리는 순간  

서버단에서는 1,2,3을 모두 보냈는데 2,3 모두 못받은걸로 판단하고 재전송을 하게 됨

이 Wait 시간?? 구현에 따라 다른데 SRTT값에 따라 정해짐 

이런 모든 상황을 TCP 혼잡 상황이라 함

![Untitled](/network/images/network6/Untitled%2045.png)

처음 TCP연결이된후에 전송은 다음의 그래프를 가지는데 

![Untitled](/network/images/network6/Untitled%2046.png)

Out of order가 나거나 Retransition, Duplicate 등이 일어나면 전송속도를 팍 줄임

![Untitled](/network/images/network6/Untitled%2047.png)

이를 줄이기위해 Reno -< New Reno 등의 방식이 나왔는데 현재는 SACK 방식을 

Loss, Out of order, 는 네트워크 인프라가 문제

Retransition 등의 문제는 TCP 정책 문제 

TCP 정책 뜯기 리눅스는 가능하나 윈도우는 안됨 

UDP 를 이용해 TCP를 흉내내볼 수 있음.

⇒ FPS 게임 → [reliable udp](http://blog2928.kc39.net/13263)

환경 만드는 법 

(서버)PC 유선, 휴대폰 SKT 테더링하여 랜선을 뽑거나 단선등을 유발해 테스트 가능 

# **망분리에 대한 이야기**

망분리

- 물리적
- 논리적 → 가상화

인터넷

Window 화면 폐쇄망 내부로는 갈 수 있는데 내부로는 갈 수 잇는데

외부 인터넷으로 가기위해 VM을 사용 

![Untitled](/network/images/network6/Untitled%2048.png)

Cloud VM PC를 설치

폐쇠 PC에서 VDI로 원격 접속해서 VM PC에서 Internet사용

![Untitled](/network/images/network6/Untitled%2049.png)

![Untitled](/network/images/network6/Untitled%2050.png)

VM에서 악성코드 감염됐을때도 SandBox로 추가로 막아서 처리가능

VDI사용시 VPN필요가없음

# **SACK에 대한 간략한 소개**