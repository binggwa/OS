# 1. 프로세스 스케쥴링
***
## 다중 프로그래밍
- 여러개의 프로세스가 시스템 내 존재
- 자원을 할당할 프로세스를 선택해야 함
  - **스케쥴링**
- 자원관리
  - 시간 분할(time sharing) 관리
    - 하나의 자원을 여러 스레드들이 번갈아 가면서 사용
    - ex) 프로세서
    - 프로세스 스케쥴링
      - 프로세서 사용시간을 프로세스들에게 분배
  - 공간 분할(space sharing) 관리
    - 하나의 자원을 분할하여 동시에 사용
    - ex) 메모리
***
## 스케쥴링의 목적
### - 시스템의 성능 향상
### 대표적 시스템 성능 지표 (index)
#### 1. 응답시간 -> 대화형, real-time 시스템에서 중요
- 작업 요청으로부터 응답을 받을 때까지의 시간
#### 2. 작업 처리량 -> 일괄처리 시스템에서 중요
- 단위 시간 동안 완료된 작업의 수
#### 3. 자원 활용도 -> 비싼 장비 사용시 중요
- 주어진 시간(Tc) 동안 자원이 활용된 시간(Tr)
### 목적에 맞는 지표를 고려하여 스케쥴링 기법을 선택
***
## 시스템 성능 지표들
- 평균 응답 시간
  - 사용자 지향적
- 처리량
  - 시스템 지향적
- 자원 활용도
- 공평성
  - FIFO
- 실행 대기 방지
  - 무기한 대기 방지
- 예측 가능성
  - 적절한 시간안에 응답을 보장하는가
- 자원 할당의 공정성 보장
- 단위시간당 처리량 최대화
- 적절한 반환시간 보장
- 예측 가능성 보장
- 오버헤드 최소화
- 자원 사용의 균형 유지
- 반환시간과 자원의 활용 간에 균형 유지
- 실행 대기 방지
- 우선순위
- 서비스 사용 기회 확대
- 서비스 수 감소 방지 etc..
***
### 대기시간
- 프로세스 도착 후 실행 시작까지의 시간
### 응답시간
- 프로세스 도착 후 첫 번째 출력까지의 시간
### 반환시간
- 프로세스 도착 후 실행 종료까지의 시간 (대기 시간 + 실행 시간)
***
## 스케쥴링 기준 (Criteria)
### - 스케쥴링 기법이 고려하는 항목들
- 프로세스의 특성
  - I/O-bounded or compute-bounded
- 시스템 특성
  - Batch system or interactive system
- 프로세스의 긴급성 (urgency)
  - Hard- or soft- real time, non-real time systems
- 프로세스 우선순위 (priority)
- 프로세스 총 실행 시간 (total service time)
***
## CPU burst vs I/O burst
### 프로세스 수행 = CPU 사용 + I/O 대기
### CPU burst 
- CPU 사용시간 
- CPU burst가 더 긴 것을 compute-bounded라 함
### I/O burst 
- I/O 대기시간
- CPU burst보다 I/O burst가 더 긴것을 I/O-bounded라 함
***
## 스케쥴링의 단계 (Level)
### - 발생하는 빈도 및 할당 자원에 따른 구분
### Long-term 스케쥴링
- 장기 스케쥴링
- Job scheduling
### Mid-term
- 중기 스케쥴링
- Memory allocation
### Short-term
- 단기 스케쥴링
- Process scheduling
***
## Long-term Scheduling
### Job scheduling
- 시스템에 제출할 (커널에 등록할) 작업 결정
  - Admission scheduling, High-level scheduling
### 다중 프로그래밍 정도(degree) 조절
- 시스템 내에 프로세스 수 조절
### I/O-bounded 와 compute-bounded 프로세스들을 잘 섞어서 선택해야 함
### 시분할 시스템에서는 모든 작업을 시스템에 등록
- Long-term scheduling이 불필요
***
## Mid-term Scheduling
### 메모리 할당 결정 (memory allocation)
- Intermediate-level scheduling
- Swapping (swap-in/swap-out)
***
## Short-term Scheduling
### Process scheduling
- Low-level scheduling
- 프로세서를 할당할 프로세스를 결정
  - Processor scheduler, dispatcher
### 가장 빈번하게 발생
- Interrupt, block(I/O), time-out etc..
### 매우 빨라야 함
- average CPU burst = 100ms
- scheduling decision = 10ms
- 10 / (100+10) = 9%의 CPU가 스케쥴링에 사용된다 <- 오버헤드
*** 
## 스케쥴링 정책 (Policy)
### 선점 vs 비선점
- Preemptive scheduling, Non-preemptive scheduling
### 우선순위
- Priority
***
## Preemptive/Non-preemptive scheduling
### Non-preemptive scheduling
- 할당 받을 자원을 스스로 반납할 때까지 사용
  - ex) system call, I/O etc..
- 장점
  - Context switch overhead가 적음
- 단점
  - 잦은 우선순위 역전, 평균 응답 시간 증가
### Preemptive scheduling
- 타의에 의해 자원을 빼앗길 수 있음
  - ex) 할당 시간 종료, 우선순위가 높은 프로세스 등장
