<p>-- ex05_where.sql</p>
<p>/*</p>
<p>   [WITH <sub>]
    SELECT column_list
    FROM table_name
    [WHERE search_condition]
    [GROUP BY group_by_expression]
    [HAVING search_condition]
    [ORDER BY order_expression [ASC|DESC]];</p>
<pre><code>SELECT column_list &gt; 3. 컬럼 지정(보고 싶은 컬럼만 가져오기) &gt; Projection
FROM table_name &gt; 1.테이블 지정
WHERE search_condition &gt; 2. 레코드마다 질문!!! 조건 지정(보고 싶은 행만 가져오기) &gt; Selection

각 절의 순서(*****)
1. FROM
2. WHERE
3. SELECT

where절
- 레코드(행)을 검색한다.
- 원하는 레코드만 추출</code></pre><p>*/</p>
<p>select first_name, salary -- 컬럼을 거르는 역할
from employees
where salary &gt;= 10000; -- 레코드를 거르는 역할</p>
<p>select * from employees;</p>
<p>select first_name, salary 
    from employees
        where salary &lt;= 3000; </p>
<p>select first_name, salary 
    from employees
        where salary &gt; 3000 and salary &lt; 10000;</p>
<p>select first_name, salary 
    from employees
        where first_name = 'Diana';</p>
<p>select *
    from employees
        where job_id = 'IT_PROG' or job_id = 'ST_MAN';</p>
<p>select * from tabs;</p>
<p>select * from employees; -- 직원 정보
select * from departments; -- 부서 정보
select * from locations; -- 회사의 위치 정보
select * from countries; -- 국가 정보
select * from regions; -- 지역 정보(대륙)
select * from jobs; -- 직원의 직무 정보
select * from job_history; -- 직원의 전직 이력 정보</p>
<p>select name, population
    from tblCountry
        where continent = 'AS';</p>
<p>select name, population
    from tblCountry
        where continent &lt;&gt; 'AS';</p>
<p>-- tblComedian
select * from tblComedian;</p>
<p>-- 1. 몸무게가 60kg 이상이고, 키가 170cm 미만인 사람을 가져오시오.</p>
<pre><code>select first
from tblComedian
where weight &gt;= 60 and height &lt; 170;</code></pre><p>-- 2. 몸무게가 70kg 이하인 여자만 가져오시오.
    select first 
    from tblComedian
    where weight &lt;= 70 and gender = 'f';</p>
<p>-- tblInsa
select *from tblInsa;</p>
<p>-- 3. 부서가 '개발부'이고, 급여가 150만원 이상 받는 직원을 가져오시오.
    select name
    from tblInsa
    where buseo = '개발부' and basicpay &gt;= 1500000;</p>
<p>-- 4. 급여 + 수당을 합한 금액이 200만원 이상 받는 직원을 가져오시오. 
    select name 
    from tblInsa
    where basicpay + sudang &gt;= 2000000;</p>
<p>-- where절에서 사용 가능한 구문들..
 select *from tblInsa; -- 직원 테이블</p>
<p> -- 연산자
 select * from tblInsa
 where basicpay &gt;= 2000000;</p>
<p> select * from tblInsa
 where  buseo = '영업부' and basicpay &gt;= 2000000;</p>
