<p>-- ex23_alter.sql</p>
<p>/*</p>
<pre><code>DDL &gt; 객체 조작
- 객체 생성: create
- 객체 수정: alter
- 객체 삭제: drop

DML &gt; 데이터 조작
- 데이터 생성: insert
- 데이터 수정: update
- 데이서 삭제: delete


테이블 수정하기
- 테이블 수정 &gt; 테이블 스키마 수정 &gt; 컬럼수정
    &gt; 컬럼명 or 자료형(길이) or 제약 사항 &gt; 수정
- 되도록 테이블을 수정하는 상황을 발생시키면 안된다.!!!
- 테이블 설계를 꼼꼼하게 + 검토

테이블 수정해야 하는 상황 발생!!
1. 기존 테이블 삭제(drop) &gt; 테이블 DDL(create) 수정 &gt; 수정된 DDL로 다시 테이블 생성/(ex13에서 많이함) / 프라이머리 키를 넣기위해 기존을 삭제 후 수정한 뒤 재생성
    a. 기존 테이블에 데이터가 없을 경우 &gt; 아무 문제 없음
    b. 기존 테이블에 데이터가 있을 경우 &gt; 미리 데이터 백업 &gt; 테이블 삭제
        &gt; 수정된 테이블 생성 &gt; 데이터 복구
    - 개발 중에 사용
    - 학습 중에 사용
    - 운영 중에는 많이 부담



2. alter 명령어 사용 &gt; 기존 테이블의 구조 변경
    a. 기존 테이블에 데이터가 없을 경우 &gt; 아무 문제 없음
    b. 기존 테이블에 데이터가 있을 경우 &gt; 상황에 따라 다름
    - 개발중에 사용
    - 학습중에 사용
    - 운영중에는 덜 부담</code></pre><p>*/</p>
<p>drop table tblEdit;</p>
<p>create table tblEdit (
    seq number primary key,
    name varchar2(20) not null
);</p>
<p>insert into tblEdit values (1, '마우스');
insert into tblEdit values (2, '키보드');
insert into tblEdit values (3, '모니터');</p>
<p>-- case 1. 새로운 컬럼을 추가하기
alter table 테이블명 add (컬럼 정의);</p>
<p>alter table tblEdit add (price number null);</p>
<p>alter table tblEdit add (qty number not null);</p>
<p>desc tblEdit;
select * from tblEdit;</p>
<p>delete from tblEdit;</p>
<p>insert into tblEdit values (1, '마우스', 1000, 1);
insert into tblEdit values (2, '키보드', 2000, 2);
insert into tblEdit values (3, '모니터', 100000, 1);</p>
<p>alter table tblEdit add (color varchar2(30) default 'white' not null);</p>
<p>select * from tblEdit;</p>
<p>-- Case 2. 컬럼을 삭제하기
alter table 테이블명 drop column 컬럼명;</p>
<p>alter table tblEdit drop column price;</p>
<p>alter table tblEdit drop column color;</p>
<p>alter table tblEdit drop column seq; --주요한 컬럼은 조심!!!</p>
<p>select * from tblEdit;</p>
<p>-- Case 3. 컬럼 수정하기</p>
<p>--ORA-12899: &quot;HR&quot;.&quot;TBLEDIT&quot;.&quot;NAME&quot; 열에 대한 값이 너무 큼(실제: 31, 최대값: 20)
insert into tblEdit values (4, '맥북 M4 프로 2025 고급형');</p>
<p>-- Case 3.1 컬럼 길이 수정하기(확장/축소)
alter table 테이블명
    modify (컬럼 정의);</p>
<p>alter table tblEdit modify (name varchar2(100)); --확장</p>
<p>alter table tblEdit modify (name varchar2(20)); --축소
alter table tblEdit modify (name varchar2(31));</p>
<p>select * from tblEdit;
desc tblEdit;</p>
<p>-- Case 3.2 NULL 제약 수정하기
alter table tblEdit modify (name varchar2(31) null); --not null &gt; null</p>
<p>insert into tblEdit values (5, null);
delete from tblEdit where seq = 5;</p>
<p>alter table tblEdit modify (name varchar2(31) not null); --null &gt; not null</p>
<p>alter table tblEdit modify (name varchar2(31) default '임시' not null);</p>
<p>alter table tblEdit modify (name varchar2(31) unique);</p>
<p>alter table tblEdit modify (name varchar2(31) check(length(name) &gt;= 3));</p>
<p>select * from user_constraints
    where table_name = 'TBLEDIT';</p>
<p>alter table tblEdit drop constraint SYS_C008490;</p>
<p>-- Case 3.3 컬럼의 자료형 수정하기
delete from tblEdit;</p>
<p>alter table tblEdit modify (name number);</p>
<p>desc tblEdit;</p>
<p>-- Case 3.4 제약 사항 조작하기
drop table tblEdit;</p>
<p>create table tblEdit (
    seq number,
    name varchar2(20) not null
);</p>
<p>insert into tblEdit values (1, '마우스');
insert into tblEdit values (2, '키보드');
insert into tblEdit values (3, '모니터');</p>
<p>alter table tblEdit
    add constraint tbledit_seq_pk primary key(seq);</p>
<p>alter table tblEdit
    drop constraint tbledit_seq_pk;</p>