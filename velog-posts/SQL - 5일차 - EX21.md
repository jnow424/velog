<p>--ex21_view.sql</p>
<p>/*</p>
<pre><code>view, 뷰
- 데이터베이스 객체 중 하나(테이블, 제약사항, 시퀀스, 뷰..)
- 가상 테이블, 뷰 테이블 등..

CREATE [OR REPLACE] VIEW 뷰이름
AS
SELECT문;</code></pre><p>*/
create or replace view vwInsa
as
select * from tblInsa;</p>
<p>select * from vwInsa;</p>
<p>-- 자주 반복 업무 &gt; '영업부' + '서울' + '과장급 이상' &gt; 명단 조회
create or replace view vwMy -- 밑의 셀렉트문을 vwMy로 묶어서 쉽게  볼 수 있다
as
select
    *
from tblInsa
    where buseo = '영업부' and city = '서울' and jikwi in ('과장', '부장');</p>
<p>select * from vwMy;</p>
<p>-- 비디오 대여점 사장 &gt; 날마다 하는 업무
create or replace view vwCheck
as
select 
    m.name as 회원,
    v.name as 비디오,
    r.rentdate as 언제,
    r.retdate as 반납,
    g.period as 대여기간,
    r.rentdate + g.period as 반납예정일,
    --floor(sysdate - (r.rentdate + g.period)) as 연체기간
    g.price as 대여가격, 
    case
        when r.rentdate + g.period &lt; r.retdate then '연체'
        when r.retdate is null then '미반납'
        else '정상반납'
    end as 상태,
    case
        when r.rentdate + g.period &lt; r.retdate then to_char (floor(r.retdate - (r.rentdate + g.period)))
        when r.retdate is null then to_char (floor(sysdate - (r.rentdate + g.period)))
        else '없음'
    end as 연체기간,
    case
        when r.rentdate + g.period &lt; r.retdate then floor(r.retdate - (r.rentdate + g.period)) * g.price * 0.1</p>
<pre><code>    when r.retdate is null then floor(sysdate - (r.rentdate + g.period)) * g.price * 0.1

    else 0
end as 연체료</code></pre><p>from tblGenre g
    inner join tblVideo v
        on g.seq = v.genre
            inner join tblRent r
                on v.seq = r.video
                    inner join tblMember m
                        on m.seq = r.member;</p>
<p>select * from vwCheck;</p>
<p>-- 서울 직원 &gt; 명단</p>
<p>-- 뷰는 셀렉트의 복사본이 아니다. 
-- 셀렉트 쿼리를 저장한것</p>
<p>create or replace view vwSeoul
as
select * from tblInsa where city = '서울'; -- 문장을 복사한것이 뷰</p>
<p>select * from vwSeoul; -- 실명 뷰
select * from (select * from tblInsa where city = '서울'); -- 익명 뷰 ----확인해보기</p>
<p>delete from tblInsa where num = 1001;</p>
<p>rollback;
/*</p>
<pre><code>뷰 사용 시 주의점!!
1. select &gt; 실행O &gt; 뷰는 읽기 전용이다. == 읽기 전용 테이블
2. insert &gt; 경우에 따라 실행O &gt; 절대 사용 금지!!
3. update &gt; 경우에 따라 실행O &gt; 절대 사용 금지!!
4. delete &gt; 경우에 따라 실행O &gt; 절대 사용 금지!!</code></pre><p>*/</p>
<p>create or replace view vwTodo
as
select * from tblTodo; -- 테이블 1개 &gt; 단순뷰</p>
<p>select * from vwTodo;
insert into vwTodo (seq, title, adddate, completedate)
    values (21, '러닝 10km 뛰기', sysdate, null);</p>
<p>create or replace view vwSales
as
select 
    c.name, s.item
from tblCustomer c
    inner join tblSales s
        on c.seq = s.cseq; -- 테이블 2개 이상 &gt; 복합뷰</p>
<p>select * from vwSales; --1. select
insert into vwSales (name, item) values ('호호호', '맥북'); -- 확인 하기</p>
<p>-- 신입 사원 &gt; 담당 없무 &gt; 전직원 문자 메시지 발송 &gt; 전직원 연락처?
-- 데이터베이스 로그인 &gt; 계정 배정 &gt; system(X), hr(O) &gt; hr 사용하세요~
-- 신입 사원용 계정 생성 &gt; hong &gt; vwTel에 대한 접근 권한 허용 </p>
<p>select * from tblInsa;</p>
<p>create or replace view vwTel
as
select name, buseo, jikwi, tel from tblInsa;</p>
<p>select * from vwTel;</p>
<p>-- 프로젝트
-- 테이블 &gt; 5개(tblMember, tblBoard, tblLog, tblHistory, tblPrice)
-- 뷰 &gt; 5개 이상 ~ 수십개 / 쿼리를 작성해두는 것이 뷰를 만든건가? 실행하면 뷰를 만드는 건가? 그럼 뷰를 여러번 실행하면 ???</p>