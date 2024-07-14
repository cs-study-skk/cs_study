# 👉SQL Injection(SQL 인젝션, SQL 삽입)
- 악의적인 사용자가 보안상의 취약점을 이용해 **임의의 SQL 쿼리문을 데이터베이스에 주입해 비정상적인 동작**을 하도롣 조작하는 공격 기법
- 공격이 비교적 쉬운 편이며, 공격에 성공할 경우 큰 피해를 입힐 수 있다

# 공격 종류 및 방법
## 1 ) Error based SQL Injection
가장 대중적인 기법으로 SQL 쿼리에 **고의적으로 오류를 발생시키고 이때 출력되는 에러의 내용으로 필요한 정보를 찾아**내는 공격 기법

### 로그인 공격 예시
로그인 페이지가 있고, 로그인을 할 때 USER_ID와 INPUT_PW를 입력받아 로그인이 진행된다고 가정

- 기본 쿼리문
```sql
SELECT user FROM Users WHERE uid = 'USRE_ID' AND upw = 'INPUT_PW';
```
- 공격 예시 : 로그인 창의 ID 부분에 'OR 1 = 1 --를 입력
```sql
SELECT user FROM Users WHERE uid = '' OR 1 = 1 --USRE_ID' AND upw = 'INPUT_PW';
```
-> --를 이용해 그 뒤의 모든 쿼리문을 주석처리해주게 됨.<br>
-> Users 테이블에 있는 모든 정보를 조회


## 2 ) Union base SQL Injection
- UNION 키워드를 사용하여 원래의 요청에 추가 정보를 얻는 공격 기법
- UNION 하려는 두 테이블의 **컬럼 수**와 **데이터 형식**이 같아야 한다.

### 게시글 조회 공격 예시
게시판이 있고, 게시글을 검색할 때 INPUT을 받아 검색이 진행된다고 가정

- 기본 쿼리문
```sql
SELECT * FROM Board WHERE title LIKE '%INPUT%' OR contents '%INPUT%'
```
- 공격 예시 : 검색 창에 'UNION SELECT null, id, passwd FROM User를 -- 입력
```sql
SELECT * FROM Board WHERE title LIKE '%' UNION SELECT null, id, passwd FROM Users --%' OR contents '%INPUT%'
```

-> Injection이 성공하게 되면 사용자의 개인 정보가 게시글과 함께 보이게 된다.


## 3) Blind SQL Injection
- 에러가 발생되지 않는 사이트에서 데이터 베이스로부터 특정한 값이나 데이터를 전달받지 않고, 단순히 **참과 거짓의 정보만 알 수 있을 때 사용**하는 공격 기법 
- limit, SUBSTR, ASCII를 사용해서 조건이 참이면 페이지가 정상적으로 출력되고 그렇지 않을 경우 출력되지 않음으로 구분

### Boolean 기반 공격 예시
- 기본 쿼리문
```sql
SELECT user FROM Users WHERE uid = 'USRE_ID' AND upw = 'INPUT_PW';
```
- 공격 예시 : 로그인 폼에 DB 테이블 명을 알아내기 위한 쿼리문을 주입, 이때 임의로 가입한 idd3이라는 아이디와 함께 구문을 주입
```sql
SELECT * FROM Users WHERE uid = 'idd3' and ASCII(SUBSTR((SELECT name FROM information_schema.tables WHERE table_type='base table' limit 0,1),1,1)) > 100 -- USRE_ID' AND upw = 'INPUT_PW';
```
-> 찾고자 하는 테이블의 테이블 명을 ASCII(SUBSTR()) 로 한 글자씩 가져와 아스키값으로 비교해가며 찾는다.


### Time 기반 공격 예시
- 기본 쿼리문
```sql
SELECT user FROM Users WHERE uid = 'USRE_ID' AND upw = 'INPUT_PW';
```
- 공격 예시
```sql
SELECT user FROM Users WHERE uid = 'idd3' OR (LENGTH(DATABASE())=1 AND SLEEP(2)) -- USRE_ID' AND upw = 'INPUT_PW';
```
-> `LENGTH(DATABASE())= 1` 에서 1을 변경해 시도하여 SLLEP(2)가 동작하는지 여부로 데이터베이스의 길이를 반환한다.



## 4 ) Stored Procedure based SQL Injection
- 공격자가 *저장 프로시저에 대한 접근 권한(시스템 권한)을 가져야 실행이 가능해 공격 난이도가 높다. 
- 공격 성공 시 직접적인 피해를 입힐 수 있다.
  
> **저장 프로시저(Stored Procedure)** <br>
> 일련의 쿼리들을 모아 하나의 함수처럼 사용하기 위한 쿼리 집합


## 5) Mass SQL Injection
- 다량의 SQL Injection 공격 (DDos)
- 사용자들이 변조된 사이트에 접속하면 좀비PC로 감염시키는 방법


# 대응 방안
### 1 ) 입력값 검증
사용자의 입력을 받을 때 검증 로직을 추가하여 값이 유효한지 검증한다.

- ', ", #, --, = 등 특수문자와 명령어 필터링
- 데이터 길이 제한

### 2 ) 서버 보안 및Error Message 노출금지
- 데이터 베이스 권한을 제한하고 신뢰 가능한 네트워크와 서버에 대해서만 접근을 허용한다.
- SQL 서버 오류 발생 시, 에러가 발생한 쿼리문이나 에러 관련 내용에 해당하는 에러 메시지를 볼 수 없도록 한다.

### Reference
https://velog.io/@sukyeongs/Database-SQL-Injection
https://velog.io/@33bini/DB-SQL-Injection
