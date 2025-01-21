<p>-- ex24_pseudo.sql</p>
<p>/*</p>
<pre><code>의사 컬럼, Pseudo, Column
- 실제 컬럼이 아닌데 컬럼처럼 행동하는 객체
- 오라클 전용

rownum (컬럼이름)
- 행번호
- 테이블의 행번호를 가져오는 객체
- 시퀀스 객체 연관X


rownum이 생성되는 시점(*****) / 이걸 이해못하면 아무것도 이해 못해요~~
1. from절이 호출될 때 rownum이 만들어 진다.
2. where절에 의해서 결과셋이 변화가 발생될 때 rownum이 다시 만들어 진다.</code></pre><p>*/</p>
<p>select 
    name, buseo, -- 컬럼(속성) &gt; 객체(레코드)마다 다른 값을 가진다. (개인 데이터)
    100, -- 상수 &gt; 객체가 모두 동일한 값을 가진다.
    basicpay * 2, -- 연산 &gt; 결국은 객체의 값을 가지고 연산 &gt; 컬럼값
    length(name), -- 함수 &gt; 결국은 객체의 값을 가지고 연산 &gt; 컬럼값
    rownum -- 의사컬럼
from tblInsa;</p>
<p>-- 게시판 &gt; 페이징
-- 1페이지 &gt; where rownum between 1 and 20
-- 2페이지 &gt; where rownum between 21 and 40
-- 3페이지 &gt; where rownum between 41 and 60</p>
<p>-- 성적 &gt; 장학금 
-- where rand between 1 and 3</p>
<p>select name, buseo, rownum from tblInsa;
select name, buseo, rownum from tblInsa where rownum = 1; -- 행번호가 1인것만 가져와줘.
select name, buseo, rownum from tblInsa where rownum &lt;= 5; -- 위에서 5개 가져와.</p>
<p>select name, buseo, rownum from tblInsa where rownum = 5;  -- 안나와요 / rownum이 1이 아니면 삭제후 새로 rownum을 1부터 생성 반복
select name, buseo, rownum from tblInsa where rownum between 11 and 20; -- 안나와요<br />select name, buseo, rownum from tblInsa where rownum between 1 and 10;  -- 나와요
select name, buseo, rownum from tblInsa where rownum between 2 and 11;  -- 안나와요
select name, buseo, rownum from tblInsa where rownum between 1 and 5;  -- 나와요
select name, buseo, rownum from tblInsa where rownum between 2 and 3;  -- 안나와요 /1이 포함되면 나와요</p>
<p>-- 원하는 rownum 부분을 추출하는 방법(<strong><strong>**</strong></strong>) &gt; 서브쿼리(sub query)
-- 위의 SQL에서 5~10등 까지 추출
-- ORA-00904: &quot;NAME&quot;: 부적합한 식별자
select name, buseo, rownum from (select name as 이름, buseo, rownum from tblInsa); -- 별칭이 아니라 개명
select 이름, buseo, rownum, 행번호 from (select name as 이름, buseo, rownum as 행번호 from tblInsa); -- 안쪽과 밖의 rownum은 다르다
select 이름, buseo, rownum as rnum2, rnum1 from (select name as 이름, buseo, rownum as rnum1 from tblInsa); -- from절일 때 마다 생성</p>
<p>-- 원하는 rownum 부분을 추출하는 방법(<strong><strong>**</strong></strong>) &gt; 서브쿼리(sub query)
select 이름, buseo, rownum as rnum2, rnum1 -- rownum(rnum2)이 동적으로 변화 됨
    from (select name as 이름, buseo, rownum as rnum1 from tblInsa) -- rownum(rnum1)이 정적으로 변화되지 않음
        where rnum1 = 5;  </p>
<p>-- 급여를 많이 받는 순으로 Top 5을 가져오시오.
select name, basicpay, rownum from tblInsa order by basicpay desc; -- rownum은 from절 이후 생성, order by는 제일 마지막 실행.
select name, basicpay, rnum1, rownum from (select name, basicpay, rownum as rnum1 from tblInsa order by basicpay desc)
    where rownum &lt;= 5; -- 안쪽 rownum이 필요한가? 필요없다</p>
<p>-- 급여를 많이 받는순으로 Top 6~10을 가져오시오.
select name, basicpay, rnum1, rnum2, rownum from -- rnum1, rnum2 는 정적이 되버려서 사용 가능
-- 3. 2번에 만든 rownum을 조건으로 사용하기 위해서
    (select name, basicpay, rnum1, rownum AS rnum2 
    -- 2. 원하는 순서대로 만든 결과셋에 rownum을 붙이기 위해서
        from (select name, basicpay, rownum as rnum1 from tblInsa order by basicpay desc))
        --1. 원하는 순서대로 정렬 
             where rnum2 = 5;</p>
<p>-- tblCountry. 인구수가 3위인 나라?
-- 1. 원하는 순서대로 정렬 
select * from tblCountry order by population desc;</p>
<p>-- 2. 1을 서브쿼리 생성  + rownum 생성
select a.*, rownum as rnum
    from (select * from tblCountry  where population is not null order by population desc) a;</p>
<p>-- 3. 2를 서브쿼리 생성
select * 
    from (select a.*, rownum as rnum 
        from (select * from tblCountry  where population is not null order by population desc) a)
            where rnum = 3;</p>
<p>-- tblComedian. 키가 큰 순서 3~5등
-- 1. 정렬
select * from tblComedian order by heigh desc;</p>
<p>-- 2. 1을 서브쿼리 + rownum 붙이기
select a.*, rownum as rnum from
    (select * from tblComedian order by height desc) a;</p>
<p>select a.*, rownum as rnum from
    (select * from tblComedian order by height desc) a where rownum = 3; --?</p>
<p>-- 3. 2를 서브쿼리 + rnum 조건
select * from
(select a.*, rownum as rnum from
    (select * from tblComedian order by height desc) a) where rownum = 3; --??</p>
<p>create or replace view vwComedian
as
select * from
    (select a.*, rownum as rnum from
        (select * from tblComedian order by height desc) a);</p>
<p>select * from vwComedian where rnum between 3 and 5;</p>
<p>--</p>
<p>-- tblInsa. 부서 인원(group by)이 가장 많은(rownum = 1) 부서?
-- 1. 정렬
select buseo, count(*) as cnt from tblInsa group by buseo order by cnt desc;</p>
<p>-- 2. 
select buseo, cnt, rownum as rnum from
    (select buseo, count(*) as cnt from tblInsa group by buseo order by cnt desc);</p>
<p>--3.
select * from
    (select buseo, cnt, rownum as rnum from
        (select buseo, count(*) as cnt from tblInsa group by buseo order by cnt desc))
            where rnum between 3 and 5;</p>
<p>-- 11g 지원 안함 &gt; 12c 이후 버전만 지원하는 구문
select * from tblInsa
    order by basicpay desc
        offset 5 rows fetch next 5 rows only;</p>