# marketAPI
$\bf \large 요구\ 사항$

1. 제품 등록과 구매는 회원만 가능하다.
2. 비회원은 등록된 제품의 목록 조회와 상세 조회만 가능하다.
    - 상세 조회 시에는 제품명, 가격, 예약 상태 정보를 포함하여 조회 가능하다.
3. 등록된 제품에는 "제품명", "가격", "제품 상태", “수량” 정보가 포함된다.
4. 제품 상태는 "판매중",  “승인 대기 중”, "판매 완료" 세가지가 존재한다.
    - 추가 판매가 가능한 수량이 남아있는 경우 - 판매중
    - 추가 판매가 불가능하고 현재 구매확정을 대기하고 있는 경우 - 승인 대기 중
    - 모든 수량에 대해 모든 구매자가 모두 구매확정한 경우 - 판매 완료
5. 구매자가 제품의 상세페이지에서 구매하기 버튼을 누르면 거래가 시작된다.
6. 거래 상태는 “예약중”, “거래 거부”, “거래 완료” 세가지가 존재한다.
7. 판매자는 거래진행중인 구매자에 대해 '판매승인'을 하는 경우 거래가 완료됩니다.
8. 다수의 구매자가 한 제품에 대해 구매하기가 가능하다. 
9. 판매자와 구매자는 제품의 상세정보를 조회하면 해당 제품에 대한 자신의 거래내역을 확인할 수 있다.
10. 모든 사용자는 내가 "구매한 제품", “판매한 제품”, "예약중인 제품", “판매중인 제품”, 의 목록을 확인할 수 있습니다.
    - “구매한 제품", "예약중인 제품", “판매한 제품”의 가격은 거래 당시의 가격 정보가 나타난다.
    - 예) 구매자 A가 구매하기 요청한 당시의 제품 B의 가격이 3000원이었고 이후에 4000원으로 바뀌었다 하더라도 목록에서는 3000원으로 나타나야합니다.

---

$\bf \large 제약\ 사항$

1. 구매 취소는 고려하지 않는다.

---

$\bf \large 개발\ 환경$

- Eclipse 2022-12R
- Java 17
- Spring Boot 3.3.0

---

$\bf \large 기술\ 스택$

- Spring Boot
    - Spring Web
    - Spring Boot DevTools
    - Spring JPA
- Spring Security
- PostgreSQL
- JWT

---

$\bf \large Database\ 명세$

- User : 사용자 정보를 저장하는 테이블

| Column | Type | 설명 | 비고 |
| --- | --- | --- | --- |
| userId | Integer | 사용자 Id | primary key
순차 발행 |
| email | varChar(64) | 로그인 Id | unique, not null |
| userPassword | varChar(64) | 비밀번호 | not null |
| phoneNumber | varChar(16) | 폰 번호 | not null |
| userName | varChar(16) | 닉네임 | not null |

- Product: 거래 물품 정보를 저장하는 테이블

| Column | Type | 설명 | 비고 |
| --- | --- | --- | --- |
| productId | Integer | 거래 물품 Id | primary key
순차 발행 |
| userId | Integer | 판매자 Id | not null |
| productStatus | Integer | 제품 상태 | not null
0: 판매중
1: 승인 대기 중
2: 판매 완료 |
| productName | varChar(64) | 현재 물품 이름 | not null1 |
| price | Integer | 현재 물품 가격(1 개당) | not null |
| quantity | Integer | 총 수량 | not null |

- Trade: 물품의 거래 내역을 저장하는 테이블

| Column | Type | 설명 | 비고 |
| --- | --- | --- | --- |
| productId | Integer | 제품 아이디 | primary key |
| tradeId | Integer | 거래 아이디 | primaryKey |
| sellerId | Integer | 판매자 Id(userId) | not null |
| buyerId | Integer | 구매자 Id(userId) | not null |
| tradeStatus | Integer | 거래 상태 | not null
0: 예약 중
1: 거래 거부
2: 거래 완료 |
| tradeProductName | varChar(64) | 거래 물품 이름 | not null |
| tradePrice | Integer | 거래 물품 가격(1 개당) | not null |
| tardeQuantity | Integer | 수량 | not null |

---

$\bf \large API\ 명세$
