<p>-- ex03_select.sql</p>
<p>/*</p>
<pre><code>SQL
- Query
- 시퀄(SEQUEL)

SELECT문
- DML, DQL
- 가장 중요한 Query
- CRUD 
- 데이터베이스의 테이블로부터 데이터를 가져오는 명령어

[WITH &lt;Sub Query&gt;]
SELECT column_list
FROM table_name
[WHERE search_condition]
[GROUP BY group_by_expression]
[HAVING search_condition]
[ORDER BY order_expression [ASC|DESC]];

SELECT column_list
FROM table_name

각 절의 순서(*****)
1. FROM
2. SELECT</code></pre><p>*/</p>
<p>select * -- 2. *(wildcard) &gt; 모든 컬럼. select all, select star
from tblType; -- 1.</p>
<p>-- hr.employees
-- 처음 본 테이블 &gt; 테이블 명세서 확인 
--                &gt; 테이블 구조 확인
desc employees;</p>
<p>select * from employees;</p>
<p>select * -- 모든 컬럼
from employees;</p>
<p>select first_name -- 단일 컬럼
from employees;</p>
<p>select first_name, last_name -- 다중컬럼 &gt; 컬럼 필터링
from employees; -- 데이터 소스 지정</p>
<p>select first_name, first_name -- 같은 컬럼을 여러번 가져오기(O)
from employees;</p>
<p>select first_name, length(first_name) -- 이름의 글자수 
from employees;</p>
<p>select last_name, first_name -- 테이블 원본의 컬럼 순서와 select절의 컬럼 순서는 무관
from employees;</p>
<p>select first_name, 100, '홍길동' -- 필요할 때가 있느니라....
from employees;</p>
<p>select *
from employee; -- ora-00942: 테이블 또는 뷰가 존재하지 않습니다. table or view%s does not exist</p>
<p>select firstname
from employees; -- ORA-00904: &quot;FIRSTNAME&quot;: 부적합한 식별자. %s: invalid identifier</p>
<p>-- select문의 결과 &gt; 항상 테이블로 반환 &gt; 결과 테이블(Result Table, ResultSet)
select * from employees;</p>
<p>select count(*) from employees;</p>