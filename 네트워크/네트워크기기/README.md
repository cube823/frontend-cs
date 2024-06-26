**이 글은 면접을 위한 CS 전공지식 노트(주홍철 저, 도서출판 길벗 2022) 2장 3절부터 5절까지 내용의 요약본입니다.**

# 2.3 네트워크 기기

**2.3.1 네트워크 기기 처리 범위**

네트워크 기기는 계층별로 처리 범위를 나눌 수 있음 (ex. 물리 계층을 처리할 수 있는 기계, 데이터 링크 계층을 처리할 수 있는 기계)

상위 계층을 처리할 수 있는 기기는 하위 계층을 처리할 수 있지만 그 반대는 불가

→ 예를 들어 L7 스위치는 어플리케이션 계층을 처리하는 기기로 그 밑의 모든 계층 프로토콜 처리 가능

→ But, AP 는 물리 계층밖에 처리할 수 없음

- 4계층에 따른 분류

  어플리케이션 계층: L7 Switch

  인터넷 계층: 라우터, L3 Switch

  데이터링크 계층: L2 스위치, 브리지

  물리 계층: AP, NIC, 리피터

**2.3.2 애플리케이션 계층을 처리하는 기기**

- **L7 스위치**

  여러 장비를 연결, 데이터 통신을 중재, 목적지가 연결된 포트로만 전기 신호를 보내 데이터를 전송하는 통신 네트워크 장비
  L7 스위치를 **로드밸런서** 라고도 부름. 서버의 부하를 분산하는 기기
  클라이언트로부터 오는 요청들을 뒤쪽의 여러 서버로 나누는 역할

  시스템을 처리할 수 있는 트래픽 증가 목표
  URL, 서버, 캐시, 쿠키들을 기반으로 트래픽 분산 + 바이러스, 불필요한 외부 데이터 등을 걸러내는 필터링 기능 + 응용 프로그램 수준 트래픽 모니터링 가능
  만약 장애 발생한 서버 존재시 → 트래픽 분산 대상 제외 필요
  주기적으로 헬스 체크를 이용하여 감시하면서 해당 기능 구현

<br/>

- **L4 스위치와 L7 스위치 차이**

  로드밸런서로는 L7 뿐만 아니라 L4 도 존재
  L4 스위치는 전송 계층을 처리하는 기기로 스트리밍 관련 서비스에선 사용 불가
  메시지를 기반으로 인식하지 못하고 IP 와 포트 기반으로 (그중 특히 포트 기반) 트래픽 분산
  L7 로드밸런서는 IP, 포트 외에도 URL, HTTP 헤더, 쿠키 등을 기반으로 트래픽 분산
  AWS 등 cloud service 에서
  L7 스위치를 이용한 로드밸런싱: ALB 컴포넌트
  L4 스위치를 이용한 로드밸런싱은 NLB 컴포넌트

<br/>

- **헬스 체크**

  L4 스위치, L7 스위치 모두 헬스 체크를 통해 정상 or 비정상 서버 판별
  헬스 체크는 전송 주기와 재전송 횟수 등을 설정한 후 반복적으로 서버에 요청 보내는 것을 의미

  주의사항: 서버에 부하가 되지 않을 만큼 요청 횟수가 적절해야 함
  TCP, HTTP 등 다양한 방법으로 요청을 보내며 이 요청이 정상적으로 이루어졌다면 정상적인 서버로 판별
  ex) TCP 요청을 보냈는데 3 way handshake 가 정상적으로 일어나지 않는다면 비정상 판별

**로드밸런서를 이용한 서버 이중화**

로드밸런서의 대표적인 기능: 서버 이중화

서비스의 안정적 운용을 위해 2대 이상의 서버는 필수적이라고 볼 수 있음

에러가 발생하여 서버 1대가 종료되더라도 서비스는 안정적으로 운용되어야 하기 때문

로드밸런서는 2대 이상의 서버를 기반으로 가상 IP 를 제공하고, 이를 기반으로 안정적인 서비스 제공

사용자 n 명이 0.0.0.12010 이란 가상 IP 에 접근할 때

