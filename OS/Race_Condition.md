# 👉Race condition이란?
여러 개의 프로세스가 공유 자원에 동시 접근할 때 실행 순서에 따라 결과 값이 달라질 수 있는 현상 

> **Critical Section(임계 구역)** <br>
> 각 프로세스에서 공유 자원에 접근하는 프로그램 코드 부분

## Race condition의 문제점
### 예측 불가능한 결과
여러 스레드나 프로세스마다 실행 속도가 달라서 잘못된 값을 읽거나 수정할 수 있다. <br>
Ex) A스레드가 수정 한 결과를 B가 수정하면서 A가 수정한 결과는 없어지게 된다.
### 일관성 손실
여러 스레드나 프로세스가 데이터를 수정 시 예기치 않은 상태로 데이터가 변경될 수 있다.
### 디버깅의 어려움
여러 스레드나 프로세스의 각각 다른 실행 속도로 인해 실행 흐름을 읽기 힘들어 디버깅이 어렵다.
### 잠금 대기 시간
Race condition을 방지하기 위해 무분별한 락(Lock)을 사용할 시, 대기 시간으로 인해 성능 저하가 발생할 수 있다.

## Race Condition의 제어 문제
### 상호배제(Mutual Exclusion)
- 두 개 이상의 프로세스가 공용 데이터에 접근하는 것을 막아야 한다.<br>
- 한 프로세스가 공용 데이터를 사용하고 있으면 그 자원을 사용하지 못하도록 막거나, 다른 프로세스가 그 자원을 사용하지 못하도록 막는다.

### 교착상태(Deadlock)
- 상호 배제에 따른 추가적인 제어 문제<br>
- 프로세스가 각자 프로그램을 실행하기 위해 두 자원 모두에 엑세스 해야 한다고 가정할 때, 프로세스는 두 자원 모두를 필요로 하므로 필요한 두 리소스를 사용하여 프로그램을 수행할 때까지 이미 소유한 리소스를 해제하지 않는다.<br> 
- 이러한 상황에서 두 프로세스는 교착 상태에 빠지게 되는 문제가 발생할 수 있다.

### 기아상태(Starvation)
- 프로세스들이 더 이상 진행하지 못하고 영구적으로 블록되어 있는 상태<br>
- 시스템 자원에 대한 경쟁 도중에 발생할 수 있고 프로세스 간의 통신 과정에도 발생할 수 있는 문제이다. <br>
- 두 개 이상의 작업이 서로 상대방의 작업이 끝나기만을 기다리고 있기 때문에 결과적으로는 아무것도 완료되지 못하는 상태가 되게 된다.

## Race condition의 예방
### Mutex
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/9cb93947-b05f-483a-8013-9ed5154ed19a)
<br>Key에 해당하는 Object로 하나의 스레드 혹은 프로세스만 공유 자원에 접근 가능하도록 관리

### Semaphore
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/e308ff2e-e38f-4351-8cb7-097d592b1455)
<br>카운터를 이용해 둘 이상의 다중 프로세스 환경에서 공유 자원의 접근을 관리

[[참고] Mutex & Semaphore](https://github.com/cs-study-skk/cs_study/blob/main/OS/Mutex%26Semaphore.md)


<br><br>


### Reference
https://velog.io/@yarogono/CS-Race-condition%EC%9D%B4%EB%9E%80
https://iredays.tistory.com/125
https://m.blog.naver.com/365blackstar/223367838123
