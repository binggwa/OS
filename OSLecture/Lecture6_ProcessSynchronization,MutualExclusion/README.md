# 1. 프로세스 동기화 & 상호배제
***
## Process Synchronization (동기화)
### 다중 프로그래밍 시스템
- 여러개의 프로세스들이 존재
- 프로세스들은 서로 독립적으로 동작
- 공유 자원 또는 데이터가 있을 때, 문제 발생 가능
### 동기화 (Synchronization)
- 프로세스들이 서로 동작을 맞추는 것
- 프로세스들이 서로 정보를 공유하는 것
***
## Asynchronous and Concurrent P's
### 비동기적 (Asynchronous)
- 프로세스들이 서로에 대해 모름
### 병행적 (Concurrent)
- 여러 개의 프로세스들이 동시에 시스템에 존재
### 병행 수행중인 비동기적 프로세스들이 공유 자원에 동시 접근할 때, 문제가 발생할 수 있음
***
## Terminologies
### Shared data (공유 데이터) or Critical data
- 여러 프로세스들이 공유하는 데이터
### Critical section (임계 영역) CS
- 공유 데이터를 접근하는 코드 영역 (code segment)
### Mutual exclusion (상호배제)
- 둘 이상의 프로세스가 동시에 critical section에 진입하는 것을 막는 것
***
## Critical Section
- 기계어 명령의 특성
  - Atomicity (원자성), Indivisible (분리불가능)
  - 한 기계어 명령의 실행 도중에 인터럽트 받지 않음
***
## Mutual Exclusion (상호배제)
- 명령 수행 과정(Race condition)에 따라 shared data에 대한 결과값이 달라질 수 있으므로
- 한 프로세스가 critical section에 들어와있는 동안, 다른 프로세스가 접근하지 못하게 막아주는 것
***
## Mutual Exclusion Methods
### Mutual exclusion primitives
- enterCS() primitive
  - Critical section 진입 전 검사
  - 다른 프로세스가 critical section 안에 있는지 검사
- exitCS() primitive
  - Critical section을 벗어날 때의 후처리 과정
  - Critical section을 벗어남을 시스템이 알림
***
## Requirements for ME primitives
### Mutual exclusion (상호배제)
- Critical section (CS)에 프로세스가 있으면, 다른 프로세스의 진입을 금지
### Progress (진행)
- CS 안에 있는 프로세스 외에는, 다른 프로세스가 CS에 진입하는 것을 방해하면 안됨
### Bounded waiting (한정대기)
- 프로세스의 CS 진입은 유한시간 내에 허용되어야 함
***
## Mutual Exclusion Solutions
### SW solutions
- Dekker's algorithm (Peterson's algorithm)
- Dijkstra's algorithm, Knuth's algorithm, Eisenberg and McGuire's algorithm
- SW solutions의 단점
  - 속도가 느림
  - 구현이 복잡함
  - ME primitive 실행 중 preemption 될 수 있음
    - 공유 데이터 수정 중은 interrupt를 억제함으로써 해결 가능
    - overhead 발생
  - Busy waiting
    - inefficient
