## JPA (Java Persistence API)
### 📌 JPA (Java Persistence API)이 무엇인가? 
- JPA는 자바에서 **ORM(Object-Relational Mapping) 기술 표준으로 사용되는 인터페이스의 모음**이다.
- 즉 **JAVA 객체**와 **관계형 데이터베이스** 간의 **매핑을 위한 API**를 말한다.
- JPA는 MyBatis와 같이 SQL문과 JAVA 코드를 연계하는 접근 방식이 아닌 **JAVA 객체와 DB 엔티티(테이블) 자체를 그대로 매핑해서 처리할 수 있는 접근 방식**을 위한 새로운 기술 표준이다.
- 그러므로 JPA는 JAVA 개발자가 좀 더 객체 지향 관점에서 개발할 수 있게 해 주고, 개발을 용이하게 해 주어 DB와 JAVA 간의 불일치를 해소한다.

<br>

> **ORM(Object-Relational Mapping)**
> <br>
> ![image](https://github.com/user-attachments/assets/abaa6c3f-f0f0-4b3b-a8d6-ba30c7175b7d)
> <br>
> - 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑해 주는 것을 말한다.
> - 즉, JAVA 클래스가 ORM을 통해 DB 테이블에 영속화되고, 영속화된 데이터가 다시 JAVA 객체로 변환되는 것을 ORM 매핑이라고 부른다.
> - JAVA를 DB 테이블로 자동으로 영속화 시켜 주는 기술을 ORM이라고 한다.
> - 이때 영속성이란 객체가 데이터베이스와 관련된 상태로 유지되는 것을 말한다. 즉, 객체의 상태가 데이터베이스에 반영되어 지속적으로 저장되거나 관리되는 것을 말한다.


<br>

### 📌 MyBatis와 JPA
![image](https://github.com/user-attachments/assets/29358fc2-22e7-4f20-aa21-8d9a08646dc3)
- 둘은 상호 배타적인 보완 관계는 아니나 각각 다른 목적과 사용 사례에 최적화된 도구이다.
- 둘 다 JAVA 코드와 DB를 이어 주는 기술로 프로젝트의 요구 사항이나 개발 팀의 역량에 따라 적절한 기술을 선택하는 게 중요함.
- JPA는 **객체지향적 설계와 데이터베이스 작업의 추상화**에 강점을 가지는 반면, MyBatis는 **SQL 작성의 자유도와 복잡한 쿼리 작업**에서 더 강점을 가진다.



## DI (Dependency injection) 
