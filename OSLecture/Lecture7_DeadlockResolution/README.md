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
### Exclusive use of resources              // 자원의 특성 - 혼자 쓰고
### Non-preemptible resources               // 자원의 특성 - 선점 못하고
### Hold and wait (Partial allocation)      // 프로세스의 특성 - 자원을 hold하며
- 자원을 하나 hold하고 다른 자원 요청  
### Circular wait                           // 프로세스의 특성 - 순환할때
### 위 조건 중 하나만이라도 없앤다면, 데드락은 절대 발생하지 않는다.
***
## Deadlock 해결 방법
### Deadlock prevention methods
- 교착상태 예방
- 4개의 deadlock 발생 필요 조건 중 하나를 제거
- 심각한 자원 낭비 발생
  - Low device utilization
  - Reduced system throughput
- 비현실적
1. 모든 자원을 공유 허용
   - exclusive use of resources 조건 제거
   - 현실적으로 불가능
2. 모든 자원에 대해 선점 허용
   - Non-preemptible resources 조건 제거
   - 현실적으로 불가능
   - 유사한 방법
     - 프로세스가 할당받을 수 없는 자원을 요청한 경우, 기존에 가지고 있던 자원을 모두 반납하고 작업 취소
     - 이후 처음(or 체크포인트)부터 다시 시작
   - 심각한 자원낭비 발생 -> 비현실적
3. 필요 자원 한번에 모두 할당 (Total allocation)
   - Hold and wait 조건 제거
   - 자원 낭비 발생
     - 필요하지 않은 순간에도 가지고 있음
   - 무한 대기 현상 발생 가능
4. Circular wait 조건 제거
   - Totally alocation을 일반화 한 방법
   - 자원들에게 순서를 부여
   - 프로세스는 순서의 증가 방향으로만 자원 요청 가능
   - 자원 낭비 발생
### Deadlock avoidance method
- 교착상태 회피
- 시스템의 상태를 계속 감시
- deadlock 상태가 될 가능성이 있는 자원 할당 요청 보류
- 시스템을 항상 safe state로 유지
  - 모든 프로세스가 정상적 종료 가능한 상태
  - Safe sequence가 존재
    - deadlock상태가 되지 않을 수 있음을 보장
- 가정
  - 프로세스의 수가 고정됨
  - 자원의 종류와 수가 고정됨
  - 프로세스가 요구하는 자원 및 최대 수량을 알고 있음
  - 프로세스는 자원을 사용 후 반드시 반납한다
- Not practical
- Dijkstra's algorithm
  - Banker's algorithm
  - Deadlock avoidance를 위한 간단한 이론적 기법
  - 가정
    - 한 종류(resource type)의 자원이 여러 개(unit)
  - 시스템을 항상 safe state로 유지
  - 자원을 하나씩 빌려준다고 가정하면서, safe sequence가 없는 경우 요청을 거절하는 알고리즘
- Habermann's algorithm
  - 다익스트라 알고리즘의 확장
  - 여러 종류의 자원 고려
    - multiple resource types
    - multiple resource units for each resource type
  - 시스템을 항상 safe state로 유지
  - request가 Ra, Rb, Rc로 나뉘어진 확장
- 특징
  - High overhead
    - 항상 시스템을 감시하고 있어야 한다
  - Low resouce utilization
    - Safe state 유지를 위해, 사용되지 않는 자원이 존재
  - Not practical
    - 가정
      - 프로세스 수, 자원 수가 고정
      - 필요한 최대 자원 수를 알고 있음
### Deadlock detectionn and deadlock recovery methods
- 교착상태 탐지 및 복구
- 데드락 방지를 위한 사전 작업을 하지 않음
  - 데드락 발생 가능
- 주기적으로 데드락 발생 확인
  - 시스템이 데드락 상태인가?
  - 어떤 프로세스가 데드락 상태인가?