### HW solution
- TestAndSet (TAS) instruction
### OS supported SW solution
- Spinlock
- Semaphore
- Eventcount/sequencer
### Language-Level solution
- Monitor
***
## Dekker's Algorithm
### Two process ME을 보장하는 최초의 알고리즘
- 플래그와 턴을 동시에 사용해 CS로의 진입을 통제
## Peterson's Algorithm
- Dekker's 보다 간단하게 구현
- turn 양보를 선행 및 상대 턴일 경우 루프
***
## N-Process Mutual Exclusion
## 다익스트라(Dijkstra)
- 최초로 프로세스 n개의 상호배제 문제를 소프트웨어적으로 해결
- 실행 시간이 가장 짧은 프로세스에 프로세서 할당하는 세마포 방법, 가장 짧은 평균 대기시간 제공
### Dijkstra 알고리즘의 flag[] 변수
- idle : 프로세스가 임계 지역 진입을 시도하고 있지 않을 때
- want-in : 프로세스의 임계 지역 진입 시도 1단계일 때
- in-CS : 프로세스의 임계 지역 진입 시도 2단계 및 임계 지역 내에 있을 때
***
## 크누스(knuth)
- 이전 알고리즘 관계 분석 후 일치하는 패턴을 찾아 패턴의 반복을 줄여서 프로세스에 프로세서 할당
- 무한정 연기할 가능성을 배제하는 해결책을 제시했으나, 프로세스들이 아주 오래 기다려야 함
***
## 램포트(lamport)
- 사람들로 붐비는 빵집에서 번호표 뽑아 빵 사려고 기다리는 사람들에 비유해서 만든 알고리즘 (Bakery algorithm)
- 준비 상태 큐에서 기다리는 프로세스마다 우선순위를 부여하여 그 중 우선순위가 가장 높은 프로세스에 먼저 프로세스를 할당함
***
## 핸슨(brinch Hansen)
- 실행 시간이 긴 프로세스에 불리한 부분을 보완하는 것
- 대기시간과 실행 시간을 이용하는 모니터 방법
- 분산 처리 프로세서 간의 병행성 제어 많이 발표
***
## HW Solutions
### Synchronization Hardware
### TestAndSet (TAS) instruction
- Test와 Set을 한번에 수행하는 기계어
- Machine instruction
  - Atomicity, Indivisible
  - **실행 중 interrupt를 받지 않음 (preemtion되지 않음)**
- Busy waiting
  - inefficient
ex) TestAndSet 명령어 - 한번에 수행된다 (Machine instruction)
```java
boolean TestAndSet (boolean *target) {
    boolean temp = *target;     // 이전 값 기록
    *target = true;             // true로 설정
    return temp;                // 값 반환
}
```
- But, 3개 이상의 프로세스의 경우, Bounded waiting 조건 위배
***
## ME with TAS Instruction
### N-Process mutual exclusion
```java
do
{
    waiting[i] = true;
    key = true;
    while (waiting[i] && key)
        key = TestAndSet(&lock);
    waiting[i] = false;
        // 임계 영역
        // 탈출 영역
    j = (i + 1) % n;
    while ((j != i) && !waiting[j]) // 대기 중인 프로세스를 찾음
        j = (j + 1) % n;
    if (j = i)                      // 대기 중인 프로세스가 없으면
        lock = false;               // 다른 프로세스의 진입 허용
    else                            // 대기 프로세스가 있으면 다음 순서로 임계 영역에 진입
        waiting[j] = false;         // Pj가 임계 영역에 진입할 수 있도록
        // 나머지 영역
} while (true);
```
- 장점
  - 구현이 간단
- 단점
  - Busy waiting
    - inefficient
- Busy waiting 문제를 해소한 상호배제 기법
  - **Semaphore**
    - 대부분의 OS들이 사용하는 기법
***
## OS supported SW solution
### Spinlock
- 정수 변수
- 초기화, P(), V() 연산으로만 접근 가능
  - 위 연산들은 indivisible (or atomic) 연산
  - OS support - OS가 연산이 쫓겨나지 않고 끝까지 수행되도록 보장
  - 전체가 한 instruction cycle에 수행 됨
```java
P(S) {  // S 는 물건의 갯수, P는 물건을 꺼내가는 것 (자물쇠를 거는 것)
    while (S <= 0) do   // 물건이 없다면, 반복
    endwhile;           // 물건이 있다면, -1 이후 루프 종료
    S <- S - 1;
}

V(S) {  // 물건을 반납하는 것, 자물쇠를 푸는 것
    S <- S + 1;
}
```
- CS 전에 P(active); , CS 후에 V(active);
- active = 1 : 임계 지역을 실행중인 프로세스 없음
- active = 0 : 임계 지역을 실행중인 프로세스 있음
- 문제점
  - 멀티 프로세서 시스템에서만 사용 가능
  - Busy waiting
***
## Semaphore
### 1965년 Dijkstra가 제안
### Busy waiting 문제 해결
### 음이 아닌 정수형 변수(S)
- 초기화 연산, P(), V() 로만 접근 가능
  - P : Probern (검사)
  - V : Verhogen (증가)
- **임의의 S 변수 하나에 ready queue 하나가 할당 됨**
### binary semaphore
- S가 0과 1 두 종류의 값만 갖는 경우
- 상호배제나 프로세스 동기화의 목적으로 사용
### Counting semaphore
- S가 0이상의 정수값을 가질 수 있는 경우
- Producer-Consumer 문제 등을 해결하기 위해 사용
  - 생산자-소비자 문제
