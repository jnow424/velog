<p>-- ex08_aggregation_function.sql</p>
<p>/*</p>
<pre><code>메서드
- 함수, 프로시저 등...
- 클래스내에서 선언된  함수


함수, Function
1. 내장 함수 (Built-in Function)
2. 사용자 정의 함수 (User Function) &gt; 표준 SQL(X), PL/SQL(O)

집계 함수, Aggregation Function
- Java STream: list.stream() &gt; count(), sum(), max(), min(), average()
- 쉬운 편 &gt; 이후에 수업 &gt; 어려움;; / 쉬울 때 잘해둘 것

1. count()
2. sum()
3. avg()
4. max()
5. min()

1. count()
- 무엇의 개수? &gt; 결과 테이블의 레코드 수를 반환한다.
- number count(컬럼명)
- null 값은 제외한다.</code></pre><p>*/</p>
<p>select * from tblComedian;
select count(*) from tblComedian; -- 10개
select count(first) from tblComedian;
select count(last) from tblComedian;
select count(gender) from tblComedian;
select count(weight) from tblComedian;
select count(height) from tblComedian;
select count(nick) from tblComedian;</p>
<p>select * from tblComedian where gender = 'm';
select count(<em>) from tblComedian where gender = 'm'; -- 8
select count(</em>) from tblComedian where gender = 'f'; -- 2</p>
<p>select * from tblCountry; -- 14
select count(name) from tblCountry; -- 14
select count(capital) from tblCountry; -- 14
select count(population) from tblCountry; -- 13 / null 값이 없음
select count(continent) from tblCountry; -- 14
select count(area) from tblCountry; -- 14</p>
<p>select count(<em>) from tblInsa; -- 전체직원수 60
select count(tel) from tblInsa; -- 연락처가 있는 직원수 57
select count(</em>) - count(tel) from tblInsa; -- 연락처가 없는 직원수 3</p>
<p>select count(<em>), count(tel), count(</em>) - count(tel) from tblInsa; -- 연락처가 없는 직원수 3</p>
<p>select count(<em>) from tblInsa where tel is not null; --57
select count(</em>) from tblInsa where tel is null; --3</p>
<p>-- tblComedian. 남자수? 여자수?
select * from tblComedian;</p>
<p>select count(<em>) from tblComedian where gender = 'm'; -- 8
select count(</em>) from tblComedian where gender = 'f'; -- 2</p>
<p>-- tblComedian. 남자수? 여자수? &gt; 1개의 테이블 &gt; 자주 사용되는 패턴(<strong>**</strong>)
select 
    count(case 
        when gender = 'm' then 1
    end),
    count(case 
        when gender = 'f' then 1
    end)
from tblComedian;</p>
<p>-- tblInsa. 기획부 몇명? 총무부 몇명? 개발부 몇명? 1개의 테이블
select count(<em>) from tblInsa where buseo = '기획부'; -- 7
select count(</em>) from tblInsa where buseo = '총무부'; -- 7
select count(*) from tblInsa where buseo = '개발부'; -- 14</p>
<p>select 
    count(case
        when buseo = '기획부' then '0'
    end) as 기획부, </p>
<pre><code>count(case
    when buseo = '총무부' then '0'
end) as 총무부,

count(case
    when buseo = '개발부' then '0'
end) as 개발부</code></pre><p>from tblInsa;</p>
<p>select 
    count(case
        when buseo = '기획부' then '0'
    end) as 기획부, </p>
<pre><code>count(case
    when buseo = '총무부' then '0'
end) as 총무부,

count(case
    when buseo = '개발부' then '0'
end) as 개발부,

