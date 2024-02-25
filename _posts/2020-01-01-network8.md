---
layout: post
comments : true
title:  " wireshark"
date:   2020-01-01 22:53:01 +0900
---

![Untitled](/network/images/network8/Untitled.png)

Wireshark

# **Wireshark 라이브 패킷 캡쳐 및 저장**

라이브 캡처 : NIC → IO 발생하는걸 실제로 캡쳐

이를 목록(메모리에 들고있음) → 화면에 보여주다 file(Trace File )로 저장도가능

![Untitled](/network/images/network8/Untitled%201.png)

![Untitled](/network/images/network8/Untitled%202.png)

더블 클릭하면 패킷 캡처 시작

잠깐만 캡처해도 엄청 쏟아져나오는데 

내가 필요로하는걸 찾아보는게 가장 중요

![Untitled](/network/images/network8/Untitled%203.png)

capture → option 뭐할지

Capture Filter   BPFs세팅

→ 원하는 부분만 수집가능

trace file 은 200mb정도까지만하고 그 이상 크기는 wireshake 로 열지 말것

![Untitled](/network/images/network8/Untitled%204.png)

http 분석을 한다면

![Untitled](/network/images/network8/Untitled%205.png)

해당방식으로 호스트 추가하면

![Untitled](/network/images/network8/Untitled%206.png)

볼 수가 잇음

컬럼추가는 인덱싱을 컴퓨터 연산으로 처리 

부하심하면 컬럼주가 지우는 방법으로 처리 할 수도 있음

![Untitled](/network/images/network8/Untitled%207.png)

URI 추가하는것도 추천

# **Wireshark 세션 컬러링 및 패킷 조립**

세션 : 연결 정도로 생각 TCP HTTP

wireshark 자체에서 번호를 붙임

TCP 처음 발견된애한테 0번 부여

![Untitled](/network/images/network8/Untitled%208.png)

6번 처음 나온 TCP 패킷

해당 컬러 지정하면 해당 통신에 대한것만 Colorring됨

![Untitled](/network/images/network8/Untitled%209.png)

tCP stream 클릭시 

패킷만 싹다 조립해서 볼 수 있음

목록에도 해당 통신만나타나도록

ctrl space 취소 

![Untitled](/network/images/network8/Untitled%2010.png)

reset 으로 취소가능

ctrl 1, 2 3 등으로 지정가능

# **Wireshark 패킷에서 파일 추출하기**

![Untitled](/network/images/network8/Untitled%2011.png)

까만색은 장애

![Untitled](/network/images/network8/Untitled%2012.png)

http 선택

![Untitled](/network/images/network8/Untitled%2013.png)

파일 전체 뽑아서 보거나 저장이가능

![Untitled](/network/images/network8/Untitled%2014.png)

Follow 하면 전체 패킷 조립해서 보기가능

![Untitled](/network/images/network8/Untitled%2015.png)

분석을 알아서 해줌

# **Wireshark로 통계정보 보기**

![Untitled](/network/images/network8/Untitled%2016.png)

여기 잘쓰는게 중요

TCP UDP 통계도 가능

# **Wireshark에서 디스플레이 필터 적용해 원하는 정보 찾기**

필터 문법

![Untitled](/network/images/network8/Untitled%2017.png)

!( A + B)

![Untitled](/network/images/network8/Untitled%2018.png)

ip.src == 24.6.173.220

ip.addr == 24.6.173.220

![Untitled](/network/images/network8/Untitled%2019.png)

이 방식으로도 필터링가능

![Untitled](/network/images/network8/Untitled%2020.png)

아래 퍼센트가 보이는 정도 

!(ip.src == 24.6.173.220)

ip.dst == 208.93.137.0/24  : 대역이 전체 다 나옴

![Untitled](/network/images/network8/Untitled%2021.png)

대충 거른다음에 

대충 stream을 찍어서 봄 

# **Wireshark로 HTTP 트래픽 분석 및 패킷 내용검색하기**

HTTP 보는법

