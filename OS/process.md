# 👉프로세스(Process)
**운영체제가 프로그램을 실행하기 위해 필요한 가장 작은 단위**로 하나 혹은 그 이상의 Thread로 실행되는 컴퓨터 프로그램의 instance이다.
<br>가상 메모리 공간, 코드, 데이터, 시스템 자원의 집합이며 프로그램 동작 그 자체를 의미한다.
<br><br>

## 프로세스 메모리 구조
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/e1a62de0-c815-4d31-a920-46d9cf607754)

### Stack
- 데이터를 일시적으로 저장하는 영역
- 매개변수, 지역변수, return 주소 등이 존재
- 컴파일러에 의해 RunTime 도중 크기가 결정되며, 함수가 호출, 종료 되는 시점에 생성, 제거
### Heap
- new, delete, malloc, free 등을 호출하여 메모리가 할당, 해제되는 영역
- Java에서는 GC(Garbage Collector)에 의해 정리
- Run time에 크기가 결정
### Data Section
- 전역변수(global), 정적변수(static) 등이 저장되는 영역 
- 컴파일 타임에 크기가 결정된다.
### Code
- 코드 자체를 구성하는 메모리 영역
- 사용자가작성한 프로그램 코드가 기계어로 변환되어 저장되는 공간
- 컴파일 타임에 크기가 결정된다.
<br><br>

## 프로세스의 상태
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/02897b02-b1c4-40fd-84f6-5e7b2004f044)

### New(Created)
- 프로세스가 생성된 상태
- 사용자가 요청한 작업이 커널에 등록되고 *PCB가 만들어진 상태
- 메모리 획득 대기 
### Ready
프로세스(process)가 메모리에 적재되어 생성된 후, CPU에 할당되기를 기다리는 상태
### Running
- CPU를 할당 받아 실행 중인 상태
- 할당 받은 특정 시간동안만큼만 CPU를 점유 
### Wait(Blocked)
프로세스가 실행되다가 할당된 시간 전에 입출력 처리 요청이나 새로운 자원 요청으로 인해 CPU를 양도하고 요청한 일이 완료되기를 기다리는 상태  
### Terminated
- process 종료된 상태
- 모든 자원이 회수되고 *PCB도 삭제됨.
<br><br>

>💡**Process Control Block (PCB)**<br>
> 프로세스마다 현재 상태를 하나의 데이터 구조에 저장해 관리하는데, 이를 Process Control Block (PCB) 라고 부른다. 
> PCB는 다른 프로세스들이 쉽게 접근할 수 없고 Kernel 영역에 저장된다.  
> <br>PCB에는 다음과 같은 정보들이 저장되어 있다.
> - 현재 Process State
> - Process ID, Parent Process ID
> - CPU registers (프로세스의 레지스터 상태를 저장하는 공간)
> - Program Counter (프로세스에서 실행되야할 다음 instruction의 주소)
> - CPU Scheduling 정보 (CPU Scheduling에 필요한 우선순위 및 scheduling queues의 pointer)
> - 메모리관리 정보 (코드, 데이터, 스택 위치정보)
> <br><br>
> PCB는 현재 프로세스가 instruction의 어떤 곳 까지 실행하였는지, register에는 어떤 값들이 저장되어 있는지 등 현재 시점의 모든 상태를 가지고 있다.
> 따라서 현재 프로세스의 상태를 PCB에 저장한다면, 다시 원래 프로세스의 실행 순서가 돌아 왔을 때, 이전에 하던 작업을 PCB를 통해 그대로 복원할 수 있다.
<br><br>

## 프로세스 통신 : IPC (Inter-Process Communication)
원칙적으로는 하나의 프로세스는 다른 프로세스의 수행에 영향을 미칠 수 없다.
<br>하지만 효율을 위해 운영체제는 IPC(Inter-Process Communication)를 제공한다.

### 프로세스 간 통신의 목적
- 정보 공유<br>
  여러 사용자가 상태나 데이터를 주고받으며 정보를 공유
- 계산 속도 향상<br>
  여러 프로세스가 동시에 작업을 병렬로 처리하기 때문에 속도 향상
- 모듈성<br>
  시스템 기능을 별도의 프로세스 또는 스레드로 분할하여 모듈 식 방식으로 시스템 구성 가능
- 편의<br>
  한 번에 여러 작업 수행

![image](https://github.com/cs-study-skk/cs_study/assets/77658108/9a636da4-d599-4bea-bbcd-4ecefd5c97fc)

### 1. 공유 메모리(shared memory) 방식
- 프로세스들이 주소공간의 일부를 공유
- 메모리에 직접 접근하기 때문에 속도가 빠름

### 2. 메세지 전달(message passing) 방식
- 프로세스 간 공유 데이터를 일체 사용하지 않고, 메세지 객체로 주고받으며 통신
- 프로세스간 주소공간이 달라 직접 전달하지 않고 커널이 대행
- 커널을 통해 데이터를 주고 받으므로 별도의 동기화 로직이 필요하지 않음
- 적은 양의 데이터 교환에 효과적이나 속도가 느림
- 커뮤니케이션 링크 생성 후 send(), receive() 시스템 콜 연산으로 통신
<br><br>

### References
https://velog.io/@curiosity806/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EB%9E%80
https://velog.io/@mingadinga_1234/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EB%9E%80-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%9D%98-%EC%83%81%ED%83%9C
https://wonit.tistory.com/81
https://yoongrammer.tistory.com/56
