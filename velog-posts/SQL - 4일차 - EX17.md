<p>-- ex17_delete.sql</p>
<p>/*</p>
<pre><code>delete문
- DML
- 원하는 행을 삭제하는 명령어

DELETE [FROM] 테이블명 [WHERE 절];</code></pre><p>*/</p>
<p>rollback;</p>
<p>select * from tblInsa;</p>
<p>delete from tblInsa where num = 1001;
delete from tblInsa where buseo = '영업부';
delete from tblInsa;</p>
<p>select count(*) from tblInsa; -- 60 &gt; 59</p>