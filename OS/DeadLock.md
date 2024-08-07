# DeadLock (교착 상태, 데드락)

## 📌 DeadLock (교착 상태, 데드락)란?
![image](https://github.com/cs-study-skk/cs_study/assets/39427152/0a14a548-f240-4421-8157-5ce2e4d54f99)
- 운영 체제에서 발생할 수 있는 문제로 여러 프로세스가 서로의 자원을 기다리며 **무한정으로 대기하는 상태**를 의미함
- 이 상태에서는 관련된 모든 프로세스가 작업을 진행할 수 없기 때문에 시스템 자원을 효율적으로 사용할 수 없고 결국 **시스템의 일부 또는 전체가 멈추는 현상** 발생

<br>
  
## 📌 DeadLock의 네 가지 필요 조건
데드락이 발생하기 위해서는 네 가지 조건이 충족되어야 한다.
#### 1. 상호 배제(Mutual Exclusion)
자원은 한 번에 한 프로세스만 사용할 수 있다. 즉, 자원이 비공유 모드로 존재한다.
#### 2. 점유와 대기 (Hold and Wait)
최소 하나의 자원을 보유한 상태에서 다른 자원을 요청하며 대기하는 프로세스가 존재한다.
#### 3. 비선점 (Non-preemption)
이미 할당된 자원은 그 자원을 보유하고 있는 프로세스에 의해 자발적으로 해제될 때까지 강제로 빼앗을 수 없다.
#### 4. 순환 대기(Circular Wait)
자원을 기다리는 프로세스들이 서로 순환 형태로 대기한다. 예를 들어, P1이 P2의 자원을 기다리고, P2가 P3의 자원을, P3가 P1의 자원을 기다리는 상황이 발생한다.

<br>

## 📌 DeadLock의 예방 (Prevention)
데드락의 발생을 원천적으로 방지하기 위해 데드락의 네 가지 필요 조건 중 하나 이상을 제거하는 방법이다.
#### 1. 상호 배제(Mutual Exclusion) 제거
모든 자원을 공유 가능한 방식으로 만든다. 그러나, 이는 대부분의 자원에 적용하기 어려운 방법이다.
#### 2. 점유와 대기 (Hold and Wait) 제거
프로세스가 자원을 요청할 때, 다른 자원을 보유하고 있지 않은 상태에서만 요청하도록 한다. 프로세스가 시작 시 필요한 모든 자원을 요청하게 하거나, 자원이 필요할 때 현재 보유한 자원을 모두 해제하고 새로 요청하는 방법이 있다.
#### 3. 비선점 (Non-preemption) 제거
원을 점유한 프로세스가 다른 자원을 요청할 때, 현재 보유한 자원을 강제로 해제하도록 한다.
#### 4. 순환 대기(Circular Wait)
자원 유형에 일관된 순서를 부여하고, 프로세스가 자원을 요청할 때 그 순서에 맞게 요청하도록 한다.

<br>

## 📌 DeadLock의 회피 (Avoidance)
- 데드락이 발생할 가능성을 줄이기 위해 시스템이 자원을 할당할 때 데드락이 발생하지 않을 수 있는 안전한 상태를 유지하는 방식으로 자원을 관리하는 기법이다.
- 프로세스가 자원을 요청할 때마다 현재 시스템 상태와 프로세스의 최대 자원 요구량을 고려해 데드락이 발생하지 않는 경우에만 자원을 할당한다.

<br>

> **안전 상태(Safe State)**
> - 시스템이 안전 상태에 있을 때, 프로세스들이 모두 정상적으로 완료될 수 있는 순서를 찾아낼 수 있다.
> - 즉, 모든 프로세스가 필요할 때 적절한 자원을 얻어 작업을 완료할 수 있는 경우를 말한다.
> 
>
> **불안전 상태(Unsafe State)**
> - 시스템이 불안전 상태에 있을 때, 데드락이 발생할 가능성이 있다.
> - 즉, 일부 프로세스가 자원을 얻지 못해 작업을 완료하지 못할 수 있는 경우이다.

<br>

- 이렇게 시스템이 자원을 할당할 때는 데드락 회피 알고리즘을 사용하게 되는데 그 알고리즘을 **은행가 알고리즘(Banker's Algorithm)** 이라고 한다.
[은행가 알고리즘 관련 블로그 게시글](https://wonin.tistory.com/330)

<br>

## 📌 DeadLock의 탐지 (Detection)
데드락이 발생하는 것을 허용하지만 주기적으로 시스템을 검사해 데드락을 탐지하는 방법이다.
- 자원 할당 그래프를 사용해 순환 대기 조건을 검사한다.
- 주기적으로 시스템 상태를 검사하여 데드락을 탐지하고, 탐지되면 회복 절차를 진행한다.

<br>

## 📌 DeadLock의 회복 (Recovery)
데드락 상태를 탐지한 후 해결하는 방법이다.
- 프로세스 종료: 데드락에 관련된 프로세스를 하나씩 종료해 데드락을 해결한다. 이 경우 비용이 크기 때문에 신중해야 하며 모든 데드락 프로세스를 종료하는 방법과 하나씩 종료하며 데드락이 해결되는지 확인하는 방법 두 가지가 있다.
- 자원 선점: 데드락을 해결하기 위해 프로세스로부터 자원을 강제로 회수한다. 피해를 최소화하기 위해 우선 순위가 낮은 프로세스부터 자원을 회수한다.
