# 1. 파일 시스템
***
## 디스크 시스템
### Disk pack
- 데이터 영구 저장 장치 (비휘발성)
- 구성
  - Sector
    - 데이터 저장/판독의 물리적 단위
  - Track
    - Platter 한 면에서 중심으로 같은 거리에 있는 sector들의 집합
  - Cylinder
    - 같은 반지름을 갖는 track의 집합
  - Platter
    - 양면에 자성 물질을 입힌 원형 금속판
    - 데이터의 기록/판독이 가능한 기록 매체
  - Surface
    - Platter의 윗면과 아랫면
### Disk drive
- Disk pack에 데이터를 기록하거나 판독할 수 있도록 구성된 장치
- 구성
  - Head
    - 디스크 표면에 데이터를 기록/판독
  - Arm
    - Head를 고정/지탱
  - Positioner (boom)
    - Arm을 지탱
    - Head를 원하는 track으로 이동
  - Spindle
    - Disk pack을 고정(회전축)
    - 분당 회전 수 (RPM, Revolutions Per minute)
***
### Disk Address
- Physical disk address
  - Sector (물리적 데이터 전송 단위)를 지정
    - Cylinder number, Surface number, Sector number
- Logical disk address : relative address
  - Disk system의 데이터 전체를 block들의 나열로 취급
    - Block에 번호 부여
    - 임의의 block에 접근 가능
  - Block 번호 -> physical address 모듈 필요 (disk driver
***
### Data Access in Disk System
1) Seek time
   - 디스크 head를 필요한 cylinder로 이동하는 시간
2) Rotational delay
   - 필요한 sector가 head 위치로 도착하는 시간
3) Data transmission time
   - 해당 sector를 읽어서 전송(or 기록)하는 시간
***
## 파일 시스템
- 사용자들이 사용하는 파일들을 관리하는 운영체제의 한 부분
- File system의 구성
  - Files
    - 연관된 정보의 집합
  - Directory structure
    - 시스템 내 파일들의 정보를 구성 및 제공
  - Partitions
    - Directory들의 집합을 논리적/물리적으로 구분
***
### file Concept
- 보조 기억 장치에 저장된 연관된 정보들의 집합
  - 보조 기억 장치 할당의 최소 단위
  - Sequence of bytes (물리적 정의)
- 내용에 따른 분류
  - Program file
    - Source program, object program, executable files
  - Date file
- 형태에 따른 분류
  - Text (ascii) file
  - Binary file
- File attributes (속성)
  - Name
  - Identifier
  - Type
  - Location
  - Size
  - Protection
    - access control information
  - User identification (owner)
  - Time, date
    - creation, late reference, last modification
- File operations
  - Create
  - Write
  - Read
  - Reposition
  - Delete
  - etc..
- OS는 file operation들에 대한 system call을 제공해야 함
***
### File Access Methods
- Sequential access (순차 접근)
  - File을 record(or bytes) 단위로 순서대로 접근
    - ex) fgetc()
- Directed access (직접 접근)
  - 원하는 Block을 직접 접근
    - ex) lseek(), seek()
- Indexed access
  - Index를 참조하여, 원하는 block을 찾은 후 데이터에 접근
***
### File System Organization
- Partitions (minidisks, volumes)
  - Virtual disk
- Directory
  - File 들을 분류, 보관하기 위한 개념
  - Operations on directory
    - Search for a file
    - Create a file
    - Delete a file
    - List a directory
    - Rename a file
    - Traverse the file system
- Mounting
  - 현재 FS에 다른 FS를 붙이는 것
***
## Directory Structure
### Logical directory structure
- Flat (single-level) directory structure
- 2-level directory structure
- Hierarchical (tree-structure) directory structure
- Acyclic graph directory structure
- General graph directory structure
***
### Flat Dirrectory Structure
- FS내에 하나의 directory만 존재
  - Single-level directory structure
- Issues
  - File naming
  - File protection
  - File management
  - 다중 사용자 환경에서 문제가 더욱 커짐
***
### 2-Level Directory Structure
- 사용자마다 하나의 directory 배정
- 구조
  - MFD (Master File Directory)
  - UFD (User File Directory)
- Problems
  - Sub-directory 생성 불가능
    - File naming issue
  - 사용자간 파일 공유 불가
***
### Hierarchical Directory Structure
- Tree 형태의 계층적 directory 사용 가능
- 사용자가 하부 directory 생성/관리 가능
  - System call이 제공되어야 함
  - Terminologies
    - Home directory, Current directory
    - Absolute pathname, Relative pathname
- **대부분의 OS가 사용**
***
### Acyclic Graph Directory Structure
- Hierarchical directory structure 확장
- Directory안에 shared directory, shared file을 담을 수 있음
- Link의 개념 사용
  - ex) Unix system의 symbolic link