## 질의응답


Q. 캡슐화와 비캡슐화 과정에 대해 설명하시오.   
A. 캡슐화 과정은 상위 계층의 헤더와 데이터를 하위 계층의 데이터 부분에 포함시키고 해당 계층의 헤더를 삽입하는 과정으로, 애플리케여션 계층에서 전송 계층을 거치며 세그먼트화 혹은 데이터그램화가 되고, 전송 계층에서 인터넷 계층을 거치며 패킷화가 되며, 인터넷 계층에서 링크 계층을 거치며 프레임화가 됩니다. 비캡슐화는 캡슐화의 반대 과정으로, 위 계층에서 상위 계층으로 가며 각 계층의 헤더 부분을 제거하는 과정을 말합니다. 비캡슐화 과정을 거치면 캡슐화된 데이터는 메시지화가 됩니다.


Q. tcp/ip 계층의 이점은 무엇인가?   
A. tcp/ip 계층들은 특정 계층이 변경되었을 때 다른 계층이 영향을 받지 않도록 설계되었기 때문에 서로 간의 간섭을 최소화할 수 있어 유지 보수가 편리하다는 이점이 있습니다. 그렇기에 특정 계층에 문제가 생기면 해당 계층만 살펴보면 됩니다.


Q. 스푸핑과 스니핑의 차이에 대해 설명하시오.  
A. 스푸핑은 LAN 상에서 스위칭 기능을 마비시키거나 속여서 특정 노드에 해당 패킷이 오도록 처리하는 것을 말합니다. 스푸핑을 적용하면 올바르게 수신부로 가야 할 패킷이 악의적인 노드에 전달됩니다.  
스니핑은 네트워크에서 움직이는 트래픽에 대해서 염탐하는것을 의미합니다. 이러한 염탐으로 네트워크에서 패킷 교환이 이루어지게 될시 취약점을 발견하거나 개인정보 ID 패스워드와 같은 정보를 가로챌 수 있습니다.


Q. 스푸핑을 어떻게 방지할 수 있을까?  
A. 스푸핑을 방지하기 위해서는 HTTPS, SSH, TLS와 같은 암호화된 네트워크 프로토콜을 사용하여 패킷을 안전하게 암호화하고 보호하거나, 스푸핑을 감지할 수 있는 소프트웨어를 설치하여 네트워크의 위변조 또는 이상을 감지합니다. 또는 VPN을 이용하여 네트워크 트래픽을 암호화하고 해커로부터 네트워크 트래픽 또는 IP 주소의 노출을 최소화시키거나 실제 IP 주소를 찾기 힘들게 할 수 있습니다.


***

Q. QUIC이 UDP 기반임에도 신뢰성/보안성이 있는 이유는 무엇인가?   
A. QUIC은 TLS 암호화를 적용하고 있으며 암호화된 QUIC handshking을 통해 보안을 강화합니다 또한 부가적인 정보를 덧붙이는 확장을 통해 사용자화하여 TCP처럼 동작합니다. 그에 따라 신뢰성을 올릴 수 있습니다.


Q. TCP 통신 방식이 UDP보다 더 오래걸리는 이유?   
A. UDP는 TCP와 달리 흐름제어, 오류제어, 손상된 세그먼트에 대한 재전송 등과 같은 부가적인 기능을 제공하지 않기 때문입니다. 오직 Checksum 필드를 통해서 최소한의 오류만 검출하기 때문에 신뢰성이 TCP에 비해서 낮은 편이지만, 이 때문에 TCP에 비해서 속도가 빠르다고 할 수 있습니다.


Q. HOL blocking이란? HOL blocking을 해결하는 방법은 무엇인가?   
A. HOL blocking이란 네트워크에서 같은 큐에 있는 패킷이 그 첫 번째 패킷에 의해 지연될 때 발생하는 성능 저하 현상을 말합니다. 먼저 다운로드 되고 있던 리소스가 느리게 받아진다면 그 뒤에 있는 것들이 대기하게 되며 다운로드가 지연되는 상태가 됩니다. HOL blocking을 해결하기 위해 멀티플렉싱을 이용할 수 있습니다. 멀티플렉싱은 리소스를 여러 프레임으로 나눠서 목적지에서 재조립하는 방식으로, 통신 링크 효율을 극대화할 수 있습니다.


Q. NAT에 대하여 NAT을 쓰는 이유와 함께 설명하시오. NAT이 사용된 예시를 들어보자   
A. NAT은 패킷이 라우팅 장치를 통해 전송되는 동안 패킷의 IP 주소 정보를 수정하여 IP 주소를 다른 주소로 매핑하는 방법으로, 주로 여러 대의 호스트가 하나의 공인 IP 주소를 사용하여 인터넷에 접속하기 위해 사용합니다. 
예를 들어, 인터넷 회선 하나를 개통하고 인터넷 공유기를 달아서 여러 PC를 연결하여 사용할 수 있는데, 이는 인터넷 공유기에 NAT 기능이 탑재되어 있기 때문에 가능합니다.

