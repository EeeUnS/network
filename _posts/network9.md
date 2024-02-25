---
layout: post
comments : true
title:  "  암호기술에 대한 최소 이론"
date:   2020-01-01 22:53:01 +0900
---

# **Hash를 알아야 블록체인이 보인다! 세 번째**

 

![Untitled](/network/images/network9/Untitled.png)

다음블록은 이전블록의 해쉬값을 가지고 이에 대해 의존적으로 해시값을 만들어버림

그러면 중간에 있는 값을 바꿀 수 없어짐.

![Untitled](/network/images/network9/Untitled%201.png)

합의를 위해 작업증명이라는게 생기는데 

새로운 블록이 생김에 대해서 hash가 특정 조건이 되도록 특정 유동적이게 가질 수 있는 컬럼값을 구하게 하기위해 계산을 시킴 → 거기에 따라 보상을 주는 식

# **비대칭키가 인터넷 환경에서 사용되는 기본 원리**

![Untitled](/network/images/network9/Untitled%202.png)

대칭키의 문제 : 대칭키를 어떻게 공유할 것 인가?

비밀키

![Untitled](/network/images/network9/Untitled%203.png)

1. 키쌍 생성
2. PC Pub, Priviate key 쌍 생성  <> Server Pub, Pri Key 생성
3. 각자의 퍼블릭 키를 교환
4. PC는 Server의 PUblic key로 보냄, 서버는 pc의 public key로 암호화해 보냄

# **인터넷 키교환 시스템의 구조**

앞에서 키쌍을 생성하는데 전산 자원을 너무 소모 많이 함

그래서 미리 생성 후 HDD에 저장

그리고 효율이 대칭키가 비대칭키보다 더 좋음

그래서 수정

![Untitled](/network/images/network9/Untitled%204.png)

1. 대칭키 생성
2. 키 교환 비대칭키를 이용해 대칭키를 교환 
3. 대칭키를 가지고 통신

해당 방법으로 할시에 왔다갔다하는것이 전용 키만 존재해 보안적으로 더 좋음

이 대칭키를 **세션키**라고도 함

<> = 디퍼 헬만 키교환 알고리즘 

# **비대칭키 시스템을 해킹하는 원리**

![Untitled](/network/images/network9/Untitled%205.png)

중간에 MITM 어택을 당한다면?

서버의 퍼블릭키가아닌 해커가 중간에 해커의 퍼블릭키를 보낸다면?

# **인증 시스템이 포함된 SSL 통신 및 인증서 검증원리**

![Untitled](/network/images/network9/Untitled%206.png)

중간에 인증기관을 추가한다

받은 서버의 public키를 인증이 필요

인증 체계

- CA : certification authority / ex verisign
- RA : reasonal authority

1. A 회사 → RA에 키쌍을 만들어달라고 요청 

이 퍼블릭키에 정보를 잔뜩 박음

![Untitled](/network/images/network9/Untitled%207.png)

이 hash값을 암호화시켜버림

이를 합쳐 인증서라부름

![Untitled](/network/images/network9/Untitled%208.png)

이제 이를 통해 인증서의 형태로 날라옴

그럼 이 인증서를 어떻게 검증하는가?

여러 복잡한데

MS가 Verisign같은곳과 협업(제휴)를 하면서 

윈도우 업데이트하면서 pc에 검증할 인증서같은걸 깜

![Untitled](/network/images/network9/Untitled%209.png)

certmgr.exe

![Untitled](/network/images/network9/Untitled%2010.png)

CA도 키 쌍이 있는데 이때 보낼때 CA Private key로 암호화 하여 보냄

![Untitled](/network/images/network9/Untitled%2011.png)

PC예서는 이 인증서 설치에 public key가 있어 이걸로 풀어버림