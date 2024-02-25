# 1. 네트워크

# 네트워크를 배우려는 사람들을 위해

OSI 7 Layer는 완벽하게 이상에 해당한다 목숨걸지 말 것.

![Untitled](/network/images/network1/Untitled.png)

![Untitled](/network/images/network1/Untitled%201.png)

- 7계층
- DoD에서 정한 4계층을 기준으로 하는것이 맞음
    - Application
    - Transport (TCP) / 식별자 Port 번호
    - Network(IP) / IP 주소
    - Access ( NIC Network Interface Card) / Mac
- 각 사이를 연결하는것
    - File
    - Divice Driver

# **MAC주소, IP주소, Port번호가 식별하는 것**

![Untitled](/network/images/network1/Untitled%202.png)

Mac address(L2) : Network Interface Card에 대한 식별자(Lan 카드 유선/무선) == 하드웨어 주소 

예를들어 노트북이 유선, 무선 랜카드 들고있으면 NIC가 두개가 있는셈

ip주소(internet protocol)(L3) : Host에 대한 식별자

host(L4) : **인터넷에 연결된** 컴퓨터

한 컴퓨터에는 ip주소가 몇개가 있을까 → n개 가능

NIC에 여러개의 ip를 사용할 수 있음 → 이걸 바인드한다고 함

![Screen Shot 2023-03-22 at 7.46.20 PM.png](network1/Screen_Shot_2023-03-22_at_7.46.20_PM.png)

MAC : 변경가능한가? → 변경가능하다.

Port번호에 대해서 주로 다루는 계층에 따라 대답이 달라 질 수 있다.

- sw개발자, 관리 : 프로세스 식별자
- 네트워크 관리하시는분(4계층 다루는 사람) : Service 식별자?

![Screen Shot 2023-03-22 at 7.47.48 PM.png](network1/Screen_Shot_2023-03-22_at_7.47.48_PM.png)

HTTP는  TCP port 80번을 씀 근데 8080을 쓰기도함

그래서 네트워크 관리하는 사람에게는 80번 열어달라기보다 HTTP 서비스 열어달라는게 더 적절한 요청이 될 수 있음 

- 물리적으로 하드웨어 쪽에 가까운사람 : interface 번호

공유기에 유선으로 랜케이블 꽂을 수 있는 단자의 번호

# **Host, Switch, Network 이들의 관계에 대해...**

Host : Network 연결 된 컴퓨터

![Screen Shot 2023-03-22 at 7.53.29 PM.png](network1/Screen_Shot_2023-03-22_at_7.53.29_PM.png)

호스트 종류

- 네트워크 자체를 이루는 호스트 → Switch
    - Router
    - Firewall
    - ips
- 네트워크를 이용주체로서 호스트 → **End-Point (단말)**
    - peer  = server + client
    - server
    - client

Internet을 이루는 가장 중대한 구성요소

1. 라우터
2. DNS

Switch : switching 하려고

FW/ IPS로 스위치임 보안때문에 스위칭을 하는 것 보안스위치라함

Router도 스위칭 함 경로찾으려고

MAC을 가지고 스위칭함 ⇒ L2 스위치

ip주소를 가지고 스위칭 → L3 스위치

HTTP의 뭘 가지고 스위칭 → L7스위치

Layer가 높을 수록 연산이 복잡함

![Screen Shot 2023-03-22 at 8.02.28 PM.png](network1/Screen_Shot_2023-03-22_at_8.02.28_PM.png)

- TCP/IP 계층달라서 슬래시 들어감.

공유기는 스위치라 할 수 있을까? 맞다면 몇 계층일까? 

# **IPv4주소 체계에 대한 암기사항**

IPv4 : 32bit

~~IPv6 : 128bit~~

![Screen Shot 2023-03-22 at 8.10.31 PM.png](network1/Screen_Shot_2023-03-22_at_8.10.31_PM.png)

ip주소는 8비트씩 끊어서 . 표시

![Screen Shot 2023-03-22 at 8.12.03 PM.png](network1/Screen_Shot_2023-03-22_at_8.12.03_PM.png)

