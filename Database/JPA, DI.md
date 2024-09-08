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


<br>

### 📌 JPA 동작
![image](https://github.com/user-attachments/assets/0a2f921b-d801-4e16-a37a-f2bace1d9473)
- JPA는 JDBC API를 사용해 데이터베이스와 데이터를 주고받는다.
  - 개발자가 JPA를 사용하면 JPA 내부에서 JDBC API를 사용해 SQL을 호출하고 DB와 통신을 한다.
  - 즉, 개발자가 직접 JDBC API를 쓰는 건 아니다.

<br>

#### 저장
![image](https://github.com/user-attachments/assets/43dcf9f6-6c19-47cd-8977-47c3823f4a4e)

- 개발자는 JPA에 Member 객체(MemberDAO에서 저장하고자 하는 객체)를 넘긴다.
- JPA는 Member Entity를 분석한다.
- Insert SQL을 생성한다.
- JDBC API를 사용해 SQL을 DB에 보낸다.

<br>

#### 조회
![image](https://github.com/user-attachments/assets/dc55e431-ec57-4aaa-b361-fa406a1aa841)

- MemberDAO를 통해 find(id)를 실행하면 JPA는 SELECT SQL을 생성한다.
- JDBC API를 통해 생성된 SQL을 보낸다.
- DB에서 반환된 정보를 ResultSet 매핑을 통해 객체로 변환해 준다.

<br>

### 📌 JPA 객체 매핑 및 매핑 어노테이션
```JAVA
@Entity
public class Item {
 
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) 
    private Long id;
 
    // 필드 이름이 카멜 케이스 일 경우 테이블 컬럼 이름을 언더 스코어로 자동 변환 해준다.
    // 따라서 아래 필드이름은 itemName으로 @Column을 사용 시 name 옵션을 사용하지 않아도 자동으로 item_name으로 변환 해준다.
    @Column(name = "item_name")
    private String itemName;
 
    // 컬럼명을 생략하면 필드명을 컬럼명으로 사용
    private Integer price;
    private Integer quantity;
 
    public Item() {} // JPA는 public, protected의 기본 생성자가 필수
 
    public Item(String itemName, Integer price, Integer quantity) {
        this.itemName = itemName;
        this.price = price;
        this.quantity = quantity;
    }
}
```
| 어노테이션 (@)          | 설명                                                                 |
|-------------------------|----------------------------------------------------------------------|
| `@Entity`               | 클래스가 JPA 엔티티임을 선언. 데이터베이스 테이블에 매핑됨.              |
| `@Table(name="table")`   | 엔티티와 매핑할 데이터베이스 테이블을 지정. 테이블 이름을 설정할 수 있음. |
| `@Id`                   | 엔티티의 기본 키를 나타냄. 해당 필드는 테이블의 primary key로 매핑됨.      |
| `@GeneratedValue`        | 기본 키의 자동 생성 전략을 지정. 예: `AUTO`, `IDENTITY`, `SEQUENCE`, `TABLE`. |
| `@Column(name="column")` | 엔티티의 필드를 특정 테이블의 컬럼에 매핑. 컬럼 이름을 설정할 수 있음.    |
| `@JoinColumn`            | 외래 키를 매핑할 때 사용. 참조할 테이블의 컬럼을 지정.                     |
| `@OneToOne`              | 1:1 관계를 매핑.                                                      |
| `@OneToMany`             | 1:N 관계를 매핑.                                                      |
| `@ManyToOne`             | N:1 관계를 매핑.                                                      |
| `@ManyToMany`            | N:N 관계를 매핑. 중간 테이블을 통해 다대다 관계를 정의.                 |
| `@MappedSuperclass`      | 상속 관계에서 부모 클래스를 매핑하여 상속받는 자식 엔티티에 필드를 전달.   |
| `@Embeddable`            | 값 타입 클래스를 정의할 때 사용. 다른 엔티티에 내장될 수 있음.           |
| `@Embedded`              | 내장 타입을 사용하는 필드에 적용. `@Embeddable`로 정의된 객체를 삽입.      |
| `@Transient`             | 데이터베이스에 저장하지 않으려는 필드에 적용.                           |
| `@Lob`                   | Large Object로, 큰 데이터를 저장할 때 사용. 텍스트나 바이너리 데이터를 처리. |
| `@Enumerated`            | 자바 Enum 타입을 데이터베이스에 매핑. `ORDINAL`(기본) 또는 `STRING` 타입 사용.|
| `@Temporal`              | 날짜/시간 타입을 매핑. `DATE`, `TIME`, `TIMESTAMP`를 지정하여 사용.        |


<br>

### 📌 JPA를 사용하는 이유?
#### 1. 생산성 (CRUD)
- JPA을 사용하면 직접 SQL문을 작성해야 데이터베이스에 접근할 수 있는 JDBC 방식과 달리 별도의 쿼리문을 작성할 필요가 없다.
- 간단한 메소드를 통해 CRUD가 가능하다.
  - 저장: `jpa.persist(member)`
  - 조회: `Member member = jpa.find(memberId)`
  - 수정: `member.setName("변경할 이름")`
  - 삭제: `jpa.remove(member)`

#### 2. 유지보수
- 기존 방식은 엔티티 클래스의 필드가 변경되면 모든 SQL을 수정해야 한다.
- JPA에서는 쿼리를 직접 작성하지 않기 때문에 필드가 변경될 경우 매핑 정보만 변경된 정보로 연결하면 SQL문이 자동으로 작성된다.

#### 3. 패러다임의 불일치 문제 해결
- 객체는 상속 구조를 만들 수 있고, 다형성 구현이 가능하지만 관계형 데이터베이스의 테이블은 상속 개념이 존재하지 않는다.
- 객체는 참조를 통해 관계를 표현하며 방향을 가지고 있지만 관계형 데이터베이스는 외래 키를 통해 관계를 표현하며 방향이 존재하지 않는다. 또한 다대다를 해결하기 위해 조인을 사용한다.
- 이런 다양한 패러다임 불일치의 문제를 JPA를 사용하면 어노테이션을 통해 해결할 수 있다.
  - 데이터베이스의 외래 키 문제는 JPA에서 `@ManyToOne`, `@OneToMany`와 같은 어노테이션으로 객체의 필드를 외래 키로 연결해 참조 관계를 자연스럽게 다룰 수 있음.
  - 단방향, 양방향 관계의 경우 데이터베이스는 방향성이 없지만 JPA에서는 `mappedBy` 같은 속성을 사용해 양방향의 관계를 명확하게 정의할 수 있다.


<br>
<br>


## DI (Dependency injection) 
### 📌 DI (Dependency Injection)이 무엇인가?
- 의존 관계(Dependency)란 한 객체가 다른 객체를 사용할 때, 즉, 한 객체가 바뀌면 이 영향도가 다른 객체에게도 미칠 때 의존성이 있다, 혹은 의존 관계에 있다라고 한다.
- 예를 들어 `A가 B를 의존한다`라는 말도 의존 대상 B가 변하면 그것이 A에게도 영향을 미치는 것을 말한다.
- 여기서 의존성 주입 (Dependency Injection, DI)란 **외부에서 두 객체 간의 관계를 결정해 주는 디자인 패턴**이다.
- 인터페이스를 사이에 둬서 클래스 레벨에서는 의존 관계가 고정되지 않고, 애플리케이션 실행 시점에 객체 간의 관계를 동적으로 주입하여 **유연성을 확보하고, 결합도를 낮게** 해 준다.
- 의존성 주입의 대표적인 방법으로는 **생성자 주입, 필드 주입, 수정자 주입** 등이 있다. (Spring 4부터는 생성자 주입을 권장함)

<br>

### 📌 DI가 필요한 이유 및 DI 장점
```JAVA
public class Coffee {...}

public class Programmer {
	private Coffee coffee = new Coffee();

	public startProgramming(){
		this.coffee.drink()
	}
}
```
- 먼저 다음과 같이 DI를 사용하지 않은 경우를 보면 Programmer 객체는 현재 Coffee에 의존하고 있다.
- 만약 Coffee 객체가 아닌 다른 객체를 사용하고 싶다면 해당 코드를 수정해야 한다.
- 즉, 결합도가 높아지게 되면서 코드 재활용성 등의 문제가 발생하게 된다.

```JAVA
public class Coffee {...}
public class Cappuccino extends Coffee {...}
public class Americano extends Coffee {...}

public class Programmer {
	private Coffee coffee;

	public Programmer(Coffee coffee){
		this.coffee = coffee
	}

	public startProgramming(){
		this.coffee.drink()
	}
}
```
- 여기서 DI 주입을 사용해서 Programmer 객체에 Coffee라는 객체를 주입해 주면 Coffee에 해당하는 다른 객체들을 Programmer에 넘겨 주어 생성하기만 하면 된다.
- 즉, 코드를 계속해서 재사용할 수 있다.

#### DI의 장점
- DI는 의존성이 낮아지기 때문에 단위 테스트가 용이해진다.
- 하나의 객체에 의존하지 않기에 코드 재활용성이 높아진다.
- 객체 간의 의존성 및 종속성을 줄이거나 없앨 수 있다.
- 객체 간의 결합도를 낮추면서 유연한 코드를 작성할 수 있다.

<br>

### 📌 DI 방식 (Spring)
#### 1. 생성자 주입 (Constructor Injection)
```JAVA
@Service 
public class UserServiceImpl implements UserService {   
    private UserRepository userRepository; 
    private MemberService memberService; 
    @Autowired 
    public UserServiceImpl(UserRepository userRepository, MemberService memberService) { 
        this.userRepository = userRepository; 
	this.memberService = memberService; 
    } 
}

// 출처: https://mangkyu.tistory.com/125 [MangKyu's Diary]
```
- 생성자 주입은 생성자 호출 시점에서 1 회 호출되는 것이 보장이 된다.
- 즉, 주입받는 객체의 변화가 없거나 반드시 객체의 주입이 필요한 경우에 강제하기 위해 사용한다.
- 스프링에서는 생성자가 1 개만 있을 경우 `@Autowired`는 생략이 가능하다.
- 생성자 주입을 권장하는 이유는
  - 객체의 불변성이 확보된다. 생성자 주입은 객체가 생성될 때 의존성을 주입받기 때문에 객체 생성 이후 의존성이 변경될 수 없다. 또한 `final`로 의존성을 상수로 선언도 가능하며 코드의 안정성을 높일 수 있다.
  - 테스트 코드의 작성이 용이하다. 생성자를 통해 의존성을 직접 주입할 수 있으므로 테스트할 때 Mock 객체나 Test Double을 쉽게 전달해 단위 테스트를 효율적으로 작성할 수 있다.
  - final 키워드와 Lombok과의 결합. 생성자 주입은 `final` 변수 할당이 가능해 객체의 의존성을 변경할 수 없게 만들며 Lombok의 `@RequiredArgsConstructor`어노텡션을 사용해 생성자를 자동으로 생성할 수 있어 코드가 간결해진다.
  - 순환 참조 문제가 발생할 때 객체의 생성과 동시에 의존성을 주입받기 때문에 순환 참조가 발생하면 애플리케이션 실행 시점에서 즉시 에러가 발생한다.

<br>

#### 2. 수정자 주입(Setter Injection)
```JAVA
@Service 
public class UserServiceImpl implements UserService { 
    private UserRepository userRepository; 
    private MemberService memberService; 
    @Autowired 
    public void setUserRepository(UserRepository userRepository) { 
        this.userRepository = userRepository; 
    } 
    @Autowired 
    public void setMemberService(MemberService memberService) { 
        this.memberService = memberService; 
    } 
}

// 출처: https://mangkyu.tistory.com/125 [MangKyu's Diary]
```
- Setter를 이용해 의존 관계를 주입하는 방법으로 주입받는 객체가 변할 수 있는 경우에 사용한다.
- `@Autowired`로 주입할 대상이 없는 경우 오류가 발생한다. (빈이 무조건 존재해야 함)
- 주입할 대상이 없도록 하려면 `@Autowired(required = false)`를 통해 설정해야 한다.
<br>

#### 2. 필드 주입(Field Injection)
```JAVA
@Service 
public class UserServiceImpl implements UserService { 
    @Autowired
    private UserRepository userRepository; 
    @Autowired
    private MemberService memberService; 
}
```
- 필드에 바로 의존 관계를 주입하는 방법이다.
- 코드가 간결해져 과거에 많이 이용된 방법이지만 이 방법은 외부에서는 변경이 불가하다.
- 테스트 코드에서 Mock 테이터를 사용해 주입하게 하면 안 된다.
- 사용을 지양해야 하며 애플리케이션의 작동과 무관한 테스트 코드나 설정을 위해서만 사용한다. 

출처
[[Spring JPA] JPA 란?](https://dbjh.tistory.com/77)
[JPA란?](https://velog.io/@dirn0568/JPA%EB%9E%80)
[JPA란 무엇인가? - JPA 동작, 사용하는 이유](https://ittrue.tistory.com/253)
[[JPA] JPA란 ? 그리고 Spring Data JPA](https://hstory0208.tistory.com/entry/JPA-JPA%EB%9E%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-Spring-Data-JPA)
[DI란 무엇인가?](https://velog.io/@jeong-god/DI%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)
