# 👉 프로세스 동기화
- 멀티 프로세스 환경에서 여러 프로세스들이 공유 자원에 접근하여 데이터를 처리할 때, 프로세스 간 상호작용을 통해 일어나는 작업의 순서를 조절하는 것
- 프로세스 동기화를 통해 여러 프로세스들이 동시에 접근하더라도 원활한 처리가 가능해짐

## 프로세스 동기화가 필요한 이유
- 여러 프로세스가 공유 자원에 접근하여 데이터를 처리할 때, 각 프로세스가 자원을 사용하는 순서에 따라 결과가 달라지기 때문.
- 프로세스 간의 순서를 조절하여 데이터 처리의 일관성을 유지할 필요가 있다.

<br>

## 임계구역 (Critical Section) 
- 공유 데이터를 변경하는 코드 영역
- 해당 영역으로의 접근을 제어함으로써, 프로세스 간 경쟁 상태 문제를 해결할 수 있다.

> **경쟁 상태 (Race Condition)** <br>
> 한 개의 자원에 여러 프로세스가 접근하려고 하는 상태 <br>
> 어떤 순서로 데이터에 접근하느냐에 따라 결과 값이 달라질 수 있는 상황으로 데이터의 일관성을 확보할 수 없음

### 임계구역 조건
**1. 상호 배제(Mutual Exclusion)**<br>
한 프로세스가 임계구역에 들어가면 다른 프로세스는 임계구역에 들어갈 수 없도록 해야한다. <br><br>
**2. 한정 대기(Bounded Waiting)**<br>
임계 구역에서 작업 중인 프로세스가 나오기를 기다리면서 무한 대기를 하면 안 된다. <br>
일정 시간 이내에 다른 프로세스가 임계구역에 들어갈 수 있도록 해야한다.<br><br>
**3. 진행(Progress)**<br>
임계 영역에서 실행 중인 프로세스가 없다면 임계영역으로 진입하려는 프로세스들 중 하나는 유한한 시간 내에 진입할 수 있어야 한다.<br> 

> **교착상태(Deadlock)** <br>
> 두 프로세스가 사용하고 있는 자원을 서로 기다리고 있을 때 발생한다.
>![image](https://github.com/cs-study-skk/cs_study/assets/77658108/2f1f82a5-438b-4338-9fee-927bf202a2e0)

<br>

# 👉 해결 방안

## 1. 알고리즘(Peterson's Algorithm)
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/b03effb0-13b7-4590-a62b-b30779cd5e0d)

boolean flag[2] : 특정 프로세스가 임계구역으로 들어갈 준비가 되었는지 여부 <br>
int turn : 임계구역으로 진입할 프로세스의 순번 <br> 

Pi 프로세스에 대해서, Pi는 flag[i] = true로 바꾸면서 Critical Section에 진입하려고 한다. 만약 상대방이 들어가고 싶고 (flag[j] == true), 현재 상대방이 Critical Section에 들어가 있으면 (turn == j) 기다린다. 그렇지 않으면 Pi가 들어간다. 
그리고나서 Critical Section을 빠져나오면 들어가고 싶지 않다고 flag[i] = false로 바꿔준다. <br>

이 방식으로는 Mutual Exclusion, Progress, Bounded waiting 모두 만족하지만, Critical Section 진입을 기다리면서 계속 CPU와 메모리를 사용하는 Busy Waiting의 문제점이 있다. 또한, 프로세스가 최대 2개인 경우만 고려한 방식으로 3개 이상일 경우 사용이 어렵다.


## 2. 하드웨어적 해결
하드웨어적으로 현재 상태를 확인하고 변경하는 Test & modify를 atomic 하게 수행할 수 있도록 지원한다. 
Test_and_Set을 이용해 메모리에서 데이터를 읽으면서 쓰는 것까지 하나의 명령어로 동시에 수행이 가능하다.

![image](https://github.com/cs-study-skk/cs_study/assets/77658108/9c460d53-8839-44f2-903c-5ed9180c8d6e)

Test_and_Set() 메소드는 lock 변수의 값을 반환하고, true로 값을 변경한다.

만약 반환된 값이 false이면 while문을 빠져나가고, true이면 while문에서 대기한다.
즉, lock 변수가 false인 경우에만 임계 구역에 진입할 수 있다.

임계 구역을 벗어나면 lock 변수를 다시 false로 변경하여 다른 스레드가 임계 구역에 진입할 수 있도록 한다.

## 3. Mutex Locks
Critical Section Problem을 해결하기 위한 가장 단순한 방법이다.<br>
하나의 프로세스가 Critical Section에서 작업 중이면 다른 프로세스들은 Critical Section에 들어갈 수 없도록 한다. 
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/6ef04375-433e-4cb4-b0da-b0841ca27908)


## 4. Semaphores
여러 프로세스나 스레드가 Critical Section에 진입할 수 있는 메커니즘이다. <br>
카운터를 이용해 동시에 자원에 접근할 수 있는 프로세스를 제한한다.<br>
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/9294a1c0-dc39-4949-8ac9-d867ed8b16a0)

![image](https://github.com/cs-study-skk/cs_study/assets/77658108/f05b07ee-e1ba-40bd-9796-de45655cc8d9)


### Reference
https://velog.io/@eompro/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EB%8F%99%EA%B8%B0%ED%99%94<br>
https://rebro.kr/176

