+++
author = "macsim"
title = "CORS"
date = "2020-11-14"
description = "CORS"
tags = [
    "CORS",
    "XMLHttpRequest",
]
+++

CORS

쓸데없이 계속 떠드는 글 안보기 <link>

하루는 어줍짢게 html과 그에 딸린 javascript 파일을 코딩하고 있을 때다. 왠지 익숙한 문구가 보였다. 브라우저 console탭에서 보이는 아래와 같은 메세지가 보였다. 

> Access to XMLHttpRequest at '\~\~\~' from origin '~~~' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.


이전에는 어떻게 대처했는지 기억도 잘 안나고 쉽게 문제가 해결된 것 같았다. 그런데 이번에는 CORS 문제가 아니었던 것 같았는데 이런 오류가 보였다. 사실 CORS가 무엇인지 어림 짐작 정도로만 알고 있었기 때문에 이 글을 쓰고 있는지도 모른다.

같은 request method를 사용했고, 같은 서버로의 요청이었는데..? 뭔가 이상함을 느끼고 back단 서버를 점검했다. ~~언제나 그렇듯~~ 처음에는 오만가지 생각이 든다. html 코딩이 조금 다른 까닭에 이런 오류가 발생한건지, xmlhttprequest 객체가 한번의 연결(?)만 허용되어 그런건지, 자유분방하지만 엉터리인 추측은 이렇듯 종종 펼쳐진다. 일을 하면서 이런 추측이 점점 줄어들 것이라 믿는다..

문제의 실마리를 찾기위해 점검이던 중에도 이상한 점을 찾기까지는 시간이 좀 걸렸는데 결국은 서버단에서 실행되는 함수가 실패로 끝난 것이었다. 그것도 고치는데 조금 헤맸지만, 이 얘기는 다음에 하는 걸로 하자. 

사실은 CORS 에러가 아닌 함수의 실패로 인한 에러 문구 였다.. 바보 같이 정확히 읽어보지도 않고 섣부른 판단을 한것이다.

---------
하지만 이번에 몰랐던 CORS에 대해 좀 알아보고자 한다. 언젠가 또 만날테니까
CORS는 <<Cross-Origin-Resource Sharing>> 의 줄임말로, 쉽게 말해 요청을 하고 응답을 하는 두 서버간의 차이에 있는 정책이라고 이해하면 되겠다.

그렇다면 '출처'는 무엇일까? <br>
http://hostname.com:80/path?user=macsim&page=1#foo
로 표시되는 서버의 위치를 뜻한다.
이 서버의 위치를 자잘하게 나누자면 이렇다.

|<div style="text-aling: left">http://</div>|protocol |
|---|--- |
|<center> www.hostname.com</center> |<center> host </center>|
|<center> :80 </center> |<center> port </center>|
|<center>path</center> | <center>path</center> |
| <center>user=macsim&page=1</center> | <center>Query String</center> |
| <center>#foo </center>| <center>Fragment </center>|


서로 다른 출처(도메인)간 정보의 요청과 응답에 대한 (보안상)규제를 하기 위해 CORS 정책이 작동하고 있었던 것이다. ~~web은 브라우저를 통해 소스가 제공된다는 점에서부터 보안이 걱정되기는 한다..~~

CORS 정책을 위반하는 경우의 예시이다.<br>
프로토콜 조차 다르면 X <br>
host가 다르면 X <br>
port가 다른 경우는 브라우저에 따라 다르고 <br>
port까지 같고 path가 다른 경우는 O <br>
path까지 같고 query string이 다른경우도 O <br>
와 같은 정책을 가지고 있다

실제 웹 브라우저가 다른 출처로 요청을 할 경우 HTTP protocol을 사용하는데 요청 헤더에 Origin이라는 필드에 보내는 서버의 '위치'를 함께 담아 보낸다.
> Origin: http://hostname.com
이후 요청을 받은 서버는 "Access-Control-Allow-Origin"이라는 필드에 자신에게 접근 가능한 출처를 보여주고 이후 응답을 받은 브라우저는 자신이 보냈던 요청의 Origin과 "Access-Control-Allow-Origin"을 비교해본 후 이 응답이 유효한 응답인지 아닌지를 결정한다.

방금 설명한 이 과정이 바로 Preflight Request이다.
'내가 직접 coding한 요청' 이전에 이러한 요청과 응답을 주고 받는 것이다.

그러니 아직 잘은 모르지만 본인이 요청을 보낼 서버에 "Access-Control-Allow-Origin" 필드를 "*"로 설정해놓거나 허용 가능한 서버를 적는다면 CORS 에러는 면할 수 있을 것이다.

CORS 문구가 보인다고 무조건 의심할것이 아니라 preflight 응답에서 "Access-Control-Allow-Origin" 필드에 유요한 서버가 들어가 있는가에 대한 문제다. 그렇다면 CORS 에러에 대한 의심은 그만해도 되기 때문이다.

