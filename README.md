# Study Center
## Oracle DB 기반 교육센터 운영 프로젝트
### 주제
교육 센터를 운영에 필요한 제반 기능들을 통합적으로 관리하는 프로그램 

### 목적
교육 센터 제반 기능의 통합적 관리, 네트워크 활성화, 계정별 기능 이용의 편의성 증진


### 사용기술
<img src="https://img.shields.io/badge/Java-007396?style=flat&logo=Java&logoColor=white" />   <img src="https://img.shields.io/badge/Oracle-F80000?style=flat&logo=oracle&logoColor=white"/>

### 개발도구
<img src="https://img.shields.io/badge/Eclipse IDE-2C2255?style=flat&logo=eclipseide&logoColor=white"/> 


### 개발기간 및 인원
- 개발인원: 5인
- 개발기간: *2023-03-28 ~ 2023-04-07*

### 개발환경
<table>
  <tr>
    <td>OS version</td>
    <td>Windows 10, 11</td>
  </tr>
  <tr>
    <td>JAVA version(Language)</td>
    <td>JDK 11.0.2</td>
  </tr>
  <tr>
    <td>Oracle version</td>
    <td> Oracle Database 11g</td>
  </tr>
  <tr>
      <td>Database</td>
      <td>SQL Developer</td>
  </tr>
    <tr>
      <td>Modeling</td>
      <td>exERD, Draw.io</td>
  </tr>
</table>

### 코드 컨벤션
- Table, Column: 캐멀 표기법
- View, Procedure, Trigger, Function: 캐멀 표기법

### 개요
1. 관리자는 교사 정보를 등록하고 교과목 개설, 교육생 선발과 성적 처리, 시험 관리 등
교육센터의 전반적인 업무를 수행한다.
2. 교사는 교육생의 성적 정보를 관리하고 자신의 강의내역을 확인할 수 있다.
3. 교육생은 강의를 수강하며 출결을 기록하고 수강 종료한 과목의 시험을 응시한다.
4. 관리자, 교사가 과정을 선택하여 교육생의 출결을 관리할 수 있다.
5. 관리자와 교육생이 수료생의 취업 이력과 관련 통계를 확인할 수 있다.
6. 관리자, 교사, 교육생이 교육센터 주변의 맛집 정보를 조회하고 추천받을 수 있다.
D24625
### 데이터 구조
#### 1. 테이블 구분
<img src="https://img.shields.io/badge/기초 정보 테이블-54AEFF?style=flat&logo=&logoColor=white"/> : 강사, 교재, 교육생 등 가공하기 전의 기본 데이터  
<img src="https://img.shields.io/badge/사건 정보 테이블-D24625?style=flat&logo=&logoColor=white"/> : 개설 과목, 커리큘럼, 수강생 등 하나의 사건이 발생하여 생성되는 데이터   
<img src="https://img.shields.io/badge/추가 정보 테이블-8F42A8?style=flat&logo=&logoColor=white"/> : 기본적인 학원 업무 외에 추가로 구상한 기능
(학원 주변 맛집 추천, 취업 히스토리, 취업 결과, 스터디 신청)

