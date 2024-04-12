---
layout: single
title: "Network Communication"
category: Server
tags:
    - network
    - socket
    - multi process
toc: true
toc_sticky: true
---
# Process & Thread
### `process`
> an executing program

- 운영체제로부터 자원을 할당받은 작업의 단위
- 프로세스별로 독립된 메모리 영역을 Code/Data/Stack/Heap의 형식으로 할당받으므로 다른 프로세스의 변수나 자료에 접근 불가능
- 프로세스의 상태(Process State)
    - 프로세스 생성(New)
    - 실행 가능(Ready)
    - 실행 상태(Running)
    - 대기(Blocked)
    - 종료(Terminated, exit)
- PC(Program Counter): 실행할 명령어의 주소를 가리키는 레지스터, 프로그램의 흐름 제어
- SP(Stack Pointer): 현재 실행중인 프로세스의 스택의 최상단을 가리키는 레지스터


### `thread`
> the basic unit to which the operating system allocates processor time

- 프로세스가 할당받은 자원을 이용하는 실행 흐름의 단위
- Code/Data/Heap 형식으로 할당된 메모리를 스레드 간에 공유하며 작동

### `PCB`(Process Control Block)
> 운영체제가 프로세스를 관리하기 위해 사용하는 데이터 구조

PCB는 다음과 같은 정보들을 가지고 있다.
- 프로세스 상태(Process State)
- 프로그램 카운터(Program Counter, PC)
- 레지스터(Registers)
- 스케줄링 정보(Scheduling Information)
- 메모리 관리 정보(Memory Management Information)
- 입출력 상태(I/O State)

### `Context Switch`
> CPU가 현재 작업 중인 프로세스에서 다른 프로세스로 넘어갈 때 지금까지의 프로세스 상태를 저장하고, 새 프로세스의 저장된 상태를 다시 적재하는 작업

context switch가 발생하는 경우는 다음과 같다.
- Multitasking
- Interrupt Handling
- User and Kernel Mode Switching

### `Multi Thread`
> 하나의 프로세스가 여러 작업을 여러 스레드를 사용해 동시에 처리하는 것

장점
- context switch를 할 때 공유하고 있는 메모리만큼 메모리 자원을 아낄 수 있다
- 스레드는 프로세스 내의 스택 영역을 제외한 모든 메모리를 공유하기 때문에 통신 부담이 적어서 응답 시간이 빠르다

단점
- 스레드 하나가 프로세스 내 자원을 망쳐버린다면 모든 프로세스가 종료될 수 있다
- 자원을 공유하기 때문에 필연적으로 여러 스레드가 함께 전역 변수를 사용할 경우 발생할 수 있는 충돌 즉, 동기화 문제가 발생하므로 교착상태가 발생하지 않도록 주의해야 한다

# IPC(Inter Process Communication)
### IPC가 필요한 이유
- 정보 공유
- 계산 속도 증가
- 모듈성
- 편리성

![IPC Models](https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/8fce5a56-d812-44cd-ad3f-071876f2a249)

### `Shared Memory` 모델
> 협력 프로세스 간 공유되는 메모리(버퍼)를 통해 동시에 통신
- 전역 변수, 공유 변수, 공유 파일을 통해 통신
- producer 프로세스가 item을 생성하고 buffer에 저장하면 consumer 프로세스가 buffer의 item을 소비하는 형식으로 두 프로세스는 동기화되어야 함

### `Massage Passing` 모델
> OS가 프로세스 간 통신 방법을 제공하고 메시지를 대리 전달
- OS가 동기화를 해주기 때문에 안전하고 충돌 가능성이 없으나 시스템 콜을 사용하기 때문에 성능이 떨어짐
- Blocking을 이용한 Synchronization

### IPC의 종류
- Anonymous Pipe: 단방향 통신 체널
- Named Pipe: 양방향 통신 채널, FIFO rnwh
- Message Queues: 메모리를 사용한 pipe으로 구조체를 기반으로 통신
- Shared Memory: 공유되는 메모리를 통해 통신
- Memory Map: 열린 파일을 메모리에 매핑시켜 공유, 대용량 데이터 공유 시 사용
- Semaphore: 프로세스 간 데이터를 동기화하고 보호하는 것이 목적으로 공유된 자원에 한번에 하나의 프로세스만 접근 가능
- socket: 네트워크 소켓을 이용하여 Client-Server 구조로 데이터 통신

# 네트워크 통신