뒷단에선 0.0.0.12011 과 0.0.0.12012 기반으로 서빙

0.0.0.12011 이 에러로 죽어도 0.0.0.12012 에 모든 사용자를 몰아 넣을 수 있음

**2.3.3 인터넷 계층을 처리하는 기기**

인터넷 계층을 처리하는 기기: L3 스위치, 라우터

- **라우터**
  라우터는 여러 개의 네트워크를 연결, 분할, 구분시켜주는 역할을 함
  다른 네트워크에 존재하는 장치끼리 서로 데이터를 주고 받을 때 **패킷 소모를 최소화 하고 경로를 최적화** 하여 **최소 경로로 패킷을 포워딩** 하는 라우팅을 하는 장비
- **L3 스위치**
  L3 스위치란 **L2 스위치의 기능**과 **라우팅 기능**을 갖춘 장비를 의미
  L3 스위치 === 라우터
  라우터는 소프트웨어 기반 라우팅 / 하드웨어 기반 라우팅으로 나뉨
  하드웨어 기반 라우팅 담당 장치 ⇒ L3 스위치 라고 부름
  | 구분 | L2 스위치 | L3 스위치 |
  | ----------- | --------------- | ------------- |
  | 참조 테이블 | MAC 주소 테이블 | 라우팅 테이블 |
  | 참조 PDU | 이더넷 프레임 | IP 패킷 |
  | 참조 주소 | MAC 주소 | IP 주소 |

**2.3.4 데이터 링크 계층을 처리하는 기기**

L2 스위치와 브리지

- **L2 스위치**
  장치들의 MAC 주소를 MAC 주소 테이블을 통해 관리
  연결된 장치로부터 패킷이 왔을 때 패킷 전송을 담당
  IP 주소를 이해하지 못해 IP 주소 기반 라우팅은 불가능
  단순히 패킷의 MAC 주소를 읽어 스위칭하는 역할
  목적지가 MAC 주소 테이블에 없다면 전체 포트에 전달하고 **MAC 주소 테이블의 주소는 일정 시간 후에 삭제**
- **브리지**
  두 개의 `근거리 통신망`을 상호 접속할 수 있도록 하는 통신망 연결 장치
  포트와 포트 사이의 다리 역할
  장치에서 받아온 `MAC 주소`를 `MAC 주소 테이블`로 관리
  브리지는 통신망 범위를 확장하고, 서로 다른 LAN 등으로 이루어진 `하나의 통신망` 을 구축할 때 사용

**2.3.5 물리 계층을 처리하는 기기**

- **NIC**

  LAN 카드라고도 부르며 Network Interface Card 의 줄임말
  2대 이상의 컴퓨터를 구성하는데 사용하며 네트워크와 빠른 속도로 데이터를 송수신할 수 있도록 컴퓨터 내에 설치하는 확장 카드
  각 LAN 카드에는 주민번호처럼 각각을 구분하기 위한 고유의 식별번호인 MAC 주소가 존재

- **AP**

  AccessPoint 는 패킷을 복사하는 기기
  AP에 유선 LAN을 연결한 후 다른 장치에서 무선 LAN 기술을 사용하여 무선 네트워크 연결을 할 수 있음
  의문: 물리계층인데 어떻게 패킷을 복사할 수 있는지?

<br/>

**2.4 IP 주소**

- **2.4.1 ARP**

  컴퓨터와 컴퓨터 간의 통신은 흔히 IP 주소 기반으로 통신한다고 알고 있지만
  정확히 얘기하면, `IP 주소에서` `ARP 를 통해` `MAC 주소를 찾아` `MAC 주소를 기반으로` `통신`함.

  - ARP란?
    IP 주소로부터 MAC 주소를 구하는 IP 와 MAC 주소의 다리 역할을 하는 프로토콜

    <br/>

**2.5 HTTP**

    HTTP 는 기본적으로 애플리케이션 계층으로 웹 서비스 통신에 사용됨

<br/>

- **2.5.1 HTTP/1.0**

기본적으로 한 연결당 하나의 요청만 처리하도록 설계. → RTT 의 증가로 이어짐