### 초기화 연산
- S 변수에 초기값을 부여하는 연산
### P(), V() 연산
```java
P(S)
if (S > 0)
    then S <- S - 1;
else wait on the queue Qs;

V(S)
if (어떤  waiting processes on Qs)
    then wakeup one of them;
else S <- S + 1;
```
### 모두 indivisible 연산
- OS support
- 전체가 한 instruction cycle에 수행 됨
***
### Semaphore로 해결 가능한 동기화 문제들
- 상호배제 문제
- 프로세스 동기화 문제
- 생산자-소비자 문제
- Reader-writer 문제
- Dining philosopher problem
- etc..
***
### Mutual exclusion 해결
- spinlock과는 달리, semaphore는 대기실 queue에서 기다리다 V(active)로 깨개 된다.
### Process shnchronization
- 프로세스들의 실행 순서 맞추기
  - 프로세스들은 병행적이며, 비동기적으로 수행
- sync를 0으로 setup
### Producer-Consumer 문제
- 생산자 프로세스
  - 메시지를 생성하는 프로세스 그룹
- 소비자 프로세스
  - 메세지를 전달받는 프로세스 그룹
- single buffer를 이용해 진행
- 2개의 semaphore변수 지정
- consumed, produced
```java
Producer Pi
repeat 
...
create a new message M;
P(consumed);
buffer <- M;
V(produced);
...
until(false);

Consumer Pi
repeat
...
P(produced);
m <- buffer;
V(consumed);
consume the message m;
...
until(false);
```
***
### N-buffer를 가진 생산자 소비자 문제
```java
Producer Pi
repeat
        ...
create a new message M;
P(mutexP);
P(nrempty); // nrempty : 비워있는 buffer 수
buffer[in] <- M;
in <- (in + 1) mod N;
V(nrfull);  // nrfull : 채워진 buffer 수
V(mutexP);
...
until(false);

Consumer Cj
repeat
        ...
P(mutexC);
P(nrfull);
m <- buffer[out];
out <- (out + 1) mod N;
V(nrempty);
V(mutexC);
...
until(false);
```
***
### Reader-Writer problem
- Reader : 데이터에 대해 읽기 연산만 수행
- Writer : 데이터에 대해 갱신 연산을 수행
- 데이터 무결성 보장 필요
  - Reader들은 동시에 데이터 접근 가능
  - Writer들이 동시 데이터 접근 시, 상호배제 필요
- 해결법
  - reader/writer에 대한 우선권 부여
  - preference solution
