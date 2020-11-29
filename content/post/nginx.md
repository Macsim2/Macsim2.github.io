+++
author = "macsim"
title = "nginx"
date = "2020-11-01"
description = "nginx description"
tags = [
    "nginx",
    "apache",
    "event-driven"
]
categories= [
    "WEB"
]
+++
nginx 란?

nginx란 reverse proxy, load balancer, mail proxy and HTTP cache 등으로 사용되는 웹 서버이다.<!--more--> 여기서 웹 서버란 소프트웨어의 의미로 world wide web 상에서 client의 request를 만족하는 기능을 담당한다. WAS(Web Aplication Service)라고 생각하면 되겠다.


## apache vs nginx
인터넷에 nginx를 쳐보니 apache라는 녀석과 많은 비교를 하고 있는 것을 볼 수 있었다. 이유인 즉슨 비교가 쉽게 되는 서로 다른 방식의 event 처리 방법을 사용하고 있었기 때문인 것 같았다.

# Apache
* MPM(Multi Processing Module) 아키텍쳐기반 MPM은 세부적으로 3가지로 나뉘게 되는데, prefork, worker, event로 나뉜다. prefork는 하나의 요청을 싱글 스레드로 처리(메모리 소모가 크다는 단점) worker는 하나의 요청을 멀티 스레드로 처리( 메모리를 공유하기 때문에 메모리 효율성은 prefork보다 좋고, 통신량이 많을 때 유리) event방식은 worker에 keep alive 메커니즘이 포함되어 있다고 한다.

![apache](/images/apache.png)

# nginx
* Event-Driven 아키텍쳐 기반 Event-Driven 방식은 event에 중점을 두어 발생한 event를 Event Handler의 기능을 하는 객체가 비동기 방식으로 먼저 처리되는 것부터 처리 한다고 한다. 고정된 프로세스만 생성하여 그 프로세스의 내부에서 비동기 방식으로 작업을 처리한다고 하니 보다 적은 스레드 비용을 가지고 많은 요청을 처리할 수 있다.

아래 그림을 보면 더 이해가 쉬울 것 같다.
![event-driven](/images/event-driven.png)


event driven에 대해 알아보면서 구체적으로 event가 어떻게 처리되는지 궁금해졌다. 이것과 관련해서 node.js를 통해 설명한 포스트를 발견할 수 있었는데 
순서는 이렇다.
1. event emit
2. Event loop가 처리
3. 처리하는 동안 다음 이벤트를 처리하는 것으로 넘어감
4. 처리가 완료되면 callback 함수를 호출함.
3번에서 event를 처리하는 동안 다음 event를 처리하는 것으로 넘어갈 수 있는 이유를 Event driven과 같은 프로세스가 처리되는 것을 javascript 코드가 아닌 I/O bound에 한해서 처리된다고 했기 때문이다.
(I/O bound는 filesystem, network, socket R/W, database 등을 처리하는 것을 말하고,
CPU bound는 CPU 자원을 사용하여 처리하는 일들이라고 생각하면 된다.)

따라서 CPU가 관여하지 않는 작업이 시작되면 기다리지 않고 바로 다른 작업을 수행할 수 있는 것이다.

(https://blog.naver.com/jhc9639/221108496101)
자세하고, 관련 실험까지 있어서 좋은 정보를 얻을 수 있었다.

서버에 부담을 줄이고자 하는 경우는 nginx의 선택이 맞겠지만 그러나 모든 경우에서 nginx가 좋다고 할수는 없다. 성능은 낮지만 apache가 가진 많은 모듈과 php과의 연동기능이 있어, 확장성과 호환성이 좋기 때문이다. 어찌보면 뻔한 답이지만 상황에 맞는 소프트웨어를 사용하면 되겠다.

netcraft에 따르면 2017년 10월 웹 서버 소프트웨어의 점유 순위는 apache가 44.89% nginx가 20.65%였는데 2020년 2월에 가서는 apache가 24.51% nginx가 36.48%를 기록했다.
물론 다른 조사기관에서는 조금씩 다르긴 하지만 nginx의 수요가 꾸준히 증가해온 것은 분명하다.  


