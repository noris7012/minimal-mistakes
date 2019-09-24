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


![Image01]({{ "/assets/image/remote-profile-01.png"}})
![Image02]({{ "/assets/image/remote-profile-02.png"}})
![Image03]({{ "/assets/image/remote-profile-03.png"}})
![Image04]({{ "/assets/image/remote-profile-04.png"}})
![Image05]({{ "/assets/image/remote-profile-05.png"}})