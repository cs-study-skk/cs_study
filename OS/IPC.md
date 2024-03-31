# 👉 IPC(Inter Process Communication)
## IPC 란?
**프로세스 간 소통 방법**을 의미한다.<br>
프로세스는 독립적 (independent)이거나 다른 프로세스와 협력(cooperating)한다.<br>
독립적인 프로세스는 동시에 실행 중인 다른 프로세스에 영향을 주지 않지만, 협력이 필요한 프로세스의 경우 다른 프로세스에 영향을 주거나 받을 수 있다.

## IPC가 필요한 이유
1. 정보 공유<br>
여러 사용자가 동일한 정보에 관심을 가지면, 이 정보에 동시에 접근할 수 있는 환경을 제공해야 한다.

2. 계산 속도 증가<br>
특정 작업 (연산)을 빠르게 실행하기 위해서는 작업은 작은 단위로 나누어서 병렬로 실행할 수 있다.

3. 모듈성<br>
시스템 기능을 프로세스 단위 또는 스레드 단위로 나눈 모듈 방식으로 시스템을 구성할 수 있다.

4. 편리성<br>
개별 사용자도 동시에 많은 작업을 수행할 수 있다. (예, 편집, 인쇄, 컴파일 등)

<br><br>
# 👉 IPC 모델
IPC에는 전통적으로 두 가지 모델이 있다.
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/73bf8c78-2dad-4001-a7c8-ec4eca2759c3)
1. Shared memory
2. Message Passing

<br><br>
# 1) Shared Memory model
- 협력 프로세스 간 **공유되는 메모리를 생성 후 이를 이용하여 정보를 교환**한다. 
- 데이터의 형식과 위치는 프로세스들에 의해 결정되며 운영체제 소관은 아니다 (커널의 도움이 거의 필요 없다).
- 성능은 좋지만 프로세스간의 동기화 문제가 발생하여 어플리케이션에서 직접 동기화를 해야한다. (충돌 가능성이 있다.)

## 방법
Shared Memory model에는 producer 프로세스와 consumer 프로세스가 있다. <br>
공유 메모리(버퍼)를 통해 동시에 (concurrently) 통신한다. <br>
생산자가 item을 생산하고 buffer에 저장하면, 소비자는 buffer의 item을 소비한다.<br>
가용한 항목이 있을 때만 소비가 이루어지도록, 생산자/소비자 간의 프로세스는 동기화되어야 한다.

## 모델예시
## Shared Memory
- 프로세스가 공유메모리 할당을 커널에 요청하면 커널은 해당 프로세스에 메모리 공간을 할당
- 어떤 프로세스건 해당 메모리 영역에 접근 가능
- 대량의 정보를 다수의 프로세스에게 배포 가능
- 커널을 통하지 않고 곧바로 메모리에 접근하기 때문에 모든 IPC 방식 중 가장 빠름.

## Memory Map
- **열린 파일을 메모리에 매핑시켜서 공유**하는 방식 (파일 + 메모리)
- 주로 파일로 대용량 데이터를 공유해야할 때 사용한다.

<br><br>
# 2) Messaging Passing Model
OS가 프로세스 간 통신 방법을 제공하고, 메세지를 대리 전달해준다.<br>
OS가 동기화를 해주기 때문에 안전하고 동기화 문제가 없으나, 시스템 콜을 사용하기 때문에 성능이 떨어진다.

>**Direct**<br>
> - A가 커널에 메시지를 전달하고 그걸 커널이 B에게 직접 전달하는 방식
> - 프로세스들은 서로의 이름을 명시한다.
> - 한 쌍에는 하나의 링크만 존재하고, 1대 1통신에 사용된다.
> - 시스템콜 사용 예시<br>
> send(P, message): P로 메시지를 보내라<br>
> receive(Q, message): Q로부터 메시지를 받아라.
> <br> <br>
>**Indirect** <br>
> - A가 mailbox 혹은 port를 두고 커널에게 알리면 커널이 B에게 해당 mailbox에 가서 메시지를 읽으라고 알려주는 방식  
> - 메세지들이 mailbox(port)를 이용하여 send와 receive가 이루어진다.
> - 각 mailbox는 고유의 id를 가져 여러 프로세스가 mailbox를 공유할 수 있다.
> - 시스템콜 사용 예시<br> 
> send(A, message) : mailbox A 로 메시지를 보내라.<br>
> receive(A, message): mailbox A로부터 메시지를 받아라.

## 모델예시
## Pipe
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/34bbee14-c18f-4b11-952d-b1abf2fbd76e)
<br>
○ **익명 Pipe**
- **단방향 통신 채널**
- 한 쪽은 보내기만하며, 반대쪽은 받기만 한다. -> 반이중(Half-Duplex) 통신이라고도 함.
- 익명 Pipe는 오로지 **부모 자식간에만 생성 가능**
- 양방향 통신을 위해서는 pipe가 2개 필요해 구현이 어려워짐. 

○ **Named Pipe (FIFO)**
- 익명 Pipe와 달리 부모 자식관계가 아닌 전혀 모르는 프로세스들 사이에서 통신이 가능
- 익명 Pipe의 확장된 개념
- 이름이 있는 pipe(file)를 생성하여 서로 다른 프로세스들이 pipe의 이름만 알면 통신 가능
- 반이중 통신으로 양방향 통신을 위해서는 pipe가 2개 필요 

## Message Queues
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/fc7767fd-0867-4295-a777-0dd35cb5abfa)
- 메모리를 사용한 Named pipe 형태 -> 단방향 통신
- 단, Pipe처럼 데이터의 흐름이 아닌 메모리 공간이다. 
- 사용할 데이터에 번호를 붙여 여러 프로세스가 동시에 데이터를 다룰 수 있다.
- 커널에서 제공하기 때문에 메모리 제한이 있을 수 있다.

## Socket
- 네트워크 소켓 통신을 이용하여 데이터를 공유 
- 서버와 클라이언트가 임의의 포트를 정해 해당 포트를 통해 데이터를 주고받는다. (1 대 1) 
- 원격에서 프로세스 간의 데이터를 공유할 때 사용
- 양방향 통신이 가능


<br><br>
**Reference**<br>
https://velog.io/@chanyoung1998/Inter-Process-CommunicationIPC-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EA%B0%84-%ED%86%B5%EC%8B%A0<br>
https://velog.io/@coastby/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-IPC-Inter-Process-Communication-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EA%B0%84-%ED%86%B5%EC%8B%A0
