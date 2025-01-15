<p>-- ex01.sql
-- 단일 라인 석
/*
    다중 라인 주석
*/</p>
<p>-- 실행문
-- 문장 종결자(;)
-- 실행할 문장을 선택(블럭)한 후 Ctrl + Enter
show user;</p>
<p>alter session set &quot;_ORACLE_SCRIPT&quot;=true;-- c## 제거문</p>
<p>-- 계정 생성하기
create user c##hong identified by java1234; 
create user dog identified by java1234;
create user hong identified by java1234;</p>
<p>grant connect to hong; -- 로그인할 수 있는 권한 부여</p>
<p>drop user c##hong; --계정 삭제</p>
<p>show user; </p>
<p>-- hr 계정 사용하기
-- &gt; hr을 생성하기</p>
<p>-- 샘플 계정
-- scott/tiger
-- hr/hr</p>
<p>-- 비밀번호 수정
alter user hr identified by java1234;</p>
<p>-- 샘플 데이터
select *from employees;</p>
<p>/*</p>
<pre><code>데이터베이스 프로그램
- Oracle(***): 오라클, 기업용 (JAVA를 많이 씀)
- MS-SQL: 마이크로소프트, 기업용 (C++을 많이 씀)
- DB2: IBM
- MySQL(**): 오라클(인수), 개인용, 소규모, 기업용 &gt; AWS(오라클이 안되서)
- MariaDB: 개인용, 소규모, 기업용 (MySQL창업자가 만듬)
- PostreSQL(*): 개인용, 소규모, 기업용

- SQLite: 초경량, 임베디드(모바일)
- H2: 스프링부터(초경량)

Oracle
- 1 ~ 23c
- 11g이후 &gt; 거의 동일


오라클(데이터베이스) &lt;-&gt; SQL Developer(클라이언트) &lt;-&gt; SQL(언어) &lt;-&gt; 사람


SQL, Struectured Query Language
- 구조화된 질의 언어
- 데이터베이스와 대화를 하기 위한 언어

1. 데이터베이스 제작사(Oracle 등)와 독립적이다.
    - 모든 데이터베이스에서 공통으로 사용하기 위해 만들어진 언어
    - DB 제작사에서 SQL 언어를 가져다가 자신의 제품에서 사용

2. 표준 SQL, ANSI-SQL
    - 모든 DB에서 적용 가능한 SQL

3. 제조사별 SQL
    - 특정 DB에서만 적용 가능한 SQL
    - Oracle &gt; PL/SQL
    - MS-SQL &gt; T-SQL

오라클 수업 = ANSI-SQL(60%) + DB 설계(10%) + PL/SQL(30%)

관계형 데이터베이스, Relational Database, RDB
- 데이터를 표(테이블)로 저장한다. 
- SQL을 사용한다.

오라클 = 관계형 데이터베이스 + 관리 시스템
       = Relational Database Managememt System
       = RDBMS


ANSI-SQL

1. DDL
    - Data Definition Language
    - 데이터 정의어
    - 테이블, 뷰, 사용자, 인덱스, 트리거 등의 데이터베이스 오브젝트를 생성/수정/삭제하는 명령어
    - 구조를 생성/관리하는 역할(***)
    a. create: 생성
    b. drop: 삭제
    c. alter: 수정

    - 데이터베이스 관리자
    - 데이터베이스 개발자
    - 프로그래머(일부)

2. DML
    - Data Manipulation Language
    - 데이터 조작어
    - 데이터를 추가/수정/삭제/조회하는 명령어(CRUD)
    - 사용 빈도가 가장 높음
    a. select: 조회(읽기)
    b. insert: 추가(생성)
    c. update: 수정
    d. delete: 삭제

    - 데이터베이스 관리자
    - 데이터베이스 개발자
    - 프로그래며(********************)

3. DCL
    - Data Control Language
    - 데이터 제어어
    - 계정 관리, 보안 제어, 트랜잭션 처리 등..
    a. commit
    b. rollback
    c. grant: 허가하다, 승인하다
    d. revoke:취소하다, 철회하다: 이전에 부여된 권한, 허가, 특권 등을 공식적으로 없애거나 무효화하는 것

    - 데이터베이스 관리자
    - 데이터베이스 개발자
    - 프로그래며(일부)

4. DQL
    - Data Query Language
    - ML 중에서 select문을 따로 부르는 표현

5. TCL
    - Transactgion Control Language
    - DCL 중에서 commit/rollback문을 따로 부르는 표현

SQL
- SQL 구문은 대소문자를 구분하지 않는다.
- 데이터는 대소문자를 구분한다.
- 공백 문자도 영향을 주지 않는다.(엔터X) &gt; 문장 종결자(;)

- 키워드(SQL 구문) &gt; 대문자
- 식별자(테이블명, 컬럼명 등) &gt; 소문자</code></pre><p>*/
select *from tabs;  -- 현재 계정이 소유하고 있는 테이블 목록을 가져오기
SELECT *FROM TABS;
SELECT *FROM tabs;</p>
<p>-- 수업 진행
select *from tabs; -- alt + ' &gt; 대소문자 패턴 적용</p>
<p>select *from tabs where table_name = 'jobs';
select *from tabs where table_name = 'JOBS';</p>