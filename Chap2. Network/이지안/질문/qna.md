# QnA day2
## 받은 질문

### 1. TCP의 4-way handshaking 에서 timeout 이 필요한 이유는 무엇인가?
> 4-way handshaking 의 마지막 ACK이 전달 과정에서 소멸되면, 다른 호스트는 자신이 좀 전에 보낸 FIN 메시지가 전송되지 못했다고 생각하고
계속해서 FIN 메시지를 전달하게 되는데 이때, 완전히 종료되지 않고 Time-wait 상태로 놓여있어야, 마지막 ACK을 재전송할 수 있고 상대 호스트도 정상 종료할 수 있음.

### 2. 스푸핑이 무엇인가? 스푸핑의 해결방법은 어떤 것이 있는가?
> 스푸핑은 LAN 상에서 스위칭 기능을 마비시키거나 속여서 특정 노드에 해당 패킷이 오도록 처리하는 것을 말한다.
> IP 스푸핑 공격은 트러스트 인증법 자체를 사용하지 않는 방법이 있고 DNS 스푸핑 공격은 중요한 사이트의 IP 주소에 대해서는 hosts 파일에 등록하여 관리하는 방법이 있습니다.

----
## 남은 질문

### 1-1. PDU가 무엇인가?
   > 네트워크의 어떠한 계층에서 계층으로 데이터가 전달될 때 한 덩어리의 단위를 말한다. 
   #### 1-2. 각 계층에서 이 PDU 를 부르는 명칭은 어떻게 달라지는가?
    - `애플리케이션 계층` : 메시지
    - `전송 계층` : 세그먼트(TCP), 데이터그램(UDP)
    - `인터넷 계층` : 패킷
    - `링크 계층` : 프레임(데이터 링크 계층), 비트(물리 계층)
### 2. TCP의 연결, 해제 과정에서 3-way, 4-way로 차이가 나는 이유는 무엇인가?
>Client 가 데이터 전송을 마쳤다고 하더라도 Server 는 아직 보낼 데이터가 남아 있을 수도 있기 때문에 연결 과정과는 다르게 해제 과정에서는 일단 FIN에 대한 ACK만 보내게 된다.
>그리고 남은 데이터를 모두 전송한 후에 FIN 을 전송하기 때문이다.