<p>-- ex11_casting_function.sql</p>
<p>/*</p>
<pre><code> 형변환 함수

 1. varchar2 to_char(숫자형): 숫자 &gt; 문자열: 간혹 사용
 1. varchar2 to_char(날짜형): 날짜 &gt; 문자열: 자주 사용
 1. number to_number(문자형): 문자열 &gt; 숫자
 1. date to_number(문자형): 문자열 &gt; 날짜

1. varchar2_to_char(숫자형 [,형식 문자열]): 숫자 &gt; 문자열

형식 문자열 구성요소
a. 9: 숫자 1개를 문자 1개로 바꾸는 역할. 빈자리는 공백으로 치환 &gt; %5d
b. 0: 숫자 1개를 문자 1개로 바꾸는 역할. 빈자리는 '0'으로 치환 &gt; %05d
c. $: 통화 기호
d. L: 통화 기호(Locale) \(원화기호)
e. .: 소수점
f. ,: 첫단위 표기</code></pre><p>*/</p>
<p>select 
    100,
    '100',
    to_char(100),
    '@' || to_char(100, '99999') || '@', -- @   100@
    '@' || to_char(100, '00000') || '@', -- @ 00100@ 
    to_char(100, '$999'), -- $100
--    to_char(100, '999달러')
    '$' ||ltrim(to_char(100, '999')), -- $ 100 &gt; $100
    to_char(100, 'L999') --        ￦100
from dual;    </p>
<p>select
    3.14,
    to_char(3.14),
    to_char(3.14, '9.99'),
    to_char(3.14, '9.9'),
    1000000,
    to_char(1000000),
    to_char(1000000, '9,999,999')<br />from dual;</p>
<p>/*</p>
<pre><code>2. varchar2 to_char(날짜형, 형식문자열): 날짜 &gt; 문자열

형식문자열
a. yyyy
b. yy
c. month
d. mon
e. mm
f. day
g. dy
h. ddd
i. dd
j. d
k. hh
l. hh24
m. mi
n. ss
o. am(pm)</code></pre><p>*/
-- sysdate/ 함수지만 옛날에 만들어서 ()가 없음
select sysdate, to_char(sysdate) from dual;</p>
<p>select to_char(sysdate, 'yyyy') from dual; -- 2025 &gt; 년(4자리)
select to_char(sysdate, 'yy') from dual; -- 25 &gt; 년(2자리)
select to_char(sysdate, 'month') from dual; -- 1월 &gt; 월(풀네임)
select to_char(sysdate, 'mon') from dual; -- 1월 &gt; 월(약어)
select to_char(sysdate, 'mm') from dual; -- 01 &gt; 월(2자리)
select to_char(sysdate, 'day') from dual; -- 수요일 &gt; 요일(풀네임)
select to_char(sysdate, 'dy') from dual; -- 수 &gt; 요일(약어)
select to_char(sysdate, 'ddd') from dual; -- 015 &gt; 일(올해의 며칠)
select to_char(sysdate, 'dd') from dual; -- 15 &gt; 일(이번달의 며칠)
select to_char(sysdate, 'd') from dual; -- 4 &gt; 일(이번주의 며칠, 요일)
select to_char(sysdate, 'hh') from dual; -- 11 &gt; 시(12H)
select to_char(sysdate, 'hh24') from dual; -- 11 &gt; 시(24H)
select to_char(sysdate, 'mi') from dual; -- 30 &gt; 분
select to_char(sysdate, 'ss') from dual; -- 54 &gt; 초
select to_char(sysdate, 'am') from dual; -- 오전 &gt; 오전/오후
select to_char(sysdate, 'pm') from dual; -- 오전 &gt; 오전/오후</p>
<p>select 
    sysdate, --25/01/15
    to_char(sysdate, 'yyyy-mm-dd'), -- 2025-01-15
    to_char(sysdate, 'hh24:mi:ss'), -- 11:33:40
    to_char(sysdate, 'yyyy-mm-dd hh24:mi:ss'), -- 2025-01-15 11:33:40
    to_char(sysdate, 'am hh:mi:ss day') -- 오전 11:33:40 수요일
from dual;</p>
<p>-- 평일 입사 or 휴일 입사
select 
    name, to_char(ibsadate, 'yyyy-mm-dd day') as ibsadate,
    case 
        --when to_char(ibsadate, 'day') in  ('일요일', '토요일') then '휴일 입사'
        when to_char(ibsadate, 'd') in  ('1', '7') then '휴일 입사' -- 안정적 / 미국에서 사용했을 시 한국어x
        else '평일 입사'
    end
from tblInsa;</p>
<p>-- rblInsa. 요일별 입사하는 인원 수? &gt; count() + decode() / case + to_char()</p>
<p>select </p>
<pre><code>count(case 
    when to_char(ibsadate, 'd') = '1' then 1
end) as 일,
count(case 
    when to_char(ibsadate, 'd') in ('2') then 1
end) as 월,
count(case 
    when to_char(ibsadate, 'd') in ('3') then 1
end) as 화,
count(case 
    when to_char(ibsadate, 'd') in ('4') then 1
end) as 수,
count(case 
    when to_char(ibsadate, 'd') in ('5') then 1
end) as 목,
count(case 
    when to_char(ibsadate, 'd') in ('6') then 1
end) as 금,
count(case 
    when to_char(ibsadate, 'd') in ('7') then 1
end) as 토,

count(decode(to_char(ibsadate, 'd'), '1', 1)) as 일,
count(decode(to_char(ibsadate, 'd'), '2', 1)) as 월,
count(decode(to_char(ibsadate, 'd'), '3', 1)) as 화,
count(decode(to_char(ibsadate, 'd'), '4', 1)) as 수,
count(decode(to_char(ibsadate, 'd'), '5', 1)) as 목,
count(decode(to_char(ibsadate, 'd'), '6', 1)) as 금,
count(decode(to_char(ibsadate, 'd'), '7', 1)) as 토</code></pre><p>from tblInsa;</p>