<p>-- ex04_Operator.sql</p>
<p>/*</p>
<pre><code>연산자, Operator

1. 산술 연산자
- +, -, *, /
- %(없음) &gt; 함수로 제공(mod())

2. 문자열 연산자(concat)
- 문자열 + 문자열 &gt; 자바
- 문자열 || 문자열 &gt; SQL

3. 비교 연산자
- &gt;, &gt;=, &lt;, &lt;=
- =(==), &lt;&gt;(!=)
- 논리값 반환 &gt; boolean 존재x &gt; 명시적 표현 불가능 &gt; 조건에서만 사용 가능
- 컬럼리스트에서는 사용 불가
- 조건절에서 사용 

4. 논리 연산자
- and(&amp;&amp;), or(||), not(!)

5. 대입 연산자
- =
- 컬럼 = 값
- update문에서 사용
- 복합 대입 연산자 없음(+=, -+...)

6. 3항 연산자
- 없음
- 제어문 없음

7. 증감 연산자
- 없음

8. SQL 연산자
- 자바 &gt; instanceof 타입
- 오라클 &gt; in, between, like, is 등...</code></pre><p>*/</p>
<p>select first_name, last_name, first_name + last_name from employees; -- (x)
select first_name, last_name, first_name || last_name from employees;</p>
<p>select salary = 24000
from employees;</p>