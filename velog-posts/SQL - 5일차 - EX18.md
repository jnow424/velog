<p>-- ex18_groupby.sql</p>
<p>/*</p>
<pre><code>[WITH &lt;Sub Query&gt;]
SELECT column_list
FROM table_name
[WHERE search_condition]
[GROUP BY group_by_expression] *
[HAVING search_condition] *
[ORDER BY order_expression [ASC|DESC]]; 

SELECT column_list &gt; 5. 컬럼 지정(보고 싶은 컬럼만 가져오기) &gt; Projection
FROM table_name &gt; 1.테이블 지정
WHERE search_condition &gt; 2. 레코드마다 질문!!! 조건 지정(보고 싶은 행만 가져오기) &gt; Selection
GROUP BY group_by_expression &gt; 3. 그룹을 나눈다. 
HAVING search_condition &gt; 4. 그룹에 대한 조건
ORDER BY order_expression [ASC|DESC] &gt; 6. 결과셋의 레코드 순서를 정한다.

각 절의 순서(*****)
1. FROM
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. ORDER BY

group by절
- 특정 기준으로 레코드들을 그룹으로 나눈다.(수단)
&gt; 각각의 그룹을 대상으로 집계 함수를 실행한다.(목적!!!!)

having절
- 그룹에 대한 조건
- having을 만족하는 그룹만 남는다. 불만족하면 버린다.

비교
- from 테이블 where 조건 &gt; 개인(객체, 레코드, 튜플, 행)에 대한 조건
- group by 그룹 having 조건 &gt; 그룹(7개) &gt; 그룹에 대한 조건 / tblInsa.buseo 기준</code></pre><p>*/</p>
<p>-- tblInsa. 부서별 평균 급여?
select * from tblInsa;</p>
<p>select round(avg(basicpay)) from tblInsa; -- 155만원 . 전체 직원</p>
<p>select distinct buseo from tblInsa; -- 7개 부서</p>
<p>select round(avg(basicpay)) from tblInsa where buseo = '기획부'; -- 1,855,714
select round(avg(basicpay)) from tblInsa where buseo = '총무부'; -- 1,714,857
select round(avg(basicpay)) from tblInsa where buseo = '개발부'; -- 1,387,857
select round(avg(basicpay)) from tblInsa where buseo = '영업부'; -- 1,601,513
select round(avg(basicpay)) from tblInsa where buseo = '홍보부'; -- 1,451,833
select round(avg(basicpay)) from tblInsa where buseo = '인사부'; -- 1,533,000
select round(avg(basicpay)) from tblInsa where buseo = '자재부'; -- 1,416,733</p>
<p>-- group by 사용하면 &gt; select절에는 반드시 집계함수만 사용이 가능하다. 
select 
    buseo,
    round(avg(basicpay)) as &quot;부서별 평균 급여&quot;,
    count(*) as &quot;부서별 인원수&quot;,
    sum(basicpay) as &quot;부서별 총 지급액&quot;,
    max(basicpay) as &quot;부서별 최고 급여&quot;,
    min(basicpay) as &quot;부서별 최저 급여&quot;
    --name -- 이름은 개인 값이기 때문 사용 x
from tblInsa
    group by buseo;</p>
<p>-- tblComedian. 남자수? 여자수?
select
    count(decode(gender, 'm', 1)) as 남자수,
    count(decode(gender, 'f', 1)) as 여자수
from tblComedian;</p>
<p>select
    gender,
    count(*)
from tblComedian
    group by gender;</p>
<p>select
    jikwi,
    count(*) 
from tblInsa
    group by jikwi;</p>
<p>select
    city,
    count(*) 
from tblInsa
    group by city;</p>
<p>select
    buseo,
    count(<em>) 
from tblInsa
    group by buseo
        order by count(</em>) desc;</p>
<p>-- 다중 그룹
select 
    buseo, jikwi, count(*), city</p>
<p>from tblInsa
    group by buseo, jikwi, city
        order by buseo asc;</p>
