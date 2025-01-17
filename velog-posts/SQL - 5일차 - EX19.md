<p>-- ex19_subquery.sql</p>
<p>/*</p>
<pre><code>Sub Query
- 하나의 SQL(SELECT, INSERT, UPDATE, DELETE)안에 또 다른 SQL(SELECT)을 사용하는 기술

Main Query
- 주목적. 바깥쪽 쿼리

Sub Query
- 메인 쿼리에서 일부 데이터 역할
- 데이터(값)을 넣을 수 있는 곳이며 모두 사용 가능</code></pre><p>*/</p>
<p>-- tblCountry, 인구수가 가장 많은 나라의 이름? 중국
select max(population) from tblCountry; -- 120660
select name from tblCountry where population = 120660; -- 중국</p>
<p>select max(population) from tblCountry;
select name from tblCountry where population = (select max(population) from tblCountry); 
--(select max(population) from tblCountry) = 120660</p>
<p>-- tblComedian. 몸무게가 가장 많이 나가는 사람?
select max(weight) from tblComedian; 
select * from tblComedian where weight = 129;</p>
<p>select * from tblComedian where weight = (select max(weight) from tblComedian);
select * from tblComedian where weight = (select min(weight) from tblComedian);</p>
<p>-- tblInsa. 평균 급여보다 많이 받는 직원은?
select * from tblInsa where basicpay &gt;= (select avg(basicpay) from tblInsa);</p>
<p>-- tblInsa. 홍길동 급여보다 작고, 김말자 급여보다 큰 직원들?
select * from tblInsa
    where basicpay between (select basicpay from tblInsa where name = '김말자')
    and (select basicpay from tblInsa where name = '홍길동')
        order by basicpay;</p>
<p>-- 서브쿼리를 잘하려면? 
-- : 어떤 질문이 서브쿼리? 어떤 질문 메인쿼리? 구분~~
-- 남자(키 166cm)의 여자친구 키?
select * from tblMen;
select * from tblWomen;</p>
<p>select name, height from tblWomen
    where couple = (select name from tblMen where height = 166);</p>
<p>/*</p>
<pre><code>서브쿼리 삽입 위치
1. 조건절(값으로 사용하겠다) &gt; 비교값으로 사용
    a. 반환값이 1행 1열 &gt; 단일값 반환 &gt; 상수 취급 &gt; 값 1개
    b. 반환값이 N행 1열 &gt; 다중값 반환 &gt; 열거형 비교 &gt; in 연산자 사용
    c. 반환값이 1행 N열 &gt; 다중값 반환 &gt; 그룹 비교 &gt; N컬럼 : N컬럼
    d. 반환값이 N행 N열 &gt; 다중값 반환 &gt; b + c &gt; in 연산자 + 그룹 비교</code></pre><p>*/</p>
<p>-- 급여가 260만원 이상 받는 직원이 &gt; 근무하는 부서 &gt; 직원들 명단?
-- ORA-01427: 단일 행 하위 질의에 2개 이상의 행이 리턴되었습니다.
--            single-row subquery returns more than one row
select * from tblInsa
    where buseo = (select buseo from tblInsa where basicpay &gt;= 2620000);</p>
<p>select * from tblInsa
    where buseo in ('기획부', '총무부');</p>
<p>select * from tblInsa
    where buseo in (select buseo from tblInsa where basicpay &gt;= 2620000);</p>
<p>-- '홍길동'과 같은 지역(city), 같은 부서(buseo)인 직원 명단? &gt; 서울, 기획부
select city, buseo from tblInsa where name = '홍길동';</p>
<p>select * from tblInsa
    where city = (select city from tblInsa where name = '홍길동')
        and buseo = (select buseo from tblInsa where name = '홍길동');
    -- where 1:1 and 1:1</p>
<p>select * from tblInsa
    where(city, buseo) = (select city, buseo from tblInsa where name = '홍길동');  </p>
