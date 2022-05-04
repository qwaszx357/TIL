# Day18 2022-05-04

## AI 플랫폼을 활용한 웹서비스 개발

### 1. Team Workshop

1. 쇼핑몰 주제를 선정하고 ERD를 설계한다.

- ERD 수정
  - 쿠폰 테이블과 추천상품 테이블 삭제 후 배송지목록 테이블을 추가하였다.

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220505004007686.png" alt="image-20220505004007686" style="zoom:80%;" />

2. 수정한 ERD를 바탕으로 DDL문을 작성한다.
   - shoppingmalldb 스키마를 생성한다.
   - 각 테이블을 drop한 뒤 create 한다.
   - primary key와 foreing key 등 제약 조건을 입력한다.

```
DROP DATABASE IF EXISTS shoppingmalldb;
CREATE DATABASE shoppingmalldb;
USE shoppingmalldb;

DROP TABLE IF EXISTS cust;
DROP TABLE IF EXISTS cate;
DROP TABLE IF EXISTS product;
DROP TABLE IF EXISTS cart;
DROP TABLE IF EXISTS buy;
DROP TABLE IF EXISTS spec;
DROP TABLE IF EXISTS dlist;

-- cust table

CREATE TABLE cust (
	id INT,
	lid VARCHAR(10) NOT NULL,
    pwd	VARCHAR(20)	NOT NULL,
	name VARCHAR(20) NOT NULL,
	addr VARCHAR(100) NOT NULL,
	regdate	DATE NOT NULL,
	tell CHAR(11) NOT NULL
);

ALTER TABLE cust ADD CONSTRAINT PRIMARY KEY(id);
ALTER TABLE cust ADD CONSTRAINT UNIQUE(lid);
ALTER TABLE cust MODIFY id INT AUTO_INCREMENT;
ALTER TABLE cust AUTO_INCREMENT = 100;

-- cate table

CREATE TABLE cate (
	id INT,
	name VARCHAR(30) NOT NULL,
	upid INT
);

ALTER TABLE cate ADD CONSTRAINT PRIMARY KEY(id);
ALTER TABLE cate ADD CONSTRAINT UNIQUE(name);
ALTER TABLE cate ADD CONSTRAINT FOREIGN KEY (upid) REFERENCES cate(id);

-- product table

CREATE TABLE product (
	id INT,
	name VARCHAR(30) NOT NULL,
	price INT NOT NULL,
	cid	INT	NOT NULL,
	img	VARCHAR(20)	NOT NULL
);

ALTER TABLE product ADD CONSTRAINT PRIMARY KEY(id);
ALTER TABLE product ADD CONSTRAINT FOREIGN KEY (cid) REFERENCES cate(id);
ALTER TABLE product ADD CONSTRAINT CHECK(price > 0);
ALTER TABLE product MODIFY id INT AUTO_INCREMENT;
ALTER TABLE product AUTO_INCREMENT = 1000;

-- cart table

CREATE TABLE cart (
	id	INT,
	uid	INT	NOT NULL,
	pid	INT	NOT NULL,
	amount INT NOT NULL,
	date DATE NOT NULL
);

ALTER TABLE cart ADD CONSTRAINT PRIMARY KEY(id);
ALTER TABLE cart ADD CONSTRAINT FOREIGN KEY (uid) REFERENCES cust(id)
ON DELETE CASCADE
ON UPDATE CASCADE;
ALTER TABLE cart ADD CONSTRAINT FOREIGN KEY (pid) REFERENCES product(id)
ON DELETE CASCADE
ON UPDATE CASCADE;
ALTER TABLE cart ALTER COLUMN amount SET DEFAULT 1;
ALTER TABLE cart ADD CONSTRAINT CHECK(amount > 0);
ALTER TABLE cart MODIFY id INT AUTO_INCREMENT;
ALTER TABLE cart AUTO_INCREMENT = 0;

-- buy table

CREATE TABLE buy (
	id	INT,
	uid	INT	NOT NULL,
	payment	VARCHAR(10)	NOT NULL,
	price INT NOT NULL,
	status VARCHAR(10) NOT NULL,
	delivery VARCHAR(10) NOT NULL,
	addr VARCHAR(100) NOT NULL,
	tell CHAR(11) NOT NULL,
	recipt VARCHAR(20) NOT NULL,
	regdate	DATE
);

ALTER TABLE buy ADD CONSTRAINT PRIMARY KEY(id);
ALTER TABLE buy ADD CONSTRAINT FOREIGN KEY (uid) REFERENCES cust(id)
ON DELETE CASCADE
ON UPDATE CASCADE;
ALTER TABLE buy MODIFY id INT AUTO_INCREMENT;
ALTER TABLE buy AUTO_INCREMENT = 0;

-- spec table

CREATE TABLE spec (
	id INT,
	bid INT NOT NULL,
	pid INT	NOT NULL,
	amount INT NOT NULL
);

ALTER TABLE spec ADD CONSTRAINT PRIMARY KEY(id);
ALTER TABLE spec ADD CONSTRAINT FOREIGN KEY (bid) REFERENCES buy(id);
ALTER TABLE spec ADD CONSTRAINT FOREIGN KEY (pid) REFERENCES product(id);
ALTER TABLE spec MODIFY id INT AUTO_INCREMENT;
ALTER TABLE spec AUTO_INCREMENT = 0;


-- dlist table

CREATE TABLE dlist (
	id	INT,
	addr VARCHAR(100) NOT NULL,
	uid	INT	NOT NULL,
	tell CHAR(11) NOT NULL
);

ALTER TABLE dlist ADD CONSTRAINT PRIMARY KEY(id);
ALTER TABLE dlist ADD CONSTRAINT FOREIGN KEY (uid) REFERENCES cust(id);
ALTER TABLE dlist MODIFY id INT AUTO_INCREMENT;
ALTER TABLE dlist AUTO_INCREMENT = 0;
```