```java
(shared variables)
var wmutex, rmutex  : semaphore := 1, 1,
    nreaders        : integer := 0

Reader Ri
repeat
        ...
P(rmutex);
if (nreaders = 0) then      // 아무도 읽고 있지 않으면,
    P(wmutex);              // 읽기 시작할테니 writer는 읽지마.
endif;
nreaders <- nreaders + 1;   // 읽기 시작한 사람 수 증가
V(rmutex);
Perform read operations;    // 읽는 연산
P(rmutex);
nreaders <- nreaders - 1;   // 한 명만 들어가서 읽는 사람 수 감소
if (nreaders = 0) then      // 마지막 독자였다면,
    V(wmutex);              // writer가 쓸 수 있도록 wmutex 잠금해제
endif;
V(rmutex);
...
until(false);

Writer Wj
repeat
        ...
P(wmutex);
Perform write operations
V(wmutex);
...
until(false);
```
***
### No busy waiting
- 기다려야 하는 프로세스는 block(asleep)상태가 됨
### Semaphore queue에 대한 wake-up 순서는 비결정적
- Starvation problem
***
## Eventcount/Sequencer
### 은행의 번호표와 비슷한 개념 - semaphore의 기아현상 해결 위한 해결책
### Sequencer
- 정수형 변수
- 생성시 0으로 초기화, 감소하지 않음
- 발생 사건들의 순서 유지
- ticket() 연산으로만 접근 가능
### ticket(S)
- 현재까지 ticket() 연산이 호출 된 횟수를 반환
- indivisible operation
### Eventcount
- 정수형 변수
- 생성시 0으로 초기화, 감소하지 않음
- 특정 사건의 발생 횟수를 기록
- read(E), advance(E), await(E,v) 연산으로만 접근 가능
### read(E)
- 현재 Eventcount 값 반환
### advance(E)
- E <- E + 1
- E를 기다리고 있는 프로세스를 깨움 (wake-up)
### await(E,v)
- V는 정수형 변수
- if(E<v) 이면 E에 연결된 QE에 프로세스 전달(push) 및 CPU scheduler 호출
```java
repeat
        ...
v <- ticket(S);
await(E,v);
...CS...
advance(E);
...
until(false);
```
***
### N-size Producer-Consumer problem 에 sequencer 도입
```java
(shared variables)
var Pticket, Cticket    : sequencer,
    In, Out             : eventcount,
    buffer              : array[0..N-1] of message;

Producer Pi
var t : integer;

repeat
        ...
Create a new message M;
t <- ticket(Pticket);       // P
awati(In.t);                // P
awati(Out,t-N+1);           // producer가 1명만 들어갈 수 있는 CS
buffer[t mod N] <- M;       // producer가 1명만 들어갈 수 있는 CS
advance(In);                // V
...
until(false)

Consumer Cj
var u : integer;

repeat
        ...
u <- ticket(Cticket);       // P
await(Out,u);               // P
await(In,u+1);              // consumer가 1명만 들어가는 CS
m <- buffer[u mod N];       // consumer가 1명만 들어가는 CS
advance(Out);               // V
Consume the message m;
...
until(false)
```
***
### No busy waiting
### No starvation
- FIFO scheduling for QE
### Semaphore 보다 더 low-level control이 가능 (순서 컨트롤)
***
## Language-Level solution
### 지금까지의 solution은 Flexible하지만, 사용하기 어려운 Low-level mechanism이었다
### High-level Mechanism
- **Monitor**
- Path expressions
- Serializers
- Critical region, conditional critical region
### language-level constructs
### Object-Oriented concept과 유사
### 사용이 쉬움
***
## Monitor
### 공유 데이터와 CS의 집합
### Conditional variable
- wait()
- signal() operations
***
## Monitor의 구조
### Entry queue (진입 큐)
- 모니터 내의 procedure(function) 수만큼 존재
### Mutual exclusion
- 모니터 내에는 항상 하나의 프로세스만 진입 가능 <- language가 보장
### Information hiding (정보 은폐)
- 공유 데이터는 모니터 내의 프로세스만 접근 가능
### Condition queue (조건 큐)
- 모니터 내의 특정 이벤트를 기다리는 프로세스가 대기
### Signaler queue (신호제공자 큐)
- 모니터에 항상 하나의 신호제공자 큐가 존재 (전화부스)
- signal() 명령을 실행한 프로세스가 임시 대기
***
### 자원 할당 문제
- 책을 예로 들어본다.
- 자원을 요청하는 requestR() function(대출), 자원을 반납하는 releaseR() function(반납)
- condition queue R_Free (책을 빌릴 수 있는가?) 물어보는 대기공간
- signaler queue가 condition queue에 있는 애를 깨우는 역할
```java
monitor resourceRiAllocator;
var RilsAvailable : boolean,
    RilsFree      : condition;

procedure requestR();
begin
    if (not R_Available) then   // 책이 없다면
        R_Free.wait();          // Free condition queue에서 기다려라
    R_Available <- false;       // 책이 있다면, 빌려가고 false로 변경
end;

procedure releaseR();
begin
    R_Available <- true;        // 책을 반납 후 true로 변경
    R_Free.signal();            // signal에 있는 대기 호출
end;

begin
    RilsAvailable <- true;
end.
```
***
### 자원 할당 시나리오
1. 자원 R 사용 가능, Monitor 안에 프로세스 없음
2. 프로세스 Pj가 모니터 안에서 자원 R을 요청
3. 자원 R이 Pj에게 할당 됨, 프로세스 Pk가 R 요청, Pm 또한 R 요청 (condition queue로 이동)
4. Pj가 R 반환, R_Free.signal() 호출에 의해 Pk가 wakeup
5. 자원 R이 Pk에게 할당 됨, Pj가 모니터 안으로 돌아와서, 남은 작업 수행
***
### Producer-Consumer Problem
- procedure 2개 fillBuf() <- P, emptyBuf() <- C
- 기다리는 공간 bufHasSpace <- P, bufHasData <- C
- signaler queue
- monitor 내에 in <- P, out <- C, validBufs <- 물건 수
***
```java
monitor ringbuffer;
var buffer      : array[0..N-1] of message,
    validBufs   : 0..N,
    in          : 0..N-1,
    out         : 0..N-1,
    bufHasData, bufHasSpace : condition;

procedure fillBuf(data : message);
begin
    if(validBufs = N) then bufHasSpace.wait();
    buffer[in] <- data;
    validBufs <- validBufs + 1;
    in <- (in + 1) mod N;
    bufHasData.signal();
end;

procedure emptyBuf(var data : message);
begin
    if(validBufs = 0) then bufHasData.wait();
    data <- buffer[out];
    validBufs <- validBufs - 1;
    out <- (out + 1) mod N;
    bufHasSpace.signal();
end;

begin
    validBufs <- 0;
    in <- 0;
    out <- 0;
end
```
***
### Reader-Writer Problem
- reader/writer 프로세스들간의 데이터 무결성 보장 기법
- writer 프로세스에 의한 데이터 접근 시에만 상호 배제 및 동기화 필요
### 모니터 구성
- 변수 2개
  - 현재 읽기 작업을 진행하고 있는 reader 프로세스의 수
  - 현재 writer 프로세스가 쓰기 작업을 진행 중인지 표시
