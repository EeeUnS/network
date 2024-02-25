---
layout: post
comments : true
title:  "기타"
date:   2020-01-01 22:53:01 +0900
---

# **HTTP의 버전 별 발전과정과 각각의 차이를 말해보세요.**

[HTTP의 버전 별 발전과정과 각각의 차이를 말해보세요.](https://youtu.be/mQAsKYMzQaU)

http tcp/ip

1.0 1.1 차이

html 가져옴 

- 사진 필요 요청시

기존 1.0은 tcp/ip기반이라

html 한번 가져올때마다 tcp connect close를 함

→ 1.1

연결을 1회만 함

html+css+js…

Host 추가

굉장히 오래 써먹음

구글쪽에서 spdy → http2.0

http 2.0

멀티세션 개념 지원

io multiplexing 가능

극단적인 성능차이를 보이게됨

HTTPS 1.1에서는 3개를 보낸다고하면 순서대로 보냄 (줄서서옴)

![Untitled](/network/images/network7/Untitled.png)

http 2에서는 한번에 같이오고 조립을 함

기존 https 형식으론 불가능

사진, 영상등의 바이너리가 많이 포함되다보니 레이어를 추가로 집어넣음 

![Untitled](/network/images/network7/Untitled%201.png)

가장 중요한 특징 여전히 TCP 사용 

![Untitled](/network/images/network7/Untitled%202.png)

또 헤더가 계속 커지다보니 압축을 시켜버림 

http 3.0

![Untitled](/network/images/network7/Untitled%203.png)

UDP 가 되어버림

브라우저 수준에서는 버리는걸 인정하고 

혼잡제어 재정의 QUIC 

TLS 필수

[웹 브라우저에 URL 입력하면 일어나는 일 - 인프라 위주](https://youtu.be/GAyZ_QgYYYo)

# **웹 브라우저에 URL 입력하면 일어나는 일 - 인프라 위주**

![Untitled](/network/images/network7/Untitled%204.png)

![Untitled](/network/images/network7/Untitled%205.png)

PC Windows

공유기 사용

DNS

주소를 칠때 url + uri 로 나눌 수 있다

주소를 찾기위해 dns를 가는데 

분산형 db구조 

ddns라는게 또 있는데 넘어감

ip주소를 흭득

![Untitled](/network/images/network7/Untitled%206.png)

근데 이 과정을 조금 더 들어가면

1. pc의 host 파일 뒤짐
2. dns cache찾음 expire 시간 체크
3. 질의 → pc네트워크 설정에따라 dns에 질의하는데 세팅에따라 공유기로 질의할 수도있음
4. 세팅이없다면 isp에 세팅된 dns사용  ip 주소 흭득
5. 최종적으로 tcp연결 
6. HTTPS Request  사용 
7. 그에따르는 Response 

공통 답변 수준

경력자정도되면 

GSLB 에 대한 답변 필요 CDN도 엮임 

nslookup [www.naver.com](http://www.naver.com) 을 쳐보면 

![Untitled](/network/images/network7/Untitled%207.png)

이름에 해당하는것을 보면 CDN을 쓴다는것을 알 수 있음 

CDN 사용하는 이유

GSLB - DNS ,System 을 만들때 Health Check 도 함 

해당 응답 주소는 본인의 주소에서 가장 원할한 주소를 줌 그래서 사람마다 다를 수 있음 

![Untitled](/network/images/network7/Untitled%208.png)

Health Check

![Untitled](/network/images/network7/Untitled%209.png)

모종의 이유로 서울쪽 서버가 죽은경우 Heath Check로 파악하고 서울 서버 ip반환이아닌 부산을 반환해주는식으로 대체가능

여기서만약 Login 이 된상태에서 서울 → 부산으로 연결이 된다면 연결지향을 위해 어떻게 구조를 잡아야할까 

![Untitled](/network/images/network7/Untitled%2010.png)

failover

부하분산

장애대응

DNS Spoofing 방지 대책

DDOS 공격 방지대책

도커

k8s

무중단 배포

[웹 브라우저에 URL 입력하면 일어나는 일 - 프론트 개발자용](https://youtu.be/mmsPSiB2o3M)

# **웹 브라우저에 URL 입력하면 일어나는 일 - 프론트 개발자용**

![Untitled](/network/images/network7/Untitled%2011.png)

![Untitled](/network/images/network7/Untitled%2012.png)

- ui
- 브라우저 엔진
- 렌더링 엔진
- 자바 스크립트 해석기 (엔진)
- 자료저장소 WEB STROREGE 공인인증서등 저장

기본적인것은 문서뷰어임

XML Parser가 중요

UI는 늘 데이터 구조에 의존적임 

이때 나오는것이 DOM 트리

html → DOM 트리 구축 → 렌더링 트리 구축 후 그림 

![Untitled](/network/images/network7/Untitled%2013.png)

이후 과정에도 js가 개입함

![Untitled](/network/images/network7/Untitled%2014.png)

TCP/IP 통신을 통해 HTTP 연결이되고 가장 먼저 오는것은 html 그다음에 넘어오는게 CSS

js 가 날라오면  이걸로 인해 문서나 style shit(CSS)가 바뀌어버림 

[유해사이트 차단 원리에 대해 말해보세요](https://youtu.be/oYmJ3QDdZ6I)

![Untitled](/network/images/network7/Untitled%2015.png)

국가가 ISP를 통제

![Untitled](/network/images/network7/Untitled%2016.png)

음란물인지에 대해서는 DNS가 가장먼저 파악가능

1. DNS 질의 응답 

DNS를  예) 구글 8.8.8.8 외부 DNS를 사용한다면 해당 부분에 문제가 없음 

1. HTTPS, HTTP  → SNI ISP에 유입되는 모든 트래픽을 감시 
    1. HTTPS세션이 생성될떄 SNI정보를 참조하면 URL이 통으로 되어있음  SPI
    2. 모니터링 → Fake를 보냄 

![Untitled](/network/images/network7/Untitled%2017.png)

tls.handshake.extensions_server_name

사용하면 볼 수 있음 

[2013년 3 20 사이버테러와 망분리](https://youtu.be/ig4cHQVeEq0)

한눈에 일하는과정보기도쉽고, 전반적인 전체적인 이해도가 올라갔다 

어떤걸 했는지 구체적으로 설명 

→