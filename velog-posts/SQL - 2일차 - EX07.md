<p>-- ex07_order.sql</p>
<p>/*</p>
<p>   [WITH <sub>]
    SELECT column_list
    FROM table_name
    [WHERE search_condition]
    [GROUP BY group_by_expression]
    [HAVING search_condition]
    [ORDER BY order_expression [ASC|DESC]]; </p>
<pre><code>SELECT column_list &gt; 3. 컬럼 지정(보고 싶은 컬럼만 가져오기) &gt; Projection
FROM table_name &gt; 1.테이블 지정
WHERE search_condition &gt; 2. 레코드마다 질문!!! 조건 지정(보고 싶은 행만 가져오기) &gt; Selection
ORDER BY order_expression [ASC|DESC] &gt; 4. 결과셋의 레코드 순서를 정한다.

각 절의 순서(*****)
1. FROM
2. WHERE
3. SELECT
4. ORDER BY

ORDER BY절
- 정렬
- 결과셋의 레코드 정렬!!. 원본 테이블의 정렬(X &gt; 오라클이 알아서 정렬)
- order by 대상컬럼(기준) asc //ascending. 오름차순(기본값) 
- order by 대상컬럼(기준) desc //descending. 내림차순</code></pre><p>*/</p>
<p>select * from tblInsa;</p>
<p>select * from tblInsa order by name;
select * from tblInsa order by name asc;
select * from tblInsa order by name desc;</p>
<p>-- 자료형 확인
-- 우위 비교 가능 &gt; 정렬 가능
select * from tblInsa order by basicpay; -- 숫자형
select * from tblInsa order by name; -- 문자형
select * from tblInsa order by ibsadate; -- 날짜형</p>
<p>-- 다중 정렬
select * from tblInsa order by buseo asc; -- 1차 정렬
select * from tblInsa order by buseo asc, jikwi asc; --2차 정렬
select * from tblInsa order by buseo asc, jikwi asc, basicpay desc; -- 3차 정렬</p>
<p>--
-- Java: 첨자가 0부터 시작(컴파일 언어)
-- SQL : 첨자가 1부터 시작(스크립트 언어, 인터프리터 언어)
select 
    num, name, buseo, jikwi     --2
from tblInsa                    --1
order by 3 asc;                 --3 , 3번째 순서 기준으로 정렬/ 비권장</p>
<p>-- 연산 결과를 가지고 정렬 가능
select * from tblInsa order by basicpay desc;
select * from tblInsa order by basicpay  + sudang desc;</p>
<p>-- 직원을 성별순으로 정렬
-- : 남자 &gt; 여자 순으로
select * from tblComedian order by gender desc;</p>
<p>select 
    name, ssn,
    case 
        when ssn like '%-1%' then '남자'
        when ssn like '%-2%' then '여자'
    end as gender
from tblInsa
    order by gender; -- 순차적인 실행단계다 보니 앞에서 사용한것을 뒤에서 사용가능</p>
<p>select 
    name, ssn,
    case 
        when ssn like '%-1%' then '남자'
        when ssn like '%-2%' then '여자'
    end 
from tblInsa
    order by 3; </p>
<p>select 
    name, ssn
from tblInsa
    order by<br />    case 
        when ssn like '%-1%' then '남자'
        when ssn like '%-2%' then '여자'
    end 
;</p>
<p>-- 직위순으로 정렬: 부장(1) &gt; 과장(2) &gt; 대리(3) &gt; 사원(4)
select 
    name, jikwi,
    case
        when jikwi = '부장' then 1 -- 매핑
        when jikwi = '과장' then 2
        when jikwi = '대리' then 3
        when jikwi = '사원' then 4
    end as numjikwi
from tblInsa
    order by numjikwi asc;</p>
<p>select 
    name, jikwi
from tblInsa
    order by<br />    case
        when jikwi = '부장' then 1 -- 매핑
        when jikwi = '과장' then 2
        when jikwi = '대리' then 3
        when jikwi = '사원' then 4
    end asc;</p>
<p>-- 정렬 + null? 
select * from tblTodo order by completedate asc; -- null을 끝쪽에
select * from tblTodo order by completedate asc nulls first; -- 강제로 null을 앞으로</p>
<p>select * from tblTodo order by completedate desc; -- null이 앞으로
select * from tblTodo order by completedate desc nulls last; -- 강제로 null을 뒤로</p>