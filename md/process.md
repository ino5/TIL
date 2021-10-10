[메인으로 이동](../README.md)

<br>

# 프로세스 개념

<br>

# 📒 목차 <a id="index"></a>

- [📖 프로세스(Process)는 무엇인가?](#1)

- [📖 쓰레드(Thread)는 무엇인가?](#2)

- [📖 메모리에서 프로세스의 레이아웃](#4)

- [📖 프로그램 vs 프로세스](#5)

- [📖 프로세스 상태](#6)

- [📖 PCB란?](#7)

- [📖 참고 자료](#8)

<br>


# 📖 프로세스(Process)는 무엇인가? <a id="1"></a> 

[목차로 이동](#index)<br>

- 실행 중인 하나의 프로그램
- 대부분 시스템에서 작업 단위
- 자원 단위

<br>

# 📖 쓰레드(Thread)는 무엇인가? <a id="2"></a> 

[목차로 이동](#index)<br>

- 스케줄링 단위 또는 실행 단위

<br>

# 📖 메모리에서 프로세스의 레이아웃 <a id="4"></a>

[목차로 이동](#index)<br>

- Text, data, heap, stack section으로 나누어진다.
    - Text section: 실행가능한 코드

    - Data section: 전역변수
    - Heap section: 프로그램 실행시간동안 동적으로 할당된 메모리
    - Stack section: 함수를 호출 할 때의 임시 데이터 저장 공간 (함수 변수, 반환 주소, 지역 변수)


- text와 data section의 크기는 프로그램 실행동안 고정되어 있다.

<br>

# 📖 프로그램 vs 프로세스 <a id="5"></a>

[목차로 이동](#index)<br>

- 프로그램은 수동적(passive)이고 프로세스는 능동적(active)
- 실행가능한 파일이 메모리에 올라갈 때 프로그램에서 프로세스가 된다.

<br>

# 📖 프로세스 상태 <a id="6"></a>

[목차로 이동](#index)<br>

- 프로세스가 현재 어떤 활동을 하고 있는지 나타낸다.
- New: 프로세스가 생성되는 중이다.
- Running: 명령(Instructions)이 실행되는 중이다.
- Waiting: 어떤 이벤트가 발생하길 기다리는 중이다. (I/O 완료 또는 신호 수신)
- Ready: 프로세서에 할당되길 기다리는 중이다.

<br>

- admitted
    - new → ready

- Interrupt
    - running → ready

- scheduler dispatch
    - ready → running

- I/O or event wait
    - running → waiting

- I/O or event completion
    - waiting → ready

<br>

# 📖 PCB란? <a id="7"></a>

[목차로 이동](#index)<br>

## PCB

- Process Control Block, task control block
- 프로세스에 대한 많은 정보를 포함한다.

<br>

## PCB가 포함하는 정보 <a id="8"></a>

- Process state
    - 프로세스 상태

- Program counter
    - 카운터는 이 프로세스에 대해 실행할 다음 명령 주소를 나타낸다.

- CPU registers
    - 인터럽트 이후 프로세스가 올바르게 실행을 계속 하기 위해 필요하다. (프로그램 카운터도 마찬가지다.)

- CPU-scheduling information

- Memory-management information

- Accounting information

- I/O status information
    - 프로세스에 할당된 I/O 장치 리스트 등이 있다.



<br>

# 📖 참고 자료 <a id="9"></a>

[목차로 이동](#index)<br>

Operating System Concepts 10th edition - Wiley (2018)

<br><br><br>

[목차로 이동](#index)

[메인으로 이동](../README.md)