<p>-- 급여가 260만원 이상 받은 직원과 같은 부서, 같은 지역 &gt; 명단?
select buseo, city from tblInsa where basicpay &gt;= 2600000;</p>
<p>select * from tblInsa
    where (buseo, city) in (select buseo, city from tblInsa where basicpay &gt;= 260000);</p>
<p>/*</p>
<pre><code>서브쿼리 삽입 위치
1. 조건절(값으로 사용하겠다) &gt; 비교값으로 사용
    a. 반환값이 1행 1열 &gt; 단일값 반환 &gt; 상수 취급 &gt; 값 1개
    b. 반환값이 N행 1열 &gt; 다중값 반환 &gt; 열거형 비교 &gt; in 연산자 사용
    c. 반환값이 1행 N열 &gt; 다중값 반환 &gt; 그룹 비교 &gt; N컬럼 : N컬럼
    d. 반환값이 N행 N열 &gt; 다중값 반환 &gt; b + c &gt; in 연산자 + 그룹 비교


2. 컬럼리스트
    - 반드시 결과값이 1행 1열이어야 한다. &gt; 스칼라 쿼리(원자값 반환)
    a. 정적 서브쿼리 &gt; 모든 행에 동일한 값을 반환 &gt; 쓸곳이 거의 없음;
    b. 상관 서브 쿼리(*****) &gt; 메인 쿼리의 일부 컬럼값을 서브 쿼리에서 사용</code></pre><p>*/</p>
<p>select
    name, buseo, basicpay,
    (select avg(basicpay) from tblInsa)
from tblInsa;</p>
<p>-- ORA-01427: 단일 행 하위 질의에 2개 이상의 행이 리턴되었습니다.
--            single-row subquery returns more than one row
select
    name, buseo, basicpay,
    (select jikwi from tblInsa)
from tblInsa;</p>
<p>-- ORA-00913: 값의 수가 너무 많습니다
select
    name, buseo, basicpay,
    (select jikwi, city from tblInsa where num = 1001)
from tblInsa;</p>
<p>select
    name, buseo, basicpay,
    (select round(avg (basicpay)) from tblInsa where buseo = a.buseo)
from tblInsa a;</p>
<p>select * from tblMen;
select * from tblWomen;</p>
<p>-- 남자(이름, 키, 몸무게) + 여자(이름, 키, 몸무게) &gt; 1개의 테이블
select 
    name height, weight, couple,
    (select height from tblWomen where name = tblMen.couple),
    (select weight from tblWomen where name = tblMen.couple)
from tblMen;</p>
<p>/*</p>
<pre><code>서브쿼리 삽입 위치
1. 조건절(값으로 사용하겠다) &gt; 비교값으로 사용
    a. 반환값이 1행 1열 &gt; 단일값 반환 &gt; 상수 취급 &gt; 값 1개
    b. 반환값이 N행 1열 &gt; 다중값 반환 &gt; 열거형 비교 &gt; in 연산자 사용
    c. 반환값이 1행 N열 &gt; 다중값 반환 &gt; 그룹 비교 &gt; N컬럼 : N컬럼
    d. 반환값이 N행 N열 &gt; 다중값 반환 &gt; b + c &gt; in 연산자 + 그룹 비교


2. 컬럼리스트
    - 반드시 결과값이 1행 1열이어야 한다. &gt; 스칼라 쿼리(원자값 반환)
    a. 정적 서브쿼리 &gt; 모든 행에 동일한 값을 반환 &gt; 쓸곳이 거의 없음;
    b. 상관 서브 쿼리(*****) &gt; 메인 쿼리의 일부 컬럼값을 서브 쿼리에서 사용


3. FROM절에서 사용
    - 서브쿼리의 결과 테이블을 또 하나의 테이블이라고 생각하고 메인 쿼리르 작성
    - 인라인 뷰(Inline View)</code></pre><p>*/</p>
<p>select * from (select name from tblInsa);</p>