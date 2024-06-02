# 👉Storage
## DAS - Direct Attached Storage(직접 연결 스토리지)
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/445c62db-0b42-464e-bcd8-469526a17e86)

저장장치가 **직접 서버에 연결**하여 사용되는 방식<br>
저장공간이 부족해질 경우 새로운 저장 공간을 가장 쉽게 확보할 수 있는 방법

#### 장점
- 쉬운 확장성
- 유선 연결로 빠른 속도

#### 단점
- 시스템들과 파일 공유가 어려움 
- 물리적인 공간이 한계에 봉착하며 확장이 더이상 어려움 

 

## NAS - Network Attached storage(네트워크 결합 스토리지)
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/cf282211-ecfc-421c-9646-f77419b6bc04)

LAN과 같은 **네트워크에 기반한 데이터 공유 방식**<br>
NAS는 서버 또는 PC에 직접 연결하지 않아도 네트워크를 통해 데이터를 주고 받을 수 있다.<br>
FTP(File Transfer Protocol), HTTP(Hypertext Transfer Protocol) 등이 NAS에 속함.<br>

#### 장점
- NAS는 네트워크를 통해 데이터를 외부로 공유할 수 있으며, 여러 장치들과 연결이 가능
- 또한, 데이터를 한곳에 집중하여 유지관리가 편리
- 데이터 보호 및 데이터 백업 솔루션을 기본적으로 제공

#### 단점
- 네트워크를 사용하기 때문에 대역폭으로 인한 전송속도에 제한
- NAS 다운시 스토리지 사용 불가능
- 용량 확장(기존 NAS 장치 자체의 업그레이드)에 제한적

 

## SAN - Storage Area Network(스토리지 영역 네트워크)
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/3ffd1cb1-347f-4356-90bc-84f0b1b279d5)

SAN은 **여러 개의 스토리지를 하나의 네트워크로 연결**시키는 방식<br>
SAN은 여러 개의 스토리지, 서버들을 중계역할을 하는 장비<br>
광케이블을 통해 SAN 스위치와 연결하여 데이터를 주고 받는다.<br>

#### 장점
- SAN은 광케이블을 사용하기에 데이터 접근이 빠르며, LAN을 사용하지 않아 네트워크 부하를 최소화할 수 있다 
- 용량 확장이 용이

#### 단점
- 가격이 상대적으로 비쌈
- 구성에 따라 네트워크 복잡도가 높아짐


# 👉I/O System
## I/O 장치의 종류
- Storage (저장장치) : SSD, HDD, DVD-ROM
- Transmission (전송장치) : 네트워크 카드, 모뎀
- Human-Interface (사용자 인터페이스 장치) : 모니터, 키보드, 마우스

## I/O 장치와 컴퓨터 시스템 사이의 통신
- Port : 장치를 위한 연결 점 (각 I/O 장치가 연결되어 있음)
- Bus : 여러 장치들이 공용하는 와이어 집합
- Controller : Port, Bus, I/O 장치들을 운영함

## Memory Mapped I/O
- I/O 장치의 레지스터가 프로세서의 주소 공간으로 매핑되는 방식<br>
- CPU는 일반 메모리 접근과 동일한 방법으로 I/O 장치에 접근할 수 있게 됨

### 작동 방식
1) 주소 매핑: 시스템의 특정 메모리 주소 범위를 I/O 장치에 할당
2) CPU 접근: CPU는 일반적인 메모리 접근 명령어(예: LOAD, STORE)를 사용하여 I/O 장치의 레지스터에 접근한다.
3) 장치 제어: CPU가 특정 메모리 주소에 데이터를 쓰거나 그 주소에서 데이터를 읽음으로써 I/O 장치를 제어하거나 상태를 읽을 수 있다.

## Polling (폴링)
- I/O 디바이스에 변화가 있는지 지속적으로 감시하는 방식<br>
- I/O 장치의 상태를 주기적으로 loop를 돌면서 전달을 반복한다.

### 작동 방식
1) CPU는 I/O 장치의 Status register를 읽는다.
2) I/O 장치가 준비되었다면 CPU는 데이터를 전송하고, 그렇지 않으면 잠시 대기 후 다시 상태를 확인한다.

## Interrupts (인터럽트)
- 컨트롤러가 자신의 상태가 바뀔 때 마다 CPU에게 통보하는 방식 -> Polling 방식보다 효율적
- CPU가 인터럽트 신호를 감지하면, 현재 상태를 저장 후 Interrupt handler routine(인터럽트가 발생했을 때 실행되는 특수한 코드)으로 이동한다.
- 처리를 수행한 후, return from interrupt 명령을 수행하여 인터럽트 전의 상태로 복귀한다.
- 이벤트가 발생하면 즉시 인터럽트가 발생하므로, 실시간 처리에 적합하다.

## DMA (직접 메모리 접근, Direct Memory Access)
- Device가 직접 Memory에 Access하는 기능을 의미 <br>
- I/O device가 직접 메모리 접근해 데이터를 올려놓고 받아옴. <br>
- CPU개입 없이 주변장치에 데이터 직접전송이 가능한 장점이 있고 interrupt 발생횟수를 줄일 수 있다.<br>

### 작동 방식
![image](https://github.com/cs-study-skk/cs_study/assets/77658108/bcec8f7c-c6eb-42d7-9e35-9b6069f871fe)
1) CPU의 명령에 의해, DMA Controller의 Address, Count, Control가 Setting된다.
> - Address: 명령을 이행할 Main Memory의 Physical Address <br>
> - Count: 실행할 명령의 개수<br>
> - Control: 실행할 명령의 종류<br>

2) DMA Controller가 I/O Device Controller에 명령을 내린다.
3) 명령을 받은 I/O Device Controller는 명령을 이행한다.
4) 명령이 완료되면, I/O Device Controller는 DMA Controller에게 ACK(Acknowledgement)한다.
5) DMA Controller는 Count 값을 1만큼 차감 후 CPU로부터 다음 명령을 전달받는다.
6) 1~5번 과정이 반복되어 I/O Request가 모두 완료되면 DMA Controller는 CPU에 Interrupt Signal을 전송한다.



### Reference
https://bin-kkwon.tistory.com/entry/CS-%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-%EC%A2%85%EB%A5%98-DAS-NAS-SAN
https://velog.io/@yuseogi0218/IO-System-%EC%9E%85%EC%B6%9C%EB%A0%A5-%EC%8B%9C%EC%8A%A4%ED%85%9C