<p>-- 급여별 그룹
-- 100만원 이하
-- 100만원 ~ 200만원
-- 200만원 이상
select
    basicpay,
    count(*)
from tblInsa
    group by basicpay;</p>
<p>select
    --basicpay,
    (floor(basicpay / 1000000) + 1) * 100 || '만원 이하' as money, -- 소수이하 버림
    count(*)
from tblInsa
    group by floor(basicpay / 1000000);</p>
<p>-- tblInsa. 남자수? 여자수?
select
    substr(ssn, 8, 1),
    count(*)
from tblInsa
    group by substr(ssn, 8, 1);</p>
<p>-- tblTodo. 한일? 안한일?
select
    --completedate,
    case
        when completedate is not null then '한일'
        when completedate is null then '안한일'
    end,
    count(*)
from tblTodo
    group by 
        case
            when completedate is not null then '한일'
            when completedate is null then '안한일'
        end;</p>
<p>-- tblInsa. 직위별 인원수
select 
    jikwi,
    count(*)
from tblInsa
    group by jikwi;</p>
<p>-- tblInsa. 직위별 &gt; 과장 + 부장? 사원 + 대리? 인원수
select 
    decode(jikwi, '과장', 1, '부장', 1, '사원', 2, '대리', 2),
    count(*)
from tblInsa
    group by decode(jikwi, '과장', 1, '부장', 1, '사원', 2, '대리', 2);</p>
<p>-- 개인 급여가 150만원 이상인 직원 수? &gt; 개인에 대한 조건(where절)
select
     count(*)
from tblInsa
    where basicpay &gt;= 1500000; --27</p>
<p>-- 급여가 150이상인 부서만 보여줘  / 조건절 2개 개인 where, 그룹 having
select
    buseo, count(*), round(avg(basicpay))
from tblInsa
    group by buseo
        having avg(basicpay) &gt;= 1500000;</p>
<p>select
    buseo, count(*), round(avg(basicpay))
from tblInsa
    where basicpay &gt;= 1500000 --개인을 걸러냄
        group by buseo
            having avg(basicpay) &gt;= 1500000; --그룹을 걸러냄</p>
<p>-- 부서별(GROUP BY) 과장수 + 부장수?(WHERE) 3명 이상(HAVING) 부서?
select
    buseo, count(<em>)
from tblInsa
    where jikwi in('과장', '부장') 
        group by buseo
            having count(</em>) &gt;= 3;</p>
<p>/*
    group by 표현식</p>
<pre><code>rollup()
- group by의 집계 결과를 좀 더 자세하게 반환</code></pre><p>*/</p>
<p>select
    buseo, 
    count(*),    -- 합
    sum(basicpay),    -- 합
    round(avg(basicpay)),    -- 평균
    max(basicpay), -- 최댓값
    min(basicpay) -- 최솟값
from tblInsa
    group by rollup(buseo);</p>
<p>select
    useo, jikwi, city
    count(*),    -- 합
    sum(basicpay),    -- 합
    round(avg(basicpay)),    -- 평균
    max(basicpay), -- 최댓값
    min(basicpay) -- 최솟값
from tblInsa
    group by rollup(buseo, jikwi, city);</p>
<p>/*</p>
<pre><code>cube()
- group by의 집계 결과를 좀 더 자세하게 반환</code></pre><p>*/</p>
<p>select
    buseo, 
    count(*),    -- 합
    sum(basicpay),    -- 합
    round(avg(basicpay)),    -- 평균
    max(basicpay), -- 최댓값
    min(basicpay) -- 최솟값
from tblInsa
    group by cube(buseo);</p>
<p>select
    buseo, jikwi, city,
    count(*),    -- 합
    sum(basicpay),    -- 합
    round(avg(basicpay)),    -- 평균
    max(basicpay), -- 최댓값
    min(basicpay) -- 최솟값
from tblInsa
    group by cube(buseo, jikwi, city);</p>