# 1. 프로세스 관리
***
## 프로세스의 정의
### 작업/프로그램
- 실행할 프로그램 + 데이터
- 컴퓨터 시스템에 실행 요청 전의 상태
### 프로세스
- 실행을 위해 시스템(커널)에 등록된 작업
- 시스템 성능 향상을 위해 커널에 의해 관리됨
- 각종 자원들을 요청하고 할당 받을 수 있는 개체
- 프로세스 관리 블록(PCB)을 할당 받은 개체
- 능동적인 개체 (active entity)
  - 실행 중에 각종 자원을 요구, 할당, 반납하며 진행
### PCB
- Process Control Block
- OS가 프로세스 관리에 필요한 정보 저장
- 커널 공간 내에 존재
- 각 프로세스들에 대한 정보를 관리
- 프로세스 생성 시, 생성된다.
***
## PCB가 관리하는 정보
### PID : Process Identification Number
- 프로세스 고유 식별 번호
### 스케쥴링 정보
- 프로세스 우선순위 등과 같은 스케쥴링 관련 정보들
### 프로세스 상태
- 자원 할당, 요청 정보 등
### 메모리 관리 정보
- Page table, segment table 등
### 입출력 상태 정보
- 할당 받은 입출력 장치, 파일 등에 대한 정보 등
### 문맥 저장 영역 (context save area)
- 프로세스의 레지스터 상태를 저장하는 공간 등
### 계정 정보
- 자원 사용 시간 등을 관리
***
## 프로세스의 종류
### 역할에 따라
#### 1. 시스템(커널) 프로세스
- 모든 시스템 메모리와 프로세서의 명령에 액세스할 수 있다
- 프로세스 실행 순서 제어, 다른 사용자 및 커널 영역을 침범하지 못하게 감시
- 사용자 프로세스 생성
#### 2. 사용자 프로세스
- 사용자 코드를 수행하는 프로세스
### 병행 수행 방법에 따라
#### 1. 독립 프로세스
- 다른 프로세스에 영향을 주지 않거나 받지 않으면서 수행하는 병행 프로세스
#### 2. 협력 프로세스
- 다른 프로세스에 영향을 주거나 받는 병행 프로세스
***
## 자원이란?
### 커널의 관리 하에 프로세스에게 할당/반납되는 수동적 개체 (passive entity)
***
## 프로세스의 상태
<img alt="ProcessState" height="300px" src="C:\Users\qudrh\JavaExercise\Coursera_memo\OS\Lecture3_ProcessManagement\ProcessStateTransitionDiagram.png" title="ProcessState" width="450px"/></img><br/>

### Created State
- 작업을 커널에 등록
- PCB를 할당 및 프로세스 생성
- 커널
  - **가용 메모리 공간 체크** 및 프로세스 상태 전이
  - **Ready** or **Suspended ready**
### Ready State
- 프로세서 외에 다른 모든 자원을 할당 받은 상태
  - 프로세서 할당 대기 상태
  - 즉시 실행 가능 상태
- Dispatch (or Schedule)
  - Ready state -> Running state
### Running State
- 프로세서와 필요한 자원을 모두 할당 받은 상태
- Preemption (Timer run-out)
  - Running state -> Ready state
  - 프로세서 스케쥴링 (e.g, time-out, priority changes)
- Block/sleep
  - Running state -> Asleep state
  - I/O 등 자원 할당 요청
### Blocked/Asleep State
- 프로세서 외에 다른 자원을 기다리는 상태
  - 자원 할당은 System call에 의해 이루어짐
- Wake-up
  - Asleep state -> Ready state
### Suspended State
- 메모리를 할당 받지 못한 상태
  - Memory image를 swap device에 보관
    - Swap device : 프로그램 정보 저장을 위한 특별한 파일 시스템
  - 커널 또는 사용자에 의해 바생
- Swap-out(suspended), Swap-in(resume b)
### Terminated/Zombie State
- 프로세스 수행이 끝난 상태
- 모든 자원 반납 후,
- 커널 내에 일부 PCB 정보만 남아 있는 상태
  - 이후 프로세스 관리를 위해 정보 수집
***
## 인터럽트 (Interrupt)
### - 예상치 못한, 외부에서 발생한 이벤트
### 인터럽트의 종류
- I/O (키보드 마우스 입력 등)
- Clock (CPU 클락)
- Console (콘솔 창에서 발생)
- Program check (프로그램에 문제가 있을 때)
- Machine check (하드웨어 문제)
- Inter-process (다른 프로세스가 쿡 찌르는 것)
- System call
### 인터럽트 처리 과정
1. 인터럽트 발생
2. 커널 개입 -> 프로세스 중단
3. 인터럽트 처리 (handling)
   1. Interrupt handling
      - 인터럽트 발생 장소, 원인 파악
      - 인터럽트 서비스 할 것인지 결정
   2. Interrupt service
      - 인터럽트 서비스 루틴 호출
## Context Switching (문맥 교환)
### Context
- 프로세스와 관련된 정보들의 집합
  - CPU register context -> in CPU
  - Code & data, Stack, PCB -> in memory
### Context Saving
  - 현재 프로세스의 Register context를 저장하는 작업
### Context restoring
  - Register context를 프로세스로 복구하는 작업
### Context switching
  - 실행 중인 프로세스의 context를 저장하고, 앞으로 실행할 프로세스의 context를 복구하는 일
    - 커널의 개입으로 이루어짐
### Context Switch Overhead
- Context switching에 소요되는 비용
  - OS마다 다름
  - OS의 성능에 큰 영향을 줌
- 불필요한 Context switching을 줄이는 것이 중요
  - ex) 스레드 사용 등