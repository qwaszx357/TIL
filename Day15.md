# Day15 2022-04-29

## AI 플랫폼을 활용한 웹서비스 개발

### 1. SQL

1. 테이블 생성

```
-- TABLE 생성
-- DROP TABLE IF EXISTS: 테이블이 이미 존재하면 삭제하고 다시 만들어라.
-- AUTO_INCREMENT : 자동으로 숫자가 입력된다.
-- NOT NULL : null 값이 있으면 안된다.
-- NOT UNIQUE : 중복 값이 들어올 수 없다.
DROP TABLE IF EXISTS itemtbl;
CREATE TABLE itemtbl(
	id INT PRIMARY KEY AUTO_INCREMENT,
	name VARCHAR(20) NOT NULL UNIQUE,
	price INT NOT NULL,
	regdate DATE
);

-- ALTER TABLE : 테이블 안에 무언가를 변경할 때 사용한다.
ALTER TABLE itemtbl AUTO_INCREMENT = 1000;
```

2. SELECT 구문

``` 
-- GROUP BY : 그룹별로 묶는다. SUM, AVG, ROUND 그룹함수와 함께 사용한다. 
-- HAVING : WHERE과 비슷한 개념이지만, 집계 함수에 대한 조건을 제한한다.
-- 회원별 구매 금액이 100보다 큰 고객을 구하시오
SELECT userID, ROUND(AVG(price * amount),1) AS pavg FROM buytbl
GROUP BY userID
HAVING pavg > 100
ORDER BY pavg DESC;

-- COUNT : 수를 셀 때 사용한다. 
-- DISTINCT : 중복 값을 제거한다.
SELECT COUNT(DISTINCT(userID)) FROM buytbl; 

-- grounpName 별 구매 고개의 수를 구하시오
-- DISTINCT 없으면 중복된 고객이 나온다.
SELECT groupName, COUNT(DISTINCT(userID)) FROM buytbl
GROUP BY groupName;

-- 회원들의 평균 키보다 큰 회원을 조회 하시오
SELECT * FROM usertbl
WHERE height > (SELECT AVG(height) FROM usertbl);

-- 회원 중 폰을 가지고 있는 회원의 수를 구하시오
-- COUNT는 null값은 세지 않는다.
SELECT COUNT(mobile1) FROM usertbl;

-- WITH ROLLUP : 중간 합계를 출력한다.
SELECT num, groupName, SUM(price * amount) FROM buytbl
GROUP BY groupName, num
WITH ROLLUP;

-- WITH절 : 가상의 테이블을 만들어 구문을 단순화한다.
-- 각 지역별 가장 키가 큰 키들의 평균을 구하시오
WITH temp(addr, max)
AS
(SELECT addr, MAX(height) FROM usertbl
GROUP BY addr)
SELECT AVG(max) FROM temp;

-- 위와 같음. 여기서 나온 결과를 테이블로 사용하겠다는 말.
SELECT AVG(a.hmax) FROM
(SELECT addr, MAX(height) AS hmax FROM usertbl
GROUP BY addr) a;

-- CONCAT : 문자와 문자를 더할 때 사용한다.
SELECT CONCAT(prodName, ' | ', groupName) FROM buytbl;

-- IF : 수식의 참과 거짓에 따라 다른 결과를 나타낸다.
SELECT userID, price * amount AS tt, IF (price * amount > 500, 'Hight', 'Low') AS level FROM buytbl;

-- IFNULL : null 일 경우 다른 문자로 대체한다.
SELECT prodName, IFNULL(groupName, 'None') FROM buytbl;

-- CASE ~ WHEN ~ ELSE ~ END : 조건에 따라 다른 결과를 출력하는 내장함수이다.
SELECT userID, amount, 
CASE
	WHEN amount >= 1 AND amount < 2 THEN 'C'
    WHEN amount >= 2 AND amount < 4 THEN 'B'
    WHEN amount >= 4 AND amount < 6 THEN 'A'
    ELSE 'None'
END AS level
FROM buytbl;

-- SUBSTRING : 원하는 곳부터 원하는 길이만큼의 문자를 반환한다.
SELECT SUBSTRING('대한민국만세', 3, 2);

-- ADDDATE : 그 후의 날짜를 계산한다.
-- SUbDATE : 그 전의 날짜를 계산한다.
SELECT mDate, ADDDATE(mDate, INTERVAL 30 DAY),
	SUbDATE(mDate, INTERVAL 30 DAY) 
FROM usertbl;

-- DATEDIFF : 그 날부터 지금까지 몇일이 지났는지 계산한다.
SELECT mDate, DATEDIFF(NOW(), mDate) FROM usertbl;

-- PERIOD_DIFF : 몇 월이 지났는지 계산한다. 
SELECT mDate,
PERIOD_DIFF(DATE_FORMAT(NOW(), '%Y%m'), DATE_FORMAT(mDate, '%Y%m'))
FROM usertbl;
```

