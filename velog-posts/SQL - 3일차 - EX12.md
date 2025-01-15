<p>-- ex12_daetime_function.sql</p>
<p>/*</p>
<pre><code>1. 날짜시간 관련 함수
2. 날짜시간 연산 함수

sysdate
- 현재 오라클의 시스템 시각 반환
- date sysdate
- Calendar.getInstance()</code></pre><p>*/</p>
<p>select sysdate from dual;</p>
<p>/*</p>
<pre><code>날짜 연산
1. 시각 - 시각 = 시간 &gt; tick - tick
2. 시각 + 시간 = 시각 &gt; now.add(+)
3. 시각 - 시간 = 시각 &gt; now.add(-)</code></pre><p>*/</p>
<p>-- 1. 시각 - 시각 = 시간
-- tblInsa. 현재 - 입사일 = 근무시간(일)
select
    name,
    to_char(ibsadate, 'yyyy-mm-dd') as 입사일,
    round(sysdate - ibsadate) as 근무일수, -- 3399(단위?)/일
    round((sysdate - ibsadate) * 24) as 근무시수,
    round((sysdate - ibsadate) * 24 * 60) as 근무분수,
    round((sysdate - ibsadate) * 24 * 60 * 60) as 근무초수, -- 513261162 / 홍길동 기준
    round((sysdate - ibsadate) / 30.4) as 근무개월수, -- 195 / 부정확 
    round((sysdate - ibsadate) / 365) as 근무년수 -- 16 / 부정확 
from tblInsa;</p>
<p>-- 맘먹고 실행하기까지 얼마나 걸렸는지?
-- Oracle 12c 이전: 식별자(테이블명, 커럼명, 별칭)는 최대 30바이트까지
-- Oracle 12c 이후: 최대 128바이트까지 확장
select 
    title,
    adddate,
    completedate,
    round((completedate - adddate) * 24) as &quot; 실행하기까지 걸린 시간&quot; -- round 반올림 30분이하라서 0이 나옴
from tblTodo
    order by round((completedate - adddate) * 24) asc;</p>
<p>/*</p>
<pre><code>2. 시각 + 시간 = 시각 &gt; now.add(+)</code></pre><p>*/</p>
<p>select
    sysdate,
    sysdate + 1,
    sysdate + 100,
    sysdate -7,
    sysdate + 30, -- 부정확(x) 한달 
    sysdate + 365, -- 부정확(x) 일년
    to_char(sysdate + (3 / 24), 'hh24:mi:ss'), -- 3시간 뒤
    to_char(sysdate + (30 / 60/ 24), 'hh24:mi:ss') -- 30분 뒤 &gt; 13:16:26<br />from dual;</p>
<p>/*</p>
<pre><code>1. 일, 시, 분, 초 &gt; 연산
a. 시각 - 시각 = 시간(일)
b. 시각 + 시간(일) = 시각
c. 시각 - 시간(일) = 시각

2. 월, 일 &gt; 연산
a. months_between()
    시각 - 시각 = 시간(월)
b. add_months()
    시각 + 시간(월) = 시각
    시각 - 시간(월) = 시각</code></pre><p>*/</p>
<p>select 
    name,
    round(sysdate - ibsadate) as 근무일수,
    round((sysdate - ibsadate) / 30.4 ) as 근무월수,
    round(months_between(sysdate, ibsadate)) as 근무월수,
    round(months_between(sysdate, ibsadate) / 12) as 근무년수
from tblInsa;</p>
<p>select
    sysdate,
    sysdate + 1,
    sysdate + 30.4,
    add_months(sysdate, 1), -- 한달 후
    add_months(sysdate, -1), -- 한달 전
    add_months(sysdate, 3 * 12) -- 3년 후</p>
<p>from dual;</p>
<p>-- 마지막 날짜
-- date last_day(날짜)
-- 인자값이 포함된 년/월의 마지막 날짜 반환
select 
    last_day(sysdate), --2025년 1월의 마지막날? &gt; 25/01/31
    last_day(add_months(sysdate, 1)) --2025년 2월의 마지막날? &gt; 2025/02/08</p>
<p>from dual;</p>