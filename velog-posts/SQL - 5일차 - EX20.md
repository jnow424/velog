<p>-- ex20_join.html</p>
<p>/*</p>
<pre><code>관계형 데이터베이스 시스템이 지양하는 것들
- 테이블을 수정해야지만 고쳐지는 것들 &gt; 구조적 문제

- 1. 테이블에 기본키가 없는 상태 &gt; 데이터 조작 불가능(레코드 식별 불가능) / 중복값 구별 불가능
- 2. NULL이 많은 상태의 테이블 &gt; 공간 낭비 + SQL 작업 불편함
   / 혼자 취미가 많아 취미1 , 취미2, 취미3 나머지 사람은 취미가 1인 상태 나머지 2,3 은 빈 공간(null)
취미가 운동인 사람
where 취미 = '운동'

취미가 요리인 사람
where 취미 = '요리'   / 취미와 2개 이상인 경우
where 취미 = '%요리%'

- 3. 데이터가 중복되는 상태 &gt; 공간 낭비 + 데이터 조작 시 문제 발생(동기화)
ex) 개인정보는 동일, 학년이 해마다 바뀔 경우, 1학년, 2학년, 3학년 정보를 중복으로 가지고 있음 

- 4. 하나의 속성값이 원자값이 아닌 상태 &gt; 더 이상 쪼개지지 않는 값을 넣어야 한다.</code></pre><p>*/</p>
<p>-- 직원 정보 테이블
-- 직원(번호, 직원명, 급여, 거주지, 담당프로젝트)
create table tblStaff
(
    seq number primary key, -- 직원번호(PK)
    name varchar2(30) not null, -- 직원명
    salary number not null, -- 급여
    address varchar2(300) not null, -- 거주지
    project varchar2(300) null -- 담당프로젝트
);</p>
<p>insert into tblStaff (seq, name, salary, address, project)
    values (1,'홍길동', 300, '서울시', '홍콩 수출'); 
insert into tblStaff (seq, name, salary, address, project)
    values (2,'아무개', 250, '인천시', 'TV 광고');
insert into tblStaff (seq, name, salary, address, project)
    values (3,'하하하', 350, '의정부시', '매출 분석');</p>
<p>select * from tblStaff;</p>
<p>-- '홍길동'에게 담당 프로젝트가 1건 추가 &gt; '고객 관리'
update tblStaff set
    project = project || ',고객 관리'
        where seq = 1;</p>
<p>-- '아무개'에게 담당 프로젝트가 1건 추가 &gt; '게시판 관리'
insert into tblStaff (seq, name, salary, address, project)
    values (4,'아무개', 250, '인천시', '게시판 관리');</p>
<p>select * from tblStaff;
-- ex20_join.html</p>
<p>select * from tblStaff;</p>
<p>-- 원인: 테이블 스키마(구조)가 잘못된 상태
-- 해결: 테이블 재구성</p>
<p>drop table tblStaff;
drop table tblProject;</p>
<p>-- 관계(Relationship)
-- 부모테이블의 PK = 자식테이블의 FK</p>
<p>-- 테이블 생성 순서
-- 부모테이블 &gt; 자식테이블</p>
<p>-- 테이블 삭제 순서
-- 자식테이블 &gt; 부모테이블
drop table tblStaff; -- ORA-02449: 외래 키에 의해 참조되는 고유/기본 키가 테이블에 있습니다
drop table tblProject;</p>
<p>create table tblStaff -- 부모 테이블
(
    seq number primary key,        -- 직원번호(PK)
    name varchar2(30) not null,    -- 직원명
    salary number not null,         -- 급여
    address varchar2(300) not null -- 거주지
);</p>
<p>create table tblProject                                 -- 자식 테이블
(
    seq number primary key,                             -- 프로젝트번호(PK)
    project varchar2(300) not null, --프로젝트명(***)
    staff_seq number not null references tblStaff(seq) -- 담당 직원 번호(FK) / 제약조건을 참조한다 tblStaff(seq)을
);</p>
<p>insert into tblStaff (seq, name, salary, address) values (1,'홍길동', 300, '서울시'); 
insert into tblStaff (seq, name, salary, address) values (2,'아무개', 250, '인천시');
insert into tblStaff (seq, name, salary, address) values (3,'하하하', 350, '의정부시');</p>
<p>insert into tblProject (seq, project, staff_seq) values (1, '홍콩수출', 1); --홍길동(1)
insert into tblProject (seq, project, staff_seq) values (2, 'TV광고', 2); --아무개(2)
insert into tblProject (seq, project, staff_seq) values (3, '매출분석', 3); --하하하(3)</p>
<p>insert into tblProject (seq, project, staff_seq) values (4, '고객관리', 1); --홍길동(1)
insert into tblProject (seq, project, staff_seq) values (5, '게시판분석', 2); --아무개(2)    </p>
<p>select * from tblStaff;
select * from tblProject;</p>
<p>-- 'TV광고' (tblProject) 담당자!! &gt; 담당직원 이름(tblStaff)</p>
<p>select name from tblStaff where seq = (select staff_seq from tblProject where Project = '고객유치'); -- 확인해보기</p>
<p>-- Case A. 신입 사원 입사 &gt; 신입 프로젝트 배정
-- A.1 신입 사원 추가 &gt; 성공!!
insert into tblStaff (seq, name, salary, address) values (4,'호호호', 250, '성남시');</p>
<p>-- A.2 신규 프로젝트 배정 &gt; 성공!!
insert into tblProject (seq, project, staff_seq) values (6, '자재매입', 4); --호호호(4)</p>
<p>-- A.3 신규 프로젝트 배정 &gt; 트러블
-- ORA-02291: 무결성 제약조건(HR.SYS_C008451)이 위배되었습니다- 부모 키가 없습니다
insert into tblProject (seq, project, staff_seq) values (7, '고객유치', 5); --??(5)</p>
<p>-- Case B. '홍길동' 퇴사
-- B.1 ' 홍길동' 삭제 &gt; 에러(X) &gt; 논리 에러
-- ORA-02292: 무결성 제약조건(HR.SYS_C008451)이 위배되었습니다- 자식 레코드가 발견되었습니다
delete from tblStaff where seq = 1;</p>
<p>-- B.2 '홍길동' 삭제 전 &gt; 업무 인수 인계(위임)
update tblProject set staff_seq = 2 where staff_seq = 1;</p>
<p>-- B.3 퇴사
delete from tblStaff where seq = 1;</p>
<p>select * from tblStaff;
select * from tblProject;</p>