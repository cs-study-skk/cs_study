# 👉 Redis
Key-Value 구조의 저장소로 비정형 데이터를 저장, 관리하기 위한 오픈 소스 기반의 NoSQL

## Redis의 특징
- Key, Value 구조

- *In-Memory 데이터 구조를 가진 저장소
  > **💡인메모리 ( In-Memory )**<br/>
  > 컴퓨터의 주기억장치인 RAM에 데이터를 올려서 사용하는 방법<br/>
  > RAM에 데이터를 저장하게 되면 메모리 내부에서 처리가 되므로, 데이터를 저장/조회할 때 하드디스크를 오고가는 과정을 거치지 않아도 되어 속도가 빠름<br/>
  > But, 서버의 메모리 용량을 초과하는 데이터를 처리할 경우, RAM의 특성인 휘발성에 따라 데이터가 유실될 수 있음

- 빠른 처리 속도<br/>
  디스크가 아닌 메모리에서 데이터를 처리하고 Redis에서 제공하는 Sorted-Set 자료구조를 사용하면 더 빠르고 간단하게 데이터 정렬 가능

- 서버 복제 지원
  Master - slave 구조로, 여러개의 복제본을 만들 수 있다.<br/>
  데이터베이스 읽기를 확장할 수 있기 때문에 높은 가용성, 클러스터를 제공한다.
  
