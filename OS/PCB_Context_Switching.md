# PCB와 Context Switching

## 📌 PCB(Process Control Block)란?
- **운영 체제가 프로세스를 제어하기 위해 정보를 저장하는 곳**
- **프로세스 상태 정보를 저장**하는 **자료 구조**
- 프로세스가 생성될 때마다 고유의 PCB가 생성되고, **프로세스가 완료되면 PCB도 함께 제거**된다.

### ✍️ PCB 구조
![image](https://github.com/cs-study-skk/cs_study/assets/39427152/b131b682-b1f4-4ef2-b386-be6a997d8a22)

**포인터**
- 프로세스의 현재 위치를 저장하는 포인터 정보

**프로세스 상태**
- 프로세스의 생성(New), 준비(Ready), 실행(Running), 대기(Waiting), 종료(Terminated)와 같은 각 상태를 저장

**프로세스 번호**
- 모든 프로세스에는 프로세스 식별자를 저장하는 프로세스 ID 또는 PID라는 고유 한 ID가 할당
  
**프로그램 카운터**
- 프로세스를 위해 실행될 다음 명령어의 주소를 포함하는 카운터를 저장

**레지스터**
누산기, 베이스, 레지스터 및 범용 레지스터를 포함하는 CPU 레지스터에 있는 정보

**메모리 제한**
운영 체제에서 사용하는 메모리 관리 시스템에 대한 정보가 포함됨 (페이지 테이블, 세그먼트 테이블 등)

**열린 파일 목록**
이 정보에는 프로세스를 위해 열린 파일 목록이 포함됨


<br>

## 📌 문맥 교환(Context Switching)란?
- CPU가 현재 작업 중인 프로세스에서 다른 프로세스로 넘어갈 때 **이전 프로세스 정보를 PCB에 저장**하고, 
새롭게 실행할 프로세스의 정보를 **PCB에서 읽어와 레지스터에서 적재하는 과정**을 말한다.
- 이때 **context**는 **CPU 가 프로세스를 실행시키기 위해 필요한 정보**이다.

## 📌 문맥 교환(Context Switching) 과정
![image](https://github.com/cs-study-skk/cs_study/assets/39427152/2595d4de-9953-421e-9848-53b16c1e659f)
- 인터럽트나 트랩에 의해서 컨텍스트를 바꿔야 한다는 요청이 들어온다.
- 기존에 실행중이던 프로세스 P0와 관련된 정보들을 PCB에 저장한다.
- 운영체제는 새롭게 실행할 프로세스 P1에 대한 정보를 해당 PCB 에서 가져와 CPU 레스터에 적재한다.


<br>



참고 블로그: 
[참고](https://yoongrammer.tistory.com/52)
[참고](https://velog.io/@nnnyeong/OS-Context-Switching-PCB-Process-Control-Block)
