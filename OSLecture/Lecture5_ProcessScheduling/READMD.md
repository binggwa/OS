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