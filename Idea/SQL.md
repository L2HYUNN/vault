## 데이터베이스와 SQL
**데이터베이스(Database):** 데이터의 집합
**DBMS(Database Management System):** 데이터베이스를 관리하고 운영하는 소프트웨어
	- 대용량 데이터를 관리해야한다.
	- 여러 사용자와 공유할 수 있어야한다.
**SQL(Structured Query Language):** DBMS에 데이터를 구축, 관리하고 활용하기 위해 사용되는 언어
**RDBMS(Relational DBMS):** 테이블이라는 최소 단위로 구성된 데이터베이스

![[Pasted image 20250119151122.png]]

DBMS는 **표준 SQL**을 준수하면서 자신들만의 특성을 반영한 SQL을 사용한다.

## 데이터베이스 모델링
우리가 살고 있는 세상에서 사용되는 사물이나 작업을 DBMS의 데이터베이스 개체로 옮기기 위한 과정

## 데이터베이스 만들기
데이터베이스 만들기 > 테이블 만들기 > 데이터 입력/수정/삭제하기 > 데이터 조회/활용하기

### 데이터베이스 만들기

```sql
CREATE SCHEMA `shop_db` ;
```

> [!info]
> `;` 구문은 1문장일 때 생략 가능하다

### 테이블 만들기

```sql
CREATE TABLE `shop_db`.`member` (
  `member_id` CHAR(8) NOT NULL,
  `member_name` CHAR(5) NOT NULL,
  `member_addr` VARCHAR(45) NULL,
  PRIMARY KEY (`member_id`));
```

```sql
CREATE TABLE `shop_db`.`product`(
	`product_name` CHAR(4) NOT NULL,
    `cost` INT NOT NULL,
    `make_date` DATE NULL,
    `company` CHAR(5) NULL,
	`amount` INT NOT NULL,
	PRIMARY KEY (`product_name`));
```

### 데이터 입력

```sql
INSERT INTO `shop_db`.`member` (`member_id`, `member_name`, `member_addr`) VALUES ('test', 'example', '경기도');
INSERT INTO `shop_db`.`member` (`member_id`, `member_name`, `member_addr`) VALUES ('test2', 'example2', '서울시');
INSERT INTO `shop_db`.`member` (`member_id`, `member_name`, `member_addr`) VALUES ('test3', 'example3', '경기도');
INSERT INTO `shop_db`.`member` (`member_id`, `member_name`, `member_addr`) VALUES ('test4', 'example4', '안산시');
```

### 데이터 수정

```sql
UPDATE `shop_db`.`member` SET `member_addr` = '먹자도' WHERE (`member_id` = 'test');
```

### 데이터 삭제

```sql
DELETE FROM `shop_db`.`member` WHERE (`member_id` = 'test4');
```

### 데이터 조회

모든 필드 조회
```sql
SELECT * FROM shop_db.member;
```

특정 필드 조회
```sql
SELECT member_name, member_addr FROM shop_db.member;
```

특정 값 조회
```sql
SELECT * FROM shop_db.member WHERE member_name = '한글자';
```

## 데이터베이스 개체
데이터베이스에 존재할 수 있는 `Object` 

### 인덱스
데이터를 조회할 때 빠르게 조회할 수 있게 도움을 주는 개체

인덱스 만들기
```sql
CREATE INDEX idx_member_name ON shop_db.member(member_name);
```

실무에서는 인덱스를 거의 반드시 사용한다

![[Pasted image 20250210223034.png]]

Full Table Scan -> 기본적은 스캔 방법   

### 뷰
실체가 없는 가상의 테이블 개체
뷰의 실체는 **SELECT** 문이다.

뷰 만들기
```sql
CREATE VIEW memeber_view AS SELECT * FROM shop_db.member;
```

뷰 사용하기
```sql
SELECT * FROM member_view;
```

### 스토어드 프로시저
스토어드 프로시저는 프로그래밍 언어처럼 동작할 수 있게 도움을 주는 개체

프로시저 생성
```sql
DELIMITER //
CREATE PROCEDURE myProc()
BRGIN
	SELECT * FROM member WHERE member_name = '나훈아';
	SELECT * FROM product WHERE product_name = '삼각김밥';
END //
DELIMITER;
```

프로시저 실행
```sql
CALL myproc();
```

## SQL 기본 문법
SELECT의 가장 기본 형식은 `SELECT ~ FROM ~ WHERE`이다.

DB를 지정해서 사용한다
```SQL
USE DB_NAME
```

```SQL
// 모든 속성
SELECT * FROM member WHERE mem_name = '블랙핑크';

// 일부 속성
SELECT * FROM mem_name, height, mem_number FROM member 

// 조건
SELECT * FROM mem_name, height, mem_number FROM member WHERE height >= 165 AND mem_number > 6;

// 사이
SELECT mem_name, height FROM member WHERE height BETWEEN 163 AND 165;
```
