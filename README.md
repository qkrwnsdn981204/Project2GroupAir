### 그룹웨어 기본 연동 기능 및 메시지 봇 구현

#### **● 프로젝트 명** : Group Air

#### **● 프로젝트 설명** : 항공사 그룹웨어 시스템

#### **● 프로젝트 소개**

> 항공에서 근무하는 직원이 출결 및 게시판 사용, 항공편 조회 등을 위한 사이트
> 직원이 명확하게 이해하기 쉽고 부서 간 의사소통이 원활하게 기능하는 조직도 구현
> 대표이사, 부장, 사원 권한에 따라 관여할 수 있는 부분 제한
> 한 눈에 알아볼 수 있는 UI 및 사용가치 있는 설계

#### **● 프로젝트 파일명** : Project23TeamGroupAir

#### **● 팀원**

> 박준우 (팀원) : 권한별 INDEX 페이지, 근태 관리 (출퇴근, 휴가), 급여 관리, 항공편관리(kakao map)

> 손** (팀장) : DB 설계, CI/CD, 로그인, 사원관리, Oauth, Mail, 조직연동

> 서** (팀원) : 회사일정, 게시판관리

> 정** (팀원) : 결제 관리

> 조** (팀원) : Layout(index), OpenWeather API, 부서관리, ChatBot

- **Preview**<br>
    - 권한별 INDEX
      <br>
      <img src="https://github.com/qkrwnsdn981204/ParkJunwooProjects/assets/154858222/f855201b-7fcd-4611-ae06-9ff412721c09" width="800" height="400"/>
      <br>
      <br>
    - 출퇴근
      <br>
      <img src="https://github.com/qkrwnsdn981204/ParkJunwooProjects/assets/154858222/4fd29a48-92ca-4acf-adf3-f5a99cde90e0" width="800" height="400"/>
      <br>
      <br>
    - 휴가
      <br>
      <img src="https://github.com/qkrwnsdn981204/ParkJunwooProjects/assets/154858222/a6e2b3c5-559c-42ac-9245-673f17198152" width="800" height="400"/>
      <br>
      <br>
    - 급여 관리
      <br>
      <img src="https://github.com/qkrwnsdn981204/ParkJunwooProjects/assets/154858222/a41bcd92-6f94-48f8-a0b2-4fd68004d824" width="800" height="400"/>
      <br>
      <br>
    - 항공편 관리
      <br>
      <img src="https://github.com/qkrwnsdn981204/ParkJunwooProjects/assets/154858222/169e7572-a877-40d7-a0ce-4b9ea0b5485d" width="800" height="400"/>

<details>

<summary> 기술 스택 </summary>

| 카테고리       | 요소                                                                                                                                     |
|------------|----------------------------------------------------------------------------------------------------------------------------------------|
| 프로그래밍 언어   | JAVA                                                                                                                                   |
| 개발 툴       | IntelliJ                                                                                                                               |
| 프레임워크      | Spring Boot 2.7.11                                                                                                                     |
| 라이브러리 및 DI | Spring WEB(MVC), Thymeleaf, Spring Data JPA, Lombok, SpringSecurity5 <br/>, websocket, validation, OAuth2, security, komoran, queryDsl |
| 데이터베이스     | MySql8                                                                                                                                 |
| ORM        | Spring Data JPA (JAVA(SQL))                                                                                                            |
| 템플릿 엔진     | Thymeleaf (HTML + Data)                                                                                                                |
| Frontend   | css, javaScript, html, ajax                                                                                                            |
| API        | OpenWeather API, kakao Map API                                                                                                         |
| 설정         | application.yml, application-oauth2.yml                                                                                                |

</details>

<details>

<summary> 프로젝트 일정 </summary>

![img.png](images/Project2/project2plan.png)

</details>

<details>

<summary> ER 다이어그램 </summary>

![img.png](images/Project2/project2ERD.png)

</details>

<details>

<summary> 기능 구현 </summary>

### 권한별 INDEX

| **No** | **권한**      | **보여지는 기능**                                 |
|--------|-------------|---------------------------------------------|
| 1      | ADMIN(관리자)  | 근태관리, 부서관리, 회사 전체 일정, 게시판 관리, 항공편 관리, 직원 관리 |
| 2      | MANAGER(부장) | 내 출근기록, 부서 인원, 오늘의 일정, 내 결재함, 내 휴가, 내 정보    |
| 3      | MEMBER(사원)  | 내 출근기록, 자신 부서, 오늘의 일정, 내게 온 결재, 항공편, 내 정보   |

### 근태관리

| **No** | **기능** | **설명**                                                                                        |
|--------|--------|-----------------------------------------------------------------------------------------------|
| 1      | 조회     | 출퇴근, 휴가 리스트 페이징 기능 , 사원 이름을 통한 조회                                                             |
| 2      | 현황     | 출근, 퇴근, 지각, 조퇴 등 상태별 인원 출근 현황 출력                                                              |
| 3      | 출퇴근    | 출근 혹은 퇴근시 시간에 따라 해당 직원 상태 변경, 총 근무시간 출력                                                       |
| 4      | 휴가 등록  | 출발일과 복귀일을 입력하여 휴가 등록 <br> 자동 휴가 일수 계산 <br> 해당 인원이 겹치는 휴가 등록시 예외 처리 <br> 유효성 검사                |
| 5      | 자동화    | 회원가입 시 근태 자동생성 <br> 휴가 인원 및 휴가 복귀인원 상태 변경  <br> 다음날이 되면 휴가인 인원 제외 '미출근'으로 상태 변경 <br> 지난 휴가 삭제 |

### 급여관리

| **No** | **기능**  | **설명**                                    |
|--------|---------|-------------------------------------------|
| 1      | 직급 별 급여 | 직급별 월급 차등 저장                              |
| 2      | 추가 수당   | 월 별 추가 근무 시간 계산 후 직급별 추가수당 차등 저장          |
| 3      | 급여 조정   | 기본급여 변경을 통한 급여 조정                         |
| 4      | 자동화     | 급여일 초과시 새로운 급여 레코드 생성 <br> 회원가입 시 급여 자동생성 |

### 항공편 관리(kakaoMap API)

| **No** | **기능**     | **설명**                                                                |
|--------|------------|-----------------------------------------------------------------------|
| 1      | 항공편 등록     | kakao 주소 API를 이용하여 출발지 목적지 저장 <br> MANAGER 권한만 기장으로 등록 가능 <br> 유효성 검사 |
| 2      | 항공편 수정, 삭제 | 항공편 정상 지연 수정기능, 항공편 삭제 기능                                             |
| 3      | 항공편 정보     | kakao map api를 이용하여 항공편 거리 출발지 도착지 간략 표시                              |
| 4      | 기장 관리      | 기장들의 자신의 전체 항공편과 오늘의 항공편 리스트 출력                                       |
| 5      | 자동화        | 항공편 상태에 따른 항공편 리스트 표시 변경 <br> 운행중인 항공편 운행중 표시 <br> 지난 항공편 자동 삭제       |

</details>



**[⬆ 위로 가기](#목차)**
