---
layout: post
comments : true
title:  " 4. proxy"
date:   2020-01-01 22:53:01 +0900
---

inline + out of path 

Application Proxy

네트웍을 이루는 수많은 요소들이있는데 기본적으로 구조가 이 셋 중 하나

1. inline
2. out of path
3. application proxy(대리자)

![Untitled](/network/images/network4/Untitled.png)

pc#1이 pc#2를 proxy로 설정하면 

![Untitled](/network/images/network4/Untitled%201.png)

pc2의 컴퓨터 상황

socket s1이 listen

socket s2에 전달 

![Untitled](/network/images/network4/Untitled%202.png)

해당 상황이면 User mode proxy 로 부름 

이러면 socket 단까지 올라가니 Socket과 stream이 사용 됨

근데 inline과 Out of Path는 Packet단에서 처리 즉 IP계층까지만 올라감 

**서로 다루는 계층이 다름**

stream형식의 데이터를 inline으로 보기에 (불가능한건아님) 부적절할 수 있음

패킷을 보기에 proxy로 개발하기에는 불가능 할 수 있음 

# Proxy의 활용 #1 우회

![Untitled](/network/images/network4/Untitled%203.png)

네이버 입장에선 중국에서 접속했다고 생각함.

우회하는 순간 9.9.9.9에서는 3.3.3.3의 모든 통신을 감청 할 수 있음

# **Proxy의 활용 두 번째, 분석**

![Untitled](/network/images/network4/Untitled%204.png)

평문이라 취약

![Untitled](/network/images/network4/Untitled%205.png)

ssl 적용 

![Untitled](/network/images/network4/Untitled%206.png)

이러저러 이유로 컴퓨터와 구글의 패킷이왔다갔다하는걸 보고싶어서 와이어샤크를 사용할때 https 적용으로 죄다 암호화가 되어 분석이 힘들 수 있다  + application 단이라 packet 단위로 보는게 또 부적절함

![Untitled](/network/images/network4/Untitled%207.png)

만약 proxy 설정을 하면 

Fiddler라는 도구를 통해 분석가능 

조작도 가능 

# **Proxy의 활용 세 번째, 감시와 보호**

![Untitled](/network/images/network4/Untitled%208.png)

외부 악성코드도 Proxy를 거치기에 보호도 가능

proxy를 만들어보는것도 추천 

→ 인터넷 접근 차단도 가능

# **Proxy의 활용 네 번째, Reverse Proxy**

![Untitled](/network/images/network4/Untitled%209.png)

웹서버도 프록시를 놓을 수 있다.

이유

인터넷 네트워크는 퍼블릭 

Proxy와 서버간의 private 구간으로 빼버리고 외부접근을 막아버리기.

그러면 감시 + 보호의 역할

감시 : 공격을 식별→ 차단

그럼 이 프록시는 Web Application Firewall이라부른다 (WAE)