HTTP 만 쳐도 필터됨

![Untitled](/network/images/network8/Untitled%2022.png)

[http.host](http://http.host) 필터찍으면 호스트 필드존재하는건 다 나옴

![Untitled](/network/images/network8/Untitled%2023.png)

host 는 Req 쪽에있음

서버 응답은?

http.server

![Untitled](/network/images/network8/Untitled%2024.png)

![Untitled](/network/images/network8/Untitled%2025.png)

웹서버 다 알수있음

http.response.code == 500 

http.host. contains ==  ‘www’

contain 동일한것만이아닌 포함된건 모두

여러 filter

![Untitled](/network/images/network8/Untitled%2026.png)

![Untitled](/network/images/network8/Untitled%2027.png)

![Untitled](/network/images/network8/Untitled%2028.png)

# **Wireshark으로 TCP 장애원인 분석하기**

![Untitled](/network/images/network8/Untitled%2029.png)

에러 내용을 구체적으로 볼때 해당 io graphs를 이용하면 좋음

![Untitled](/network/images/network8/Untitled%2030.png)

장애대응 순서 무조건 밑에서 위로

Hardware 수준에서 문제가 잇었는지  : 랜케이블 빠졌는지?

네트워크 사이에선 

- Lost
- Out of order

![Untitled](/network/images/network8/Untitled%2031.png)

이런걸 보정하기위해

OS단에선

- Retransemission
- Duplicated Ack

![Untitled](/network/images/network8/Untitled%2032.png)

어플리케이션단 Windows Size 문제

![Untitled](/network/images/network8/Untitled%2033.png)

![Untitled](/network/images/network8/Untitled%2034.png)

IO graph를 보자

들쭉날쭉한데 반듯한게 좋음

빨간색 TCP Errors는 위의 5개중 어느거라도 해당하면 표시됨

이를 끊어볼 필요가있음

Lost Segment tcp.analysis.ack_lost_segment

![Untitled](/network/images/network8/Untitled%2035.png)

그래서 개별 등록

tcp.analysis.out_of_order

![Untitled](/network/images/network8/Untitled%2036.png)

tcp.analysis.retransmission

tcp.analysis.duplication_ack

![Untitled](/network/images/network8/Untitled%2037.png)

그래프보면 에러처리하다 트래픽이 튀어버림

tcp.analysis.zero_window

![Untitled](/network/images/network8/Untitled%2038.png)

![Untitled](/network/images/network8/Untitled%2039.png)

제로윈도우되면서 통신이 아예 안됨

수신측 버퍼가 full인 상황

![Untitled](/network/images/network8/Untitled%2040.png)

![Untitled](/network/images/network8/Untitled%2041.png)

요 그래프가 장애랑 바로 연관이 있음

seq number가 증가하는데 빈구간이있으면 이건 송수신을 아예 안했다는것

일직선으로 딱 나오는게 가장 이상적인 상황

![Untitled](/network/images/network8/Untitled%2042.png)

![Untitled](/network/images/network8/Untitled%2043.png)

별로 상황이 안좋은 상황

# **HTTPS 트래픽을 평문으로 보는 방법**

![Untitled](/network/images/network8/Untitled%2044.png)

private 키가 필요한 상황

edit → preferences 에

protocols → tls가 존재

![Untitled](/network/images/network8/Untitled%2045.png)

근데 세션키는 결국 브라우저가 만드니 그것만 알면됨

![Untitled](/network/images/network8/Untitled%2046.png)

![Untitled](/network/images/network8/Untitled%2047.png)

여기에 Log파일을 등록

분석이 되서나옴.

# **암호화된 HTTPS 트래픽을 보는 또 다른 방법 Fiddler**

![Untitled](/network/images/network8/Untitled%2048.png)

아래에는 제대로 안나옴 (압축되어서)

암호화때문에 너무 불편해서잘 안사용\

SSL inspection 

Fiddler 추천

- 프로세스 지정이가능

![Untitled](/network/images/network8/Untitled%2049.png)