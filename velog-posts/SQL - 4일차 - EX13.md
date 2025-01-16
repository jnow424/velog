<p>-- ex13_ddl.sql</p>
<p>/*</p>
<pre><code>DML(데이터 조작)

1. 기본 DML &gt; select &gt; ex01 ~ ex12  
2. DDL &gt; 테이블(구조)
3. 확장 DML
4. 데이터 모델링
5. PL/SQL


1. DDL
    - Data Definition Language
    - 데이터 정의어
    - 테이블, 뷰, 사용자, 인덱스, 트리거 등의 데이터베이스 오브젝트를 생성/수정/삭제하는 명령어
    - 구조를 생성/관리하는 역할(***)
    a. create: 생성
    b. drop: 삭제
    c. alter: 수정


테이블 생성하기 &gt; 스키마 정의하기 &gt; 컬럼 정의하기 &gt; 컬럼의 이름, 자료형, 제약 사항

CREATE TABLE 테이블명
(
    컬럼 정의,
    컬럼명 자료형(길이) NULL 제약사항
);

제약 사항, Constraint
- 데이터베이스 객체 중 하나
- 해당 컬럼에 들어갈 데이터(값)에 대한 조건(*****)
    1. 조건을 만족하면 &gt; 삽입
    2. 조건을 불만족하면 &gt; 에러 발생
- 데이터 무결성을 보장하는 도구(*****)

1. NOT NULL 제약 사항
    - 해당 컬럼이 반드시 값을 가져야 한다.
    - 해당 컬럼에 값이 없으면 에러 발생
    - 필수값

2. PRIMARY KEY, PK
    - 테이블의 행을 구분하기 위한 제약 사항 &gt; 식별자 역할을 부여
    - 모든 테이블은 반드시 1개의 기본키가 존재해야 한다.(***********)
    - 기본키

3. FOREIGN KEY
    - 나중에

4. UNIQUE
    - 유일하다. &gt; 레코드간의 같은 컬럼은 중복값을 가질 수 없다.
    - null을 가질 수 있다. &gt; 식별자가 될 수 없다. 
    - PK = UQ + NN
    ex) 초등학교 학생 정보
        - 학생(번호(PK), 이름, 직책(UQ))
            1, 홍길동, 반장
            2, 아무개, null
            3, 강아지, 부반장
            4, 고양이, null

5. CHECK
    - 사용자 정의형 
    - where절 조건 &gt; 제약 조건

6. DEFAULT
    - 기본값 설정
    - 제한(X) &gt; 도와주는 역할(O)
    - insert/update 작업 시 &gt; 컬럼에 값을 안넣으면 null 대입
                            &gt; 미리 설정한 값을 대신 대입
    - 기본값(내가 값을 넣지 않으면 알아서 넣어줌)</code></pre><p>*/</p>
<p>-- 메모 테이블
-- 아래의 테이블 제약 사항이 없다.
-- 왜 문제가 생기는지 보여주기 위함
-- 컬럼 정의 &gt; null 표시 &gt; 이 컬럼의 값을 생략해도 됩니다.(Optional). null 허용</p>
<p>create table tblMemo (
    -- 컬럼명 자료형(길이) NULL 제약사항
    seq number(3) null,         -- 메모번호
    name varchar2(30) null,     -- 작성자
    memo varchar2(1000) null,   -- 메모내용
    regdate date null           -- 작성날짜</p>
<p>);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (1, '홍길동', '메모입니다.', sysdate);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (1, '홍길동', '메모입니다.', '2025-01-12');</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (1, '홍길동', '메모입니다.', to_date('2025-01-12 14:30:00', 'yyyy-mm-dd hh24:mi:ss'));</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (2, '홍길동', '메모입니다.', null);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (3, '홍길동', null, null);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (3, null, null, null);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (null, null, null, null);</p>
<p>drop table tblMemo;</p>
<p>-- not null
create table tblMemo (
    -- 컬럼명 자료형(길이) NULL 제약사항
    seq number(3) not null,         -- 메모번호(NN)
    name varchar2(30) null,         -- 작성자
    memo varchar2(1000) not null,   -- 메모내용(NN)
    regdate date null           -- 작성날짜
);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (1, '홍길동', '메모입니다.', sysdate);</p>
<p>-- SQL 오류: ORA-01400: NULL을 (&quot;HR&quot;.&quot;TBLMEMO&quot;.&quot;MEMO&quot;) 안에 삽입할 수 없습니다. cannot insert NULL into (%s)
insert into tblMemo (seq, name, memo, regdate)
    values (1, '홍길동', null, sysdate);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (1, '홍길동', '', sysdate); -- 빈문자열(null로 판단함)    </p>
<p>drop table tblMemo;</p>
<p>--primary key
create table tblMemo (
    -- 컬럼명 자료형(길이) NULL 제약사항
    seq number(3) primary key,         -- 메모번호(PK) &gt; primary key(UQ + NN)
    --A-00905: 누락된 키워드
    --name varchar2(30) primary,         -- 작성자
    name varchar2(30) null,
    memo varchar2(1000) not null,   -- 메모내용(NN)
    regdate date null           -- 작성날짜
);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (1, '홍길동', '메모입니다.', sysdate);</p>
<p>-- SQL 오류: ORA-00001: 무결성 제약 조건(HR.SYS_C008418)에 위배됩니다.
insert into tblMemo (seq, name, memo, regdate)
    values (1, '홍길동', '메모입니다.', sysdate);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (2, '홍길동', '메모입니다.', sysdate);</p>
