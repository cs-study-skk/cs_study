# 👉SQL (관계형 DB)
![image](https://github.com/user-attachments/assets/c612e61e-a746-476e-9dfc-f87eb6d4187a)

## 정의
- Structured Query Language (구조화 된 쿼리 언어)
- 고정된 행, 열로 구성된 테이블에 데이터를 저장하는 데이터베이스
- MySQL, Oracle, MariaDB, PostgresSQL 등이 있다.

## 특징
- 데이터는 **정해진 데이터 스키마에 따라 테이블에 저장**된다.
- 데이터는 **관계를 통해 여러 테이블에 분산**된다.

### 1) 엄격한 스키마
데이터는 Table(테이블)에 record(레코드)로 저장되면, 각 테이블에는 명확하게 정의된 structure(구조)가 있다.<br>
구조는 필드의 이름과 데이터 유형으로 정의된다. -> 스키마를 준수하지 않는 레코드는 추가할 수 없다.<br>

### 2) 관계
![image](https://github.com/user-attachments/assets/a2f7088c-5fc3-4153-899a-fe5c456f8e05)

하나의 테이블에서 중복 없이 하나의 데이터만을 관리하기 때문에, 다른 테이블에서 부정확한 데이터를 다룰 위험이 없어진다.

## SQL 장점
- 명확하게 정의된 스키마, 데이터 무결성 보장
- 관계는 각 데이터를 중복없이 한번만 저장

## SQL 단점
- 덜 유연함. 데이터 스키마를 사전에 계획하고 알려줘야 함. (나중에 수정하기 힘듦)
- 관계를 맺고 있어서 조인문이 많은 복잡한 쿼리가 만들어 질 수 있음
<br/><br/>

# 👉NoSQL (비관계형 데이터베이스)
![image](https://github.com/user-attachments/assets/13524867-7893-4435-a9ed-08c29cd98f23)

## 정의
- 관계형 DB의 반대 즉, 스키마도 없고, 관계도 없다.
- Redis, MongoDB 등이 있다.

## 특징
![image](https://github.com/user-attachments/assets/f6b601a6-1c69-49f9-9ee0-a7fc67c30e40)

- 레코드는 **documents(문서)**, 테이블은 **collection(컬렉션)**이라고 부른다.
- 다른 구조의 데이터를 같은 컬렉션에 추가할 수 있다.
- 문서는 JSON 데이터와 비슷한 형태를 가지고 있다.
- 관련 데이터를 관계형 데이터베이스처럼 여러 테이블에 나누어 담지 않고 동일한 컬렉션에 넣는다. (조인이 필요없다)

## NoSQL 장점
- 스키마가 없어서 유연함. 언제든지 저장된 데이터를 조정하고 새로운 필드 추가 가능
- 데이터는 애플리케이션이 필요로 하는 형식으로 저장됨. 데이터 읽어오는 속도가 빨라짐
- 수직 및 수평 확장이 가능

## NoSQL 단점
- 유연성으로 인해 데이터 구조 결정을 미루게 될 수 있음
- 데이터 중복을 계속 업데이트 해야 함
- 데이터가 여러 컬렉션에 중복되어 있기 때문에 수정 시 모든 컬렉션에서 수행해야 함

<br/><br/>
> ### Scaling (확장)
> ![image](https://github.com/user-attachments/assets/7b3b17e3-1819-4fe0-bdb3-23f534178586)<br/>
> **Horizontal(수평적) 확장**
> - 더 많은 서버가 추가되고, 데이터베이스가 전체적으로 분산됨을 의미
> - 따라서 하나의 데이터베이스에서 작동하지만 여러 호스트에서 작동
> 
> **Vertical(수직적) 확장**
> - 단순히 데이터베이스 서버의 성능을 향상시키는 것 (ex. CPU 업그레이드)
> - 데이터 저장 방식으로 인해 SQL 데이터베이스는 일반적으로 수직적 확장만 지원


<br/><br/>

# SQL VS NoSQL
### SQL 데이터베이스 사용이 더 좋을 때
- 관계를 맺고 있는 데이터가 자주 변경되는 애플리케이션의 경우
- 변경될 여지가 없고, 명확한 스키마가 사용자와 데이터에게 중요한 경우
  
### NoSQL 데이터베이스 사용이 더 좋을 때
- 정확한 데이터 구조를 알 수 없거나 변경/확장 될 수 있는 경우
- 읽기를 자주 하지만, 데이터 변경은 자주 없는 경우
- 데이터베이스를 수평으로 확장해야 하는 경우 (막대한 양의 데이터를 다뤄야 하는 경우)
<br/><br/>


### Reference
https://gyoogle.dev/blog/computer-science/data-base/SQL%20&%20NOSQL.html
https://velog.io/@octo__/SQL-vs-NoSQL
https://velog.io/@yuseogi0218/SQL%EA%B3%BC-NOSQL%EC%9D%98-%EC%B0%A8%EC%9D%B4