- 조건 큐 2개
  - reader/writer 프로세스가 대기해야 할 경우에 사용
- 프로시져 4개
  - reader(writer) 프로세스가 읽기(쓰기) 작업을 원할 경우에 호출, 읽기(쓰기) 작업을 마쳤을 때 호출
***
```java
monitor readers_and_writers;
var numReaders      : integer,
    writing         : boolean,
    readingAllowed, writingAllowed : condition;

procedure beginReading;
begin
    if(writing or queue(writingAllowed)) then readingAllowed.wait();
    numReaders <- numReaders + 1;
    if(queue(readingAllowed)) then readingAllowed.signal();
end;

procedure finishReading;
begin
    numReaders <- numReaders - 1;
    if(numReaders = 0) then writingAllowed.signal();
end;

procedure beginWriting;
begin
    if(numReaders > 0 or writing) then writingAllowed.wait();
    writing <- true;
end;

procedure finishWriting;
begin
    writing <- false;
    if(queue(readingAllowed)) then readingAllowed.signal();
    else writingAllowed.signal();
end;

begin
    numReaders <- 0;
    writing <- false;
end
```
***
### Dining philosopher problem
- 5명의 철학자
- 철학자들은 생각하는 일, 스파게티 먹는 일만 반복함
- 공유 자원 : 스파게티, 포크
- 스파게티를 먹기 위해서는 좌우 포크 2개 모두 들어야 함 (총 5개)
***
```java
do forever
    pickup(i);
    eating;
    putdown(i);
    thinking;
end
```
```java
monitor dining_philosophers;
var numForks        : array[0..4] of integer,
    ready           : array[0..4] of condition,
    i               : integer;

procedure pickup(me);
var
    me : integer;
begin
    if(numForks[me] != 2) then ready[me].wait();
    numForks[right(me)] <- numForks[right(me)] - 1;
    numForks[left(me)] <- numForks[left(me)] - 1;
end;

procedure putdown(me);
var
    me : integer;
begin
    numForks[right(me)] <- numForks[right(me)] + 1;
    numForks[left(me)] <- numForks[left(me)] + 1;
    if(numForks[right(me)] = 2) then ready[right(me)].signal();
    if(numForks[left(me)] = 2) then ready[left(me)].signal();
end;

begin
    for i <- 0 to 4 do
        numForks[i] <- 2;
end
```
### Monitor의 장점
- 사용이 쉽다
- Deadlock 등 error 발생 가능성이 낮음
### Monitor의 단점
- 지원하는 언어에서만 사용 가능 (language 지원)
- 컴파일러가 OS를 이해하고 있어야 함
  - Critical section 접근을 위한 코드 생성