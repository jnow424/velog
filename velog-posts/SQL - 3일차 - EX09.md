<p>-- ex09_numeric_function.sql
/*</p>
<pre><code>숫자 함수, 수학 함수

round()
- 반올림 함수
- number round(컬럼명): 정수 반환
- number round(컬럼명, 소수이하 자릿수): 원하는 자릿수 실수 반환
                        -1 &gt; 반대로 정수자리 반올림</code></pre><p>*/</p>
<p>select avg(basicpay) from tblInsa; -- 1556526.66666666666666666666666666666667
select round(avg(basicpay)) from tblInsa; -- 1556527 반올림
select round(avg(basicpay), 1) from tblInsa; -- 1556526.7
select round(avg(basicpay), 2) from tblInsa; -- 1556526.67
select round(avg(basicpay), 0) from tblInsa; -- 1556527</p>
<p>select round(3.14) from tblInsa; -- 인원수 만큼 출력
select 3.14 from tblInsa where name = '홍길동'; -- 하나를 출력하기 위해 이렇게 작성하기 힘듬</p>
<p>select * from dual; --  시스템 테이블 &gt; 1행 테이블을 제공</p>
<p>select round(3.14) from dual;</p>
<p>/*</p>
<pre><code>floor(), trunc()
- 절삭 함수
- 무조건 내림 함수
- number floor(컬럼명): 정수 반환
- number trunc(컬럼명): 정수 반환
- number trunc(컬럼명, 소수이하 자릿수): 실수 반환</code></pre><p>*/</p>
<p>select 
    3.6789,
    round(3.6789),
    floor(3.6789),
    trunc(3.6789),
    trunc(3.6789, 1),
    trunc(3.6789, 2)
from dual;</p>
<p>/*</p>
<pre><code>ceil()
- 무조건 올림 함수
- number ceil(컬럼명)</code></pre><p>*/</p>
<p>select 
    3.14,
    round(3.14),
    ceil(3.14)
from dual;</p>
<p>select
    round(3.14),
    floor(3.14), -- 바닥/무조건 내림/속한 층의 바닥에 가까운 곳으로
    ceil(3.14), -- 천장/무조건 올림/ ex) 실링팬/속한 층의 천장에 가까운 곳으로 
    round(3.64),
    floor(3.64),
    ceil(3.64),
    floor(3.99999999999999999),
    ceil(3.999999999999999999)</p>
<p>from dual;</p>
<p>/*</p>
<pre><code>- 나머지 함수
- number mod(피제수, 제수)</code></pre><p>*/</p>
<p>select 
    10 / 3,
    floor(10 / 3) as 몫,
    mod(10 , 3) as 나머지
from dual;</p>
<p>select 
    abs(10), abs(-10), -- 절댓값
    power(2,2), power(2,3), power(2,4), -- 제곱
    sqrt(4), sqrt(9), sqrt(16) -- 루트 
from dual;</p>