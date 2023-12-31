# QnA day5
## 받은 질문
1.  Transaction 이란 무엇인가요?  
데이터베이스의 상태를 변화시키기 위해서 수행하는 작업들의 단위입니다.  


2. Transaction이 가지고 있는 특성은 어떤 게 있나요?  
트랜잭션의 특성은 크게 원자성, 일관성, 격리성, 지속성으로 4가지로 나뉩니다.  
첫 번째로 원자성은 한 트랜잭션의 작업이 데이터베이스에 모두 반영되거나 모두 반영되지 않아야 한다는 특성을 나타냅니다.  
두 번째로 일관성은 데이터베이스의 작업 처리 결과가 모두 일관된 방식으로 적용되어야한다는 것을 의미합니다.  
세 번째로 격리성은 트랜잭션을 실행하는 도중에 어떤 다른 트랜잭션의 연산이 끼어들 수 없다는 특성입니다.  
마지막으로 지속성은 성공적으로 완료된 트랜잭션의 경우 그 결과은 영구적으로 반용되어야 한다는 것을 의미합니다.  
   

2. 데이터베이스의 이상 현상(anomaly)에 대해서 설명해주세요.  
   이상 현상의 종류에는 갱신 이상, 삽입 이상, 삭제 이상이 있습니다.  
   첫 번째로 `갱신 이상`은 중복된 데이터 중에 일부만 갱신해 데이터의 불일치가 발생하는 것을 나타내고   
   두 번째로 `삽입 이상`은 불필요한 정보를 함께 저장하지 않고서는 어떤 정보를 저장하는 것이 불가능한 것을 가리킵니다.  
   마지막으로 `삭제 이상`은 필요한 정보를 함께 삭제하지 않고서는 어떤 정보를 삭제하는 것이 불가능할 때를 말합니다.  


## 남은 질문
1. RDBMS와 NoSQL의 차이점은?
   **RDBMS**
   - 2차원의 행과 열로 데이터의 관계를 관리하는 데이터베이스
   - RDBMS는 관계형 데이터베이스 관리 시스템으로서 테이블 간의 관계에서 서로의 칼럼을 기준으로 Join이 가능하다.
   - 장점: 스키마에 맞추어 데이터를 관리하기 때문에 데이터의 정합성을 보장할 수 있다.
   - 단점: 시스템이 커질 수록 쿼리가 복잡해지고 성능이 저하되며, 수평적 확장이 어렵다.
   **NoSQL**
   - RDBMS가 비대해짐에 따라 관계가 복잡해져, 이를 극복하기 위해 등장하게 된 데이터베이스
   - NoSQL에서는 NoSQL은 스키마 없이 key-value형태로 관리하고 테이블 간 관계를 정의하지 않음. 따라서, Join이 불가능하고 트랜잭션을 지원하지 않음.
   - 장점: NOSQL은 스키마 없이 Key-Value 형태로 데이터를 관리하여 좀 더 자유롭게 데이터를 관리할 수 있다.
   - 단점: 중복된 데이터가 추가 가능하여, 이에 대한 관리가 필요하다.


2. 정규화란 무엇인가?
    데이터의 중복을 줄이고 테이블 간 관계를 명확하게 정의하기 위해서 관계형 데이터베이스에서 데이터를 구조화하는 작업
   - 정규화의 장점과 단점은?
     **장점**
   - 구조 확장시 재디자인을 최소화할 수 있다. 
   - 중복 데이터를 제거할 수 있다. 
   - 하나의 큰 테이블을 분리하여 데이터를 작고 관리하는 쉬운 단위로 관리하기 때문에 유지 관리와 성능이 향상됨.
    **단점** ( 역정규화를 하는 이유 )
   - JOIN이 많이 발생해 오히려 성능이 떨어질 수 있다.


3. 트랜잭션이 무엇인가요?
- 데이터베이스의 상태를 변화시키기 위해서 수행하는 작업의 단위
- 1. 원자성 : 트랜잭션과 관련된 일이 모두 수행되었거나 되지 않았거나를 보장하는 특징
- 2. 일관성 : 허용된 방식'으로만 데이터를 변경해야 하는 것 모든 데이터는 여러 가지 조건, 규칙에 따라 유효함을 가져야 한다.
- 3. 격리성 : 트랜잭션 수행 시 서로 끼어들지 못하는 것
- 4. 지속성 : 성공적으로 수행된 트랜잭션은 영원히 반영되어야 하는 것


4. 데이터베이스의 무결성 중 하나를 설명해주세요
   1. `개체 무결성` : 기본키로 선택된 필드는 빈 값을 허용하지 않음.
   2. `참조 무결성` : 서로 참조 관계에 있는 두 테이블의 데이터는 항상 일관된 값을 유지해야 함.
      기본 키와 참조 키 간의 관계가 항상 유지됨을 보장합니다.  
      - 참조되는 테이블의 행을 이를 참조하는 참조키가 존재하는 한 삭제될 수 없고, 기본키도 변경될 수 없습니다.
   3. `고유 무결성` : 모두 고유한 값을 가져야 함.
   4. `NULL 무결성` : NULL이 될 수 없다.


5. commit과 rollback 연산이 무엇인가?
    commit, rollback은 언제 일어나는가??


6. 낮은 단계의 Isolation Level을 이용할 때 발생하는 현상?
    - Dirty Read : 어떤 트랜잭션에서 아직 실행이 끝난지 않은 다른 트랜잭션에 의한 변경 사항을 보게 될 수 있음.
    - Phantom Read : 한 트랜잭션 안에서 일정 범위의 레코드를 두 번 이상 읽을 때, 첫 번째 쿼리에서 없던 레코드가 두 번째 쿼리에서 나타나는 현상
    - Non-Repeatable Read : 한 트랜잭션에서 같은 쿼리를 두 번 수행할 때 그 사이에 다른 트랜잭션이 값을 수정함으로써 두 쿼리의 결과가 상이하게 나타나는 비 일관성 현상


7. 격리수준은 어떤 게 있는가?  
SERIALIZABLE / REPEATABLE_READ / READ_COMMITTED / READ_UNCOMMITTED  
   - SERIALIZABLE로 설정했을 때 단점은?  
      교착 상태가 일어날 확률이 많음, 성능이 가장 떨어짐.
