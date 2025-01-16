<p>-- ex16_update.sql</p>
<p>/*</p>
<pre><code>update 문 
- DML
- 원하는 행의 원하는 컬럼값을 수정하는 명령어

UPDATE 테이블명 SET 컬럼명 = 값 [, 컬럼명 = 값] x N [WHERE 절];</code></pre><p>*/</p>
<p>-- 트랜직션 관리</p>
<p>commit; -- 저장
rollback; -- 저장한 값으로 되돌리기</p>
<p>select * from tblCountry;</p>
<p>-- 대한민국: 서울 &gt; 세종
update tblCountry set capital = '세종'; --14개 행 이(가) 업데이트되었습니다. 조심해야한다!!!!!!! where 꼭 작성
update tblCountry set capital = '세종' where name = '대한민국';</p>
<p>update tblCountry set 
        capital = '제주',
        population = 5000,
        continent = 'EU'
            where name = '대한민국';</p>
<p>-- AS &gt; 인구수 10% 증가 &gt; 반영
update tblCountry set
    population = population * 1.1
    where continent = 'AS';</p>
<p>/*</p>
<ol>
<li>전체가 세종으로 변경</li>
<li>대한민국을 지정해줘야함</li>
<li>다양히 지정</li>
</ol>
<p>*/</p>
<p>select * from tblCountry;</p>