서버로부터 파일을 가져올 때마다 TCP 3-way-handshake 를 계속해서 열어야하기 때문에 RTT 가 증가하는 단점 존재

- RTT 증가를 해결하기 위한 방법
  연결할 때마다 RTT 소모로 서버 부담 증가, 사용자 응답 길어지는 이슈 지속적으로 발생
  이를 해결하기 위해 이미지 스플리팅, 코드 압축, 이미지 base64 인코딩 사용
  이미지 스플리팅 - 이미지 하나의 파일에 다 때려넣어서 호출 수를 하나로 줄이는 방법
  코드 압축 - tsx → mjs 로 번들링하는 것처럼 코드 크기 자체를 줄이는 과정
  이미지 Base64 인코딩 → 이미지 파일을 64진법으로 이루어진 문자열로 인코딩하는 방법
  서버와의 연결을 열고 이미지에 대해 서버에 http 요청을 할 필요가 없어짐
  But 37% 정도 크기가 커짐

  <br/>

- **2.5.2 HTTP/1.1**

  HTTP/1.0 의 단점을 보완하기 위해 나온 프로토콜로 매번 TCP 연결을 하는 것이 아닌

  한 번 TCP 를 초기화한 후 keep-alive 옵션으로 여러 개의 파일을 송수신할 수 있게 변경

  HTTP/1.0 도 keep-alive 속성이 있지만 표준화 되지는 않았음 → HTTP/1.1 부터 표준화되어 기본 옵션으로 설정

  - **HOL Blocking이란?**

    HOL Blocking (Head of Line Blocking) 은 네트워크에서 같은 큐에 있는 패킷이 첫 번째 패킷에 의해 지연될 때 발생하는 성능 저하를 의미

    ex) image.png 의 다운속도가 늦어질 때 style.css 와 data.xml 이 늦게 받아지는 현상

    **무거운 헤더 구조**

    HTTP/1.1의 헤더에는 쿠키 등 많은 메타데이터가 들어 있고 압축되지 않아 무거웠음

- **2.5.3 HTTP/2.0**

  SPDY 프로토콜에서 파생된 HTTP/1.x 보다 지연시간을 줄이고 응답 시간을 더 빠르게 할 수 있음

  멀티플렉싱, 헤더 압축, 서버 푸쉬, 요청의 우선순위 처리 지원 프로토콜

- **멀티플렉싱이란?**
  여러 개의 스트림을 사용하여 송수신한다는 의미 → 스트림의 패킷 손실되어도 해당 스트림에만 영향을 미치고 나머지 스트림은 멀쩡하게 동작할 수 있음
  - 스트림이란?
    시간이 지남에 따라 사용할 수 있게 되는 일련의 데이터 요소를 가리키는 데이터 흐름
    병렬적인 스트림들을 통해 데이터를 서빙하고 있으며 스트림 내의 데이터들도 쪼개져 있음. 애플리케이션에서 받아온 메시지를 독립된 프레임으로 조각내어 서로 송수신한 후 다시 조립하며 데이터를 주고받는 형식
    → 그 결과 단일 연결을 사용하여 병렬로 여러 요청을 받을 수 있고 응답을 줄 수 있음
    HTTP/1.x 에서 발생하는 HOL Blocking (Head of Network) 해결 가능

**헤더 압축**

`허프만 코딩 압축 알고리즘`을 사용하는 HPACK 압축 형식을 가짐

허프만 코딩이란? - 문자열을 문자 단위로 쪼개 빈도수를 세어 빈도가 높은 정보는 적은 비트 수를 사용하여 표현하고 빈도가 낮은 정보는 비트 수를 많이 사용하여 표현해서 전체 데이터의 표현에 필요한 비트양을 줄이는 원리

**서버 푸쉬**

HTTP/1.1 은 클라이언트가 서버에 요청을 해야 파일을 다운로드 받을 수 있었다면, HTTP/2 는 클라이언트 요청 없이 서버가 바로 리소스를 푸쉬할 수 있음

html 파일과 css 파일을 받아와야 하는 경우일 때

