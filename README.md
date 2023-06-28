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

### 데이터 구조
![6조ERD](https://github.com/0hsoyeop/Neulbom/assets/131536077/88821775-f907-4be4-bb71-3d9cab8eb582)

### 담당업무
- 교육생 취업 결과 관리
- 교육생 스터디 신청
- 교육생 취업 히스토리
- 더미 데이터 생성
- PL/SQL

---
### 구현기능
- 취업 히스토리 테이블 (tblJobHistroy)에 데이터 추가하는 프로시저
```sql
-- procInsertJobHistory
exec procInsertJobHistory(<교육생seq>, '<직무>', ‘<기업명>', '<기업규모>', '<지원후기>', '<합격불합격>', '<지원날짜>');
```

#### 취업 히스토리 뷰 생성
```sql
create or replace view vwJobHistory
as
select
	stdn.studentSeq as studentSeq,
	substr(a.name,1,1) || lpad('*', length(a.name)-2, '*') ||
    	substr(a.name, length(a.name), 1) as name,ㅍ
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
	case
    	when cf.certificateName is not null then cf.certificateName
    	when cf.certificateName is null then '없음'
	end as certificate,
	c.courseTitle as courseTitle,
	case
    	when
        	stdn.completeOrFail is null and sysdate > opc.startDate
        	and sysdate < stdn.completeOrFailDate then '수강중'
    	when stdn.completeOrFail = 1
        	and sysdate > opc.endDate then stdn.completeOrFailDate ||' (수료)'
    	when stdn.completeOrFail = 0 then stdn.completeOrFailDate || ' (중도탈락)'
    	when stdn.completeOrFail is null and sysdate < opc.startDate then '개강전'
	end as completeOrFail
from tblStudent stdn
	inner join tblSpec spec
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
#### 취업히스토리 조회
![취업결과조회](https://github.com/0hsoyeop/Neulbom/assets/131536077/09920069-9754-4195-8dab-4bed0f5b9287)

