<p>-- ex14_sequence.sql</p>
<p>/*</p>
<pre><code>시퀀스, Sequence
- 데이터베이스 객체 중 하나(테이블, 제약사항, 계정, 시퀀스)
- 오라클 전용 객체(다른 DB에는 없음)
- 일련 번호를 생성하는 객체(*****)
- 주로 식별자 만들 때 사용한다. &gt; 주로 PK 값으로 사용된다.
                                &gt; 다양한 용도로도 사용된다.

시퀀스 객체 생성하기
- CREATE SEQUENCE 시퀀스명;

시퀀스 객체 제거하기
- DROP SEQUENCE 시퀀스명;

시퀀스 객체 사용하기
- 시퀀스 명.nextVal
- 시퀀스 명.currVal</code></pre><p>*/</p>
<p>create sequence seqNum;</p>
<p>select seqNum.nextVal from dual; -- 11 &gt; 12 &gt; 13 : 스택.pop()
-- ORA-08002: 시퀀스 SEQNUM.CURRVAL은 이 세션에서는 정의 되어 있지 않습니다
select seqNum.currVal from dual; -- 18 &gt; 18 &gt; 18 : 스택peek()</p>
<p>select * from tblMemo;</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (seqNum.nextVal, '아무개', '할일 정리하기', sysdate);</p>
<p>-- 쇼핑몰 &gt; 상품번호 &gt; ABC001, ABC002, ABC003
select 'ABC' || seqNum.nextVal from dual; -- ABC41
select 'ABC' || lpad(seqNum.nextVal, 3, '0') from dual;  -- ABC061
select 'ABC' || trim(to_char(seqNum.nextVal, '000')) from dual;   -- ABC062  ABC067</p>
<h2 id="---62">-- 62</h2>
<p>/*</p>
<pre><code>시퀀스 객체 생성하기
- CREATE SEQENCE 시퀀스명;

시퀀스 객체 생성하기
- CREATE SEQENCE 시퀀스명
            INCREMENT BY N  -- 증감치
            START WITH N    -- 시작값
            MAXVALUE N      -- 최댓값
            MINVALUE N      -- 최솟값
            CYCLE           -- 순환유무
            CACHE N;        -- 캐시 크기 (메모리의 임시공간)</code></pre><p>*/</p>
<p>drop sequence seqTest;</p>
<p>create sequence seqTest
--            increment by 2  --/ 2씩 증가
--            increment by -1 --/ -1씩 감소
--            start with 11   --/ 시작을 10 부터
--            maxvalue 10     -- ORA-08004: 시퀀스 SEQTEST.NEXTVAL exceeds MAXVALUE은 사례로 될 수 없습니다 / 최대 10 까지만
--            minvalue 1      -- ORA-08004: 시퀀스 SEQTEST.NEXTVAL goes below MINVALUE은 사례로 될 수 없습니다 / 초소 1 까지
--            cycle           -- /뺑뺑이
            cache 20 </p>
<pre><code>        ;</code></pre><p>select seqTest.nextVal from dual; -- cache설명 &gt; 5 &gt; 10 벼락맞음 &gt; 26 캐쉬가 마지막 을 20개 했다고 생각하고 5 + 20을 한후 출력</p>