<p>-- 급여 + 수당 &gt;= 200만원
select name, basicpay, sudang, basicpay + sudang
from tblInsa
where (basicpay + sudang) &gt;= 2000000;</p>
<p>/*</p>
<pre><code>오라클에는 옵티마이저라는 것이 있어서 btween이 느리지만 빠른 연산자로 변환해서 처리를 해주기 때문에 실제로는 느리지 않다.


between 연산자
- where절에서 사용 &gt; 조건으로 사용
- 컬럼명 between 최솟값 and 최댓값
- 범위 조건
- 가독성 향상
- 최솟값, 최댓값 포함O</code></pre><p>*/
select * from tblInsa where basicpay &gt;= 1000000 and basicpay &lt;= 1500000;
select * from tblInsa where basicpay &lt;= 1500000 and basicpay &gt;= 1000000;
select * from tblInsa where 1500000 &gt;= basicpay and basicpay &gt;= 1000000;
-- 같은 값
select * from tblInsa where basicpay between 1020000 and 1420000; </p>
<p>-- 비교 연산(연산자 + between)
-- 1. 숫자형
select *from tblInsa where basicpay &gt;= 1000000 and basicpay &lt;= 1500000;
select *from tblInsa where basicpay between 1000000 and 1500000;</p>
<p>--2. 문자형
select * from tblInsa where name &gt;= '이순신'; --문자코드 비교, name.compareTo(&quot;이순신&quot;)
select * from employees where first_name &gt;= 'J' and first_name &lt;= 'L';
select * from employees where first_name between 'J' and 'L';</p>
<p>--3. 날짜시간형
select * from tblInsa where ibsadate &gt;= '2010-01-01'; -- 날짜 리터럴
select * from tblInsa where ibsadate &gt;= '2010-01-01' and ibsadate &lt;= '2010-12-31';
select * from tblInsa where ibsadate between '2010-01-01' and '2010-12-31';</p>
<p>/*</p>
<pre><code>in 연산자
- where절에서 사용 &gt; 조건으로 사용
- 열거형 조건
- 컬럼명 in (값, 값 [, 값] x N)
- 가독성 향상</code></pre><p>*/</p>
<p>-- tblInsa. 부서 &gt; 개발부 + 총무부 + 인사부
select * from tblInsa
where buseo = '개발부' or buseo = '총무부' or buseo = '인사부';</p>
<p>select * from tblInsa
where buseo in ('개발부', '총무부', '인사부');</p>
<p>-- 서울 or 인천 + 과장 + 부장 + 급여(250~300)
select * from tblInsa
where city in ('서울', '인천')
and jikwi in ('과장', '부장')
and basicpay between 2500000 and 3000000;</p>
<p>/*</p>
<pre><code>like 
- where절에서 사용 &gt; 조건으로 사용
- 패턴 비교(패턴을 찾는 역할)
- 컬럼명 like '패턴 문자열'
- 정규 표현식 &gt; 간단한 버전

패턴 문자열의 구성 요소
1. _: 임의의 문자 1개
2. %; 임의의 문자 N개(0~무한대)</code></pre><p>*/</p>
<p>-- 김OO
select name from tblInsa;
select name from tblInsa where name like '김<strong>';
select name from tblInsa where name like '<em>길</em>';
select name from tblInsa where name like '</strong>수';
select name from tblInsa where name like '김%';</p>
<p>select *from employees where first_name like 'S_____'; --제한된 글자 
select *from employees where first_name like 'S%'; 
select *from employees where first_name like '%s'; -- 이름이 대문자 S가 없어서  소문자 s
select *from employees where first_name like '%s%';-- 중간에 s가 들어가는 사람 / 가장 많이쓰는 패턴</p>
<p>-- 여직원, 남직원
select * from tblInsa where ssn like '<strong>__</strong>-2______';
select * from tblInsa where ssn like '%-1%';</p>
<p>/*</p>
<pre><code>RDBMS에서의 null
- 컬럼값(셀)이 비어있는 상태
- null 상수 제공 &gt; null
- 대부분의 언어는 null은 연산의 대상이 될 수 없다.(******************************)

null 조건
- where절에서 사용
- 컬럼명 is null
- 컬럼명 is not null</code></pre><p>*/</p>
<p>select * from tblCountry;</p>
<p>-- 인구수가 미기재된 나라?
select * from tblCountry where population = null; --(x)
select * from tblCountry where population is null;</p>
<p>-- 인구수가 개재된 나라?
select * from tblCountry where population &lt;&gt; null; --(x)
select * from tblCountry where not population is null;
select * from tblCountry where population is not null;</p>
<p>-- 연락처가 없는 직원?
select * from tblInsa where tel is null; </p>
<p>-- 실행 완료한 일?
select * from tblTodo where completedate is not null; 
-- 실행 미완료할 일?
select * from tblTodo where completedate is null;</p>
<p>-- 도서관 &gt; 대여 테이블(컬럼: 대여날짜, 반납날짜)
-- 아직 반납을 안한 사람? 
select * from 대여 where 반납날짜 is null;</p>
<p>-- 반납을 완료한 사람?
select * from 대여 where 반납날짜 is not null;</p>