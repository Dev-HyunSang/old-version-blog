---
id: 6
title: "WebRTC에서 TURN 서버는 무엇일까요?"
subtitle: "TypeScript 초보자 프로젝"
date: "2021.02.25"
tags: "WebRTC"
---
안녕하세요, 꾸준하게 WebRTC에 관련하여서 공부하고 있습니다. WebRTC를 공부하다 보면 WebRTC에서 TURN 서버를 사용합니다.  
오늘의 궁금증은 TURN 서버는 무슨 역할이고 무엇을 하는지에 대해서 이야기 해 볼려고 합니다.

# TURN 서버는 무엇인가요?
일단 TURN 서버를 알기 전 기본적인 WebRTC가 무엇인지, 어떻게 동작하는지에 대해서 알아야 합니다.  
[**WebRTC이란 무엇인가요?**](https://www.parkhyunsang.com/article/0.html) 를 읽고 오시면 기본적인 WebRTC의 동작원리를 알 수 있으실 겁니다.  
WebRTC는 [P2P(Peer-to-Peer)](https://ko.wikipedia.org/wiki/P2P) 방식을 활용하여서 WebRTC 애플리케이션이 동작합니다.  
이러한 P2P 방식을 피어간 트래픽을 릴레이하기 위해선 서버가 필요합니다. 클라이언트가 동일한 로컬 네트워크에 상주하지 않는 한 직접 소켓을 연결(사용)할 수 없는 경우가 많습니다.  
이러한 문제점을 해결하는 일반적인 방법은 TURN 서버를 사용하는 것입니다.  
TURN은 **Traversal Using Relay NAT**를 의미하며 네트워크 트래픽을 중계하기 위한 프로토콜입니다.  

현재는 자체 호스팅 응용 프로그램(오픈 소스 COTURN 프로젝트 등)과 클라우드 제공 서비스로 온라인에서 사용할 수 있는 TURN 서버에 대한 몇 가지 옵션이 있습니다.

온라인으로 TURN 서버를 이용하게 되면 클라이언트 응용 프로그램이 올바른 ```RTCConfiguration``` 사용하면 됩니다.  
아래 코드는 샘플 구성을 보여 ```RTCPeerConnection``` TURN 서버 호스트 이름이 ```turn:my-turn-server.mycompany.com:19403```에서 실행되는 구성 오브젝트는 서버에 대한 액세스 보안을 위한 정보 특성도 지원합니다.

```js
const iceConfiguration = {
    iceServers: [
        {
            urls: 'turn:my-turn-server.mycompany.com:19403',
            username: 'optional-username',
            credentials: 'auth-token'
        }
    ]
}

const peerConnection = new RTCPeerConnection(iceConfiguration);
```
RTCPeerConnection는 UDP 상에서 피어(Peer)들 간의 직접 통신 설정을 시도합니다. 만약 이를 실패하면, RTCPeerConnection는 TCP에 의존합니다. 이것도 실패하면 종단점(Endpoint)들 사이의 데이터 릴레이를 수행하는 TURN 서버들이 대안으로 사용될 수 있습니다.  
TURN은 시그널링 데이터가 아니라 피어(Peer)들 사이의 오디오/비디오/데이터 스트리밍 릴레이를 위해 사용됩니다!

![Images](https://www.html5rocks.com/ko/tutorials/webrtc/infrastructure/turn.png)

이 다이어그램은 다음과 같이 동작하는 TURN을 보여줍니다. 순수 STUN이 성공하지 못했으므로 각 피어(Peer)는 TURN 서버의 사용에 의존합니다.

# 참고하면 좋은 문서 또는 출처
[WebRTC TURN 서버](https://webrtc.org/getting-started/turn-server?hl=ko) 를 보고 글을 작성하였습니다.
- [RFC 5766](https://tools.ietf.org/html/rfc5766)
- [TURN](https://ko.wikipedia.org/wiki/TURN)
- [[WebRTC] STUN 과 TURN 에 대하여 #1 - 개요](https://alnova2.tistory.com/1110)
- [Build the backend services needed for a WebRTC app](https://www.html5rocks.com/ko/tutorials/webrtc/infrastructure/) 
