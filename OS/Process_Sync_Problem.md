# 👉 프로세스 동기화
- 멀티 프로세스 환경에서 여러 프로세스들이 공유 자원에 접근하여 데이터를 처리할 때, 프로세스 간 상호작용을 통해 일어나는 작업의 순서를 조절하는 것
- 프로세스 동기화를 통해 여러 프로세스들이 동시에 접근하더라도 원활한 처리가 가능해짐

## 프로세스 동기화가 필요한 이유
- 여러 프로세스가 공유 자원에 접근하여 데이터를 처리할 때, 각 프로세스가 자원을 사용하는 순서에 따라 결과가 달라지기 때문이다.
- 이를 해결하기 위해, 프로세스 간의 순서를 조절하여 데이터 처리의 일관성을 유지할 필요가 있다.

## 임계구역 (Critical Section) 
- 공유 데이터를 변경하는 코드 영역
- 해당 영역으로의 접근을 제어함으로써, 프로세스 간 경쟁 상태 문제를 해결할 수 있다.

> **경쟁 상태 (Race Condition)** <br>
> 한 개의 자원에 여러 프로세스가 접근하려고 하는 상태 <br>
> 어떤 순서로 데이터에 접근하느냐에 따라 결과 값이 달라질 수 있는 상황<br>
> 데이터의 일관성을 확보할 수 없음

### 임계구역 조건
**1. 상호 배제(Mutual Exclusion)**<br>
한 프로세스가 임계구역에 들어가면 다른 프로세스는 임계구역에 들어갈 수 없도록 해야한다. <br><br>
**2. 한정 대기(Bounded Waiting)**<br>
임계 구역에서 작업 중인 프로세스가 나오기를 기다리면서 무한 대기를 하면 안 된다. <br>
일정 시간 이내에 다른 프로세스가 임계구역에 들어갈 수 있도록 해야한다.<br><br>
**3. 진행(Progress)**<br>
임계 영역에서 실행 중인 프로세스가 없다면 임계영역으로 진입하려는 프로세스들 중 하나는 유한한 시간 내에 진입할 수 있어야 한다.<br> 

> **교착상태(Deadlock)** <br>
> 서로 Lock을 놓기만을 기다리는 상황으로 두 개 이상의 프로세스가 어떤 사건을 기다리고 있는데, 같이 기다리는 프로세스 중 하나만이 그 사건을 발생시킬 수 있는 상황

# 👉 해결 방안
## 1. 스핀락
```
import java.util.concurrent.atomic.AtomicBoolean;

public class SpinLock {
    private AtomicBoolean isLocked = new AtomicBoolean(false);
    
    public void lock() {
        while (!isLocked.compareAndSet(false, true)) {
            // isLocked 값이 false이면 true로 설정하고, 이전 값이 false인 경우에만 루프를 빠져나옵니다.
            Thread.yield(); // 다른 스레드에게 실행을 양보합니다.
        }
    }
    
    public void unlock() {
        isLocked.set(false);
    }
}
```
스핀락은 반복문을 돌면서 계속해서 임계 구역에 진입 가능 여부를 확인한다.
계속해서 반복문을 돌기 때문에 성능적으로 비효율적이다. 그러나 특정한 경우, 컨텍스트 스위칭 시간이 더 짧거나 멀티 코어 프로세스일 경우 대기시간보다 응답시간이 짧아서 더 빠른 성능을 가질 수 있다.

## 1. 잠금 (Lock)
특정 프로세스가 공유 자원에 접근 중일 때 다른 프로세스는 접근하지 못하도록 Lock을 걸어 해결한다.
```
public class CriticalSection {
    private int count = 0;
    private Lock lock = new ReentrantLock();

    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }

    public int getCount() {
        return count;
    }
}
```
increment() 메소드는 임계구역에 접근하는 코드이다. 
lock() 메소드를 호출하여 잠금을 획득하고, 임계구역에 접근한 후에는 unlock() 메소드를 호출하여 잠금을 해제한다.

이를 통해 여러 스레드가 동시에 increment() 메소드에 접근하더라도 한 번에 하나씩 실행되도록 보장할 수 있다.

## 2. 세마포어 (Semaphore)
```
public class SharedResource {
   private Semaphore semaphore = new Semaphore(1); // 카운팅 세마포어 생성

   public void accessResource() {
      try {
         semaphore.acquire(); // 세마포어 값을 감소시킴
         // 공유 자원에 대한 작업 수행
      } catch (InterruptedException e) {
         e.printStackTrace();
      } finally {
         semaphore.release(); // 세마포어 값을 증가시킴
      }
   }
}
```
생성자에는 해당 세마포어의 초기값을 전달한다.
위 코드에서는 카운팅 세마포어를 생성하고, 초기값으로 1을 전달하였다.

accessResource() 메서드에서는 공유 자원에 대한 작업을 수행하기 전에 semaphore.acquire() 메소드를 호출하여 세마포어 값을 1 감소시키고, 작업을 마치고 finally 블록에서는 semaphore.release() 메소드를 호출하여 세마포어 값을 1 증가시킨다.

이를 통해 다른 스레드가 공유 자원에 접근할 수 있도록 한다.

## 3. 하드웨어적 해결
```
import java.util.concurrent.atomic.AtomicBoolean;

public class TestAndSetExample {
    
    private AtomicBoolean lock = new AtomicBoolean(false); // 초기값은 false

    public void criticalSection() {
        while (lock.getAndSet(true)); // lock이 false인 경우 true로 변경하고, 그렇지 않은 경우 lock이 true가 될 때까지 대기
        // 임계 구역 시작
        // ...
        // 임계 구역 종료
        lock.set(false); // lock을 false로 변경하여 다른 스레드가 임계 구역에 진입할 수 있도록 함
    }
}
```
AtomicBoolean 클래스를 사용하여 lock 변수를 생성한다. lock 변수는 임계 구역에 진입할 때 사용하는 변수로, 초기값은 false로 설정된다.

getAndSet(true) 메소드를 사용하여 lock 변수의 값을 true로 변경한다. 이때, getAndSet 메소드는 원래 lock 변수의 값을 반환하고, true로 값을 변경한다.

만약 반환된 값이 false이면 while문을 빠져나가고, true이면 while문에서 대기한다.
즉, lock 변수가 false인 경우에만 임계 구역에 진입할 수 있다.

임계 구역을 벗어나면 lock 변수를 다시 false로 변경하여 다른 스레드가 임계 구역에 진입할 수 있도록 한다.
