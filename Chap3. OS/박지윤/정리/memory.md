## 메모리

### 1️⃣ 메모리 계층
* 메모리 계층은 레지스터, 캐시, 메모리, 저장장치로 구성되어 있음
* `레지스터`
  * CPU 안에 있는 작은 메모리
  * 휘발성, 속도 가장 빠름
  * 기억 용량 가장 적음
* `캐시`
  * L1, L2 캐시를 지칭
  * 휘발성, 속도 빠름
  * 기억 용량 적음
* `주기억장치`
  * RAM을 가리킴 (하드디스크로부터 일정량의 데이터를 복사해서 임시 저장하고 이를 필요시마다 CPU에 빠르게 전달하는 역할을 함)
  * 휘발성, 속도, 기억 용량 보통
* `보조기억장치`
  * HDD, SSD를 일컬음
  * 비휘발성, 속도 낮음
  * 기억 용량 많음


* 계층 위로 올라갈수록 가격은 비싸지고 용량은 작아지고 속도는 빨라짐
* 이러한 계층이 있는 이유는 경제성과 캐시 때문


* **캐시 (cache)**
  * 데이터를 미리 복사해놓는 임시 저장소이자 빠른 장치와 느린 장치에서 속도 차이에 따른 병목 현상을 줄이기 위한 메모리
  * 데이터를 접근하는 시간이 오래 걸리는 경우를 해결하고 무언가를 다시 계산하는 시간을 절약할 수 있음
  
  * 캐싱 계층
    * 속도 차이를 해결하기 위해 계층과 계층 사이에 있는 계층
    * 캐시 메모리와 보조기억장치 사이에 있는 주기억장치는 보조기억장치의 캐싱 계층
    
  * `지역성의 원리`
    * 캐시 계층을 두는 것 말고 캐시를 직접 설정할 때는 자주 사용하는 데이터를 기반으로 설정해야 함
    * 자주 사용하는 데이터에 대한 근거 >> 지역성
    * 시간 지역성 (temporal locality): 최근 사용한 데이터에 다시 접근하려는 특성
    * 공간 지역성 (spatial locality): 최근 접근한 데이터를 이루고 있는 공간이나 그 가까운 공간에 접근하는 특성
    
  * `캐시히트와 캐시미스`
    * 캐시히트
      * 캐시에서 원하는 데이터를 찾음
      * 해당 데이터를 제어 장치를 거쳐 가져오게 됨
      * 위치도 가깝고 CPU 내부 버스를 기반으로 작동하기 때문에 빠름
    * 캐시미스
      * 해당 데이터가 캐시에 없음
      * 주 메모리로 가서 데이터를 찾아옴
      * 시스템 버스를 기반으로 작동하기 때문에 느림
    
  * `캐시 매핑`
    * 캐시가 히트되기 위해 매핑하는 방법
    * 레지스터는 주 메모리에 비하면 굉장히 작고 주 메모리는 굉장히 크기 때문에 작은 레지스터가 캐시 계층으로써 역할을 잘 해주려면 매핑을 어떻게 하느냐가 중요함
    * 캐시 매핑 분류
      * 직접 매핑 (directed mapping)
        * 메모리가 1~100이 있고 캐시가 1~10이 있다면, 1:1~10, 2:1~20... 이런 식으로 매핑하는 것을 말함
        * 처리가 빠르지만 충돌 발생이 잦음
      * 연관 매핑 (associative mapping)
        * 순서를 일치시키지 않고 관련 있는 캐시와 메모리를 매핑함
        * 충돌이 적지만 모든 블록을 탐색해야 하므로 속도가 느림
      * 집합 연관 매핑 (set associative mapping)
        * 직접 매핑과 연관 매핑을 합친 것
        * 순서는 일치시키지만 집합을 둬서 저장하며 블록화되어 있기 때문에 검색은 좀 더 효율적임
        * 메모리가 1~100이 있고 캐시가 1~10이 있다면 캐시 1~5에는 1~50의 데이터를 무작위로 저장시키는 것을 말함
  
  * `웹 브라우저의 캐시`
    * 쿠키, 로컬 스토리지, 세선 스토리지가 있음
    * 보통 사용자의 커스텀한 정보나 인증 모듈 관련 사항들을 웹 브라우저에 저장해서 추후 서버에 요청할 때 자신을 나타내는 아이덴티티나 중복 요청 방지를 위해 쓰임
    * 쿠키
      * 만료기한이 있는 키-값 저장소
      * 4KB까지 데이터를 저장할 수 있고 만료기한을 정할 수 있음
      * 쿠키를 설정할 때는 document.cookie로 쿠키를 볼 수 없게 httponly 옵션을 거는 것이 중요함
      * 서버, 클라이언트에서 만료기한 등을 정할 수 있음 (보통 서버)
    * 로컬 스토리지
      * 만료기한이 없는 키-값 저장소
      * 10KB까지 데이터를 저장할 수 있음
      * 웹 브라우저를 닫아도 유지되고 도메인 단위로 저장, 생성됨
      * HTML5를 지원하지 않는 웹 브라우저에서는 사용할 수 없으며 클라이언트에서만 수정 가능함
    * 세션 스토리지
      * 만료기한이 없는 키-값 저장소
      * 탭 단위로 세션 스토리지를 생성하며, 탭을 닫을 때 해당 데이터가 삭제됨
      * 5MB까지 데이터를 저장할 수 잇음
      * HTML5를 지원하지 않는 웹 브라우저에서는 사용할 수 없으며 클라이언트에서만 수정 가능함
      
  * `데이터베이스의 캐싱 계층`
    * 데이터베이스 시스템을 구축할 때도 메인 데이터베이스 위에 레디스(redis) 데이터베이스 계층을 캐싱 계층으로 둬서 성능을 향상시키기도 함


