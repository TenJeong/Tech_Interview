# 네트워크 정리


# 2.5.1

**HTTP/1.0**

- 하나의 연결당 하나의 요청을 처리하도록 설계되었고 이는 RTT의 증가를 의미한다.
- 서버로부터 파일을 가져올때마다 TCP의 3 Way-Hand Shake를 계속해서 열여줘야 하는 문제점이 존재한다.
- 이를 해결하기 위해 이미지 스플리팅, 코드 압축, 이미지 Base64Encoding이 존재한다.
    - 이미지 스플리팅: 여러개의 이미지를 하나로 합쳐서 송수신하고 수신한 측에서 이미지의 Background의 position을 이용해서 이미지를 표기하는 방식이다.
    - 코드 압축: 개행문자와 빈칸을 하나로 없애서 코드의 크기를 최소화하는 방법이다.
    - 이미지 Base64인코딩을 하게되면 이미지에 대한 Http요청을 할 필요가 없지만 원래 이미지의 크기에 비해 약 37%가 더 커지는 단점이 있다.

**HTTP/1.1**

- keep-alive라는 조건으로 여러개의 파일을 송수신할 수 있게 바뀌었다.
- 하지만 요청한 이미지의 개수에 비례해서 대기 시간이 길어질 수 있다.
- HOL Blocking이 존재한다.
    
    → 네트워크에 존재하는 큐의 첫번째 패킷에 의해 지연될때 발생하는 성능 저하를 의미 한다.
    
- 많은 메타 데이터가 헤더에 존재하고 압축 되지 않기에 헤더가 무겁다는 단점이 존재한다.

**HTTP/2.0**

- 지연시간을 줄이고 응답 시간을 더 빠르게 할 수 있다.
- 멀티플렉싱, 헤더압축, 서버 푸시, 요청의 우선순위 처리 등이 존재한다.
- 멀티 플렉싱은 여러 개 의 스트림을 사용하여 송수신한다는 것을 의미한다.
    - 하나의 TCP 연결에 대해서 여러개의 스트림을 열수 있다.
- 헤더 압축에 허프만 코딩 압축 알고리즘을 사용한다.
- 서버 푸시: 클라이언트의 요청 없이 바로 서버가 리소스를 푸시 할 수 있다.
    - (.html, .css, .js…)

**HTTPS**

- 어플리케이션계층과 전송계층 사이에 SSL/TLS계층을 넣어 신뢰할 수 있는 HTTP 요청을 의미한다.
- HTTP/2는 HTTPS위에서 동작한다.
- SSL/TLS
    - 전송 계층에서 보안을 제공하는 프로토콜이며 클라이언트와 서버가 통신을 할 때 제 3자가 메세지를 도청하거나 변조하지 못하도록 한다.
    - 보안 세션을 기반으로 데이터를 암호화하며 인증 메커니즘, 암호화 알고리즘, 해싱 알고리즘이 사용된다.
- 클라이언트에서 서버로 사이퍼 슈트를 전달한다 → 서버는 클라이언트의 사이퍼 슈트의 암호화 알고리즘을 제공할 수 있는지 확인한다. → 만약 제공이 가능하다면 인증 메커니즘이 진행된다.
- 사이퍼 슈트
    - 프로토콜, AEAD 사이퍼모드(암호화 알고리즘) ,해싱 알고리즘이 나열된 규약을 의미
- 인증 메커니즘
    - CA에서 발급한 인증서를 기반으로 이루어진다.
    - CA에서 발급한 인증서는 사용자가 접속한 서버가 신뢰 할 수 있는 서버임을 보장한다.
- 암호화 알고리즘 (디피 헬만 키 교환 알고리즘)
    - 공개 키 값을 서로 공유하고 A,B의 비밀키를 혼합하여 공통의 암호키(PSK)를 생성한다.
- 해싱 알고리즘은 데이터를 추정하기 힘든 더 작고, 섞여 있는 조각으로 만드는 알고리즘이다.
- 해시 : 다양한 길이를 가진 데이터를 고정된 길이를 가진 데이터로 매핑한 값을 의미한다.
- 해싱 : 임의의 데이터를 해시로 바꿔주는 일이며 해시 함수가 이를 담당
- 해시 함수 : 임의의 데이터를 입력으로 받아 일정한 길이의 데이터로 바꿔주는 함수
- TLS 1.3에서는 동일 사이트 재방문시 보안세션이 동작하지 않는다. 이를 0-RTT라고 한다.
- SEO는 검색엔진이 이해하기 쉽도록 홈페이지 구조와 페이지를 개발하는 것을 의미한다.
    - 캐노니컬 설정
    - 메타 설정
    - 페이지 속도 개선
    - 사이트맵 관리
- HTTPS 구축방법은
    1. 직접 CA에서 구매한 인증키를 기반으로 HTTPS 서비스를 구축하거나 
    2. 서버 앞단에 HTTPS를 제공하는 로드 밸런서를 두거나 
    3. 서버 앞단에 HTTPS를 제공하는 CDN을 둬서 구축한다.

**HTTP/3**

- TCP기반으로 동작하는 HTTP/2와 달리 HTTP/3는 UDP기반으로 동작한다. 그리고 QUIC라는 계층위해서 동작한다.
- QUIC는 TCP를 사용하지 않기 때문에 3-way-handShake 과정을 거치지 않아도 된다.
- QUIC는 첫연결 설정에 1-RTT만 소요된다.
- QUIC 순방향 오류 수정 메커니즘을 사용하기에 낮은 패킷 손실률을 보인다. (패킷 수신측에서 에러 검출하고 수정)