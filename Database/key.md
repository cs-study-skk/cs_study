# 데이터 베이스 키(KEY)

## 📌 데이터 베이스 키(KEY)란?
![image](https://github.com/user-attachments/assets/2ecd056b-27d5-4b03-8127-73b4cbe810e8)
- 데이터 베이스의 테이블 각 행(row)을 **고유하게 식별하기 위해 사용되는 속성 또는 속성들의 집합**이다.
- 키는 데이터 베이스의 **무결성을 보장**하고 **효율적인 데이터 검색과 관리를 가능**하게 한다.

<br>

## 📌 데이터 베이스 키(KEY)를 사용해야 하는 이유
- 고유 식별
  - **기본 키(PK)** 를 사용해 테이블의 각 행을 고유하게 식별할 수 있다.
  - 중복 데이터를 방지하고 데이터의 무결성을 유지하는 데 필수적이다.
- 데이터 무결성
  - **기본 키** 는 **반드시 유일해야 하고 NULL 값을 가질 수 없다.** -> 각 행이 고유하고 완전한 데이터를 가지도록 보장
  - 또한 **외래 키** 는 한 테이블의 데이터가 다른 테이블의 데이터와 일관성을 유지하도록 보장한다. -> 데이터 간의 참조 무결성 유지
- 효율적인 데이터 검색
  - 데이터 베이스는 키를 사용해 **인덱스를 생성**한다.
  - 인덱스는 데이터 검색 속도를 크게 향상 시켜 대규모 데이터 베이스에서는 빠른 조회가 가능하게 한다.
- 관계 설정
  - 외래 키를 사용해 **테이블 간의 관계 정의**를 할 수 있다.
  - 데이터 베이스 내의 데이터들이 서로 어떻게 연결되어 있는지를 명확하게 할 수 있다.
- 데이터 무결성 제약 조건
  - 키를 사용해 기본 키 제약 조건, 외래 키 제약 조건 등 다양한 무결성 제약 조건을 정의할 수 있다. -> 데이터의 일관성 유지

> **참조 무결성** 이란?
> <br>
> ![image](https://github.com/user-attachments/assets/6114e667-c03a-4ec8-bc59-6c474e3c0ab8)
> <br>
> 관계형 데이터베이스 모델에서 **2 개의 관련 있는 관계 변수 간의 일관성**
> <br>
> 즉, **기본 키와 외래 키 간의 관계가 항상 유지되도록 보장**되는 것을 말함

<br>

## 📌 데이터 베이스 키(KEY)의 종류
### 1. 기본 키 (Primary Key)
- 테이블 내에서 특정 레코드(행)을 식별하기 위해 사용되는 키로 중복되는 값이 없어야 한다.
- 또한 기본 키는 유일성을 유지하면서도 최소한의 속성만 포함한 최소성을 보장한다.
- 하나의 테이블에는 하나의 기본 키만 존재할 수 있으며 기본 키는 자동으로 인덱스가 생성돼 검색 속도를 향상시킨다.
- 일반적으로 숫자 또는 문자열의 형태로 사용된다.
- NULL 값을 허용하지 않는다.

### 2. 슈퍼 키 (Super Key)
- 한 테이블에서 각 행을 고유하게 식별할 수 있는 하나 이상의 속성들의 집합이다.
- 반드시 유일성을 보장하기에 슈퍼 키를 사용하면 테이블 내에 중복된 행이 존재할 수 없다.
- 슈퍼 키는 최소성을 보장하지 않기 때문에 불필요한 속성이 포함될 수 있다.
- 하나의 테이블에서 여러 개의 슈퍼 키가 존재할 수 있고, 모든 후보 키, 기본 키, 대체 키는 슈퍼 키의 부분 집합이다.

<img width="725" alt="image" src="https://github.com/user-attachments/assets/54dd7316-eb8b-4fb9-872b-ce2ef10845d9">

### 3. 후보 키(Candidate Key)
- 최소 슈퍼 키로 한 테이블에서 각 행을 고유하게 식별할 수 있는 최소 속성들의 집합이다.
- 유일성을 유지하면서 중복 속성을 제거한 키이다.
- 테이블에서는 하나 이상의 후보 키를 가질 수 있다.
- 후보 키도 유일성을 보장하기 위해 NULL이나 빈 값을 허용하지 않는다.

### 4. 대체 키(Alternate Key)
- 기본 키로 선택되지 않은 나머지 후보 키이다.
- 예를 들어 학생 테이블에서 학번과 주민등록번호가 후보 키일 경우 기본 키는 학번이 되고 남는 키인 주민등록번호가 대체 키가 된다.
- 만약 기본 키가 없어지게 되면 이를 대체할 수 있는 키를 말한다.

### 5. 외래 키(Foreign Key)
- 외래 키는 한 테이블의 속성(또는 속성 집합)이 다른 테이블의 기본 키(Primary Key)를 참조하여 두 테이블 간의 관계를 정의하는 키이다.
- 참조될 테이블(A)이 먼저 만들어지고, 참조하는 테이블(B)에 값이 입력이 되어야 하며 이때 참조될 열(A)의 값은 참조될 테이블(A)에서 기본 키로 설정되어 있어야 한다.
- 외래 키는 참조되는 테이블의 기본 키와 동일한 키 속성을 가진다.
- 두 테이블 간의 관계를 정의하고 참조 무결성을 유지하는 데 사용된다.

### 6. 조합 키(Composite Key)
- 조합 키는 두 개 이상의 속성으로 구성된 키로 각 속성의 조합을 통해 각 행을 고유하는 식별하는 키이다.
- 두 개 이상의 속성을 결합해 각 행을 고유하게 식별하는 데 사용된다.

```SQL
-- 학생 테이블
CREATE TABLE Students (
    STUDENT_ID INT PRIMARY KEY,       -- 기본 키
    NAME VARCHAR(100),
    BRDT DATE,
    NATIONAL_ID CHAR(13) UNIQUE       -- 후보 키 (유일성 제약 조건)
);

-- 수강 테이블
CREATE TABLE Enrollments (
    STUDENT_ID INT,                   -- 외래 키
    COURSE_ID INT,
    REG_DT DATE,
    PRIMARY KEY (STUDENT_ID, COURSE_ID),  -- 조합 키
    FOREIGN KEY (STUDENT_ID) REFERENCES Students(STUDENT_ID)
);
```


