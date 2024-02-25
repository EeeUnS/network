---
layout: post
comments : true
title:  " 2. 웹"
date:   2020-01-01 22:53:01 +0900
---

# **웹 서비스를 만드신 분에 대하여...**

# 초창기 웹 서비스 구조

티모시 버너스리 가 창안 

html + http

잠깐 얘기를 넘어가서

문서를 다루는 프로그래밍으로 보면 

![Untitled](/network/images/network2/Untitled.png)

1. 자료구조
2. 제어체계
3. ui

유지보수를 위해 세가지로 쪼개는것이 중요

웹도 비슷한 상황

HTTP 버전

![Untitled](/network/images/network2/Untitled%201.png)

현재도 1.1을 제일 많이 사용

→ 2.0으로 넘어가고 있는 상황

이후는 1.1 버전을 주로 설명

HTTP의 대전제 : TCP/IP통신

연결 가정

HTTP통신의 가장큰 특징 Stateless

이 tcp/ip연결을 전제하는데 ⇒ 연결은 상태를 나타내지만 http는 stateless이기에 훗날 약간 문제를 만듬

![Untitled](/network/images/network2/Untitled%202.png)

URL 입력 →  DNS → 서버 ip

서버쪽으로 GET 요청 → 응답 HTML 

GET 메소드는 클라입장에선 READ와 동일 

1. 구문 분석(TAG) → 자료구조 생성 (비선형 트리 구조) DOM
2. 화면에 렌더링 

이정도가 1.0버전

초기 웹브라우저는 원격 문서 뷰어정도

# **웹 서비스 3대 요소**

저번 상황을 정리하면 

웹은

⇒ 단방향 작용 함

단지 요청 - 응답만 함

시간이 지나 HTML의 성격이 변함 UI적인 부분도 신경쓰게 됨

![Untitled](/network/images/network2/Untitled%203.png)

여기서 자료구조는 HTML에 해당한다고보면 UI만 따로 떼서 만들어놓은게 CSS라 볼 수 있음 

서버는 HTML로만 존재하다 새로운 파일들을 가지게됨 CSS, 사진 등 

![Untitled](/network/images/network2/Untitled%204.png)

이제 요청시 HTML만 오는것이아닌 CSS, 사진도 함께옴  (오는 순서도 HTML , css, 사진 순)

웹이 계속 발전하여…

브라우저의 정보를 넘겨주기도함

Login시 Click 하면 id, pwd정보도 같이 넘김

이때는 주로 POST

![Untitled](/network/images/network2/Untitled%205.png)

송수신만 담당하던 웹서버가 추가적인 처리도 담당하게 됨 

양방향 상호작용이 됨 

이러면서 문맥(상태, 전이)이 생기게 됨 

![Untitled](/network/images/network2/Untitled%206.png)

근데 HTTP는 stateless니 이 상태를 어디에 기억을 해야할 필요가 생겨버림

그렇게 DB가 붙어버림

![Untitled](/network/images/network2/Untitled%207.png)

그래서 이 결과 HTML이 동적으로 생성됨 

![Untitled](/network/images/network2/Untitled%208.png)

Not Found 일 수도있음

![Untitled](/network/images/network2/Untitled%209.png)

![Untitled](/network/images/network2/Untitled%2010.png)

웹브라우저가 복잡해지면서 브라우저는 한가지 일을 더하게 되는데

1. 연산을 담당하게된다

그렇게 엔진이 들어가는데 이 처음 Script이름은 Mocha였음

![Untitled](/network/images/network2/Untitled%2011.png)

이후 이름이 바뀌어서

- Mocha
- Live
- JavaScript 가됨

![Untitled](/network/images/network2/Untitled%2012.png)

그래서 이 서버의 전달요소에 JavaScript도 추가가 됨

클라이언트단에서의 기억을 어떻게 구현할까

Cookie : Key + value (범위 + 기간)

# **WAS, JVM 그리고 RESTful API**

서버에서 처리 연산을 담당하는 구조가 이제 추가되었는데 WAS라고 함 (Web Application Service )

WAS는 Web server와 연결되어 

- View : 눈에 보이는것을 처리
- Model : DB와 연결
- Control : URL 에대한 리소스 로직 처리

이런 형태로 비즈니스 로직을 처리하는 모델을 MVC모델이라 함 

![Untitled](/network/images/network2/Untitled%2013.png)

이

Web Server, WAS, db 셋을 각각 하나의 티어로 묶어서 3Tier Web Solution이라 부름 

![Untitled](/network/images/network2/Untitled%2014.png)

성능 이슈가 생길때

DB와 WAS단에서 성능 이슈가 생길 수 있는데 이를 감시하는 솔루션이 APM(application performance mangement)

대표적으로 Scouter APM이라는것이 존재 상용중엔 제니퍼

최근의 웹 변화 

클라의 request 요청은 동일 

- GET은 read에 대응
- POST 는 쓰기에 대응

⇒ 입출력과 비슷함

그런데 응답으로 여러 DATA로 오는데 그 중에 JSON만 덜렁 오고있음

요새 UI가 매우 다양한데 → HTML이 UI에 민감하다보니 

json만 보내고 클라 자신에게 맞게 HTML을 직접 생성하는 식으로 

![Untitled](/network/images/network2/Untitled%2015.png)

그럼 이 생성하는 코드가  Js사용 근데 생성하는 코드도 길어지니 FrameWork가 탄생 (리액트, 뷰…)

![Untitled](/network/images/network2/Untitled%2016.png)

그 이 요청들도 정리하고 보면

- 생성 Create
- 읽기 Read
- 쓰기 Update
- 삭제 Delete

의 기본적인 기능에 해당(CRUD)

이런 정리해서 기능을 제공하고 클라단에서는 그냥 호출을 하는식으로하면 

이런 API를 **Restful api** 가 된다.

![Untitled](/network/images/network2/Untitled%2017.png)

웹서버와 인터넷 망이 붙을때 세가지 요소가 들어간다

IPS SSL WAF(Web application firewall)

![한_장으로_살펴보는_웹_서비스_구조.png](network2/%25ED%2595%259C_%25EC%259E%25A5%25EC%259C%25BC%25EB%25A1%259C_%25EC%2582%25B4%25ED%258E%25B4%25EB%25B3%25B4%25EB%258A%2594_%25EC%259B%25B9_%25EC%2584%259C%25EB%25B9%2584%25EC%258A%25A4_%25EA%25B5%25AC%25EC%25A1%25B0.png)