- 서버 푸쉬 x 의경우
  클라이언트 html 파일 요청 → 서버 html 파일 응답 → 클라이언트 css 파일 요청 → 서버 css 파일 응답
- 서버 푸쉬 o 의 경우
  클라이언트 html 파일 요청 → 서버 Html 파일 응답 → 서버 css 파일 푸쉬

**2.5.4 HTTPS**

HTTP/2 는 HTTPS 위에서 동작. HTTPS 는 애플리케이션 계층과 전송 계층 사이에 신뢰 계층인 SSL/TLS 계층을 넣은 신뢰할 수 있는 HTTP 요청을 의미. 이를 통해 `통신을 암호화` 할 수 있음

- **SSL/TLS**
  SSL 1.0 → SSL 2.0 → SSL 3.0 → TLS 1.0 → TLS 1.3 까지 버전 업되며 명칭이 TLS 로 변경되었으나 일반적으로 SSL/TLS 라고 많이 부름
  SSL/TLS 는 전송계층에서 (TCP/UDP) 보안을 제공하는 프로토콜
  클라이언트와 서버가 통신할 때 SSL/TLS 를 통해 제 3자가 메시지를 도청하거나 변조하지 못하도록 함
  SSL/TLS 는 보안 세션을 기반으로 데이터를 암호화.
  보안 세션이 만들어질 때 `인증 메커니즘`, `키 교환 알고리즘`, `해싱 알고리즘`이 사용됨
  프로세스: 자원을 할당받는 단위의 양
  **보안 세션**
  보안이 시작하고 끝나는 동안 유지되는 세션 의미
  SSL/TLS 는 핸드쉐이크를 통해 보안 세션을 생성, 이를 기반으로 상태 정보 등을 공유

  - 세션이란?
    운영체제가 어떠한 사용자로부터 자신의 자산 이용을 허락하는 일정한 기간 의미. 즉, 사용자는 일정 시간 동안 응용 프로그램, 자원 등을 사용할 수 있음
    **TLS의 핸드 쉐이크**
    클라이언트와 서버가 키를 공유하고, 이를 기반으로 인증, 인증 확인등의 작업이 일어나는 단 한 번의 1 RTT 가 생긴 이후 데이터를 송수신
    클라이언트에서 `사이퍼 슈트` 를 서버에 전달하면 서버는 받은 `사이퍼 슈트` 의 암호화 알고리즘 리스트를 제공할 수 있는지 확인
    제공할 수 있다면 서버에서 클라이언트로 인증서를 보내는 인증 메커니즘 시작 → 이후 해싱 알고리즘 등으로 암호화된 데이터 송수신 시작됨
  - **사이퍼 슈트란?**
    `사이퍼 슈트`는 프로토콜, AEAD 사이퍼 모드, 해싱 알고리즘이 나열된 규약 의미
    다섯 개가 존재
    1. TLS_AES_128_GCM_SHA256
    2. TLS_AES_256_GCM_SHA384
    3. TLS_CHACHA20_POLY1305_SHA256
    4. TLS_AES_128_CCM_SHA256
    5. TLS_AES_256_CCM_8_SHA256
       TLS - 프로토콜
       AES_128_GCM - AEAD 사이퍼 모드
       SHA256 - 해싱 알고리즘
  - **인증 메커니즘**

    CA 에서 발급한 인증서 기반으로 이루어짐

    CA에서 발급한 인증서는 안전한 연결을 시작하는 데 있어 필요한 `공개키` 를 클라이언트에 제공하고 사용자가 접속한 `서버가 신뢰` 할 수 있는 서버임을 보장

    인증서는 서비스 정보, 공개키, 지문, 디지털 서명 등으로 이루어짐

    CA 는 아무기업이나 할 수 없으며 신뢰성이 엄격하게 공인된 기업만 참여 가능 - 구글, GoDaddy, Comodo, GlobalSign, 아마존 등

    - **CA 발급 과정**
      서비스가 인증서를 받기 위해선 사이트 정보와 공개키를 CA에 제출해야 함
      이후 CA는 공개키를 해시한 값인 지문을 사용하는 CA의 비밀키 등을 기반으로 CA 인증서 발급

  - **암호화 알고리즘**
    `디피-헬만 키 교환 암호화 알고리즘` 을 근간으로 사용
    디피-헬만 키 교환 암호화 알고리즘은 암호키를 교환하는 하나의 방법
    y = g^x % p
    식에서 g 와 x와 p를 안다면 y 는 구하기 쉽지만 g 와 y 와 p 만 안다면 x 는 구하기 어렵다는 원리 기반 알고리즘
    상호 간에 공개값 공유 → 각자의 비밀값과 혼합 → 혼합 값 공유 → 각자의 비밀값과 혼합 → 공통 암호키 생성
    이렇게 클라이언트와 서버 모두 개인키와 공개키를 생성하고 서로에게 공개키를 보내고 공개키와 개인키를 결합하여 PSK(사전 합의된 비밀키) 가 생성된다면 악의적인 공격자가 개인키 또는 공개키를 가지고도 PSK 가 없기 때문에 할 수 있는게 없음. 이를 이용해 키 암호화 진행
  - **해싱 알고리즘**
    데이터를 추정하기 힘든 더 작고, 섞여 있는 조각으로 만드는 알고리즘
    SSL/TLS 는 해싱 알고리즘으로 SHA-256, SHA-384 를 많이 씀
    - SHA-256
      해시 함수 결과값이 256비트인 알고리즘 의미
      비트코인을 비롯한 많은 블록체인에서 사용됨
      SHA-256 알고리즘은 해싱을 해야할 메시지에 1을 추가하는 등 전처리를 하고 전처리된 메시지를 기반으로 해시를 반환
      - 해시란?
        다양한 길이를 가진 데이터를 고정된 길이를 가진 데이터로 매핑한 값
      - 해싱이란?
        임의의 데이터를 해시로 바꿔주는 일. 해시 함수가 이를 담당
      - 해싱함수란?
        임의의 데이터를 입력으로 받아 일정한 길이의 데이터로 바꿔주는 함수
        **TLS 1.3 은 사용자가 이전에 방문한 사이트로 다시 방문한다면 SSL/TLS 에서 보안세션을 만들 때 걸리는 통신이 필요 없음. 이를 0-RTT 라고 함**
  - **SEO 에도 도움이 되는 HTTPS**
    Google 정책 때문
    SEO 방법으로는 - 캐너니컬 설정, 메타 설정, 페이지 속도 개선, 사이트맵 관리 등이 존재
  - **HTTPS 구축 방법**
    1. 직접 CA 에서 구매한 인증키 기반 HTTPS 서비스 구축
    2. 서버 앞단에 HTTPS 를 제공하는 로드밸런서 추가
    3. 서버 앞단에 HTTPS를 제공하는 CDN 추가

