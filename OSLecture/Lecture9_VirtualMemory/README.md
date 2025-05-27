# 1. 가상 메모리
***
## Virtual Storage (Memory)
### Non-continuous allocation
- 사용자 프로그램을 여러 개의 block으로 분할
- 실행 시, 필요한 block들만 메모리에 적재
  - 나머지 block들은 swap device에 존재
### 기법들
- Paging system
- Segmentation system
- Hybrid paging/segmentation system
***
## Address Mapping
### Continuous allocation
- Relative address (상대 주소)
  - 프로그램의 시작 주소를 0으로 가정한 주소
- Relocation (재배치)
  - 메모리 할당 후, 할당된 주소(allocation address)에 따라 상대 주소들을 조정하는 작업
### Non-continuous allocation
- Virtual address (가상 주소) = relative address
  - Logical address (논리 주소)
  - 연속된 메모리 할당을 가정한 주소
- Real address (실제 주소) = absolute (physical)
  - 실제 메모리에 적재된 구조
- Address mapping
  - Virtual address -> real address
***
### Block mapping
- 사용자 프로그램을 block 단위로 분할/관리
  - 각 block에 대한 address mapping 정보 유지
- Virtual address : v = (b,d)
  - b = block number <- 시작점
  - d = displacement(offset) in a block <- 시작점으로부터 얼마나 크게 배치할것인가
- Block map table (BMT)
  - Address mapping 정보 관리
    - Kernel 공간에 프로세스마다 하나의 BMT를 가짐
  - Residence bit : 해당 블록이 메모리에 적재되었는지 여부 (0/1)
- BMT에 담겨있는 block number b를 기준으로 residence bit가 1이라면 real address를 찾아가, d만큼 더해 실제 주소를 찾는다
- 정리
1. 프로세스의 BMT에 접근
2. BMT에서 block b에 대한 항목(entry)를 찾음
3. Residence bit 검사
   1) Residence bit = 0의 경우, swap device에서 해당 블록을 메모리로 가져 옴, BMT 업데이트 후 3-2단계 수행
   2) Residence bit = 1의 경우, BMT에서 b에 대한 real address 값 a 확인
4. 실제 주소 r 계산 (r = a + d)
5. r을 이용하여 메모리에 접근
***
## Paging system
### 프로그램을 같은 크기의 블록으로 분할
### Terminologies
- Page
  - 프로그램의 분할된 block
- Page frame
  - 메모리의 분할 영역
  - Page와 같은 크기로 분할
### 특징
- 논리적 분할이 아님 (크기에 따른 분할)
  - Page 공유 및 보호 과정이 복잡함
    - Segmentation 대비
- Simple and Efficient
  - Segmentation 대비
- No external fragmentation
  - Internal fragmentation 발생 가능
***
### Address Mapping
- Virtual address : v = (p,d)
  - p : page number
  - d : displacement(offset)
- Address mapping
  - PMT(Page Map Table) 사용
- Address mapping mechanism
  - Direct mapping (직접 사상)
  - Associative mapping (연관 사상)
    - TLB (Translation Look-aside Buffer)
  - Hybrid direct/associative mapping
***
### Direct mapping
- Block mapping 방법과 유사
- 가정
  - PMT를 커널 안에 저장
  - PMT entry size = entrySize
  - Page size = pageSize
- 정리
1. 해당 프로세스의 PMT가 저장되어 있는 주소 b에 접근
2. 해당 PMT에서 page p에 대한 entry 찾음
  - p의 entry 위치 = b + p * entrySize
3. 찾아진 entry의 존재 비트 검사
   1) Residence bit = 0인 경우 **(page fault)**(Context switching 발생(I/O) -> 오버헤드가 크다) swap device에서 해당 page를 메모리로 적재, PMT를 갱신한 후 3-2 단계 수행
   2) Residence bit = 1인 경우 해당 entry에서 page frame 번호 p'를 확인
4. p'와 가상 주소의 변위 d를 사용하여 실제 주소 r 형성
  - r = p' * pageSize + d
5. 실제 주소 r로 주기억장치에 접근
- 문제점
  - 메모리 접근 횟수가 2배
    - 성능 저하
  - PMT를 위한 메모리 공간 필요
- 해결방안
  - Associative mapping (TLB)
  - PMT를 위한 전용 기억장치 사용
    - Dedicated register or cache memory
  - Hierarchical paging
  - Hashed page table
  - Inverted page table
***
### Associative Mapping
- TLB(Translation Look-aside Buffer) 에 PMT 적재
  - Associative high-speed memory
- PMT를 병렬 탐색
- Low overhead, high speed
- Expensive hardware
  - 큰 PMT를 다루기가 어려움
***
### Hybrid Direct/Associative Mapping
- 두 기법을 혼합하여 사용
  - HW 비용은 줄이고, Associative mapping의 장점 활용
