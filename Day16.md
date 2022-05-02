



# Day16 2022-05-02

## AI 플랫폼을 활용한 웹서비스 개발

### 1. JSON 데이터

- 현대의 웹과 모바일 응용 프로그램 등과 데이터를 교환하기 위한 개방형 표준 포맷을 말한다.
- 속성과 값으로 쌍을 아루며 구성되어 있다.
- 실행 방법

```
SELECT JSON_OBJECT('empno', empno, 'empname', empname)
AS JSONDATA
FROM emp;
```

### 2. 조인 (JOIN)

- 두 개 이상의 테이블을 서로 묶어서 하나의 결과 집합으로 만들어 내는 것이다.

1. INNER JOIN (내부 조인)
   - 가장 많이 사용하는 조인이다.
   - 조인하려는 양쪽 테이블 모두 내용이 있는 것만 조인되는 방식이다.
   - 실행 방법

```
SELECT e.empno, e.empname, d.deptname, t.titlename FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
INNER JOIN title t ON e.titleno = t.titleno;
```

2. OUTER JOIN (외부 조인)
   - 양쪽에 있는 테이블 뿐만 아니라, 한쪽에만 내용이 있어도 그 결과가 표시되는 조인 방식이다.
   - OUTER JOIN 은 어느 테이블을 기준으로 할 것인지 쓴다. (LEFT, RIGHT,FULL)
   - 실행 방법

```
SELECT e.empname, d.deptname, t.titlename FROM emp e
LEFT OUTER JOIN dept d ON e.deptno = d.deptno
LEFT OUTER JOIN title t ON e.titleno = t.titleno;
```

3. CROSS JOIN (상호 조인)
   - 한 쪽 테이블의 행이 다른 테이블의 모든 행과 조인되는 방식이다.
   - 실행 방법

```
SELECT * FROM emp e
CROSS JOIN title t;
```

4. SELF JOIN (자체 조인)
   - 자기 자신과 자기 자신이 조인하는 방식이다.
   - 실행 방법

```
SELECT e1.empname, e2.empname FROM emp e1
SLEF JOIN emp e2 ON e1.manager = e2.empno;
```

5. UNION / UNION ALL
   - UNION - 중복된 열은 제거되고 데이터가 정렬된다.
   - UNION ALL - 중복된 열까지 모두 출력된다.

### 3. Workshop

```
-- 1. 직원정보를 출력 한다. 직원의 연봉 정보와 연봉의 세금 정보를 같이 출력 한다. 세금은 10%
SELECT empname, empno, salary, (salary * 0.1) AS fee FROM emp;

-- 2. 직원정보 중 2001 이전에 입사 하였고 월급이 4000만원 미만인 직원을 조회
SELECT * FROM emp
WHERE hdate < '2001-01-01'
AND salary < 4000;

-- 3. manager가 있는 직원 중 이름에 '자' 가 들어가 있는 직원정보 조회
SELECT * FROM emp
WHERE manager IS NOT NULL
AND empname LIKE '%자%';

-- 4. 월급이 2000미만은 '하' 4000미만은 '중' 4000이상은 '고' 를 출력
SELECT empname, salary,
CASE
	WHEN salary >= 4000 THEN '고'
    WHEN salary < 4000 THEN '중'
    WHEN salary < 2000 THEN '저'
END AS level
FROM emp;

-- 5. 부서 별 월급의 평균을 구하시오 단, 평균이 3000 이상인 부서만 출력
SELECT e.deptno, d.deptname, AVG(salary) AS asalary FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
GROUP BY d.deptno
HAVING AVG(salary) >= 3000;

-- 6. 부서 별 대리와 사원 연봉의 평균을 구하시오 단, 평균이 2500 이상인 부서만 출력
SELECT e.deptno, d.deptname, AVG(salary) AS asalary FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
INNER JOIN title t ON e.titleno = t.titleno
WHERE t.titlename IN ('대리','사원')
GROUP BY d.deptno
HAVING AVG(salary) >= 2500;
 
-- 7. 2000년 부터 2002년에 입사는 직원들의 월급의 평균을 구하시오
SELECT AVG(e.salary) AS aslary FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
INNER JOIN title t ON e.titleno = t.titleno
WHERE DATE_FORMAT(hdate,'%Y') BETWEEN '2000' AND '2002';

-- 8. 부서 별 월급의 합의 ranking을 1위부터 조회 하시오
SELECT a.deptno, a.deptname, a.ssum FROM
(SELECT e.deptno, d.deptname, SUM(e.salary) AS ssum FROM emp e
LEFT OUTER JOIN dept d ON e.deptno = d.deptno
LEFT OUTER JOIN title t ON e.titleno = t.titleno
GROUP BY e.deptno) a
ORDER BY a.ssum DESC;

-- 9. 서울에서 근무하는 직원들을 조회 하시오
SELECT * FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
INNER JOIN title t ON e.titleno = t.titleno
WHERE d.deptloc = '서울';

-- 10. 이영자가 속한 부서의 직원들을 조회 하시오
SELECT * FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
INNER JOIN title t ON e.titleno = t.titleno
WHERE e.deptno = 
(SELECT e.deptno FROM emp e
WHERE e.empname = '이영자');

-- 11. 김강국의 타이틀과 같은 직원들을 조회 하시오
SELECT * FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
INNER JOIN title t ON e.titleno = t.titleno
WHERE e.titleno = 
(SELECT e.titleno FROM emp e
WHERE e.empname = '김강국');


-- 12. 2000 년 이후 입사 한 사원들의 정보를 출력. 사번, 이름, 타이틀, 부서, 지역
SELECT e.empno, e.empname, t.titlename, d.deptname, d.deptloc FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
INNER JOIN title t ON e.titleno = t.titleno
WHERE DATE_FORMAt(e.hdate,'%Y') >= '2000';

-- 13. 부서 이름 별 월급의 평균을 구하시오 단, 평균이 3000 이상인 부서만 출력
SELECT d.deptname, AVG(salary) AS asalary FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
INNER JOIN title t ON e.titleno = t.titleno
GROUP BY d.deptname
HAVING AVG(salary) >= 3000;

-- 14.  대구 지역의 직원 들의 평균 연봉을 구하시오
SELECT d.deptname, d.deptloc, AVG(salary) AS asalary FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
INNER JOIN title t ON e.titleno = t.titleno
WHERE d.deptloc = '대구';

-- 15. 홍영자 직원와 같은 부서 직원들의 근무 월수를 구하시오 
SELECT e.empname, d.deptname, PERIOD_DIFF(DATE_FORMAT(NOW(), '%Y%m'), DATE_FORMAT(e.hdate, '%Y%m')) AS hmonth
FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
INNER JOIN title t ON e.titleno = t.titleno
WHERE e.deptno = 
(SELECT e.deptno FROM emp e
WHERE e.empname = '홍영자');

-- 16. 입사 년수가 가장 많은 직원 순으로 정렬 하여 순위를 정한다. 이름, 부서명, 직위, 년수
SELECT e.empname, d.deptname, t.titlename, (YEAR(NOW()) - YEAR(hdate)) AS ydate FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
INNER JOIN title t ON e.titleno = t.titleno
ORDER BY ydate DESC;
```