> What happens when you type a URL into your browser? [[출처](https://aws.amazon.com/blogs/mobile/what-happens-when-you-type-a-url-into-your-browser/)]

1. You type https://url-of-the-site in your browser and press Enter
2. Browser looks up IP address for the domain
3. Browser initiates TCP connection with the server
4. Browser sends the HTTP request to the server
5. Server processes request and sends back a response
6. Browser renders the content


브라우저 주소창에 웹사이트의 url을 입력했을 때 일어나는 자세한 과정은 다음과 같다.

사용자가 브라우저 주소창에 url 입력한다. 브라우저가 DNS(Domain Name System)을 사용하여 사이트 서버의 IP 주소를 찾는다. 크롬 브라우저는 네트워크에서 통신 가능한 형태인 패킷으로 만든다. 패킷을 네트워크에 흘려 보낸다. 네트워크 중간에 있는 라우터들이 패킷을 읽고 사이트 서버로 전달한다. 사이트 서버는 이 패킷을 다시 풀어, 브라우저 서버가 읽을 수 있는 형태로 만들고 브라우저 서버에 전달한다.

### OSI(Open Systems Interconnection) 7계층
> 통신 시스템의 개념적인 모델로 서로 다른 컴퓨터 시스템들이 서로 통신할 수 있는 표준을 제공

![OSI Model](https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/eeb399dc-d715-40a4-ab45-e4348503091e)

1. 물리 계층(Physical Layer): 디지털 데이터를 아날로그적인 전기적 신호로 변환
2. 데이터 링크 계층(Data Link Layer): MAC 주소를 사용해 네트워크 데이터를 어느 컴퓨터로 보낼지 담당
3. 네트워크 계층(Network Layer): IP주소를 사용하여 네트워크 데이터를 어느 컴퓨터로 보낼지 담당
4. 전송 계층(Transport Layer): 컴퓨터로 들어온 네트워크 데이터를 어느 포트로 보낼지 담당
5. 세션 계층(Session Layer): 통신이 실제로 이루어지는 통신 세션을 구성
6. 표현 계층(Presentation Layer): 계층 간 데이터를 적절히 표현하는 부분 담당
7. 응용 계층(Application Layer): 웹 서비스의 UI 부분, 사용자의 입출력(I/O) 담당


### TCP/IP 4계층
- IP(Internet Protocol): 인터넷 주소 체계, 소스 장치에서 대상 장치로 정보 패킷을 전달
- TCP(Transmission Control Protocol): 전송 제어 프로토콜, 패킷 순서 지정 또는 오류 검사

![OSI & TCP/IP](https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/bf6ffba2-2af6-4932-927e-07c875353a7f)

1. 네트워크 인터페이스 계층(Network Interface Layer)
2. 인터넷 계층(Internet Layer): 컴퓨터 간 라우팅 담당
3. 전송 계층(Transport Layer): 프로세스 간의 시뢰성 있는 데이터 전송 담당
4. 응용 계층(Application Layer): HTTP, Telnet, SSH, FTP와 같은 프로토콜 사용

### HTTP & HTTPS
HTTP(Hypertext Transfer Protocol)는 클라이언트와 서버 간 통신을 위한 프로토콜로 OSI 네트워크 통신 모델의 응용 계층 프로토콜에 해당한다.

HTTPS(Hypertext Transfer Protocol Secure)는 HTTP 요청 및 응답에 SSL(Secure Sockets Layer)/TLS(Transport Layer Security) 기술을 결합하여 보안을 강화한 프로토콜이다. HTTPS 웹 사이트는 데이터를 교환하기 전에 브라우저와 CA(Certificate Authority)로부터 발급받은 SSL/TLS 인증서를 교환하며 암호화된 형태의 데이터를 주고받을 수 있다.

# Socket
> 네트워크에로 연결되어 있는 컴퓨터의 통신의 접점에 위치한 통신 객체

- 소켓 구성 요소: 인터넷 프로토콜, 로컬 IP 주소, 로컬 포트, 원격 IP 주소, 원격 포트
- IPC(Inter-Process Communication)의 수단을 제공
- datagram socket: UDP 소켓, 비 연결지향
- stream socket: TCP 전용 소켓
![TCP socket system call](https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/1510b716-0166-484a-a8df-9ab94a00fc4b)
    - 시스템 콜
        1. socket(): 소켓 생성
        2. bind(): 생성한 소켓에 실제 아이피 주소와 포트번호를 부여(서버에서만 사용)
        3. listen(): 연결 요청 대기, 소켓 활성화 - 파라미터로 받은 backlog 크기만큼 backlog queue 생성(연결지향인 TCP에서만 사용)
        4. accept(): 연결 요청을 수락, 새로운 소켓 할당 후 데이터 송수신

- UDP(User Datagram Protocol): 사용자 데이터그램 프로토콜, 비디오 재생 또는 DNS 조회와 같이 시간에 민감한 전송에 사용

# System Call
> 운영 체제의 커널이 제공하는 서비스에 대해 응용 프로그램의 요청에 따라 커널에 접근하기 위한 인터페이스

### System Call이 필요한 이유
유저레벨의 프로그램은 유저레벨의 함수들만으로 많은 기능을 구현하기 힘들기 때문에 커널의 도움을 반드시 받아야 하며 이러한 작업은 유저모드에서 수행할 수 없어 커널모드로 전환한 후에 권한을 얻을 수 있다.

- `user mode`: PC register가 사용자 프로그램이 올라가 있는 메모리 위치를 가리키고 있는 상태로 메모리에서 자료를 읽어와 CPU에서 계산하고 결과를 메로리에 쓰는 명령을 수행
- `kernel mode`: PC register가 운영체제가 존재하는 부분을 가리키고 있는 상태로 보안이 필요한 명령, 입출력 장치, 타이머 등 각종 장치에 접근하는 명령을 수행
유저레벨의 프로그램은 유저레벨의 함수들만으로 많은 기능을 구현하기 힘들기 때문에 커널에 관련된 것은 커널보드로 전환해야 작업을 수행할 권한이 생긴다. 