3. DML로 데이터를 입력, 삭제, 조회한다.

```
-- cust
SELECT * FROM cust;
INSERT INTO cust VALUES (NULL,'id01','pwd01','lee','Seoul','2020-05-03','01011112222');
INSERT INTO cust VALUES (NULL,'id02','pwd02','kim','Suwon','2021-06-01','01011113333');
INSERT INTO cust VALUES (NULL,'id03','pwd03','park','Incheon','2021-11-20','01011114444');
INSERT INTO cust VALUES (NULL,'id04','pwd04','lee','Busan','2021-06-01','01011115555');

-- dlist
SELECT * FROM dlist;
INSERT INTO dlist VALUES (NULL,'Seoul',100,'01011112222');
INSERT INTO dlist VALUES (NULL,'Busan',100,'01022222222');
INSERT INTO dlist VALUES (NULL,'Suwon',101,'01011113333');
INSERT INTO dlist VALUES (NULL,'Seoul',101,'01022223333');
INSERT INTO dlist VALUES (NULL,'Incheon',102,'01011114444');
INSERT INTO dlist VALUES (NULL,'Busan',103,'01011115555');

-- cate
SELECT * FROM cate;
INSERT INTO cate VALUES (10,'pants',NULL);
INSERT INTO cate VALUES (20,'shirts',NULL);
INSERT INTO cate VALUES (30,'skirt',NULL);

-- product
SELECT * FROM product;
INSERT INTO product VALUES (NULL,'short pants',10000,10,'img01');
INSERT INTO product VALUES (NULL,'long pants',15000,10,'img02');
INSERT INTO product VALUES (NULL,'short shirts',20000,20,'img03');
INSERT INTO product VALUES (NULL,'short skirt',10000,30,'img04');

-- cart
SELECT * FROM cart;
INSERT INTO cart VALUES (NULL,100,1001,5,SYSDATE());
INSERT INTO cart VALUES (NULL,100,1002,2,SYSDATE());
INSERT INTO cart VALUES (NULL,101,1001,2,SYSDATE());
INSERT INTO cart VALUES (NULL,101,1003,3,SYSDATE());
INSERT INTO cart VALUES (NULL,102,1000,3,SYSDATE());
INSERT INTO cart VALUES (NULL,103,1003,4,SYSDATE());

-- buy
SELECT * FROM buy;
INSERT INTO buy VALUES (NULL,100,'card',100000,'구매확정','배송전','Seoul','01011112222','lee',SYSDATE());
INSERT INTO buy VALUES (NULL,101,'cash',50000,'구매확정','배송완료','Suwon','01022223333','kim',SYSDATE());
INSERT INTO buy VALUES (NULL,102,'card',150000,'구매확정','배송전','Busan','01011114444','park',SYSDATE());

-- spec
SELECT * FROM spec;
INSERT INTO spec VALUES (NULL,1,1000,5);
INSERT INTO spec VALUES (NULL,1,1001,2);
INSERT INTO spec VALUES (NULL,2,1002,2);
```

4. JOIN을 걸어 여러 테이블을 한 번에 조회한다.
   - c.id를 그룹으로 묶어 고객의 아이디, 이름, 연락처 등을 조회한다.

```
SELECT c.id AS uid, c.lid, c.name, d.addr, d.tell, ct.pid, ct.amount, b.payment, b.price, b.status, b.delivery FROM cust c
LEFT OUTER JOIN dlist d ON c.id = d.uid
LEFT OUTER JOIN cart ct ON c.id = ct.uid
LEFT OUTER JOIN product p ON p.id = ct.pid
LEFT OUTER JOIN cate ca1 ON ca1.id = p.cid
LEFT OUTER JOIN cate ca2 ON ca1.id = ca2.upid
LEFT OUTER JOIN buy b ON c.id = b.uid
LEFT OUTER JOIN spec s ON s.bid = b.id
GROUP BY c.id;
```

5. 필요한 정보들을 묶어 VIEW를 생성한다.
   - 장바구니 정보를 조회할 수 있는 v_cart와 주문 내역을 조회할 수 있는 v_buy를 생성한다.

```
CREATE VIEW v_cart
AS
SELECT c.id AS uid, c.lid, c.name, ct.pid, ct.amount  FROM cart ct
LEFT OUTER JOIN cust c ON ct.uid = c.id
LEFT OUTER JOIN dlist d ON c.id = d.uid
LEFT OUTER JOIN product p ON p.id = ct.pid
LEFT OUTER JOIN cate ca1 ON ca1.id = p.cid
LEFT OUTER JOIN cate ca2 ON ca1.id = ca2.upid
LEFT OUTER JOIN buy b ON c.id = b.uid
LEFT OUTER JOIN spec s ON s.bid = b.id
GROUP BY c.id;

CREATE VIEW v_buy
AS
SELECT c.id AS uid, c.lid, c.name, d.addr, d.tell, ct.pid, ct.amount, b.payment, b.price, b.status, b.delivery FROM buy b
LEFT OUTER JOIN cust c ON b.uid = c.id
LEFT OUTER JOIN spec s ON s.bid = b.id
LEFT OUTER JOIN dlist d ON c.id = d.uid
LEFT OUTER JOIN cart ct ON c.id = ct.uid
LEFT OUTER JOIN product p ON p.id = ct.pid
LEFT OUTER JOIN cate ca1 ON ca1.id = p.cid
LEFT OUTER JOIN cate ca2 ON ca1.id = ca2.upid
GROUP BY c.id;
```

