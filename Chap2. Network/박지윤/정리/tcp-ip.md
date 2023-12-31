## TCP/IP 4계층 모델

### 1️⃣ 계층 구조
* TCP/IP 4계층
  1. 애플리케이션 계층
  2. 전송 계층
  3. 인터넷 계층
  4. 링크 계층

* OSI 7계층
  1. 애플리케이션 계층
  2. 프레젠테이션 계층
  3. 세션 계층
  4. 전송 계층
  5. 네트워크 계층
  6. 데이터 링크 계층
  7. 물리 계층


* TCP/IP 계층과 달리 OSI 계층은 애플리케이션 계층을 세 개로 쪼개고 링크 계층을 데이터 링크 계층, 물리 계층으로 나눠서 표현하는 것이 다르며, 인터넷 계층을 네트워크 계층으로 부른다
* 이 계층들은 특정 계층이 변경되었을 때 다른 계층이 영향을 받지 않도록 설계되었다


* TCP/IP 4계층 자세히 살펴보기
  * 애플리케이션 (application) 계층
    * 응용 프로그램이 사용되는 프로토콜 계층
    * 웹 서비스, 이메일 등 서비스를 실질적으로 사람들에게 제공하는 층
    * 예시
      * FTP: 장치와 장치 간의 파일을 전송하는 데 사용되는 표준 통신 프로토콜 
      * HTTP: World Wide Web을 위한 데이터 통신의 기초이자 웹 사이트를 이용하는 데 쓰는 프로토콜
      * SSH: 보안되지 않은 네트워크 서비스를 안전하게 운영하기 위한 암호화 네트워크 프로토콜
      * SMTP: 전자 메일 전송을 위한 인터넷 표준 통신 프로토콜
      * DNS: 도메인 이름과 IP 주소를 매핑해주는 서버

  * 전송 (transport) 계층
    * 송신자와 수신자를 연결하는 통신 서비스를 제공하는 층
    * 연결 지향 데이터 스트림 지원, 신뢰성, 흐름 제어를 제공할 수 있으며 애플리케이션과 인터넷 계층 사이의 데이터가 전달될 때 중계 역할을 한다
    * 예시
      * TCP
        * 패킷 사이의 순서를 보장하고 연결지향 프로토콜을 사용해서 연결하여 신뢰성을 구축해서 수신 여부를 확인할 수 있다
        * 가상 회선 패킷 교환 방식(각 패킷에는 가상회선 식별자가 포함되며 모든 패킷을 전송하면 가상회선이 해제되고 패킷들은 전송된 순서대로 도착하는 방식)을 사용한다
        
        * 연결 성립 과정 ➡ 3-way handshake
          * SYN 단계: 클라이언트는 서버에 클라이언트의 ISN을 담아 SYN을 보낸다. ISN은 새로운 TCP 연결의 첫 번째 패킷에 할당된 임의의 시퀀스 번호를 말하며 이는 장치마다 다를 수 있다.
          * SYN + ACK 단계: 서버는 클라이언트의 SYN을 수신하고 서버의 ISN을 보내며 승인번호로 클라이언트의 ISN+1을 보낸다.
          * ACK 단계: 클라이언트는 서버의 ISN+1한 값인 승인번호를 담아 ACK를 서버에 보낸다.
          * 3-way handshake 과정 이후 신뢰성이 구축되고 데이터 전송을 시작한다
          
        * 연결 해제 과정 ➡ 4-way handshake
          1. 먼저 클라이언트가 연결을 닫으려고 할 때 FIN으로 설정된 세그먼트를 보낸다. 그리고 클라이언트는 FIN_WAIT_1 상태로 들어가고 서버의 응답을 기다린다.
          2. 서버는 클라이언트로 ACK라는 승인 세그먼트를 보낸다. 그리고 CLOSE_WAIT 상태에 들어간다. 클라이언트가 세그먼트를 받으면 FIN_WAIT_2 상태에 들어간다.
          3. 서버는 ACK를 보내고 일정 시간 이후에 클라이언트에 FIN이라는 세그먼트를 보낸다.
          4. 클라이언트는 TIME_WAIT 상태가 되고 다시 서버로 ACK를 보내서 서버는 CLOSED 상태가 된다. 이후 클라이언트는 어느 정도의 시간을 대기한 후 연결이 닫히고 클라이언트와 서버의 모든 자원의 연결이 해제된다.
          
        * 왜 일정 시간 뒤에 연결을 닫을까?
          * 지연 패캣이 발생할 경우를 대비하기 위해서: 패킷이 뒤늦게 도달하고 이를 처리하지 못하면 데이터 무결성 문제가 발생
          * 두 장치가 연결이 닫혔는지 확인하기 위해서: 만약 LASK_ACK 상태에서 닫히게 되면 다시 새로운 연결을 하려고 할 때 장치가 LAST_ACK로 되어 있기 때문에 접속 오류가 발생
        ➡ TIME_WAIT 필요!!
          * 소켓이 바로 소멸되지 않고 일정 시간 유지되는 상태를 말한다
          * 지연 패킷 등의 문제를 해결하는 데 쓰인다
          * OS마다 유지 시간이 다르다
          
      * UDP
        * 순서를 보장하지 않고 수신 여부를 확인하지 않는다
        * 데이터그램 패킷 교환 방식(패킷이 독립적으로 이동하며 최적의 경로를 선택하여 가는데, 하나의 메시지에서 분할된 여러 패킷은 서로 다른 경로로 전송될 수 있으며 도착한 순서가 다를 수 있는 방식)을 사용한다
        
  * 인터넷 (Internet) 계층
    * 장치로부터 받은 네트워크 패킷을 IP 주소로 지정된 목적지로 전송하기 위해 사용되는 계층
    * 패킷을 수신해야 할 상대의 주소를 지정하여 데이터를 전달한다
    * 상대방이 제대로 받았는지에 대해 보장하지 않는 비연결형적인 특징을 가지고 있다
    * IP, ARP, ICMP 등

  * 링크 (Link) 계층
    * 전선, 광섬유, 무선 등으로 실질적으로 데이터를 전달하며 장치 간에 신호를 주고받는 '규칙'을 정하는 계층
    * 물리 계층과 데이터 링크 게층으로 나누기도 한다
      * 물리 계층: 무선 LAN과 유선 LAN을 통해 0과 1로 이루어진 데이터를 보내는 계층
      * 데이터 링크 계층: '이더넷 프레임'을 통해 에러 확인, 흐름 제어, 접근 제어를 담당하는 계층
      
    * 유선 LAN (IEEE802.3)
      * 전이중화(full duplex) 통신
        * 양쪽 장치가 동시에 송수신할 수 있는 방식
        * 송신로와 수신로로 나눠서 데이터를 주고받는다
        * 현대의 고속 이더넷은 이 방식을 기반으로 통신한다
      
      * CSMA/CD
        * 데이터를 보낸 이후 충돌이 발생한다면 일정 시간 이후 재전송하는 방식
        * 수신로와 송신로를 각각 둔 것이 아니고 한 경로를 기반으로 데이터를 보내기 때문에 데이터를 보낼 때 충돌에 대비해야 한다
        
    * 유선 LAN을 이루는 케이블
      * 트위스트 페어 케이블 (twisted pair cable, tp 케이블)
        * 하나의 케이블처럼 보이지만 실제로는 여덟 개의 구리선을 두 개씩 꼬아서 묶은 케이블
        * 구리선을 실드 처리하지 않고 덮은 UTP 케이블(LAN 케이블)과 실드 처리하고 덮은 STP로 나뉜다
      * 광섬유 케이블
        * 광섬유로 만든 케이블
        * 레이저를 이용해서 통신하기 때문에 구리선과는 비교할 수 없을 만큼의 장거리 및 고속 통신이 가능하다
        * 보통 100Gbps의 데이터를 전송한다
        * 광섬유 내부와 다른 밀도를 가지는 유리나 플라스틱 섬유로 제작해서 한 번 들어간 빛이 내부에서 계속적으로 반사하며 전진하여 반대편 끝까지 가는 원리를 이용한다
        * 빛의 굴절률이 높은 부분을 코어(core)라고 하며 낮은 부분을 클래딩(cladding)이라 한다
    
    * 무선 LAN (IEEE802.11)
      * 반이중화(half duplex) 통신
        * 양쪽 장치는 서로 통신할 수 있지만, 동시에는 통신할 수 없으며 한 번에 한 방향만 통신할 수 있는 방식
        * 일반적으로 장치가 신호를 수신하기 시작하면 응답하기 전에 전송이 완료될 때까지 기다려야 한다
        * 둘 이상의 장치가 동시에 전송하면 충돌이 발생하여 메시지가 손실되거나 왜곡될 수 있기 때문에 충돌 방지 시스템이 필요하다
        * 무선 LAN 장치는 수신과 송신에 같은 채널을 이용하기 때문에 반이중화 통신을 사용한다
        
      * CSMA/CA
        * 장치에서 데이터를 보내기 전에 캐리어 감지 등으로 사전에 가능한 한 충돌을 방지하는 방식
        * 과정
          1. 데이터를 송신하기 전에 무선 매체를 살핀다
          2. 캐리어 감지: 회선이 비어 있는지를 판단한다
          3. IFS(Inter FrameSpace): 랜덤 값을 기반으로 정해진 시간만큼 기다리며, 만약 무선 매체가 사용 중이면 점차 그 간격을 늘려가며 기다린다
          4. 이후에 데이터를 송신한다
          
    * 무선 LAN(WLAN)을 이루는 주파수
      * 2.4GHz: 장애물에 강한 특성을 가지고 있지만 전자레인지, 무선 등 전파 간섭이 일어나는 경우가 많다
      * 5GHz: 사용할 수 있는 채널 수도 많고 동시에 사용할 수 있기 때문에 상대적으로 깨끗한 전파 환경을 구축할 수 있다
    
      * 와이파이 (wifi)
        * 전자기기들이 무선 LAN 신호에 연결할 수 있게 하는 기술
        * 이를 사용하려면 흔히 공유기라고 하는 무선 접속 장치(AP, Access Point)가 있어야 한다
        * 무선 접속 장치를 통해 유선 LAN에 흐르는 신호를 무선 LAN 신호로 바꿔주어 신호가 닿는 범위 내에서 무선 인터넷을 사용할 수 있다
        * 무선 LAN을 이용한 다른 기술 - 지그비, 블루투스
      
      * BSS (Basic Service Set)
        * 기본 서비스 집합
        * 단순 공유기를 통해 네트워크에 접속하는 것이 아닌 동일 BSS 내에 있는 AP들과 장치들이 서로 통신이 가능한 구조
        * 근거리 무선 통신을 제공하고, 하나의 AP만을 기반으로 구축이 되어 있어 사용자가 한 곳에서 다른 곳으로 자유롭게 이동하며 네트워크에 접속하는 것은 불가능하다
      
      * ESS (Extended Service Set)
        * 하나 이상의 연결된 BSS 그룹
        * 장거리 무선 통신을 제공하며 BSS보다 더 많은 가용성과 이동성을 지원한다
        * 사용자는 한 장소에서 다른 장소로 이동하며 중단 없이 네트워크에 계속 연결할 수 있다
    
    * 이더넷 프레임
      * 데이터 링크 계층은 이더넷 프레임을 통해 전달받은 데이터의 에러를 검출하고 캡슐화한다


