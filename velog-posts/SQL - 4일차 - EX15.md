<p>-- ex15_insert.sql</p>
<p>/*</p>
<pre><code>insert문
- DML
- 테이블에 데이터를 추가하는 명령어

INSERT INTO 테이블명 (컬럼리스트) VALUES(값리스트);</code></pre><p>*/</p>
<p>drop table tblMemo;</p>
<p>create table tblMemo (<br />    seq number(3) constraint tblmemo_seq_pk primary key, 
    name varchar2(30) default '익명',<br />    memo varchar2(1000),<br />    regdate date default sysdate not null<br />);</p>
<p>create sequence seqMemo;</p>
<p>-- 1. 표준
-- : 원본 테이블의 정의된 컬럼 순서대로 &gt; 컬럼 리스트와 값 리스트를 작성하는 방식
-- : 특별한 이유가 없으면 이 방식을 사용한다.
insert into tblMemo (seq, name, memo, regdate)
                values (seqMemo.nextval, '홍길동', '메모입니다.', sysdate);</p>
<p>-- 2. 컬럼 리스트의 순서는 원본 테이블과 상관없다.
-- : 컬럼 리스트의 순서와 값 리스트의 순서만 동일하면 된다.
-- : 보통 원본 테이블의 컬럼 순서대로 작성하는 편이다.
insert into tblMemo (name, memo, regdate, seq)
                values ('홍길동', '메모입니다.', sysdate, seqMemo.nextval);</p>
<p>-- 3. SQL 오류: ORA-00947: 값의 수가 충분하지 않습니다
insert into tblMemo (seq, name, memo, regdate)-- 4개
                values (seqMemo.nextval, '홍길동', '메모입니다.'); -- 3개</p>
<p>-- 4. SQL 오류: ORA-00913: 값의 수가 너무 많습니다 
insert into tblMemo (seq, name, memo) -- 3개
                values (seqMemo.nextval, '홍길동', '메모입니다.', sysdate); -- 4개</p>
<p>-- 5. null을 허용한 컬럼 조작하기
-- 5.a null 상수 사용 &gt; 명시적으로 null 추가
insert into tblMemo (seq, name, memo, regdate)
                values (seqMemo.nextval, '홍길동', null, sysdate);</p>
<p> -- 5.b 컬럼 생략 &gt; 암시적으로 null 추가
 insert into tblMemo (seq, name, regdate)
                values (seqMemo.nextval, '홍길동', sysdate);</p>
<p>-- 6. default 컬럼 조작하기
-- 6.a 컬럼 생략 &gt; null 추가 &gt; default 호출
insert into tblMemo (seq,memo, regdate)
                values (seqMemo.nextval, '메모입니다.', sysdate);</p>
<p>-- 6.b null 상수 &gt; default 동작 안함 &gt; 사용자 의지 반영
insert into tblMemo (seq, name, memo, regdate)
                values (seqMemo.nextval, null, '메모입니다.', sysdate);</p>
<p>-- 6.c default 상수 &gt; 명시적으로 default 호출 
insert into tblMemo (seq, name, memo, regdate)
                values (seqMemo.nextval, default, '메모입니다.', sysdate);</p>
<p>-- 7. 단축 표현
-- 7.a 컬럼 리스트는 생략할 수 있다
insert into tblMemo 
                values (seqMemo.nextval, '홍길동', '메모입니다.', sysdate);</p>
<p>-- 7.b 컬럼 리스트를 생략하면, 테이블의 원본 컬럼 순으로 값리스트를 작성해야 한다. 
insert into tblMemo 
                values ('홍길동', '메모입니다.', sysdate, seqMemo.nextval);</p>
<p>-- 7.c null 컬럼을 생략 불가능 &gt; 반드시 null 상수 사용
insert into tblMemo 
                values (seqMemo.nextval, '홍길동', sysdate); --(X)
insert into tblMemo 
                values (seqMemo.nextval, '홍길동', null, sysdate); --(O)</p>
<p>-- 7.d default 컬럼을 생략 불가능 &gt; 반드시 default 상수 사용
insert into tblMemo 
                values (seqMemo.nextval, '메모입니다.', sysdate); --(X)
insert into tblMemo 
                values (seqMemo.nextval, default, '메모입니다.', sysdate); --(O)</p>
<p>-- 8.
-- tblMemo 테이블 &gt; 복사 &gt; tblMemoCopy 생성
create table tblMemoCopy (<br />    seq number(3) constraint tblMemoCopy_seq_pk primary key, 
    name varchar2(30) default '익명',<br />    memo varchar2(1000),<br />    regdate date default sysdate not null<br />);</p>
<p>select * from tblMemo;</p>
<p>insert into tblMemoCopy select * from tblMemo; -- 서브쿼리 데이터 카피 쿼리<br />select * from tblMemoCopy;    </p>
<p>-- 9. 
-- tblMemo 테이블 &gt; 복사 &gt; tblMemoCopy2 생성
-- 테이블 구조 복사(O) &gt; 데이터를 확인해서 추측
-- 대량 데이터 + 임시 테이블 &gt; 테스트용(더미용)
desc tblMemoCopy2;
-- 확인 결과 제약 사항 복사 안됨(X)</p>
<p>create table tblMemoCopy2
as
select * from tblMemo;</p>
<p>insert into tblMemoCopy2 
                values (1, default, '메모입니다.', sysdate);</p>
<p>select * from tblMemoCopy2;</p>