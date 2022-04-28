# Day14 2022-04-28

## AI 플랫폼을 활용한 웹서비스 개발

### 1. SQL

1. 시스템 구축
   - MySQL
2. DDL, DML
   - DDL : CREATE(생성), DROP(삭제), ALTER(제약사항)
   - DML : SELECT, INSERT, DELETE, UPDATE
3. ERD
   - 요구사항 정의
   - 테이블을 설계하고 구축

> 설계한 뒤 DDL로 구축하고 DML로 in, out한다.

### 2. import하기

- cmd 명령 프롬프트 창에서 mysql에 접속한다.

``` 
c:\backup>mysql -u admin1 -p
Enter password: ******
```

- sqldb라는 데이터 베이스 생성한다.

```
mysql> CREATE SCHEMA sqldb;
```

- sqlDB.sql 파일을 import한다.

```
mysql> USE sqldb;
mysql> SOURCE sqlDB.sql;
```

### 3. SELECT의 구문

```
-- AS : 세금의 컬럼명을 fee로 바꾼다.
SELECT empno,empname,salary,salary * 0.2 AS fee FROM emp;

-- AND : 두개의 조건을 모두 충족하는 정보만 출력한다.
-- IS NULL: 값이 없는 정보를 출력한다. 
-- IS NOT NULL : 값이 있는 정보를 출력한다.
SELECT * FROM usertbl
WHERE addr = '서울'
AND birthYear > 1970
AND mobile1 IS NOT NULL;

-- OR : 둘 중 하나의 조건에 해당하는 정보를 출력한다.
SELECT * FROM usertbl
WHERE height > 170 OR birthYear < 1970;

-- = : 같은 값의 정보를 출력한다. (==는 사용할 수 없다.)
SELECT * FROM usertbl
WHERE height = 182;

-- >=, <= : 연산에 부합하는 정보를 춝력한다.
SELECT * FROM usertbl
WHERE height >= 180 AND height <= 183;

-- BETWEEN : ~ 사이에 해당하는 정보를 출력한다.
SELECT * FROM usertbl
WHERE height BETWEEN 180 AND 183;

-- IN : () 안에 해당하는 모든 정보를 출력한다.
SELECT * FROM usertbl
WHERE height IN (182, 170, 172);

-- date_format() : 날짜에서 특정 값만 비교할 때 사용한다.
-- %Y(년), %m(월), %d(일), %H(시), %i(분), %s(초)
SELECT * FROM usertbl
WHERE date_format(mDate,'%Y') < '2010';

-- 가입 년도가 2005년과 2008년 사이의 회원을 조회 하시오
SELECT * FROM usertbl
WHERE date_format(mDate,'%Y') BETWEEN '2005' AND '2008';

-- 가입 월이 04월, 07월인 회원을 조회 하시오
SELECT * FROM usertbl
WHERE date_format(mDate,'%m') IN ('04','07');

-- LIKE : 조건에 해당하는 정보를 출력한다.
-- 이름 중에 '김'이 들어가는 정보를 조회한다. 이름 글자 수는 상관 없다.
SELECT * FROM usertbl
WHERE name LIKE '%김%';

-- 이름은 3글자이고 가운데 글자에 '종'이 들어가는 정보를 출력한다.
SELECT * FROM usertbl
WHERE name LIKE '_종_';

-- 윤종신 회원과 같은 지역의 회원을 조회 하시오
SELECT * FROM usertbl
WHERE addr = (SELECT addr FROM usertbl
WHERE name = '윤종신');

-- 윤종신 회원보다 키가 큰 회원을 조회 하시오
SELECT * FROM usertbl
WHERE height > (SELECT height FROM usertbl
WHERE name = '윤종신');

-- 경남 지역의 회원들 키와 동일한 회원들을 조회 하시오
SELECT * FROM usertbl
WHERE height  In (SELECT height FROM usertbl
WHERE addr = '경남');

-- ORDER BY : 정렬하여 출력한다. ASC(오름차순), DESC(내림차순)
SELECT * FROM usertbl
WHERE addr = '서울'
AND birthYear < 1980
ORDER BY name;

-- 두개 같음
SELECT * FROM usertbl
ORDER BY height DESC, name ASC;
SELECT * FROM usertbl
ORDER BY 3 DESC, 2 ASC;

-- DISTINCT : 중복되는 값을 제거하고 출력한다.
SELECT DISTINCT addr FROM usertbl;

-- LIMIT : 개수를 제한하여 출력한다.
-- 2번째 값부터 5개의 개수를 출력한다.
SELECT * FROM usertbl
ORDER BY height
LIMIT 2,5;
```