### 2️⃣ 메모리 관리
* **가상 메모리 (Virtual Memory)**
  * 컴퓨터가 실제로 이용 가능한 메모리 자원을 **추상화**하여 이를 사용하는 사용자들에게 매우 큰 메모리로 보이게 만드는 것을 말함
  * 가상적으로 주어진 주소를 가상 주소(logical address)라고 하며, 실제 메모리상에 있는 주소를 실제 주소(physical address)라고 함
  * 가상 주소는 *메모리관리장치(MMU)* 에 의해 실제 주소로 변환되며, 이에 의해 사용자는 실제 주소를 의식할 필요 없이 프로그램을 구축할 수 있음
  * 가상 메모리는 가상 주소와 실제 주소가 매핑되어 있고 프로세스의 주소 정보가 들어 있는 *페이지 테이블* 로 관리되며, 이때 속도 향상을 위해 *TLB* 를 사용함
  
  * `TLB`
    * 메모리와 CPU 사이에 있는 주소 변환을 위한 캐시
    * 페이지 테이블에 있는 리스트를 보관하며 CPU가 페이지 테이블까지 가지 않도록 속도를 향상시킬 수 있는 캐시 계층


* 스와핑 (Swapping)
  * 가상 메모리에는 존재하지만 실제 메모리인 RAM에는 없는 데이터나 코드에 접근할 경우 *페이지 폴트*가 일어나게 되는데
  * 이때 메모리에서 당장 사용하지 않는 영역을 하드디스크로 옮기고 하드디스크의 일부분을 마치 메모리처럼 불러와 쓰는 것을 말함
  * 이를 통해 마치 페이지 폴트가 일어나지 않은 것처럼 만듦

  
* 페이지 폴트 (Page Fault)
  * 프로세스의 주소 공간에는 존재하지만 지금 해당 컴퓨터의 RAM에는 없는 데이터에 접근했을 경우 발생
  * 페이지 폴트와 그로 인한 스와핑 과정
    1. CPU는 물리 메모리를 확인하여 해당 페이지가 없으면 트랩을 발생해서 운영체제에 알린다.
    2. 운영체제는 CPU의 동작을 잠시 멈춘다.
    3. 운영체제는 페이지 테이블을 확인하여 가상 메모리에 페이지가 존재하는지 확인하고, 없으면 프로세스를 중단하고 현재 물리 메모리에 비어 있는 프레임이 있는지 찾는다. 물리 메모리에도 없다면 스와핑이 발동된다.
    4. 비어 있는 프레임에 해당 페이지를 로드하고, 페이지 테이블을 최신화한다.
    5. 중단되었던 CPU를 다시 시작한다.
  * 페이지(page): 가상 메모리를 사용하는 최소 크기 단위
  * 프레임(frame): 실제 메모리를 사용하는 최소 크기 단위


* 스레싱 (Thrashing)
  * 메모리의 페이지 폴트율이 높은 것을 의미 >> 컴퓨터의 심각한 성능 저하를 초래
  * 스레싱은 메모리에 너무 많은 프로세스가 동시에 올라가게 되면 스와핑이 많이 일어나서 발생함
  * 악순환
    1. 페이지 폴트가 일어나면 CPU 이용률이 낮아짐
    2. CPU 이용률이 낮아지면 운영체제는 가용성을 높이기 위해 더 많은 프로세스를 메모리에 올리기 됨
  * 해결 방법
    * 매모리를 늘림
    * HDD를 사용한다면 SSD로 바꿈
    * 운영체제에서의 해결방법 - 작업 세트, PFF

  * `작업 세트 (working set)`
    * 프로세스의 과거 사용 이력인 지역성을 통해 결정된 페이지 집합을 만들어서 미리 메모리에 로드하는 것
    * 미리 메모리에 로드하면 탐색에 드는 비용을 줄일 수 있고 스와핑 또한 줄일 수 있음

  * `PFF(Page Fault Frequency)`
    * 페이지 폴트 빈도를 조절하는 방법으로 상한선과 하한선을 만드는 방법
    * 만약 상한선에 도달한다면 프레임을 늘리고 하한선에 도달한다면 프레임을 줄임