- 여러 Data Type(Collection)을 지원
  ![image](https://github.com/user-attachments/assets/3325f381-a564-4e75-8bae-3a7eba69a61f)
  <br/>
  
- *AOF, *RDB 백업<br/>
  인메모리 데이터 저장소가 가지는 휘발성의 특성으로 데이터가 유실될 경우를 방지하여 데이터를 DISK에 백업하 기능을 제공
  > **💡AOF (Append On File) 방식**<br/>
  > Redis의 모든 write/update 연산 자체를 모두 log 파일에 기록하는 형태
  > <br/><br/>
  > **💡RDB(Snapshotting) 방식**<br/>
  > 순간적으로 메모리에 있는 내용 전체를 디스크에 담아 영구 저장하는 방식

- Redis Sentinel 및 Redis Cluster를 통한 자동 *파티셔닝을 제공<br/>
  Master와 Slaves로 구성하여 여러대의 복제본을 만들 수 있고, 여러대의 서버로 읽기를 확장 가능
  > **💡파티셔닝 ( Partitioning )**<br/>
  > 다수의 Redis 인스턴스가 존재할 때 데이터를 여러 곳으로 분산 시키는 기술<br/>
  > 각 Redis 인스턴스는 전체 키 중 자신에게 할당된 일부 파티션의 키들만 관리하게 됨<br/>

- 싱글 스레드<br/>
  ![image](https://github.com/user-attachments/assets/63517184-7b40-454c-a574-5dfc0d776b0e)<br/>
  멀티스레드 환경이 아니므로 Context Switch가 발생하지 않아 효율적인 시스템 리소스 사용이 가능하다.<br/>
  한번에 하나의 명령만 수행이 가능하므로 Race Condition이 거의 발생하지 않아 Deadlock이 발생하지 않는다. <br/>

## Redis 아키텍처
![image](https://github.com/user-attachments/assets/220b2ae5-5b98-4ad2-be77-81d21003e278) <br/>

### 1) Replication: Master와 Replica(Slave)로 구성
![image](https://github.com/user-attachments/assets/3b9b113a-8838-4db6-a13b-4e343cde30da) <br/>

- 레디스 서버 2대(마스터-슬레이브)로 구성한다.
- 슬레이브는 마스터의 데이터를 실시간으로 전달받아 보관한다.
- 마스터 다운 시 슬레이브 서버가 이를 대체한다. 이때, 수동으로 슬레이브 서버를 마스터로 변경해야한다.
- 한 마스터에 슬레이브를 여러 대 구성할 수 있다.

### 2) Sentinel : Sentinel, Master, Replica로 구성 / HA(High Availability) 구성
![image](https://github.com/user-attachments/assets/93104be6-df4c-4251-8dcf-219bc2d6963a) <br/>

- Sentinel 노드가 마스터와 레플리카 노드를 감시
- Master가 비정상 상태일 때 자동으로 복구 -  다른 레플리카를 마스터 서버로 변경
- 다운된 마스터가 다시 시작되면 센티널이 슬레이브로 전환시
- 애플리케이션은 Sentinel과 연결하기 때문에, 장애 상황 발생 시 연결 정보 변경 필요 없음
- Sentinel 노드도 장애 상황이 발생할 수 있기 때문에 반드시 3대 이상의 홀수로 존재해야 함.
  과반수 이상의 Sentinel이 동의(Quorum based)가 있어야 Failover 진행

### 3) Cluster : 클러스터에 포함된 노드들이 서로 통신
![image](https://github.com/user-attachments/assets/8b34e412-6c70-42e8-9378-b444d6868a96) <br/>

- Redis 3.0 이후부터 제공
- 클러스터에 포함된 노드들이 서로 통신하면서 HA를 유지한다. 
- 클러스터 내부에는 센티넬과 동일하게 마스터와 레플리카는 짝을 이루어 데이터를 복제한다.

- 센티넬과 동일하게 Master와 Replica는 짝을 이루어 데이터를 복제
- 클러스터를 구성하기 위해 최소 3개의 마스터 노드는 반드시 필요.
레플리카 노드의 개수는 0개 혹은 그 이상으로 설정 가능.
고가용성을 위해 반드시 한 개 이상의 레플리카를 설정하는 것이 좋음
- 데이터를 마스터 노드들에 *샤딩
  > **sharding(샤딩)** <br/>
  > 대량의 데이터를 처리하기 위해 여러 개의 데이터베이스에 분할하는 기술
  > 해시 함수를 사용하여 데이터 분배, 데이터의 키 값을 해시 함수로 넘긴 후 리턴 값을 사용하여 어떤 노드에 저장할지 결정한다.
- 각 마스터 서버는 데이터의 처리와 센티널의 역할을 같이 수행
- 애플리케이션은 레디스 클러스터의 노드 중 하나라도 연결되면 클러스터의 전체 상태 정보를 확인 가능
장애 상황 발생 또는 노드 확장(Scale out) 시 애플리케이션의 Redis 서버 연결 정보 변경 필요 X

## Redis 사용 시 주의할 점
- **시간 복잡도** <br/>
  Single Threaded(싱글 스레드) 사용으로 한번에 하나의 명령만 수행이 가능하므로 처리 시간이 긴 요청의 경우 장애가 발생

- ***메모리 단편화** <br/>
  크고 작은 데이터를 할당하고 해제하는 과정에서 메모리의 파편화가 발생하여 응답 속도가 느려질 수 있음

  > **💡메모리 단편화 (Memory Fragmentation)**
  > ![image](https://github.com/user-attachments/assets/745d2307-167b-4804-a888-b46660a368ac) <br/>
  > RAM에서 메모리를 할당받고 해제하는 과정에서 위와같이 부분부분 빈 공간이 생기는데,<br/>
  > 새로운 메모리 할당 시에 사용 가능한 메모리가 충분히 존재하지만 메모리의 크기만큼의 부분이 없어 마지막 부분에 할당되어 메모리 낭비가 심하게 됨<br/>
  > 이 현상이 계속되면 실제 physical 메모리가 커져 프로세스가 죽는 현상이 발생 할 수도 있으므로, redis를 사용 시에 메모리를 적당히 여유있게 사용하는 것이 좋음
<br/>

- **주기적인 모니터링** <br/>
  메모리 사용량이 너무 많으면 Redis 서버의 성능 저하나 장애로 이어질 수 있기 때문에 주기적인 모니터링을 통해 메모리를 관리해주어야함



### Reference
https://velog.io/@wnguswn7/Redis%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-Redis%EC%9D%98-%ED%8A%B9%EC%A7%95%EA%B3%BC-%EC%82%AC%EC%9A%A9-%EC%8B%9C-%EC%A3%BC%EC%9D%98%EC%A0%90
https://velog.io/@banggeunho/%EB%A0%88%EB%94%94%EC%8A%A4Redis-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90.-%EC%A0%95%EC%9D%98-%EC%A0%80%EC%9E%A5%EB%B0%A9%EC%8B%9D-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%9C%A0%ED%9A%A8-%EA%B8%B0%EA%B0%84
https://azderica.github.io/01-db-nosql-redis/