<p>-- SQL 오류: ORA-01400: NULL을 (&quot;HR&quot;.&quot;TBLMEMO&quot;.&quot;SEQ&quot;) 안에 삽입할 수 없습니다. cannot insert NULL into (%s)
insert into tblMemo (seq, name, memo, regdate)
    values (null, '홍길동', '메모입니다.', sysdate);</p>
<p>select * from tblMemo where seq = 2; -- PK를 검색 조건 &gt; 유일한 행 검색 가능!!</p>
<p>drop table tblMemo;</p>
<p>-- unique
create table tblMemo (</p>
<pre><code>seq number(3) primary key,       -- 메모번호(PK) &gt; primary key(UQ + NN)
name varchar2(30) unique,          -- 작성자(UQ)
memo varchar2(1000) not null,   -- 메모내용(NN)
regdate date null           -- 작성날짜</code></pre><p>);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (1, '홍길동', '메모입니다.', sysdate);</p>
<p>-- ORA-00001: 무결성 제약 조건(HR.SYS_C008423)에 위배됩니다
insert into tblMemo (seq, name, memo, regdate)
    values (2, '홍길동', '메모입니다.', sysdate);</p>
<p>-- null은 중복 가능
insert into tblMemo (seq, name, memo, regdate)
    values (2, null, '메모입니다.', sysdate);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (3, null, '메모입니다.', sysdate);</p>
<p>drop table tblMemo;</p>
<p>-- 
create table tblMemo (</p>
<pre><code>seq number(3) primary key,       -- 메모번호(PK) &gt; primary key(UQ + NN)
name varchar2(30) null,          -- 작성자
memo varchar2(1000) not null,   -- 메모내용(NN)
regdate date null,           -- 작성날짜

-- 중요도(1(중요), 2(보통), 3(안중요))
-- priority number(1) check( priority &gt;= 1 and priority &lt;=3)
priority number(1) check(priority between 1 and 3),

-- 카테고리(할일, 공부, 약속)
--category varchar2(10) check(category = '할일' or category = '공부')
 category varchar2(10) check(category in ('할일', '공부', '약속'))</code></pre><p>);</p>
<p>insert into tblMemo (seq, name, memo, regdate, priority, category)
    values (1, '홍길동', '메모입니다.', sysdate, 2, '할일');</p>
<p>-- ORA-02290: 체크 제약조건(HR.SYS_C008425)이 위배되었습니다
insert into tblMemo (seq, name, memo, regdate, priority, category)
    values (1, '홍길동', '메모입니다.', sysdate, 5, '할일');</p>
<p>-- ORA-02290: 체크 제약조건(HR.SYS_C008426)이 위배되었습니다
insert into tblMemo (seq, name, memo, regdate, priority, category)
    values (1, '홍길동', '메모입니다.', sysdate, 2, '점심');</p>
<p>drop table tblMemo;</p>
<p>create table tblMemo (</p>
<pre><code>seq number(3) primary key,       -- 메모번호(PK) &gt; primary key(UQ + NN)
name varchar2(30) default '익명',    -- 작성자
memo varchar2(1000) not null,    -- 메모내용(NN)
regdate date default sysdate         -- 작성날짜</code></pre><p>);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (1, '홍길동', '메모입니다.', sysdate);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (2, null, '메모입니다.', sysdate);</p>
<p>-- 명시적으로 작성 
insert into tblMemo (seq, memo, regdate)
    values (3, '메모입니다.', sysdate);</p>
<p>insert into tblMemo (seq, memo)
    values (4, '메모입니다.');</p>
<p>select * from tblMemo;</p>
<p>/*</p>
<pre><code>제약 사항을 만드는 방법(코드 작성법)

1. 컬럼 수준에서 만드는 방법
    - 위에서 선언한 방법
    - 컬럼을 정의할 때 제약 사항동 같이 선언

2. 테이블 수준에서 만드는 방법
    - 컬럼 선언과 제약 사항 선언을 분리해서 선언하는 방법
    - 코드 관리 용이(가독성 향상)

3. 외부에서 만드는 방법
    - 테이블 수정 명령어
    - 나중에</code></pre><p>*/</p>
<p>drop table tblMemo;</p>
<p>-- 1. 컬럼 수준
create table tblMemo (
    --             constraint 제약사항명      제약사항 종류
    seq number(3) constraint tblmemo_seq_pk primary key, -- 메모번호(PK) 
    name varchar2(30) ,                                   -- 작성자
    memo varchar2(1000),                                  -- 메모내용(NN)
    regdate date                                          -- 작성날짜</p>
<p>);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (1, '홍길동', '메모입니다.', sysdate);</p>
<p>drop table tblMemo;</p>
<p>-- 1. 테이블 수준
create table tblMemo (</p>
<pre><code>seq number(3) ,   -- 메모번호(PK) 
name varchar2(30) ,    -- 작성자
memo varchar2(1000) default '익명',    -- 메모내용(NN)
regdate date not null,      -- 작성날짜

constraint tblmemo_seq_pk primary key(seq),
constraint tblmemo_name_uq unique(name),
constraint tblmemo_memo_ck check(length(memo) &gt;= 5)</code></pre><p>);</p>
<p>insert into tblMemo (seq, name, memo, regdate)
    values (1, '홍길동', '메모입니다.', sysdate); </p>
<p>select * from tblmemo;</p>