- 작은 크기의 TLB 사용
  - PMT : 메모리(커널 공간)에 저장
  - TLB : PMT 중 일부 entry들을 적재
    - 최근에 사용된  page들에 대한 entry 저장
  - Locality (지역성) 활용
    - 프로그램의 수행과정에서 한번 접근한 영역을 다시 접근(temporal locality) 또는 인접 영역을 다시 접근(spatial locality)할 가능성이 높음
- 정리
  - 프로세스의 PMT가 TLB에 적재되어 있는지 확인
  1) TLB에 적재되어 있는 경우,
     - residence bit를 검사하고 page frame 번호 확인
  2) TLB에 적재되어 있지 않은 경우,
     - Direct mapping으로 page frame 번호 확인
     - 해당 PMT entry를 TLB에 적재함
***
## Memory management
### Page와 같은 크기로 미리 분할하여 관리/사용
- Page frame
- FPM 기법과 유사
### Frame table
- Page frame당 하나의 entry
- 구성
  - Allocated/available field
  - PID field
  - Link field : For free list (사용가능한 fp들을 연결)
  - AV : Free list header (free list의 시작점)
***
### Page Sharing
- 여러 프로세스가 특정 page를 공유 가능
  - Non-continuous allocation
- 공유 가능 page
  - Procedure pages
    - Pure code (reenter code)
  - Data page
    - Read-only data
    - Read-write data
      - 병행성(concurrency) 제어 기법 관리하에서만 가능
- Procedure Page Sharing
  - 메모리의 같은 곳이지만, 지칭하는 표현법이 달라질 수 있는 문제점
  - 프로세스들이 shared page에 대한 정보를 PMT의 같은 entry에 저장하도록 함으로써 문제점 해결
***
### Page Protection
- 여러 프로세스가 page를 공유할 때,
  - Protection bit 사용
***
### Paging System 요약
- 프로그램을 고정된 크기의 block으로 분할 (Page) / 메모리를 block size로 미리 분할 (page frame)
  - 외부 단편화 문제 없음
  - 메모리 통합/압축 불필요
  - 프로그램의 논리적 구조 고려하지 않음
    - Page sharing/protection이 복잡
- 필요한 page만 page frame에 적재하여 사용
  - 메모리의 효율적 활용
- Page mapping overhead
  - 메모리 공간 및 추가적인 메모리 접근이 필요
  - 전용 HW 활용으로 해결 가능
    - 하드웨어 비용 증가
***
## Segmentation System
### 프로그램을 논리적 block으로 분할 (Segment)
- Block의 크기가 서로 다를 수 있음
- ex) stack, heap, main procedure, shared lib, etc..
### 특징
- 메모리를 미리 분할하지 않음
  - VPM과 유사
- Segment sharing/protection이 용이함
- Address mapping 및 메모리 관리의 overhead가 큼
- No internal fragmentation
  - External fragmentation 발생 가능
***
### Address mapping
- Virtual address : v = (s,d)
  - s : segment number
  - d : displacement in a segment
- Segment Map Table (SMT)
- Address mapping mechanism
  - Paging system과 유사
- 순서
1. 프로세스의 SMT가 저장되어 있는 주소 b에 접근
2. SMT에서 segment s의 entry 찾음
   - s의 entry 위치 = b + s * entrySize
3. 찾아진 Entry에 대해 다음 단계들을 순차적으로 실행
   1) 존재 비트가 0인 경우, missing **segment fault**, swap device로부터 해당 segment를 메모리로 적재, SMT를 갱신
   2) 변위(d)가 segment 길이보다 큰 경우 (d > ls), segment overflow exception 처리 모듈을 호출
   3) 허가되지 않은 연산일 경우 (protection bit field 검사), segment protection exception 처리 모듈을 호출
4. 실제 주소 r 계산 (r = as + d)
5. r로 메모리에 접근 
***
### Segment sharing/protection
- 논리적으로 분할되어 있어, 공유 및 보호가 용이함
***
### Segmentation System 요약
- 프로그램을 논리단위로 분할 (segment)/ 메모리를 동적으로 분할
  - 내부 단편화 문제 없음
  - Segment sharing/protection이 용이함
  - Paging system 대비 관리 overhead가 큼
- 필요한 segment만 메모리에 적재하여 사용
  - 메모리의 효율적 활용
- Segment mapping overhead
  - 메모리 공간 및 추가적인 메모리 접근이 필요
  - 전용 HW 활용으로 해결 가능
***
### Paging과 Segmentation 차이
Page : Segmentation
Simple, Low overhead : High management overhead
No logical concept for partitioning : Logical concept for partitioning
Complex page sharing mechanism : Simple and easy sharing mechanism
***
## Hybrid Paging/segmentation system
- Paging과 Segmentation의 장점 결합
- 프로그램 분할
  1. 논리 단위의 segment로 분할
  2. 각 segment를 고정된 크기의 page들로 분할
- Page단위로 메모리에 적재