- Resource allocation graph (RAG) 사용
***
### Resource Allocation Graph (RAG)
- 데드락 검출을 위해 사용
- Directed, bipartite Graph -> 방향성이 있으며, P,R의 2파트로 이루어진 그래프
- Directed graph G = (N, E)
  - N = {Np, Nr} where
    - Np is the set of processes = {P1, P2, ..., Pn}
    - Nr is the set of resources = {R1, R2, ..., Rm}
  - Edge는 Np와 Nr 사이에만 존재
    - e = (Pi, Rj) : 자원 요청
    - e = (Rj, Pi) : 자원 할당
  - Rk : k type의 자원
  - tk : Rk의 단위 자원 수
    - 리소스에 속하는 어떤 자원에는 tk가 존재하는데, Rk의 수를 의미한다
  - |(a,b)| : (a,b) edge의 수
***
### Graph reduction procedure
- 주어진 RAG에서 edge를 하나씩 지워가는 방법
- Completely reduced
  - 모든 edge가 제거 됨
  - deadlock에 빠진 프로세스가 없음
- Ireducible
  - 지울 수 없는 edge가 존재
  - 하나 이상의 프로세스가 deadlock 상태
- Unblocked process > edge를 지운다!
  - 필요한 자원을 모두 할당 받을 수 있는 프로세스
  - 프로세스 Pi가 요청하는 모든 자원 j에 대해, Pi에서 Rj로 가는 edge의 갯수가,
  - Rj의 수 tj에서 (Rj에서 Pk로 가는 edge의 갯수를 모든 k에 대해서 더한 값 <- Rj가 할당된 수)를 뺀 값보다 작으면 지운다
  - 즉, 요청이 남은 수보다 작거나 같으면 지우겠다는 뜻
- Graph reduction procedure
  1. Unblocked process에 연결된 모든 edge를 제거
  2. 더 이상 unblocked process가 없을 때까지 1번 반복
  3. 최종 그래프에서
     - 모든 edge가 제거됨 (Completely reduced)
       - 현재 상태에서 deadlock이 없음
     - 일부 edge가 남음 (irreducible)
       - 현재 deadlock이 존재 
- 특징
  - High overhead
    - 검사 주기에 영향을 받음
    - Node의 수가 많은 경우
  - Low overhead deadlock detection methods (Special case)
    - case1) single-unit resources
      - Cycle detection
    - case2) single-unit request in expedient state
      - Knot detection
***
### Avoidance vs Detection
- Avoidance
  - 최악의 경우를 생각
    - 앞으로 일어날 일을 고려
  - Deadlock이 발생하지 않음
- Detection
  - 최선의 경우를 생각
    - 현재만 고려
  - Deadlock 발생 시 Recovery 과정이 필요
***
### Deaklock Recovery
- 데드락을 검출한 후 해결하는 과정
- Process termination
  - Deadlock 상태에 있는 프로세스를 종료시킴
  - 강제 종료된 프로세스는 이후 재시작 됨
  - Termination cost model
    - 종료시킬 deadlock 상태의 프로세스 선택
    - 우선순위
    - 종류
    - 총 수행 시간
    - 남은 수행 시간
    - 종료 비용
    - etc..
- Resource preemption
  - Deadlock 상태 해결 위해 선점할 자원 선택
  - 선점된 자원을 가지고 있는 프로세스에서 자원을 빼앗음
    - 자원을 빼앗긴 프로세스는 강제 종료됨
***
### termination 선택
- Lowest-termination cost process first
  - simple
  - low overhead
  - 불필요한 프로세스들이 종료될 가능성이 높음
- Minimum cost recovery
  - 최소 비용으로 deadlock 상태를 해소할 수 있는 process 선택
    - 모든 경우의 수를 고려해야 함
  - complex
  - high overhead
  - O(2^k)
### Resource preemption
- deadlock 상태 해결을 위해 선점할 자원 선택
- 해당 자원을 가지고 있는 프로세스를 종료시킴
  - **deadlock 상태가 아닌 프로세스가 종료될 수도 있음**
  - 해당 프로세스는 이후 재시작 됨
- 선점할 자원 선택
  - preemption cost model이 필요
  - minimum cost recovery method 사용
  - O(r)
***
### Checkpoint-restart method
- 프로세스의 수행 중 특정 지점(checkpoint)마다 context를 저장
- Rollback을 위해 사용
  - 프로세스 강제 종료 후, 가장 최근의 checkpoint에서 재시작