---
layout: single
title:  "VisualVM 을 이용해 원격 프로파일링하기"
date:   2019-01-30 10:20:00 +0900
categories: Post
---

라이브 서비스를 하다보면 가끔씩 원격으로 서버 프로파일링을 해보고 싶다는 유혹이 들때가 있다.

검색해보면 관련해서 여러 방법들이 나오는데, 제대로 동작하는 방법은 몇개 없었다. (내가 잘못 설정한 걸 수도 있다.)

이것저것 설정을 바꿔보다가 찾아낸 설정을 공유하고자 한다.

OS 는 Ubuntu 16.04, JDK 는 1.7 을 사용했다.

먼저 JVM 옵션으로 다음 값을 설정해준다.

-Dcom.sun.management.jmxremote=true -Dcom.sun.management.jmxremote.port=7172
-Dcom.sun.management.jmxremote.local.only=false -Djava.rmi.server.hostname=123.123.123.123
-Dcom.sun.management.jmxremote.rmi.port=9997
-Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false

port 값들(7172, 9997) 는 임의로 정한 값이다.

위의 JVM 옵션으로 서버를 기동시킨 뒤에 visualVM 을 실행한다.

visualVM 이 실행되면 Remote 를 우클릭 한뒤 Add Remote Host 를 통해 원격 호스트를 추가한다.
![Image01]({{ "/assets/image/remote-profile-01.png"}})


Add Remote Host 팝업이 열리면 아래에 있는 Advanced Settings 를 누른 후 Port 값을 jmxremote.rmi.port 값 (위 예제에서는 9997)으로 바꿔준다. Host Name 은 서버의 IP (위 예제에서는 123.123.123.123) 로 설정한다.
![Image02]({{ "/assets/image/remote-profile-02.png"}}) 


Remote Host 가 추가되면 해당 호스트를 우클릭 한뒤 Add JMX Connection 을 이용해 JMX Connection 을 추가한다.
![Image03]({{ "/assets/image/remote-profile-03.png"}})


Connection 정보를 host:port 형태로 입력해준다. (위의 예제에서는 123.123.123.123:7172)
![Image04]({{ "/assets/image/remote-profile-04.png"}})


JMX Connection 이 추가되면 이 때부터 모니터링이 가능하다.
![Image05]({{ "/assets/image/remote-profile-05.png"}})


주의할 점은 IP 주소는 클라이언트(VisualVM 을 실행하는 컴퓨터)가 서버에 접속할 때 사용하는 IP여야 한다는 것이다.

클라우드 서비스를 이용할 경우 서버는 공인 IP 없이 내부 IP만 가지고 있을 수 있다. (이 경우 공인 IP로 접속하면 클라우드가 내부 서버로 연결해준다.)

따라서 서버의 경우 공인 IP를 알 수 없기 때문에 JVM 입장에서 이 값이 당연히 의미가 없을 줄 알았는데 이상하게 공인 IP로 설정해야 제대로 동작한다. (왜 그런지는 관련 코드를 봐야 알 것 같다.)


그 다음 주의할 점은, 위 설정에서 보듯이 ssl, authenticate 가 false 로 되어있기 때문에 보안에 취약하다. 그럼에도 불구하고 저렇게 설정해서 쓰고 있는 이유는 필자의 경우 애시당초 방화벽에서 해당 포트로의 접근을 특정 PC만 접근할 수 있게 막고 있기 때문이다.

따라서 혹시 누군가 원격 프로파일링을 쓰게 된다면 방화벽을 설정하든, ssl 설정을 하든 해서 보안에 주의하는게 좋겠다.