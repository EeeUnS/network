---
layout: post
comments : true
title:  " 3. 네트워크 2"
date:   2020-01-01 22:53:01 +0900
---

# LAN과  WAN을 구별하는 방법

지역의 크기로 구분을 지으니 잘 납득이 안되더라

아래 설명은 내 주관적인 의견이 많으니 참고

![Untitled](/network/images/network3/Untitled.png)

- H/W : Physical → Physicla Network가 LAN 영역 1,2계층
- S/W : Logical → 여기에 속하는 IP는 Vitual Network라 할 수 있음 이게 → WAN의 영역

- LAN : 하드웨어로 설명되는 네트워크
- WAN : 논리로 설명되는 네트워크

2계층에서 중요한 식별자 MAC 주소 (48bit) 이 MAC주소로 식별되는 네트워크또한 LAN임

- 4계층 식별자 IP : WAN 식별
- MAC 주소에 기초하여 작동하는 네트워크 - LAN 식별

ip주소, mac주소 중에 특수 주소라는게 존재함 방송주소라고 함 : Broadcasting의 적용 주소 범위가 LAN이라고 함

# **패킷의 생성 원리와 캡슐화**

![Untitled](/network/images/network3/Untitled%201.png)

Process → Socket → TCP 로 넘어갈때 Stream 을 send할때 자동으로 Segmentation 이 될때 분해가 일어남 

Packet는 약 1500 Byte(MTU)크기 만큼되고

구조는 아래와 같음

- Header
- PayLoad

헤더 구조

![Untitled](/network/images/network3/Untitled%202.png)

이때 헤더에서 20바이트씩먹으니 PayLoad 크기는 1460byte 이것이 (MSS)최대 세그먼트 크기가됨 

그러니 스트림을 1460씩 짤라서 보냄

![Untitled](/network/images/network3/Untitled%203.png)

보안등의 이유로 패킷을 조사할 수 있음 이를 DPI라 부름 (Deep Packet Insepction)

![Untitled](/network/images/network3/Untitled%204.png)

예를들어 소켓 send를 통해 2000byte 를 보낸다치면 적어도 패킷 두개가 갔다고 생각하면 됨 

Segment(L4 내용물) → encapsulation → Packet(L3 택배)/ Header(송장) → encapsulation → Frame (L2 트럭)

# L2 스위치에 대해서

L2 :  여기에 속하는 정보는 MAC 주소 

L2 스위치 : 맥 어드래스로 스위칭 하는것

ipconfig /all 을 할 시 볼수있음 

![Untitled](/network/images/network3/Untitled%205.png)

물리적 주소 

NetWork란 Swtich의 집합체로 볼 수 있음 

![Untitled](/network/images/network3/Untitled%206.png)

네트워크 도면

PC 간 연결 : L2 Access 

스위치간 연결 : L2 Distibution 

네트워크 자체와 네트워크를 이용하는 이용 주체를 구분

→ 이용 주체 EndPoint

EndPoint가 네트워크에 처음만나는 스위치가 L2 Access 스위치라함 (무선일수도있음)

![Untitled](/network/images/network3/Untitled%207.png)

![Untitled](/network/images/network3/Untitled%208.png)

상위계층으로 가는 연결선을 up link라함

그런데 Link up과 Link Down 이란 용어도 있는데 Link up은 랜케이블이 들어왔을때 녹색불이 들어와 연결 표시가될때 Link up이라함  ↔ Down 은 반대 

![Untitled](/network/images/network3/Untitled%209.png)

각 스위치의 모양

![Untitled](/network/images/network3/Untitled%2010.png)

그림의 공유기 L2 스위치는 interface가 16개 (port)

엔드포인트에 연결할수있는 갯수는 최대 16개인데 uplink 포트 하나 빠지면 15개까지 가능 

# **IP헤더 형식과 의미 요약**

![Untitled](/network/images/network3/Untitled%2011.png)

![Untitled](/network/images/network3/Untitled%2012.png)

전 영상에서 ip헤더가 20바이트라고했는데 해당 포맷에서 앞의 20byte가 언급한 20byte에 해당한다 

- option 이 40바이트 까지 있을 수 있고(주로없음)
- data가 이론상 최대 65546 byte =2^16 = 64kb 까지있을 수 있다.

하지만 현재 MTU는 여전히 1500바이트인 상황 

![Untitled](/network/images/network3/Untitled%2013.png)

