# 1. 교착상태 Deadlock and Resource types
***
- 어느 프로세서도 원하는 작업을 할 수 없는 상태
## Deadlock의 개념
### Blocked/Asleep state
- 프로세스가 특정 이벤트를 기다리는 상태
- 프로세스가 필요한 자원을 기다리는 상태
### Deadlock state
- 프로세스가 발생 가능성이 없는 이벤트를 기다리는 경우
  - 프로세스가 deadlock 상태에 있음
- 시스템 내에 deadlock에 빠진 프로세스가 있는 경우
  - 시스템이 deadlock 상태에 있음
- 데드락은 일어나지 못하는 asleep 상태에 존재함 vs starvation은 CPU를 기다리고 있는 ready 상태에 존재함
***
## 자원의 분류
### 일반적 분류
- Hardware resources vs Software resources
### 다른 분류법
- 선점 가능 여부에 따른 분류
- 할당 단위에 따른 분류
- 동시 사용 가능 여부에 따른 분류
- 재사용 가능 여부에 따른 분류
***
### 선점 가능 여부에 따른 분류
- Preemptible resources
  - 선점 당한 후, 돌아와도 문제가 발생하지 않는 자원
  - Processor, memory 등 (context switching으로 CPU 문제 없음, swap-device로 인해 memory 문제 없음)
- Non-preemptible resources
  - 선점 당하면, 이후 진행에 문제가 발생하는 자원
  - Rollback, restart 등 특별한 동작이 필요
  - e.g. disk drive 등
***
### 할당 단위에 따른 분류
- Total allocation resources
  - 자원 전체를 프로세스에게 할당
  - e.g. Processor, disk drive 등
- Partitioned allocation resources
  - 하나의 자원을 여러 조각으로 나누어, 여러 프로세스들에게 할당
  - e.g. memory 등
***
### 동시 사용 가능 여부에 따른 분류
- Exclusive allocation resources
  - 한 순간에 한 프로세스만 사용 가능한 자원
  - e.g. Processor, memory, disk drive 등
- Shared allocation resource
  - 여러 프로세스가 동시에 사용 가능한 자원
  - e.g. Program(sw), shared data 등
***
### 재사용 가능 여부에 따른 분류
- SR (Serially-reusable Resources)
  - 시스템 내에 항상 존재하는 자원
  - 사용이 끝나면, 다른 프로세스가 사용 가능
  - e.g. Processor, memory, disk drive, program 등
- CR (Consumable Resources)
  - 한 프로세스가 사용한 후에 사라지는 자원
  - e.b. signal, message 등
***
## Deadlock과 자원의 종류
### Deadlock을 발생시킬 수 있는 자원의 형태
- Non-preemptible resources (뺏을 수 없기 때문)
- Exclusive allocation resources (혼자 쓰기 때문)
- Serially reusable resources (재사용 가능하기 때문)
### CR을 대상으로 하는 Deadlock model
- 너무 복잡!
***
## Deadlock 발생의 예
- 2개의 프로세스 (P1, P2)
- 2개의 자원 (R1, R2)
## Deadlock 모델 (표현법)
### Graph 모델
- Node
  - 프로세스 노드(P1, P2), 자원 노드(R1, R2)
- Edge
  - Rj -> Pi : 자원 Rj이 프로세스 Pi에 할당 됨
  - Pi -> Rj : 프로세스 Pi가 자원 Rj을 요청 (대기 중)
### State Transition Model
- 예제
  - 2개의 프로세스와 A type의 자원 2개(unit) 존재
  - 프로세스는 한번에 자원 하나만 요청/반납 가능
- State
0. 자원이 없는데 요청 x
1. 자원이 없는데 요청
2. 자원 수 1개일때 요청 x
3. 자원 수 1개일때 요청
4. 자원 수 2개, 요청 x
***
## Deadlock 발생 필요 조건
### Exclusive useof resources               // 자원의 특성
### Non-preemptible resources               // 자원의 특성
### Hold and wait (Partial allocation)      // 프로세스의 특성
- 자원을 하나 hold하고 다른 자원 요청  
### Circular wait                           // 프로세스의 특성