**2.5.5 HTTP/3**

world wide web 에서 정보를 교환하는 데 사용되는 HTTP 의 세번째 버전

TCP 위에서 돌아가는 HTTP/2 와 달리 HTTP/3 는 QUIC 라는 계층 위에서 돌아가며, TCP 기반이 아닌 UDP 기반으로 돌아감

전송계층: UDP

TLS 계층(전송계층 내): QUIC 으로 서빙되는 TLS 형태

HTTP/2 의 장점을 흡수해 멀티플렉싱을 가지고 있으며, 초기 연결 설정 지연 시간 감소라는 장점 존재

QUIC 은 TCP 를 활용하지 않기 때문에 통신을 시작할 때 번거로운 3-way-handshake 과정을 거치지 않아도 괜찮음

QUIC 은 첫 연결 설정에 1RTT 만 소요됨. 클라이언트가 서버에 어떤 신호 한 번을 주고, 서버도 거기에 응답하기만 하면 바로 본 통신을 시작할 수 있음.

QUIC 는 순방향 오류 수정 메커니즘(ForwordErrorCorrection) 가 적용되어 있다.

이는 전송한 패킷이 손실되었다면, 수신 측에서 에러를 검출하고 수정하는 방식을 의미

열악한 네트워크 환경에서도 낮은 패킷 손실률을 자랑함
