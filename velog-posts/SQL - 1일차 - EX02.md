<p>-- ex02_datatype.sql</p>
<p>/*</p>
<pre><code>관계형 데이터베이스
- 프로그래밍 요소 제공(X)
- SQL &gt; 대화형 언어 &gt; 문장 단위의 작업으로 구성된다. 
- 자료형 &gt; 테이블 정의할 때 사용(= 컬럼의 자료형)


ANSI-SQL 자료형
- 오라클 자료형

1. 숫자형
    - 정수, 실수
    a. number
        - (유효자리)38자리 이하의 숫자를 표현하는 자료형
        - 5~22byte
        - 1x10^130 ~ 9.9999X10^125

        - number: 정수 or 실수(모든영역)
        - number(precision): 전체 자리수 &gt; 정수만 저장 ex) number(3)
        - number(precision, scale): 전체 자리수 + 소수이하 자리수 &gt; 정수/실수 저장

2. 문자형
    - 문자 + 문자열
    - char vs nchar &gt; n의 의미? &gt; national(유니코드)
    - char vs varchar &gt; var의 의미? &gt; 공간 관리

    a. char
        - 고정 자릿수 문자열 &gt; 무조건 10자리를 확보. 남는자리는 스페이스로 채워라
        - char(n): 최대 n자리 문자열, n(바이트) &gt; UTF-8
            - char(n byte) &gt; 바이트
            - char(n char) &gt; 문자수
       - 최소크기: 1바이트
       - 최대크기: 2000바이트

    b. nchar
        - 고정 자릿수 문자열
        - nchar(n): 최대 n자리 문자열, n(문자수) &gt; UTF-16
        - 최소크기: 1문자
        - 최대크기: 1000문자

    c. varchar2(오라클이 먼저 사용하던 이름이어서 똑같은 이름을 사용하기 위해 2를 붙임) 
        - varchar(variable char, 바차)
        - 가변 자릿수 문자열 &gt; 삽입된 데이터 크기만큼의 공간만 차지한다. 나머지 7칸을 반환한다.
        - varchar2(n): 최대 n자리 문자열, n(바이트)
        - 최소크기: 1바이트
        - 최대크기: 4000바이트

    d. nvarchar2
     - varchar(variable char, 바차)
        - 가변 자릿수 문자열 &gt; 삽입된 데이터 크기만큼의 공간만 차지한다. 나머지 7칸을 반환한다.
        - varchar2(n): 최대 n자리 문자열, n(문자수)
        - 최소크기: 1바이트
        - 최대크기: 2000바이트

    e. clob, nclop
        - 대용량 텍스트
        - character large object
        - 최대 128TB
        - 참조형


3. 날짜/시간형
    a. date(*)
        - 년월일시분초
        - 기원전 4712년 1월 1일 ~ 9999년 12월 31일

    b. timestamp
        - 년월일시분초 + 밀리초 + 나노초

    c. interval
        - 시간
        - 틱값 저장용

4. 이진 데이터형
    - 비 텍스트 데이터
    - 이미지, 동영상, 음악, 실행파일, 압축 파일 등.....
    - 게시판 &gt; 첨부파일 &gt; 파일명만 DB에 저장
    a. blob, binary large object
        - 최대 128TB


결론 
1. 숫자 &gt; number
2. 문자열 &gt; varchar2
3. 날짜 &gt; date


테이블 선언(생성)

create table 테이블명 (
    컬럼 정의,
    컬럼 정의, 
    컬럼 정의, 
    컬럼명 자료형
);</code></pre><p>*/</p>
<p>-- 수업: 테이블명 &gt; 헝가리언 표기법
drop table tblType;</p>
<p>create table tblType (
    -- 컬럼 정의
    -- num NUMBER
    -- num number(3)
    -- num number(4,2) -- -99.99 ~ 99.99</p>
<pre><code>-- txt char(10) -- 오라클은 어떤 인코딩 방식을 사용하는가? UTF-8
-- txt nchar(10)

txt1 char(10),
txt2 varchar2(10)</code></pre><p>);</p>
<p>select *from tabs;
desc tblType;-- 간단한 확인</p>
<p>-- 데이터 추가하기
-- insert into 테이블명 (컬럼명) values(값);
insert into tblType (num) values (100); -- num = 100
insert into tblType (num) values (3.14);
insert into tblType (num) values (3.99); -- 반올림 유무(O)
insert into tblType (num) values (123456789);
insert into tblType (num) values (12345678901234567890123456789012345678);
insert into tblType (num) values (1234567890123456789012345678901234567890);
insert into tblType (num) values (123456789012345678901234567890123456789012345);
insert into tblType (num) values (-123456789012345678901234567890123456789012345);
insert into tblType (num) values (999);
insert into tblType (num) values (-999);
insert into tblType (num) values (1000);    --(X)
insert into tblType (num) values (-1000);   --(X)
insert into tblType (num) values (99.99);
insert into tblType (num) values (-99.99);
insert into tblType (num) values (99.99);</p>
<p>insert into tblType (txt) values ('A'); -- 문자열 리터럴
insert into tblType (txt) values ('ABC');
insert into tblType (txt) values ('ABCDEFGHIJ');
insert into tblType (txt) values ('ABCDEFGHIJk');   -- (x)
insert into tblType (txt) values ('가');
insert into tblType (txt) values ('가나다');
insert into tblType (txt) values ('가나다라');  --(x)
insert into tblType (txt) values ('가나다a');
insert into tblType (txt) values ('가나다라마바사아자차');
insert into tblType (txt) values ('가나다라마바사아자차카');</p>
<p>insert into tblType (txt1, txt2) values ('ABC', 'ABC');</p>
<p>-- 데이터 가져오기
-- select *from 테이블명;</p>
<p>select *from tblType;</p>