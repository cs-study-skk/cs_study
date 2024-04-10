# 👉 뮤텍스(Mutex)
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/ad116acc-a995-4e2d-b012-8dcb97fe355e)

- 공유된 자원의 데이터 혹은 임계영역(Critical Section) 등에 **하나의 Process 혹은 Thread**가 접근하는 것을 막아줌 (동기화 대상이 하나)
- 한 프로세스에 의해 소유될 수 있는 Key를 기반으로 한 상호배제 기법 
- Key에 해당하는 어떤 객체(Object)가 있으며, 이 객체를 소유한 스레드/프로세스만 이 공유자원에 접근 가능
- 즉 뮤텍스 객체를 두 스레드가 동시에 사용할 수 없다.

<br>

# 👉 세마포어(Semaphore)
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/6df5043a-e26c-485b-a20a-c87380d33bb5)

- 공유된 자원의 데이터 혹은 임계영역(Critical Section) 등에 **여러 Process 혹은 Thread**가 접근하는 것을 막아줌 (동기화 대상이 하나 이상)
- 멀티 프로그래밍 환경에서 공유된 자원에 대한 접근을 제한하는 방법
- 사용하고 있는 스레드/프로세스의 수를 공통으로 관리하는 하나의 값을 이용
- 공유 자원에 접근할 수 있는 프로세스의 최대 허용치만큼 동시에 접근 가능
- 각 프로세스는 세마포어의 값을 확인하고 변경 가능
- 자원을 사용하지 않는 상태가 될 때 대기하던 프로세스가 즉시 자원을 사용한다.

<br>

# 뮤텍스와 세마포어의 차이
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/01cff557-bf1e-438b-9098-67f1171fb026)

### 1. 동기화 대상의 개수
Mutex는 동기화 대상이 only 1개일 때 사용<br>
Semaphore는 동기화 대상이 1개 이상일 때 사용<br>

### 2. 변화 여부
세마포어는 뮤텍스가 될 수 있지만 뮤텍스는 세마포어가 될 수 없다.

### 3. 자원의 소유
Mutex는 자원 소유가능 + 책임을 가지는 반면 Semaphore는 자원 소유가 불가하다.

### 4. 해제
Mutex는 소유하고 있는 스레드 만이 이 Mutex를 해제할 수 있다.<br>
반면, Semaphore는 Semaphore를 소유하지 않는 스레드가 Semaphore를 해제할 수 있다.

<br><br>

### Reference
https://velog.io/@heetaeheo/%EB%AE%A4%ED%85%8D%EC%8A%A4Mutex%EC%99%80-%EC%84%B8%EB%A7%88%ED%8F%AC%EC%96%B4Semaphore%EC%9D%98-%EC%B0%A8%EC%9D%B4