### 2. Workshop

1. 개인 workshop 1

```
-- 1. 부서별, 직급별 연봉 평균을 구하시오
SELECT deptno, titleno, AVG(salary) AS asalary from emp
GROUP BY deptno, titleno;

-- 2. 입사 년도 별 월급의 평균을 구하시오
SELECT date_format(hdate,'%Y') AS ydate, AVG(salary) AS asalary FROM emp
GROUP BY date_format(hdate, '%Y');

-- 3. 부서 별 입사 월을 기준으로 연봉의 합을 구하시오
SELECT deptno, date_format(hdate, '%m') AS mdate, SUM(salary) AS ssalary FROM emp
GROUP BY deptno, mdate
ORDER BY deptno ASC;

-- 4. 이영업이 속한 부서의 연봉의 평균을 구하시오
SELECT deptno, AVG(salary) AS asalary FROM emp
WHERE deptno = (SELECT deptno FROM emp
WHERE empname = '이영업');

-- 5. 홍영자 직급과 같은 직원들의 연봉 평균보다 많이 받는 직원을 구하시오
SELECT * FROM emp
WHERE salary > (SELECT AVG(salary) FROM emp
WHERE titleno = (SELECT titleno FROM emp
WHERE empname = '홍영자'));

-- 6. 회사내 메니져는 총 몇명인지 구하시오
SELECT COUNT(DISTINCT(manager)) FROM emp;

-- 7. 2000-01-01 부터 2002-12-31일까지 입사한 직원들의 연봉 평균을 구하시오
SELECT AVG(salary) AS asalary FROM emp
WHERE hdate BETWEEN '2000-01-01' AND '2002-12-31';
```

2. 개인 workshop 2

```
-- 1. 오늘 날짜 기준으로 입사 일부터 며칠이 지났고 몇달이 지났는지 출력 하시오
SELECT hdate, DATEDIFF(NOW(), hdate) AS hday,
PERIOD_DIFF(DATE_FORMAT(NOW(), '%Y%m'), DATE_FORMAT(hdate, '%Y%m')) AS hmonth
FROM emp;

-- 2. 직원들 연봉이 4000이상이면 high, 2500 이상이면 middle, 2500이하면 low 출력
SELECT empname, salary, 
CASE
	WHEN salary >= 4000 THEN 'high'
    WHEN salary >= 2500 THEN 'middle'
    ELSE 'low'
END AS level
FROM emp;

-- 3. 부서 별 연봉 평균의 합을 구하시오
WITH temp(asalary)
AS
(SELECT AVG(salary) AS asalary FROM emp
GROUP BY deptno)
SELECT SUM(asalary) AS sasalary FROM temp;

-- 4. 부서 별 오늘 날짜 기준으로 입사일 평균을 구하시오
SELECT deptno, AVG(DATEDIFF(NOW(), hdate)) AS ahdate FROM emp
GROUP BY deptno;

-- 5. 이말숙 직원과 같은 해에 입사는 직원을 조회 하시오
SELECT * FROM emp
WHERE YEAR(hdate) =
(SELECT YEAR(hdate) FROM emp
WHERE empname = '이말숙');

-- 6. 부서별 최고 임금을 받는 직원의 평균을 구하고 그 평균 보다 많이 받는 직원을 조회 하시오
SELECT * FROM emp
WHERE salary >
(SELECT AVG(a.msalary) FROM
(SELECT deptno, MAX(salary) AS msalary FROM emp
GROUP BY deptno) a);
```

3. Team workshop
   - 4개의 문제를 만들고 풀이를 작성한다.

```
-- 1. 매니저가 있는 사람 중 매니저가 같은 직원들의 연봉 평균을 구하시오
SELECT manager, ROUND(AVG(salary),0) AS asalary FROM emp
WHERE manager IS NOT NULL
GROUP BY manager;

-- 2. 각 직급에서 임금이 가장 높은 사람을 직급별 대표자로 선정하여 대표자의 정보를 조회하시오.
-- 최고 임금자가 여러명일 경우 모두 대표자로 선정한다.
SELECT * FROM emp
WHERE salary IN (SELECT a.max FROM
(SELECT titleno, MAX(salary) AS max FROM emp
GROUP BY titleno) a);

-- 3. 평균 근속일수가 가장 높은 부서와 그 부서의 근속일수 평균을 구하시오
SELECT deptno, MAX(a.ahdate) AS mhdate FROM
(SELECT deptno, DATEDIFF(NOW(), hdate) AS ahdate FROM emp
GROUP BY deptno) a;

-- 4. 봄 (3월, 4월, 5월)에 입사한 사람의 연봉평균을 구하시오
SELECT AVG(salary) AS asalary FROM emp
WHERE salary = 
(SELECT salary FROM emp
WHERE DATE_FORMAT(hdate, '%m') IN ('03','04','05'));
```

