<p>-- ex10_string_function.sql</p>
<p>/*</p>
<pre><code>문자열 함수

대소문자 변환 
- upper(), lower(), initcap()
- varchar2() upper(컬럼명)
- varchar2() lower(컬럼명)
- varchar2() initcap(컬럼명)</code></pre><p>*/</p>
<p>select
    'javaTEST',
    upper('javaTEST'), -- JAVATEST
    lower('javaTEST'), -- javatest
    initcap('javaTEST') -- Javatest
from dual;</p>
<p>-- emplyees. 이름에 'de'이 포함 된 직원?
select first_name from employees
    where first_name like '%de%'
        or first_name like '%DE%'
        or first_name like '%De%'
        or first_name like '%dE%'
;</p>
<p>select first_name from employees
    where upper(first_name) like '%DE%';</p>
<p>/*</p>
<pre><code>문자열 추출
- substr()
- varchar2 substr(컬럼명, 시작위치, 가져올 문자 개수)
- varchar2 substr(컬럼명, 시작위치)</code></pre><p>*/</p>
<p>select 
    name,
    substr(name, 1, 3),
    substr(name, 3)
from tblCountry;</p>
<p>select 
    name, ssn,
    '19' || substr(ssn, 1, 2) as 생년,
    substr(ssn, 3, 2) as 생월,
    substr(ssn, 5, 2) as 생일,
    substr(ssn, 8, 1) as 성별<br />from tblInsa;</p>
<p>-- tblInsa &gt; 김, 이, 박, 정, 최 &gt; 각각 몇명?</p>
<p>select * from tblInsa where name like '김%';</p>
<p>select * from tblInsa where substr(name, 1, 1) = '김'; -- 12
select * from tblInsa where substr(name, 1, 1) = '이'; -- 14
select * from tblInsa where substr(name, 1, 1) = '박'; -- 2
select * from tblInsa where substr(name, 1, 1) = '최'; -- 1
select * from tblInsa where substr(name, 1, 1) = '정'; -- 5</p>
<p>select
    count(case
        when substr(name, 1, 1) = '김' then 1
    end) as 김,
    count(case
        when substr(name, 1, 1) = '이' then 1
    end) as 이,
    count(case
        when substr(name, 1, 1) = '박' then 1
    end) as 박,
    count(case
        when substr(name, 1, 1) = '최' then 1
    end) as 최,
    count(case
        when substr(name, 1, 1) = '정' then 1
    end) as 정
from tblInsa;</p>
<p>/*</p>
<pre><code>문자열 길이
- length()
- number length(컬럼명)</code></pre><p>*/</p>
<p>-- 함수 &gt; 대부분의 절에서 사용 가능</p>
<p>-- SELECT절
select name, length(name) from tblCountry;</p>
<p>-- 조건절에서 사용
select 
    name, length(name) -- as 사용 x  절의 실행 순서에 맞지 않음 order by 절은 select절 이후에 실행 해서 가능
from tblCountry
    where length(name) &gt; 3;</p>
<p>select 
    name, length(name) 
from tblCountry
    order by length(name) desc;</p>
<p>select 
    name, length(name)
from tblCountry
    order by 2 desc;</p>
<p>select 
    name, length(name) as ln
from tblCountry
    order by ln desc;</p>
<p>-- tblInsa. 남자 &gt; 여자
select name, ssn from tblInsa
    order by substr(ssn, 8, 1) asc;</p>
<p>/*</p>
<pre><code>문자열 검색
- instr()
- 검색어의 위치 반환
- number instr(컬럼명, 검색어)
- number instr(컬럼명, 검색어, 시작위치)
- number instr(컬럼명, 검색어, 시작위치, -1) // lsatIndexOf() 
- 못찾으면 0을 반환</code></pre><p>*/</p>
<p>select 
    '안녕하세요. 홍길동님',
    instr('안녕하세요. 홍길동님', '홍길동'),
    instr('안녕하세요. 홍길동님', '아무개'),
    instr('안녕하세요. 홍길동님. 홍길동', '홍길동') as aaa,
    instr('안녕하세요. 홍길동님. 홍길동', '홍길동', 11),
--    instr('안녕하세요. 홍길동님. 홍길동', '홍길동', aaa),
    instr('안녕하세요. 홍길동님. 홍길동', '홍길동', instr('안녕하세요. 홍길동님', '홍길동')+3),
    instr('안녕하세요. 홍길동님. 홍길동', '홍길동', -1)
from dual;</p>
<p>/*</p>
<pre><code>패딩
- lpad(), rpad()
- left padding, right padding
- varchar2 lpad(컬럼명, 개수, 문자)
- varchar2 rpad(컬럼명, 개수, 문자)</code></pre><p>*/</p>
<p>-- String.format(&quot;%5s&quot;, &quot;A&quot;)
select
    lpad('A', 5),
    lpad('A', 5, 'B'),
    lpad('A', 5, 'BC'),
    lpad('A', 5, 'BCD'),
    lpad('AA', 5, 'B'),
    lpad('AAA', 5, 'B'),
    lpad('AAAA', 5, 'B'),
    lpad('AAAAA', 5, 'B'),
    lpad('AAAAAA', 5, 'B'),
    lpad('1', 3, '0'), -- String.format(&quot;%03d&quot;, num)
    rpad('A', 5, 'B')
from dual;</p>
<p>/*</p>
<pre><code>공백 제거
- trim(), ltrim(), rtrim()
- varchar2 trim(컬럼명)
- varchar2 ltrim(컬럼명)
- varchar2 rtrim(컬럼명)</code></pre><p>*/</p>
<p>select 
    '   하나  둘   셋    ',
    trim('   하나  둘   셋    '),
    ltrim('   하나  둘   셋    '),
    rtrim('   하나  둘   셋    ')
from dual;</p>
<p>/*</p>
<pre><code>문자열 치환
- replace()
- varchar2 replace(컬럼명, 찾을 문자열, 바꿀 문자열)
- varchar2 regex_replace(컬럼명, 찾을 문자열, 바꿀 문자열)</code></pre><p>*/</p>
<p>select 
    replace('홍길동', '홍', '김'),
    replace('홍길동', '이', '김'),
    replace('홍홍홍', '홍', '김')
from dual;</p>
<p>/*</p>
<pre><code>split() &gt; 없음 &gt; 배열(x)</code></pre><p>*/</p>
<p>/*</p>
<pre><code>문자열 치환
- decode()
- replace() 유사 &gt; replace() 반복
- varchar2 decode(컬럼명, 찾을 문자열, 바꿀 문자열)
- varchar2 decode(컬럼명, 찾을 문자열, 바꿀 문자열 [,찾을 문자열, 바꿀 문자열] x N)</code></pre><p>*/</p>
<p>-- tblComedian. 성별 &gt; 남자, 여자
select 
    last || first as name, gender,
    case
        when gender = 'm' then '남자'
        when gender = 'f' then '여자'
    end as g1,
    replace(replace(gender, 'm', '남자'), 'f', '여자')  as g2,
    decode(gender, 'm', '남자', 'f', '여자') as g3
from tblComedian;</p>
<p>-- tblComedian. 남자수? 여자수?
select
    count(case
        when gender = 'm' then 1
    end),
    count(case
        when gender = 'f' then 1
    end),</p>
<pre><code>count(decode(gender, 'm', 1)),
count(decode(gender, 'f', 1))</code></pre><p>from tblComedian;</p>