- version : ipv4에서의 version 일반적으로 4
- Total Length : 16비트 → 그래서 Data는 2^16bit까지 가능
- IHL : Internet Header Length 일반적으로 5 윗 한줄이 32bit = 4byte인데 X 5 = 20byte로 헤더가 20바이트라 값이 5
- TOS Type of service : 생략
- Identification : 두번째줄은 주로 단편화에 관련된 값들이 기입
    
    MTU가 가끔 1400밖에 안되는 놈들이 있는데  이런놈한테 1500byte 송신하면 수신을 못하는 상황 발생 그래서 얘를 또 짜르고 조립도 해야함. 그런것에 관련된 라인
    
- TTL Time To Live : Internet =~ Router의 집합체 (패킷을 유통) Router를 거칠때마다 TTL값이 1씩감소 ⇒ 8비트 대역 크기 0 ~ 255 개의 경우의 수가 되는데 0이 되면 라우터가 패킷을 버림
- Protocol : 4계층 헤더가 무엇인지 Data를 해석
- Header Checksum : checksum
- Source address
- Destination address

# **Wireshark의 내부구조와 작동원리**

![Untitled](/network/images/network3/Untitled%2014.png)

Driver 와 IP사이에는 Filter라는게 존재

- bypass : filter에 걸려져 통과
- drop : 막히는 경우

- Out bound : NIC 를 통해 바깥으로 나가는 경우
- In Bound : 안으로 들어오는 경우

![Untitled](/network/images/network3/Untitled%2015.png)

이 Filter에서 지나가는 모든 것을 통과시키나 이를 보는것을 Sensor라 함 

- 항상 bypass - 감시, 수집

와이어샤크를   설치할때 이 Sensor가 설치가 됨 

현재는 Npcap 이라함 , 옛날에는 WinPcap

그래서 와이어샤크는 얘를 수집하고 디코딩, 분석함 

목적따라 도감청, sniffer가 될 수도 있음

# Router의 내부구조와 inline

router : L3, Packet

![Untitled](/network/images/network3/Untitled%2016.png)

이 라우터들도 호스트라 할 수 있다 ip가 부여됨  = Computer라 할 수 있음 

![Untitled](/network/images/network3/Untitled%2017.png)

해당 라우터의 경우 NIC가 두개가 있다고 볼 수 있음

![Untitled](/network/images/network3/Untitled%2018.png)

그래서 패킷이 오면 파란색 선처럼 타고 넘어갈텐데.

이 레이어를 거치는 비용이 굉장히 큼

이때 NIC 1에서 2로 가기위한 처리가 1(유저),2(커널),3(하드웨어), 중에서 날 수 있는데 

3에서 처리를 하는거를 하드웨어 가속했다고 볼 수 있음 

Pakcet 처리를 IP 단에서 한다고 보면 

![Untitled](/network/images/network3/Untitled%2019.png)

패킷을 레이어 단에서 읽고 쓰기를 안할 수도있다 이건 그냥 패킷이 버려진것 Drop

필터에서 전달에 관해 아래 처리만을 한다면 라우터라 할 수 있음

- bypass
- drop

그런데 보안적인 이유로 packet 필터링을 한다면 방화벽이라 할 수 있다. (패킷 필터링 방화벽)

Inline 

![Untitled](/network/images/network3/Untitled%2020.png)

패킷 단위 처리를 하는 장치 : inline 장치

# **Inline 구조와 Out of path 구조**

![Untitled](/network/images/network3/Untitled%2021.png)

게이트웨이 10.10.10.1

![Untitled](/network/images/network3/Untitled%2022.png)

그럼 해당 라우터 안쪽으로 내부망이라 봄 

Inline 장치는 톨게이트라 보면 됨

![Untitled](/network/images/network3/Untitled%2023.png)

Port Mirroring : 이때 스위치에 특정 포트를 빼서 연결되어있는 모든 포트를 복사하여 읽기만 함 이를 Out of path라함  → Sensor로서의 작동을 할 가능성이 높음, (장애대응 탐지용으로 사용 가능) 

근데 모두 복사를하다보니 해당 스위치 cpu가 튀어 과부하가 걸릴 수 있음 

![Untitled](/network/images/network3/Untitled%2024.png)

그래서 Tab 스위치로 따로 copy 용 스위치가 따로 있기도 함, 모든 패킷을 bypass 시킴