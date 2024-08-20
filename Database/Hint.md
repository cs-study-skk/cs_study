## ✔ 힌트(HINT)란?
- 속도 개선 등의 목적으로 SQL 튜닝 시 사용되는 지시 구문이다. SQL에는 실행 계획(Ctrl+P)이라는 것이 존재하는데 QUERY에 대해 ORACLE이 실행 계획을 정해 주긴 하지만 이 경로가 항상 최적의 실행 계획이 되지는 않는다. 그럴 경우 사용자가 HINT를 줌으로써 ORACLE이 최적의 플랜을 찾을 수 있도록 지시해 주는 것이다. 

## ✔ 힌트 사용 방법 및 자주 사용하는 목록 
### 1. 힌트 사용 방법 
```
SELECT /*+ (힌트)/ 
       컬럼명
  FROM TABLE명
 WHERE 조건;
  -- 힌트는 쿼리 서두에 명시하며, 힌트는 하나만 사용 가능한 것이 아니라 여러 개의 힌트가 필요할 경우,
  -- 여러 개의 힌트를 사용할 수 있다. /*+ leading (M) USE_NL(M V)*/와 같은 방식으로. 
  
ex) /*+ INDEX(A IDX_SUBJECT) USE_NL(M A V.M) LEADING(M) */
-- A의 테이블을 IDX_SUBJECT라는 인덱스를 타게 하고 
-- M, A, V.M(V라는 View의 M 테이블)를 Nested Loop를 돌게 하며
-- M 테이블을 먼저 조회 및 조인한다. (Driving Table에 주로 사용)
```
#### 2. 힌트 목록
##### 힌트는 액세스 경로, 조인 순서, 최적화 목표, 병렬 및 직렬 처리를 변경할 때, 데이터 값을 정렬해야 할 때, 또한 드라이빙 순서를 변경해 주고자 할 때 사용할 수 있다. 

**1) 최적화 목표**
-  ALL_ROWS: 전체 처리 속도를 최소화시키기 위한 힌트로 Full Scan을 선호한다. (목적: Best Throughput)

- FIRST_ROWS: 조건에 맞는 첫 번째 ROW를 리턴하기 위한 Resource 소비를 최소화 시키기 위한 힌트이며 Full Scan보다는 Index Scan을 선호한다. 또한 Sort Merge Join보다는 Nested Loop Join을 선호한다. (목적: Best Response Time)

**2)  접근 방법**
- FULL(table명): 해당 table을 Full Scan 하도록 지정할 때 사용한다.

- HASH(table명): 해당 table을 Hash Scan 하도록 지정할 때 사용한다. 해시 키로 만들어진 Cluster 내에 저장된 Table에만 적용된다.

- CLUSTER(table명): 해당 table을 Cluster Scan 하도록 지정할 때 사용하며 클러스터링된 객체에만 적용된다.

- INDEX(table명 index명): 플랜에서 ORACLE이 타고 있는 경로보다 더 속도가 개선될 수 있는 index가 있을 경우 index를 강제적으로 지정해 줘 해당 index를 타도록 지정할 때 사용한다.

- INDEX_ASC(table명 index명): 지정된 index를 오름차순으로 타게 지정해 준다. (INDEX 힌트의 경우 default가 ASC)  

- INDEX_DESC(table명 index명): 지정된 index를 내림차순으로 타게 지정해 준다. 주로 MAX 값을 구할 때 유용하다.

- INDEX_FFS(table명 index명): Full Index Scan 하도록 지정할 때 사용한다.

- INDEX_SS(table명 index명): Index Skip Scan 하도록 지정할 때 사용한다. 
 ** Index Skip Scan: 루트 또는 브랜치 블록에서 읽은 컬럼 값의 정보를 이용해 조건에 부합하는 record를 포함할 가능성이 있는 블록만 골라서 접근하는 방식.
 
**3) 조인 순서**
- LEADING(table명): table명은 하나 이상이 와도 무관하며 나열한 순서대로 조인하도록 지시하는 힌트이다. 드라이빙의 경우 데이터 조회(스캔)의 범위를 줄여 주는 것이 목표이며 그러한 조건이 있는 테이블을 선두로 다음과 같은 힌트를 작성한다. (LEADING의 경우 테이블 명시 순서에 따라 속도 차이가 날 수 있으므로 어떤 테이블이 우선으로 가야 스캔 범위가 줄어들지를 파악한 후 사용해야 한다.)
ex) 외래환자리스트 조회 쿼리라면, PT_NO (환자번호)가 첫 번째 드라이빙 조건이 되고, 두 번째 드라이빙 조건은 MED_DT (진료일자)가 되게 된다.

- ORDERED: FROM절에 나열된 테이블 순서대로 조인하도록 지시하는 힌트이다.

**4) 조인 방식**
- USE_NL(table명): 테이블 조인 시 Nested Loop (중첩루프) 형식으로 조인하도록 지시하는 힌트이다. 지정된 테이블은 Inner Table이 된다.

- USE_HASH(table명): 지정된 테이블들이 Hash Join을 하도록 지시하는 힌트이다.

- USE_MERGE(table명): 지정된 테이블들이 Sort Merge 형식으로 조인하도록 지시하는 힌트이다.

- DRIVING_SITE(table명): 쿼리의 실행이 ORACLE에 의해 지정된 SITE가 아닌 다른 SITE에서 일어나도록 실행 주체를 지정한다. 

**5) 병렬 처리**
- PARALLEL(table명, degree): 쿼리에 포함된 table의 degree를 설정해 준다. 테이블 스캔을 DML 병렬 방식으로 처리하도록 할 때 사용하는 힌트이다.
 ** degree란 하나의 작업 수행에 대한 server process의 개수를 의미하며 시스템의 CPU 수, 시스템의 최대 Process 수, 테이블이 striping 되어 있을 시 테이블들이 걸쳐 있는 disk의 수, data의 위치, query의 형태 등으로 결정이 된다. 
 
- PARALLEL_INDEX(table명, index명, degree): 인덱스 스캔을 병렬 방식으로 처리하도록 유도하는 힌트이다.

- NOPARALLEL(table명): 쿼리가 병렬 처리 하지 않도록 지정해 준다.

