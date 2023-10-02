# Chapter 4: 데이터베이스
## Section 4.6: 조인의 종류
- **조인(Join)**: 하나의 테이블이 아닌 두 개 이상의 테이블을 묶어서 하나의 결과물을 만드는 것을 말한다.
- MySQL에서는 'JOIN', MongoDB에서는 'lookup'을 통해 쿼리를 처리한다.
- 조인의 대표적 종류: 내부 조인(Inner join), 왼쪽 조인(Left outer join), 오른쪽 조인(Right outer join), 합집합 조인(Full outer join)

<img src="./resource/join.png" width=500px>

### 4.6.1 내부 조인
- 두 테이블 간에 교집합을 나타낸다.
- e.g.)
> SELECT * FROM TableA A INNER JOIN TableB B ON A.key = B.key

### 4.6.2 왼쪽 조인
- 테이블 B의 일치하는 부분의 레코드와 함께 테이블 A를 기준으로 완전한 레코드 집합을 생성한다.
- 테이블 B에 일치하는 항목이 없다면 해당 값은 null이 된다.
- e.g.)
> SELECT * FROM TableA A LEFT JOIN TableB B ON A.key = B.key

### 4.6.3 오른쪽 조인
- 테이블 A에서 일치하는 부분의 레코드와 함께 테이블 B를 기준으로 완전한 레코드 집합을 생성한다.
- 테이블 A에 일치하는 항목이 없다면 해당 값은 null이 된다.
- e.g)
> SELECT * FROM TableA A RIGHT JOIN TableB B ON A.key = B.key

### 4.6.4 합집합 조인
- 양쪽 테이블에서 일치하는 레코드와 함께 테이블 A와 테이블 B의 모든 레코드 집합을 생성한다.
- 일치하는 항목이 없다면 누락된 쪽에 null 값이 포함되어 출력된다.
- e.g.)
> SELECT * FROM TableA A FULL OUTER JOIN TableB B ON A.key = B.key