* 계층 간 데이터 송수신 과정
  * 애플리케이션 계층에서 전송 계층으로 전송자가 보내는 요청 값들이 캡슐화 과정을 거쳐 전달된다
  * 다시 링크 계층을 통해 해당 서버와 통신을 하고, 해당 서버의 링크 계층으로부터 애플리케이션까지 비캡슐화 과정을 거쳐 데이터가 전송된다
  
  * 캡슐화 과정
    * 상위 계층의 헤더와 데이터를 하위 계층의 데이터 부분에 포함시키고 해당 계층의 헤더를 삽입하는 과정
    * 애플리케이션 계층 ➡ 전송 계층 : 세그먼트 또는 데이터그램화
    * 전송 계층 ➡ 인터넷 계층 : 패킷화
    * 인터넷 계층 ➡ 링크 계층 : 프레임화
    

### 2️⃣ PDU
* PDU (Protocol Data Unit)
  * 네트워크의 어떠한 계층에서 계층으로 데이터가 전달될 때 한 덩어리의 단위
  * 제어 관련 정보들이 포함된 '헤더', 데이터를 의미하는 '페이로드'로 구성된다
  * 계층마다 부르는 명칭이 다르다
    * 애플리케이션 계층: 메시지
    * 전송 계층: 세그먼트(TCP), 데이터그램(UDP)
    * 인터넷 계층: 패킷
    * 링크 계층: 프레임(데이터 링크 계층), 비트(물리 계층)

  * PDU 중 아래 계층인 비트로 송수힌하는 것이 모든 PDU 중 가장 빠르고 효율성이 높지만 애플리케이션 계층에서는 문자열을 기반으로 송수신을 하는데, 그 이유는?
    * 헤더에 authorization 값 등 다른 값들을 넣는 확장이 쉽기 때문이다