* 메모리 할당
  * 메모리에 프로그램을 할당할 때는 시작 메모리 위치, 메모리의 할당 크기를 기반으로 할당
  * 연속 할당과 불연속 할당으로 나뉨
  * `연속 할당`
    * 메모리에 '연속적으로' 공간을 할당하는 것을 말함
    * `고정 분할 방식 (fixed partition allocation)`
      * 메모리를 미리 나누어 관리하는 방식
      * 메모리가 미리 나뉘어 있기 때문에 융통성이 없음
      * 내부 단편화 발생
    * `가변 분할 방식 (variable partition allocation)`
      * 매 시점 프로그램의 크기에 맞게 동적으로 메모리를 나눠 사용하는 방식
      * 내부 단편화 발생 안함
      * 외부 단편화 발생
      * 종류
        * 최초적합: 위쪽이나 아래쪽부터 시작해서 홀을 찾으면 바로 할당
        * 최적적합: 프로세스의 크기 이상인 공간 중 가장 작은 홀부터 할당
        * 최악적합: 프로세스의 크기와 가장 많이 차이가 나는 홀에 할당
  * 내부 단편화 (internal fragmentation)
    * 메모리를 나눈 크기보다 프로그램이 작아서 들어가지 못하는 공간이 많이 발생하는 현상
  * 외부 단편화 (external fragmentation)
    * 메모리를 나눈 크기보다 프로그램이 커서 들어가지 못하는 공간이 많이 발생하는 현상
  * `불연속 할당`
    * 메모리를 연속적으로 할당하지 않는 것을 말함
    * 메모리를 동일한 크기의 페이지(보통 4KB)로 나누고 프로그램마다 페이지 테이블을 두어 이를 통해 메모리에 프로그램을 할당
    * `페이징 (paging)`
      * 동일한 크기의 페이지 단위로 나누어 메모리의 서로 다른 위치에 프로세스를 할당
      * 홀의 크기가 균일하지 않은 문제가 없어지지만 주소 변환이 복잡해짐
    * `세그멘테이션 (segmentation)`
      * 페이지 단위가 아닌 의미 단위인 세그먼트(segment)로 나누는 방식
      * 프로세스는 코드, 데이터, 스택, 힙 등으로 이루어지는데 코드와 데이터 등을 기반으로 나눌 수도 있고 함수 단위로 나눌 수도 있음을 의미
      * 공유와 보안 측면에서 좋지만 홀 크기가 균일하지 않은 문제가 발생
    * `페이지드 세그멘테이션 (paged segmentation)`
      * 공유나 보안을 의미 단위의 세그먼트로 나누고 물리적 메모리는 페이지로 나누는 방식


* 페이지 교체 알고리즘
  * `오프라인 알고리즘 (offline algorithm)`
    * 먼 미래에 참조되는 페이지와 현재 할당하는 페이지를 바꾸는 알고리즘
    * 미래에 사용되는 프로세스를 알 수 없기에 사용할 수 없는 알고리즘이지만 다른 알고리즘과의 성능 비교에 대한 기준을 제공함
  
  * `FIFO (First In First Out)`
    * 가장 먼저 온 페이지를 교체 영역에 가장 먼저 놓는 방법
  
  * `LRU (Least Recently Used)`
    * 참조가 가장 오래된 페이지를 바꿈
    * 오래된 것을 파악하기 위해 각 페이지마다 계수기, 스택을 두어야 하는 문제가 있음
    * 구현
      * 해시 테이블과 이중 연결 리스트를 이용
      * 해시 테이블은 이중 연결 리스트에서 빠르게 찾을 수 있도록 사용하고, 이중 연결 리스트는 한정된 메모리를 나타냄
  
  * `NUR (Not Used Recently)`
    * LRU에서 발전한 형태, 일명 clock 알고리즘
    * 먼저 0과 1을 가진 비트를 둠
    * 1은 최근에 참조되었고 0은 참조되지 않음을 의미
    * 시계 방향을 돌면서 0을 찾고 0을 찾은 순간 해당 프로세스를 교체하고 해당 부분을 1로 바꿈
  
  * `LFU (Least Frequently Used)`
    * 가장 참조 횟수가 적은 페이지를 교체
    * 즉, 많이 사용되지 않은 것을 교체

