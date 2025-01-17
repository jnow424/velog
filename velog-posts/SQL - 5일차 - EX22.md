<p>-- ex22_union.sql</p>
<p>/*</p>
<pre><code>A = { 10, 20, 30 }
B = { 30, 40, 50 }
A U B = { 10, 20, 30, 40, 50 }

조인: 테이블 x 테이블
유니온: 테이블 + 테이블

유니온, union
- 테이블과 테이블을 합치는 기술
- 반드시 두 결과셋의 스키마가 동일해야 한다./컬럼구조가 같아야 한다.</code></pre><p>*/</p>
<p>select * from tblMen
union
select * from tblWomen;</p>
<p>-- ORA-01789: 질의 블록은 부정확한 수의 결과 열을 가지고 있습니다.
select * from tblInsa
union
select * from tblTodo;</p>
<p>-- 이건 합쳐져요~ 근데~ 딱봐도 하면 안되겠죠? 어거지로 맞출수 있겠....
select name, ibsadate from tblInsa
union
select title, adddate from tblTodo;</p>
<p>-- 회사 &gt; 게시판 &gt; 부서별 게시판
-- DB는 책과 같다 &gt; 순서대로 찾는다 &gt; 앞부분은 빠르게 찾는다 &gt; 뒷 부분은 늦는다.
select * from 영업부게시판;
select * from 총무부게시판;
select * from 개발부게시판;</p>
<p>-- 사장님 &gt; 모든 부서의 게시물 &gt; 한번에 열람 &gt; UNION 
select * from 영업부게시판
union
select * from 총무부게시판
union 
select * from 개발부게시판;</p>
<p>-- 야구선수 &gt; 공격수, 수비수
select 이름, 체중, 키, 타율 from 공격수;
select 이름, 체중, 키, 방어율 from 수비수;</p>
<p>-- '홍길동' 선수</p>
<p>select 이름, 체중, 키 from 공격수
union
select 이름, 체중, 키 from 수비수;</p>
<p>select * from(
                select 이름, 체중, 키 from 공격수
                union
                select 이름, 체중, 키 from 수비수)where 이름 = '홍길동';</p>
<p>create table tblAAA (
    name varchar2(30) primary key,
    color varchar2(30) not null
);</p>
<p>create table tblBBB (
    name varchar2(30) primary key,
    color varchar2(30) not null
);</p>
<p>insert into tblAAA values ('강아지', '검정');
insert into tblAAA values ('고양이', '노랑');
insert into tblAAA values ('토끼', '갈색');
insert into tblAAA values ('거북이', '녹색');
insert into tblAAA values ('송아지', '회색');</p>
<p>insert into tblBBB values ('강아지', '검정');
insert into tblBBB values ('고양이', '노랑');
insert into tblBBB values ('호랑이', '주황');
insert into tblBBB values ('사자', '회색');
insert into tblBBB values ('노루', '검정');</p>
<p>select * from tblAAA;
select * from tblBBB;</p>
<p>-- union &gt; 수학의 집합 &gt; 중복 제거
select * from tblAAA
union
select * from tblBBB;</p>
<p>-- union all &gt; 중복 허용
select * from tblAAA
union all
select * from tblBBB;</p>
<p>select * from tblAAA
union all
select distinct * from tblBBB;</p>
<p>select distinct * from
    (select * from tblAAA
    union all
    select * from tblBBB);</p>
<p>-- intersect &gt; 교집합
select * from tblAAA
intersect
select * from tblBBB;</p>
<p>-- minus &gt; 차집합
-- A &gt; B
select * from tblAAA
minus
select * from tblBBB;</p>
<p>-- B &gt; A
select * from tblBBB
minus
select * from tblAAA;</p>
<p>-- 관계대수 연산 </p>