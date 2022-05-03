# Day17 2022-05-03

## AI 플랫폼을 활용한 웹서비스 개발

### 1. Schema 생성하기

1. cmd 명령 프롬포트 창을 띄운다.
2. mysql에 접속한다.
   - mysql -u amdin1 -p
   - 비밀번호 입력
3. 데이터베이스를 확인한다.
   - SHOW DATABASES;
4. 스키마를 생성한다.
   - CREATE SCHEMA 스키마 이름;
5. 스키마로 이동한다.
   - USE 스키마 이름;
6. 테이블을 확인한다.
   - SHOW TABLES;

### 2. TABLE 생성하기

1. 오류가 나지 않도록 DROP을 이용해 기존에 있는 테이블은 삭제하고 새로 만든다.

```
- DDL
-- 만약 존재하면 삭제하고 다시 만든다.
DROP DATABASE IF EXISTS shoppingdb;
CREATE DATABASE shoppingdb;
USE shoppingdb;

DROP TABLE IF EXISTS cart;
DROP TABLE IF EXISTS cust;
DROP TABLE IF EXISTS product;
DROP TABLE IF EXISTS cate;
```

2. 테이블을 생성한다.

```
-- NOT NULL : NULL 값이 들어갈 수 없다.
CREATE TABLE cust(
	id VARCHAR(10),
    name VARCHAR(20) NOT NULL,
    addr VARCHAR(100) NOT NULL,
    regdate DATE NOT NULL
);

-- AUTO_INCREMENT : 열에 자동으로 번호를 메긴다.
ALTER TABLE product MODIFY id INT AUTO_INCREMENT;
ALTER TABLE product AUTO_INCREMENT = 1000;
```

3. 기본 키 제약 조건
   - PRIMARY KEY 값을 지정한다.
   - 기본키로 설정된 열은 NULL값이 들어갈 수 없어서, NOT NULL을 빼도 된다.

```
ALTER TABLE cust ADD CONSTRAINT PRIMARY KEY(id);
```

4. 외래 키 제약 조건
   - FOREIGN KEY : uid가 cust 테이블의 id를 참조한다.
   - ON DELETE CASCADE / ON UPDATE CASCADE : 기준 테이블의 데이터가 변경되었을 때 외래 키 테이블도 자동으로 적용되도록 설정한다.

```
ALTER TABLE cart ADD CONSTRAINT FOREIGN KEY(uid) 
REFERENCES cust(id)
ON DELETE CASCADE
ON UPDATE CASCADE;
```

5. UNIQUE 제약 조건
   - 중복되지 않는 유일한 값을 입력해야 하는 조건이다.
   - PRIMARY KEY와 비슷하지만 차이점은 NULL값이 허용된다는 점이다.

```
ALTER TABLE cate ADD CONSTRAINT UNIQUE(name);
```

6. CHECK 제약 조건
   - 입력되는 데이터를 점검하는 기능을 한다.

```
ALTER TABLE product ADD CONSTRAINT CHECK(price > 0);
```

7. DEFAULT 정의
   - 값을 입력하지 않았을 때, 자동으로 입력되는 기본 값을 정의하는 방법이다.

```
ALTER TABLE cust ALTER COLUMN addr SET DEFAULT 'Seoul';
```

### 3. TABLE 삭제하기

1. 테이블 삭제
   - 외래 키 제약 조건의 기준 테이블은 삭제할 수 없다. 먼저 외래 키가 생성된 외래 키 테이블을 삭제해야 한다.

```
DELETE TABLE cust;
```

### 4. VIEW

- 뷰는 테이블과 동일하게 사용되는 개체이다.
- 뷰는 기본적으로 읽기 전용으로 사용되지만, 뷰를 통해서 테이블을 수정할 수 있다.

1. 뷰 생성하기

```
CREATE VIEW v_cart
AS
SELECT c.id, cu.id AS uid, cu.name AS uname, ca.name AS cname, 
p.id AS pid, p.name AS pname, p.price, c.regdate FROM cart c
INNER JOIN cust cu ON c.uid = cu.id
INNER JOIN product p ON c.pid = p.id
INNER JOIN cate ca ON ca.id = p.cid;
```

2. 뷰의 장점
   - 보안에 도움이 된다.
   - 복잡한 쿼리를 단순화 시켜줄 수 있다.
3. 뷰를 통해 데이터의 수정이나 삭제를 할 수 없는 경우
   - 집계 함수를 사용한 뷰
   - UNION ALL, JOIN 등을 사용한 뷰
   - DISTINCT, GROUP BY 등을 사용한 뷰

### 5. Team Workshop

- 쇼핑몰 주제를 선정하고 ERD를 설계한다.
- 한 번의 주문에 여러 개의 물건을 주문할 수 있도록 설계한다.

<img src="C:\Users\hasun\Downloads\Shopping   (2).png" alt="Shopping   (2)" style="zoom:67%;" />