- Context switch overhead가 큼
- Time-sharing system, real-time system 등에 적합
***
## Priority
### - 프로세스의 중요도
### Static priority (정적 우선순위)
- 프로세스 생성시 결정된 priority가 유지 됨
- 구현이 쉽고, overhead가 적음
- 시스템 환경 변화에 대한 대응이 어려움
### Dynamic priority (동적 우선순위)
- 프로세스의 상태 변화에 따라 priority 변경
- 구현이 복잡, priority 재계산 overhead가 큼
- 시스템 환경 변화에 유연한 대응 가능
***
# 2. 프로세스 스케쥴링 - 기본 스케쥴링 알고리즘
***
## FCFS (Fisrt-Come-First-Service)
### Non-preemptive scheduling
### 스케쥴링 기준 (Criteria)
- **도착 시간** (ready queue 기준)
- 먼저 도착한 프로세스를 먼저 처리
### 자원을 효율적으로 사용 가능
- High resource utilization
### Batch system에 적합, interactive system에 부적함
### 단점
- Convoy effect
  - 하나의 수행시간이 긴 프로세스에 의해 다른 프로세스들이 긴 대기시간을 갖게 되는 현상 (대기시간 >> 실행 시간)
- 긴 평균 응답시간 (response time)
***
## RR (Round-Robin)
### Preemptive scheduling
### 스케쥴링 기준 (Criteria)
- 도착 시간 (ready queue 기준)
- 먼저 도착한 프로세스를 먼저 처리
### *자원 사용 제한 시간 (time quantum) 이 있음*
- System parameter
- 프로세스는 할당된 시간이 지나면 자원 반납
  - timer-runout
- 특정 프로세스의 자원 독점(monopoly) 방지
- Context switch overhead가 큼
### 대화형, 시분할 시스템에 적합
### Time quantum이 시스템 성능을 결정하는 핵심 요소
- very large (infinite) -> FCFS
- very small time quantum -> processor sharing
  - 사용자는 모든 프로세스가 각각의 프로세서 위에서 실행되는 것처럼 느낌
    - 체감 ㅍ프로세서 속도 = 실제 프로세서 성능의 1/n
  - High context switch overhead
***
## SPN (Shortest-Process-Next)
### Non-preemptive scheduling
### 스케쥴링 기준 (Criteria)
- 실행시간 (burst time 기준)
- Burst time이 가장 작은 프로세스를 먼저 처리
  - SJF(Shortest Job First) scheduling
### 장점
  - 평균 대기시간 (WT) 최소화
  - 시스템 내 프로세스 수 최소화
    - 스케쥴링 부하 감소, 메모리 절약 -> 시스템 효율 향상
  - 많은 프로세스들에게 빠른 응답 시간 제공
### 단점
  - Starvation (무한대기) 현상 발생
    - BT가 긴 프로세스는 자원을 할당 받지 못할 수 있음
      - Aging 등으로 해결 (e.g. HRRN)
  - 정확한 실행시간을 알 수 없음
    - 실행시간 예측 기법이 필요
***
## SRTN (Shortest Remaining Time Next)
### SPN의 변형
### Preemptive scheduling
- 잔여 실행 시간이 더 적은 프로세스가 ready 상태가 되면 선점됨
### 장점
- SPN의 장점 극대화
### 단점
- 프로세스 생성시, 총 실행 시간 예측이 필요함
- 잔여 실행을 계속 추적해야 함 = overhead
- Context switching overhead
### -> 구현 및 사용이 비현실적
***
## HRRN (High-Response-Ratio-Next)
### SPN의 변형
- SPN + Aging concepts, Non-preemptive scheduling
### Aging concepts
- 프로세스의 대기 시간 (WT)을 고려하여 기회를 제공
### 스케쥴링 기준 (Criteria)
- Response ratio가 높은 프로세스 우선
### **Response ratio = ( WT + BT ) / BT (응답률)**
- SPN의 장점 + Starvation 방지
- 실행 시간 예측 기법 필요 (오버헤드)
***
### 공평성 기준
- FCFS
- RR
### 효율성, 성능 기준
- SPN
- SRTN
- HRRN
- 문제점 : 실행시간 예측 부하 -> 해결을 위해 MLQ, MFQ
***
## MLQ (Multi-level Queue)
### 작업(or 우선순위)별 별도의 ready queue를 가짐
- 최초 배정된 queue를 벗어나지 못함
- 각각의 queue는 자신만의 스케쥴링 기법 사용
### Queue 사이에는 우선순위 기반의 스케쥴링 사용
- e.g. fixed-priority preemptive scheduling
### 장점
- 빠른 응답시간
### 단점
- 여러 개의 Queue 관리 등 스케쥴링 overhead
- 우선순위가 낮은 queue는 starvation 현상 발생 가능
***
## MFQ (Multi-level Feedback Queue)
### 프로세스의 Queue간 이동이 허용된 MLQ
### Feedback을 통해 우선 순위 조정
- 현재까지의 프로세서 사용 정보 (패턴) 활용
### 특성
- Dynamic priority
- Preemptive scheduling
- Favor short burst-time processes
- Favor I/O bounded processes
- Improve adaptability
### 프로세스에 대한 사전 정보 없이 SPN, SRTN, HRRN 기법의 효과를 볼 수 있음
### 단점
- 설계 및 구현이 복잡, 스케쥴링 overhead가 큼
- Starvation 문제 여전
### 변형
- 각 준비 큐마다 시간 할당량을 다르게 배정
  - 프로세스의 특성에 맞는 형태로 시스템 운영 가능
- 입출력 위주 프로세스들을 상위 단계의 큐로 이동, 우선 순위 높임
  - 프로세스가 block될 때 상위의 준비 큐로 진입하게 함
  - 시스템 전체의 평균 응답 시간 줄임, 입출력 작업 분산 시킴
- 대기 시간이 지정된 시간을 초과한 프로세스들을 상위 큐로 이동
  - 에이징 기법
### Parameters for MFQ scheduling
- Queue의 수
- Queue별 스케쥴링 알고리즘
- 우선 순위 조정 기준
- 최초 Queue 배정 방법 etc..