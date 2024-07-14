# JOIN

## 📌 JOIN이란?
![](https://velog.velcdn.com/images/ssongji/post/c4022030-7d92-41b6-88fd-5f349f494124/image.png)
- 두 개 혹은 그 이상의 테이블을 `공통 필드(특정 KEY)`를 가지고, 이를 결합해 하나의 결과 집합으로 통합하는 SQL 연산을 말한다.
- 또한 JOIN은 **스타 스키마**로 분산되어 있는 정보를 통합하는 데 사용된다.

<br>

> **스타 스키마**란?
> <br>
> ![image](https://github.com/user-attachments/assets/fc699a95-2640-4314-a945-c18f69f1f9b4)
> <br>
> 스타 스키마는 데이터 웨어하우스 설계에서 사용되는 데이터 모델링 기법이다.
> <br>
> 이런 스타 스키마는 사실 테이블(Fact Table)이 중앙에 존재하고, 별 모양으로 여러 차원 테이블(Dimension Table)로 구성이 되어 있다.
> <br>
> <br>
> **사실 테이블 (Fact Table)**
> <br>
> 이때 사실 테이블은 비즈니스 프로세스의 측정 가능한 이벤트를 저장하며, 수치 데이터(매출액, 수량)와 외래 키를 포함한다.
> <br>
> 외래 키는 차원 테이블의 기본 키를 참조한다.
> <br>
> <br>
> **차원 테이블(Dimension Table)**
> <br>
> 사실 테이블의 각 이벤트에 대한 내용을 제공한다.
> <br>
> 설명 데이터(날짜, 지역, 제품 정보 등)을 포함한다.
> <br>
> 텍스트 데이터로 주로 구성되어 있으며, 각 차원 테이블에는 고유 기본 키가 있다.


<br>

## 📌 JOIN 시 고려해야 할 점
- 먼저 중복 레코드가 없고 Primary Key의 uniqueness가 보장되어야 한다. (JOIN을 하기에 앞서 항상 데이터의 정확성을 검증해야 한다.)
- 조인하는 테이블들간의 관계를 명확하게 정의해야 한다.
    - ONE TO ONE
    	- `완전한 ONE TO ONE`과 `한쪽이 부분 집합이 되는 ONE TO ONE`이 존재한다.
    - ONE TO MANY & MANY TO ONE
    	- 예를 들어 `EMPLOYEE` 테이블과 `DEPARTMENT` 테이블이 있다고 했을 때 `DEPARTMENT` 관점에서 하나의 지점에 여러 직원이 속할 수 있으므로 둘은 `ONE TO MANY` 관계이다.
        - 중복이 존재한다면 데이터가 증폭되는 문제가 발생할 수 있음.
    - MANY TO MANY
 - 어떤 테이블을 베이스로 잡을지(FROM에 사용할지) 결정해야 함.


 <br>

## 📌  JOIN의 종류
![](https://velog.velcdn.com/images/ssongji/post/b7b525dd-7e86-44d4-9f9f-cd7e55f050a4/image.png)
### 1) INNER JOIN
- 양쪽 테이블에서 매치되는 레코드들만 리턴한다.
- 양쪽 테이블에서 매치되는 레코드들만 리턴하기 때문에 양쪽 필드가 모두 채워진 상태로 리턴된다.
- INNER JOIN의 경우 INNER JOIN이라고 해도 되고 JOIN만 써 주어도 default로 INNER JOIN이 된다.
```sql
SELECT *
  FROM RAW_DATA.VITAL V
  JOIN RAW_DATA.ALERT A
    ON V.VITALID = A.VITALID;
```

![](https://velog.velcdn.com/images/ssongji/post/f7dad15b-c663-4c3e-b7ee-f329d3319fa1/image.png)
- KEY인 VITALID가 매칭되는 데이터가 하나밖에 존재하지 않는다. 나머지 ALERT 테이블의 데이터는 VITALID가 NULL이기 때문에 다음과 같은 하나의 데이터가 나오게 된다.

### 2) LEFT JOIN
- 왼쪽 테이블을 기준으로 모든 레코드들을 리턴한다.
- 만약 오른쪽 테이블의 데이터 중 왼쪽 레코드와 매칭이 되는 경우가 있다면 왼쪽 레코드와 매칭된 경우만 채워진 상태로 리턴된다.
```sql
SELECT *
  FROM RAW_DATA.VITAL V
  LEFT JOIN RAW_DATA.ALERT A
         ON V.VITALID = A.VITALID;
```

![](https://velog.velcdn.com/images/ssongji/post/dda07bf5-0357-4f65-996a-25d92ff17c65/image.png)
- Python 코드에서는 `NULL`이 `NONE`으로 나오게 된다.
- VITAL 테이블의 네 개의 데이터 중 하나만이 ALERT 테이블과 매칭되기 때문에 이 케이스만 데이터가 나오게 되고 나머지는 ALERT 테이블의 값이 NONE으로 나오게 된다.

### 3) RIGHT JOIN
- 오른쪽 테이블을 기준으로 모든 레코드들을 리턴한다.
- `LEFT JOIN`과 방향만 바뀐 개념으로 만약 왼쪽 테이블의 데이터 중 오른쪽 레코드와 매칭되는 경우가 있다면 오른쪽 레코드와 매칭되는 경우만 채워진 상태로 리턴된다.
```sql
SELECT *
  FROM RAW_DATA.VITAL V
 RIGHT JOIN RAW_DATA.ALERT A
         ON V.VITALID = A.VITALID;
```

![](https://velog.velcdn.com/images/ssongji/post/b36c9441-574c-4c60-b0c3-cb6319bd325f/image.png)
- ALERT 테이블의 세 개의 데이터 중 하나만이 VITAL 테이블과 매칭되기 때문에 이 케이스만 데이터가 나오게 되고 나머지는 VITAL 테이블의 값이 NONE으로 나오게 된다.


### 4) FULL JOIN
- 왼쪽 테이블과 오른쪽 테이블의 모든 레코드들을 리턴한다.
- 매칭되는 경우에만 양쪽 테이블들의 모든 필드들이 채워진 상태로 리턴된다.
```sql
SELECT *
  FROM raw_data.Vital V
FULL JOIN raw_data.Alert A
       ON V.VITALID = A.VITALID
```

![](https://velog.velcdn.com/images/ssongji/post/c3f3b7e2-e6c4-4615-90e0-54108be42238/image.png)
- VITAL 테이블과 ALERT 테이블의 모든 데이터가 나온 것을 볼 수 있다. 다만 매칭이 되는 데이터가 있다면 매칭이 되어 나오게 된다.

### 5) CROSS JOIN
- 왼쪽 테이블과 오른쪽 테이블의 모든 레코드들의 조합을 리턴한다.
- VITAL 테이블에는 네 개의 데이터가 있었고, ALERT 테이블에는 세 개의 데이터가 존재하는데 이런 경우 `4 * 3 = 12` 개의 데이터가 조합돼서 나온다.
- `CROSS JOIN`은 JOIN한 테이블 데이터 사이에서 나올 수 있는 모든 경우의 수를 조합해 주기 때문에 `ON 절`을 통해 `KEY`를 매칭해 줄 필요가 없다.
```sql
SELECT *
  FROM RAW_DATA.VITAL V
CROSS JOIN RAW_DATA.ALERT A;
```

![](https://velog.velcdn.com/images/ssongji/post/f5c01bf8-a764-49db-873a-dbbb3fa96738/image.png)
- ALERT 테이블의 세 개의 데이터와 VITAL 테이블의 네 개의 데이터의 모든 조합이 나오게 된다. 총 열두 개의 데이터가 나오는 것을 확인할 수 있다.

### 6) SELF JOIN
- 동일한 테이블을 alias를 달리 해서 자기 자신과 조인한다.
- 예시는 기본적인 문법이지만 보통 SELF 조인은 같은 테이블에서 특정 정보를 뽑고자 할 때 각각 다른 조건을 주어야 하는 경우 사용한다.
```
SELECT *
  FROM raw_data.Vital V1
  JOIN raw_data.Vital V2
    ON V1.VITALID = V2.VITALID
```

