<p>-- ex06_column.sql</p>
<p>-- SELECT column_list
-- 컬럼 리스트에 올 수 있는 표현들 &gt; 값을 넣겠다는 것/ 값을 최종적으로 표현</p>
<p>-- 1. 컬럼
select name from tblCountry; -- 특정 컬럼</p>
<p>select * from tblCountry; -- 모든 컬럼</p>
<p>select *, name from tblCountry; -- *와 다른 컬럼은 동시에 사용 불가능 </p>
<p>-- 오라클
-- 모든 요소들이 계정에 소속된다.
select name from tblCountry; -- this.tblCountry
select name from hr.tblCountry;</p>
<p>select hr.tblCountry.name from hr.tblCountry;</p>
<p>select tblCountry.<em>, name from tblCountry;
select c.</em>, name from tblCountry c; -- 테이블 이름을 다르게 출력할 때 짧은 단어를 사용하여 변경</p>
<p>-- ORA-00904: &quot;TBLCOUNTRY&quot;: 부적합한 식별자
select tblCountry.<em>, name --2.  --tblCountry.</em> &gt; c.* 수정
from tblCountry  c; --1.</p>
<p>-- 2. 연산식
select name, basicpay, sudang, basicpay + sudang from tblInsa; -- select에서도 연산이 된다?</p>
<p>-- 식별자 수정 &gt; 별칭(Alias)
-- 대상: 컬럼명, 테이블명
select name, basicpay, sudang, basicpay + sudang as total from tblInsa;
select name as 이름, basicpay as 급여, sudang as 수당, basicpay + sudang as 총급여 from tblInsa;
-- name 과 이름은 다르다. name 을 버리고 이름을 사용, 쿼리를 실행 할때만..</p>
<p>-- 3. 상수
select 100 from tblCountry;</p>
<p>--4. 함수(메서드) &gt; 반환값 &gt; 값 
select name, length(name) as length from tblCountry;</p>
<p>select name, length(name) as &quot;문자 수&quot; from tblCountry; -- &quot;&quot; 문자열 x, 말이 안되는 상황에서 어거지로 사용하</p>
<p>select name, length(name) as &quot;select&quot; from tblCountry; -- 이스케이프 하는 느낌, 최후의 수단</p>
<p>select name, length(name) as &quot;length&quot; from tblCountry; -- 가독성으로 하는경우도 있음</p>
<p>/*</p>
<pre><code>distinct
- Java Steam: list.stream().distinct().forEach()
- 중복값 제거
- 컬럼 리스트에서 사용
- 특정 컬럼 대상(X) &gt; 레코드 대상(O)</code></pre><p>*/</p>
<p>-- tblCountry에는 어떤 종류의 대륙이 있습니까? / 중복값을 배제하는 것을 원함
select continent from tblCountry;
select distinct continent from tblCountry;</p>
<p>-- tblInsa에는 어떤 부서들이 있나?
select buseo from tblInsa;
select distinct buseo from tblInsa;
select jikwi from tblInsa;
select distinct jikwi from tblInsa;</p>
<p>select name from tblInsa;
select distinct name from tblInsa; -- 동명이인이 없음.</p>
<p>select distinct buseo, name from tblInsa; -- 데이터 병합이 안댐 
-- disinct는 buseo, name에 전체 적용되는 것, 컬럼리스트 전체에 적용</p>
<p>select buseo, jikwi from tblInsa; --60
select distinct buseo, jikwi from tblInsa; --23</p>
<p>/*</p>
<pre><code>case
- 대부분의 절에서 사용 가능
- switch case 역할
- 조건을 만족하면 then을 반환
- 조건을 불만족하면 null을 반환(***)</code></pre><p>*/</p>
<p>select 
    last || first as name, 
    gender,
    case
        -- when 조건 then 값
        when gender = 'm' then '남자' -- 1
        when gender = 'f' then '여자' -- 2
    end as genderName,</p>
<pre><code>    case gender  -----??????????????
        when 'm' then '남자'
        when 'f' then '여자'
    end as genderName2</code></pre><p>from tblComedian;</p>
<p>select 
    name, continent,
    case
        when continent = 'AS' then '아시아'
        when continent = 'EU' then '유럽'
        when continent = 'AF' then '아프리카'
        -- else '기타 등등' / 나머지를 기타 등등으로 해주세요.
        -- else continent -- 나머지는 원본값을 가지도록 해주세요.
        -- else 100 -- ORA-00932: 일관성 없는 데이터 유형: CHAR이(가) 필요하지만 NUMBER임
        -- else name -- 최악의 경우</p>
<pre><code>end as continentName</code></pre><p>from tblcountry;</p>
<p>select 
    last || first as name,
    weight,
    case
        when weight &gt; 90 then '과체중'
        when weight &gt; 50 then '정상체중'
        else '저체중'
    end as weightState,</p>
<pre><code>case
    when weight &gt;= 50 and weight &lt;= 90 then '정상체중'
    else '주의체중'
end as weightState2,

case 
    when weight between 50 and 90 then '정상체중'
    else '주의체중'
end as weightState3</code></pre><p>from tblComedian;</p>
<p>-- 사원, 대리 &gt; 현장직
-- 과장, 부장 &gt; 관리직
select 
    name, jikwi,
    case
        when jikwi = '사원' or jikwi = '대리' then '현장직'
    else '관리직'
    end,
    case
        when jikwi in ('사원', '대리') then '현장직'
        else '관리직'
    end</p>
<p>from tblInsa;</p>
<p>select 
    name, ssn,
    case
        when ssn like '%-1%' then '남자' 
        when ssn like '%-2%' then '여자'<br />    end as gender</p>
<p>from tblInsa;
        -- where ssn like '%-1%;</p>
<p>select
    title,
    case
        when completedate is not null then '완료'
        when completedate is null then '미완료'
    end as state
--    ,case completedate
--        when not null then '완료'
--        when nill then '미완료'
--    end as state
from tblTodo;</p>