앞에 24비트씩을 끊어서 Network ID라 부른다 뒤의 8비트를 HostID라 한다

이거는 예시고

IP주소는 = NetID + HostID

이 네트워크 ID의 길이를 나타내는게 NetMask

예를들어 NetMask가 255.255.255.0 이다 하면 AND연산해서 나온게 네트워크 id가 됨.

# **개발자 입장에서 Port번호 이해하기**

![Screen Shot 2023-03-22 at 8.19.05 PM.png](network1/Screen_Shot_2023-03-22_at_8.19.05_PM.png)

TCP/IP를 UserMode 어플리케이션으로 제공하는게 본질은 파일이나 소켓이라 한다.

소켓은 파일같은거지만 TCP 소켓인경우 attach 되는 정보중 하나가 port 번호임

port번호는 16비트 정보인데  0 ~ 65535 임 이중에 0과 65535 를 뺀 65533개를 사용함

# **Switch가 하는 일은 Switching 이다.**

![Untitled](/network/images/network1/Untitled%203.png)

Switch : 교차로 

경로, 인터페이트 : 길

switching : 경로 선택하는 것 

그래서 목적지까지 어떻게 빨리 갈까?

Internet = Router의 집합체(라우터는 L3 스위칭)

여기서 교차로는 Router가됨

그림의 교차로로 보면은 4개의 네트워크 인터페이스를 가진 Router가 됨 

한곳에서 정보(패킷)가 도착하면 세곳중 하나를 고르는게 스위칭

빠르게 보내기위해 라우터끼리 통신함

라우터 몇개가 망가진다고 인터넷 전체가 손상되지않음

최적화된 이정표 = **라우팅 테이블** 

# 네트워크 데이터 단위 정리

![Untitled](/network/images/network1/Untitled%204.png)

![Untitled](/network/images/network1/Untitled%205.png)

프로세스 존재 TCP/IP 존재 그사이를 잊는 추상화된 File interface = socket

TCP IP 와 NIC 를 잊는 Divice Driver 

File 은 기본적으로 Stream이라고 생각할 것

시작은 있는데 끝은 없음.

길이가 굉장히 길어질 수 있음.

- TCP에서 다루는 단위 Segment
- IP수준에서 다루는 단위 : PAcket
- 그 밑에서 Divice, NIC 단 단위 Frame

![Untitled](/network/images/network1/Untitled%206.png)

그럼 이제 File에다 Write를 한다고 보자.

- Stream 데이터를 TCP Segment화 함 → Segmentation (일정 길이로 분해 ) 됨
- 이 일정길이의 최댓값 : Maximum Segment Size
- 이 MSS는 Packet의 최대 크기에 기반해서 만들어짐.
- Packet의 최대크기 : MTU maximum transport unit : 일반적으로 1500바이트 (1.5kb)

1.5mb stream은 1000개의 Packet으로 쪼개져서 보내진다고 보면됨.

이 Packet이 Frame 화되는게 Encapculation 이라 함

MTU > MSS

# 네트워크 인터페이스 선택 원리와 기준

![Untitled](/network/images/network1/Untitled%207.png)

컴퓨터가 유선도 연결되어있고 무선도 연결되어있으면 ip 주소는 몇개 일까?

- 두 개

유선은 KT를 쓰고 무선은 SKT를 썼다고하자 

두개의 인터넷이 연결된상태에서 크롬을 썼다 그러면 소켓이 열릴건데 TCP/IP binding이 되고 각각의 ip에 대해서 device driver 가 연결된다

유선 #1  skt → Router DNS  

무선 #1  kt → Router DNS 

![Untitled](/network/images/network1/Untitled%208.png)

그럼 네이버를 본다고했을때 유선을 통해서 할까 무선을 통해서 할까 

연결된 Network Interface 선택이 필요 

![Untitled](/network/images/network1/Untitled%209.png)

route print 

![Untitled](/network/images/network1/Untitled%2010.png)

해당테이블에 근거해서 선택 

interface에 주소가 박혀있음 

매트릭값 : 비용