- 관계 테이블 파악하기    
기초정보에서 하나의 '사건'이 발생하여 또 다른 사건 테이블이 만들어진다.   
(예) *지원자가 면접을 보고 합격하면 교육센터의 교육생이 된다.*   
=> <b>'지원자'</b>는 <b>기초정보</b>, <b>'면접'</b>은 <b>사건</b>, <b>'교육생'</b>은 사건을 통해서 만들어진 <b>사건 테이블</b>이다.
![6조ERD](https://github.com/0hsoyeop/Neulbom/assets/131536077/88821775-f907-4be4-bb71-3d9cab8eb582)
---
### 🧐가장 어려웠던 부분🧐
#### 표현하고자 한 상황
- 교육센터에는 과목, 과정에 대한 정보가 있다.
- 1개의 과정은 1~N개의 과목으로 구성된다.
- 개설이 확정된 과정은 테이블로 그 정보를 확인할 수 있다.

#### ❌ 문제점 ❌
- 과정에 과목이 연결되는 사건이 표현되지 않았다.
- 개설 과목에 대한 정보가 없다.
- 과목과 과정 사이에 N:N 관계가 존재한다.

#### 💡해결💡
- 어떤 과목이 새로운 과정에 배정되는 사건은  **'커리큘럼'** 테이블로 표현해야 한다.
- 과목 테이블에서도 **'개설과목'** 테이블이 만들어져야 한다.
- 과정 중 일부는 **'개설과정'** 이 되고 강의실이나 시작일 등의 정보를 가지고 있다.
- **'개설과목'** 테이블은 참조키로 이루어진 관계 테이블로 만들어진다.

### ⬇️여러번 수정을 거쳐 아래와 같이 완성하였다!⬇️

![image](https://github.com/0hsoyeop/Neulbom/assets/131536077/1455bf21-3d11-4030-a44b-dbfefb7509b6)


---

### 담당업무
- 교육생 취업 결과 관리
- 교육생 스터디 신청
- 교육생 취업 히스토리
- 더미 데이터 생성
- PL/SQL


### 구현 기능 소개
#### 1. 취업 히스토리란?
- 교육생들이 기업에 입사 지원한 이력과 스펙, 합격 여부를 나타내는 기능
- 다른 교육생들이 이 정보를 토대로 취업에 필요한 스펙을 더 구체적으로 준비할 수 있을 것

#### 2. 취업 결과
- 취업에 성공한 교육생의 직무, 기업명, 기업 규모, 취업 후 만족도를 나타내는 기능

### 취업 히스토리 View 만들고 조회하기
- 학생 이름 (*로 마스킹하여 표시)
- 학생의 스펙 정보를 학생 테이블에서 가져오기
- 자격증 있으면 정보 출력
- 교육 수료 여부를 날짜 기준으로 판별
	- 현재 날짜가 수료일 전: 수강중
	- 현재 날짜가 수료일 이후: 수료
	- 중도 탈락 날짜가 있을 경우: 중도탈락 

```sql
create or replace view vwJobHistory
as
select
	stdn.studentSeq as studentSeq, -- 학생 테이블의 학생 seq
	substr(a.name,1,1) || lpad('*', length(a.name)-2, '*') ||
    	substr(a.name, length(a.name), 1) as name,  -- 학생 이름 (*로 마스킹하여 표시)
	jh.applyDate as applyDate,
	case
		when jr.hireDate is null then '미취업'
    		when jr.hireDate is not null then to_char(jr.hireDate)
	end as hireDate,
	jh.companyName as company,
	jh.job as job,
	jh.companySize as companySize,
	jh.review as review,
	jh.pass as pass,
	case
    		when jr.satisfaction is not null then jr.satisfaction || '점'
    		when jr.satisfaction is null then '-'
	end as satisfaction,
	spec.education as education,
	spec.major as major,
	case -- 자격증 있으면 정보 출력
    		when cf.certificateName is not null then cf.certificateName
    		when cf.certificateName is null then '없음'
	end as certificate,
	c.courseTitle as courseTitle,
	case -- 교육 수료 여부를 날짜 기준으로 판별
    		when stdn.completeOrFail is null and sysdate > opc.startDate
        		and sysdate < stdn.completeOrFailDate then '수강중'
    		when stdn.completeOrFail = 1
        		and sysdate > opc.endDate then stdn.completeOrFailDate ||' (수료)'
    		when stdn.completeOrFail = 0 then stdn.completeOrFailDate || ' (중도탈락)'
    		when stdn.completeOrFail is null and sysdate < opc.startDate then '개강전'
	end as completeOrFail
from tblStudent stdn
	inner join tblSpec spec -- 학생의 스펙 정보를 학생 테이블에서 가져오기
    		on stdn.studentSeq = spec.studentSeq 
        		inner join tblApplicant a
            			on a.applicantSeq = stdn.applicantSeq
                			inner join tblOpenCourse opc
                    				on stdn.openCourseSeq = opc.openCourseSeq
                        				inner join tblCourse c
                            					on opc.courseSeq = c.courseSeq
         	                      					 inner join tblJobHistory jh
                                    						on stdn.studentSeq = jh.studentSeq
                                           						 left outer join tblJobResult jr
                                               							 on stdn.studentSeq = jr.studentSeq
                                                   							 left outer join tblCerificate cf
                                                       								 on spec.specSeq = cf.specSeq
        order by applyDate, hireDate, studentSeq, completeOrFailDate;

```




<br>
<br>

#### ⬇️취업 히스토리 조회 결과⬇️
- **지원자 정보, 입사 지원한 기업의 정보, 합격 여부, 스펙, 만족도, 교육센터 수료 과정** 등을 아래와 같이 나타내었다.
![image](https://github.com/0hsoyeop/Neulbom/assets/131536077/7dac04ea-7ab5-48e1-962d-009918a7cb27)

<br>
<br>

- **취업 히스토리 테이블 (tblJobHistroy)에 데이터 추가하는 프로시저**
	- DB에 추가할 sequence 도출
	- 학생 번호, 직무, 기업명, 기업규모, 리뷰, 합격여부, 지원일자를 입력하여 데이터를 추가할 수 있다.
```sql
-- procInsertJobHistory
create or replace procedure procInsertJobHistory(
	pstudentSeq tblStudent.studentSeq%type,
	pjob tblJobHistory.job%type,
	pcompanyName tblJobHistory.companyName%type,
	pcompanySize tblJobHistory.companySize%type,
	preview tblJobHistory.review%type,
	ppass tblJobHistory.pass%type,
	papplyDate tblJobHistory.applyDate%type
)
is
begin
	insert into tblJobHistory values ((select max(jobHistorySeq)+1 from tblJobHistory), pstudentSeq, pjob, pcompanyName, pcompanySize, preview, ppass, papplyDate);
	dbms_output.put_line('취업 히스토리를 추가하였습니다.');
end procInsertJobHistory;
 
```


<br>
<br>

### 취업 결과 View 만들고 조회하기
- 학생 이름 (*로 마스킹하여 표시)
- 학생 테이블과 취업결과 테이블에서 일치하는 학생 정보만 표시
- 학생 번호, 고용일자, 기업명 순서대로 정렬

```sql
create or replace view vwJobResult
as
select 
    stdn.studentSeq as studentSeq,
    substr(a.name,1,1) || lpad('*', length(a.name)-2, '*') ||
        substr(a.name, length(a.name), 1) as name,
    jr.hireDate as hireDate,
    jr.job as job,
    jr.company as company,
    jr.companySize as companySize,
    jr.satisfaction as satisfaction,
    c.courseTitle as courseTitle
from tblStudent stdn
    inner join tbljobResult jr
        on stdn.studentSeq = jr.studentSeq 
            inner join tblApplicant a
                on a.applicantSeq = stdn.applicantSeq
                    inner join tblOpenCourse openC
                        on stdn.openCourseSeq = openC.openCourseSeq
                            inner join tblCourse c
                                on openC.courseSeq = c.courseSeq
                                    order by studentSeq, hireDate, company;

```
<br>
<br>

#### ⬇️취업 결과 조회⬇️
- **지원자 정보, 취업날짜, 기업명, 기업규모, 취업 후 만족도, 교육센터 수료 과정** 등을 아래와 같이 나타내었다.  
![image](https://github.com/0hsoyeop/Neulbom/assets/131536077/e8d23f33-73be-425c-9da3-f57956d6ba97)

<br>
<br>

### 기업별 리뷰 통계 조회하는 프로시저
- 취업 히스토리(JobHistory) 테이블에서 기업명, 리뷰, 리뷰수를 집계하여 나타낸다.
  
```sql
create or replace procedure procCompanyReview
is
	pcompany tblJobHistory.companyName%type;
	preview tblJobHistory.review%type;
	pcount number;
	cursor vcursor
	is
	select company, review, count(*) as cnt
    	from vwJobHistory
        	group by company, review
            	order by company;
begin
	dbms_output.put_line(rpad('기업명', 15) || rpad('지원자리뷰', 15) || rpad('리뷰수', 15));
	dbms_output.put_line('----------------------------------------');
	open vcursor;
    	loop
        	fetch vcursor into pcompany, preview, pcount;
        	exit when vcursor%notfound;
        	
            dbms_output.put_line(rpad(pcompany,15) || rpad(preview, 15) || rpad(pcount || '명',15));
    	end loop;
	close vcursor;
end procCompanyReview;

```
<br>
<br>

#### ⬇️ 기업별 지원자 리뷰 통계 조회⬇️
- PL/SQL 화면에서 출력한 모습
- 기업명을 기준으로 오름차순 정렬한다.
  
![image](https://github.com/0hsoyeop/Neulbom/assets/131536077/cd03a303-f0d4-49f7-a736-26cfb3a2129b)

