# 👉SQL 튜닝이란
**관계형 데이터베이스에서 최소한의 자원을 사용해 서버의 처리속도를 높이는 것** <br/>
> **SQL 튜닝이 필요한 이유?**
> <br/>쿼리 튜닝은 시스템 성능에 직접적인 영향을 미친다.
> <br/>최적화되지 않은 쿼리는 응답 속도 저하, 자원낭비, 심지어는 서비스 장애까지 초래할 수 있다.

## 인덱스 활용
인덱스를 사용하면 데이터베이스가 특정 열을 쉽게 찾을 수 있다.

### 적절한 인덱스 생성
인덱스로 생성하기 좋은 컬럼은 아래와 같다.
- 고유한 값 혹은 자주 조회되는 컬럼
- 조건절(WHERE, JOIN, ORDER BY, GROUP BY 등)에 사용되는 컬럼
- 자주 업데이트 되지 않는 컬럼 
- null 값이 적은 컬럼
  
**예) 인덱스 생성**

```sql
SELECT *
FROM orders
WHERE order_date > '2021-01-01'
ORDER BY order_id
```
이 쿼리에서는 WHERE 절에서 order_date 열에 대한 조건을 사용하고, ORDER BY 절에서 order_id 열을 사용한다. 
<br/>따라서, order_date와 order_id 열에 대한 인덱스를 만들어 쿼리의 실행 속도를 향상시킬 수 있다.


### 인덱스컬럼은 변형하여 사용하지 않는다.
**예) 특정 년도의 데이터만 필터링**
- before
```sql
SELECT *
FROM sales
WHERE YEAR(date) = 2021;
```
- after
```sql
SELECT *
FROM sales 
WHERE date >= '2021-01-01' AND date <= '2021-12-31';
```
데이터 원본을 변형하여, 내가 찾고자 하는 범위와 비교하는 연산은 데이터베이스가 인덱스를 제대로 활용할 수 없게 된다. 
<br/>때문에 다음과 같이 쿼리를 바꿔 **데이터를 '변형'하지 않고, 원본 형태를 유지**하면서 필요한 데이터를 찾는다.

## 서브쿼리 최소화
서브쿼리는 복잡한 쿼리를 작성할 때 유용하지만, 성능 저하의 원인이 될 수 있어 사용을 최소화하는 것이 좋다. 
<br/>서브쿼리 대신 JOIN, UNION, EXISTS 등의 다른 방법을 사용하여 쿼리를 작성하는 것이 좋다. 
<br/><br/>
**예) 성별이 여성인 고객이 주문한 모든 주문을 가져옴**
- before
```sql
SELECT *
FROM orders
WHERE customer_id IN (
  SELECT customer_id
  FROM customers
  WHERE gender = 'F'
)
```
- after
```sql
SELECT orders.*
FROM orders
JOIN customers
ON orders.customer_id = customers.customer_id
WHERE customers.gender = 'F'
```
JOIN을 사용하여 같은 결과를 얻을 수 있으므로 서브쿼리를 사용하는 것은 부적절하다.

## 절의 끝에서만 와일트 카드를 사용
와일드카드는 문자화 된 값들을 찾을 때 아주 광범위하게 찾게 돼 효율성이 매우 떨어진다. 
<br/>와일드카드를 사용하게 되면, 데이터베이스는 조건을 만족하는 값을 찾기 위해 모든 레코드를 뒤져야 한다.
<br/><br/>
**예) 이름이 John~인 사용자를 조회**
- before
```sql
SELECT * FROM users WHERE name LIKE '%John';
```
- after
```sql
SELECT * FROM users WHERE name LIKE 'John%';
```
다음과 같이 쿼리를 변경하면 데이터베이스가 인덱스를 활용해서 검색 범위를 효과적으로 좁힐 수 있다.
<br/>데이터베이스는 우선 인덱스에서 'John'으로 시작하는 첫 번째 항목을 빠르게 찾아내고 'John'으로 시작하지 않는 첫 번째 항목이 나올 때까지만 검색한다.

## 쿼리 실행 계획 확인 
쿼리 실행 계획은 데이터베이스가 **쿼리를 실행할 때 어떤 방식으로 데이터를 검색하는지** 알려준다. 
<br/>따라서 쿼리 실행 계획을 확인하여 쿼리의 성능을 분석하는 것이 중요하다. 
 
<br/>대부분의 데이터베이스 관리 시스템에서 쿼리 실행 계획을 확인할 수 있으며, 각 데이터베이스 관리 시스템마다 확인하는 방법이 다르다. 
<br/><br/>
**예) MySQL에서의 실행 계획 조회**
- query
```sql
EXPLAIN SELECT *
FROM orders
WHERE order_date > '2021-01-01'
ORDER BY order_id
```
- 실행 계획
```
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+--------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                    |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+--------------------------+
|  1 | SIMPLE      | orders| NULL       | ALL  | NULL          | NULL | NULL    | NULL | 1000 |    11.11 | Using where; Using filesort|
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+--------------------------+
```
이 쿼리 실행 계획에서는 orders 테이블을 전체 스캔하고, WHERE 절에서 order_date 열을 사용하여 데이터를 필터링한다. 
<br/>하지만, 인덱스를 사용하지 않아 성능이 저하될 수 있다.
<br/>이와 같이 쿼리 실행 계획을 확인하여 성능 저하의 원인을 파악하고, 인덱스나 JOIN 조건 등을 수정하여 성능을 개선할 수 있다.




## Reference
https://chung-develop.tistory.com/145
https://velog.io/@hyunspace/about-sql-tuning