count(case
    when buseo not in ('기획부', '총무부', '개발부') then '0'
end) as 나머지부서,
count(*) as 총직원수</code></pre><p>from tblInsa;</p>
<p>/*</p>
<pre><code>2. sum()
- 해당 컬럼값의 합을 구한다.
- number sum(컬럼명)
- 숫자형에만 적용 가능</code></pre><p>*/</p>
<p>select * from tblCountry;
select count(area) from tblCountry; -- 14 레코드 개수
select count(name) from tblCountry; -- 14 레코드 개수
select count(*) from tblCountry; -- 14</p>
<p>select sum(area) from tblCountry; -- 컬럼값의 합
select sum(name) from tblCountry; -- ORA-01722: 수치가 부적합합니다
select sum(*) from tblCountry;
select sum(ibsadate) from tblCountry;</p>
<p>select 
    sum(basicpay) as &quot;총 지출 급여&quot;,
    sum(sudang) as &quot;총 지출 수당&quot;,
    sum(basicpay) + sum(sudang) as &quot;총 지출&quot;,
    sum(basicpay + sudang) as &quot;총 지출&quot;</p>
<p>from tblInsa;</p>
<p>/*</p>
<pre><code>3. avg()
- 해당 컬럼값의 평균값을 구한다.
- number avg(컬럼명)
- 숫자형에만 적용 가능</code></pre><p>*/</p>
<p>-- tblInsa. 평균 급여
select sum(basicpay) / 60 from tblInsa; -- 1556526
select sum(basicpay) / count(*) from tblInsa; -- 1556526
select avg(basicpay) from tblInsa; -- 1556526</p>
<p>--tblCountry. 14개국의 평균 인구수?
select sum(population) / 14 from tblCountry; -- 202652 / 14 = 14475
select sum(population) / count(*) from tblCountry; -- 14475
select avg(population) from tblCountry; -- 15588 // 조심!!!</p>
<p>select count(<em>) from tblcountry; -- 14
select sum(population) / count(</em>) from tblCountry;
select sum(population) / count(population) from tblCountry; -- 15588</p>
<p>-- 상황에 맞게 사용
-- 회사 &gt; 성과급 지급 &gt; 출처 &gt; 1팀
-- 1. 균등 지급 &gt; 총성과금 &gt; 모든 직원수 = sum() / count(*)
-- 2. 차등 지급 &gt; 총성과금 &gt; 1팀 직원수 = sum() / count(1팀) = avg()</p>
<p>/*</p>
<pre><code>4. max()
    - 최댓값 반환
    - object max(컬럼명)

5. min()
    - 최솟값 반환
    - object min(컬럼명)</code></pre><p>*/</p>
<p>select max(basicpay), min(basicpay) from tblInsa; -- 숫자형
select max(name), min(name) from tblInsa; -- 문자형
select max(ibsadate), min(ibsadate) from tblInsa; -- 날짜형</p>
<p>select
    count(*) as 직원수,
    sum(basicpay) as 총급여합,
    avg(basicpay) as 평균급여,
    max(basicpay) as 최고급여,
    min(basicpay) as 최저급여
from tblInsa;</p>
<p>-- 집계함수 사용 시 주의점!!!!!! &gt; 집계 함수의 특성(이해!!!!)</p>
<p>-- ORA-00937: 단일 그룹의 그룹 함수가 아닙니다. not a single-group group function
-- 컬럼 리스트(SELECT절)에서는 일반 컬럼과 집계 함수를 동시에 사용할 수 없다.
-- 서브쿼리로 해결</p>
<p>-- 요구사항] 직원들 이름과 총직원수를 가져오시오.
select name, count(<em>) from tblInsa;
select name from tblInsa;
select count(</em>) from tblInsa;</p>
<p>-- 요구사항] 평균 급여보다 더 많이 받는 직원들?
select avg(basicpay) from tblInsa; -- 1556526</p>
<p>-- ORA-00934: 그룹 함수는 허가되지 않습니다. group function is not allowed here
-- WHERE절에는 절대로 집계 함수를 사용할 수 없다.
-- WHERE절 &gt; 개인(레코드)에 대한 조건절 / 한사람 한사람 확인 / 한사람씩 확인해서 집계함수를 사용이 안됨 / 우회는 할 수 있음
-- 서브쿼리로 해결</p>
<p>select * from tblInsa where basicpay &gt;= 1556526; -- 27
select * from tblInsa where basicpay &gt;= avg(basicpay); -- 27</p>