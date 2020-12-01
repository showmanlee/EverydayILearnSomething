OSI 참조 모델의 **전송 계층**에서 사용하는 프로토콜들이다.

## TCP(*Transmission Control Protocol*)

**연결형 서비스** 방식의 **가상 회선**을 제공하는 프로토콜이다. TCP에서 송신측과 수신측은 ***3-Way Handshake***라는 논리적인 연결을 성립한 상태에서 통신하고, **흐름제어, 혼잡제어, 신뢰성 높은 전송**을 통해 더 안전한 연결이 가능하다.

일반적인 인터넷 연결에서 기본으로 사용되는 프로토콜이며, 패킷 처리를 담당하는 IP 프로토콜과 묶여 ***TCP/IP***라고 부른다.

### 패킷 구조

 * 0~15비트: *Source Port*
 * 16~31비트: *Destination Port*
 * 32~63비트: *Sequence Number*
 * 64~95비트: *Acknowledgement Number*
 * 96~99비트: *Header Length*
 * 100~103비트: *Reserved*
 * 104~111비트: *Flags*
 * 112~127비트: *Window Size*
 * 128~143비트: *Checksum*
 * 144~159비트: *Urgert Point*
 * 160~비트: *Options & Data*

### *3-Way Handshake & 4-Way Handshake*

TCP에서 클라이언트-서버간 송수신 과정 전 논리적인 접속을 성립하고, 송수신 이후 연결을 해제하는 방법이다.

연결을 성립할 때 사용되는 *3-Way Handshake* 과정은 다음과 같다.

> 1. 클라이언트에서 서버에 `SYN(Synchronize Sequence Numbers)` 패킷을 보낸다. `SYN` 패킷은 접속을 요청하는 패킷으로, 클라이언트는 `SYN` 패킷을 보낸 이후 `SYN_SENT` 상태가 되어 서버의 `SYN+ACK` 패킷을 기다린다.
> 2. 클라이언트의 `SYN` 패킷이 `LISTEN` 상태의 서버에 도달하면 서버는 접속 요청을 수락하는 `ACK(ACKnowledgements)` 패킷과 `SYN` 패킷을 보낸다. 이후 클라이언트는 `SYN_RECEOVED` 상태가 되어 클라이언트의 `ACK` 패킷을 기다린다.
> 3. 서버의 `ACK+SYN` 패킷을 받은 클라이언트는 서버에서 `ACK` 패킷을 전송하고, `ESTABLISHED` 상태가 되어 통신 준비를 마친다. `ACK` 패킷을 받은 서버 또한 `ESTABLISHED` 상태가 되어 통신 상태에 돌입한다.

이 과정에서 동기화를 위해 사용된 `ACK` 패킷들은 첫 번째 `SYN` 패킷의 `SEQ Number` 값에 1을 더한 값을 `ACK Number` 값으로 사용한다. 클라이언트와 서버는 주고 받는 `ACK Number` 값이 동일한지 확인한 후, `ESTABLISHED` 상태가 되어 통신을 시작하는 것.

서버가 `SYN_RECEOVED` 상태가 되면 클라이언트의 `ACK` 신호를 받기 위해 해당 연결 정보를 백로그 큐(*Backlog Queue*)에 저장을 하고 일정 시간 동안 대기하는 상태가 되는데, 이 상태를 악용해 서버에 유효하지 않은 `SYN` 패킷을 많은 양 보내서 백로그 큐를 터뜨리는 **`SYN Flooding`** 공격법이 있다.

연결을 해제할 때 사용되는 *4-Way Handshake* 과정은 다음과 같다.

> 1. 클라이언트에서 `FIN` 패킷을 보낸다. `FIN` 패킷은 연결 종료를 요청하는 패킷으로, 클라이언트는 `FIN_WAIT_1` 상태가 되어 서버의 `ACK` 패킷을 기다린다.
> 2. `FIN` 패킷을 받은 서버는 `CLOSE_WAIT` 상태가 되고, 종료 요청을 수락하는 `ACK` 패킷을 서버로 보낸다. 서버의 `ACK` 패킷을 받은 클라이언트는 `FIN_WAIT_2` 상태가 되어 서버의 `FIN` 패킷을 기다린다.
> 3. 서버 애플리케이션의 연결 종료 과정이 끝나면 서버는 클라이언트에게 연결을 해제해도 좋다는 `FIN` 패킷을 보내고, 클라이언트의 `ACK` 패킷을 기다리는 `LAST_ACK` 상태가 된다.
> 4. `FIN` 패킷을 받은 클라이언트는 서버에게 `ACK` 패킷을 보내고 `TIME_WAIT` 상태가 된다. `ACK` 패킷을 받는 서버는 `CLOSED` 상태가 되어 대기 상태에 돌입한다.
> 5. 특정 시간(*Maximum Segment Life;**MSL***)이 지난 후, 클라이언트 역시 `CLOSED` 상태가 되어 다음 연결 요청 전까지 대기 상태에 돌입한다.

## UDP(*User Datagram Protocol*)

**비연결형 서비스** 방식의 **데이터그램**(독립적으로 관리되는 패킷) 서비스를 제공하는 프로토콜이다. TCP와는 달리 Checksum을 제외한 연결 확인 과정을 거치지 않고(애플리케이션에서 별도 처리 필요) 일방적으로 전송하기 때문에 **신뢰성이 떨어진다.** 다만 TCP보다는 네트워크 부하가 적고 연결 속도가 빠르다.

### 패킷 구조

 * 0~15비트: *Source Port*
 * 16~31비트: *Destination Port*
 * 32~47비트: *Length*
 * 48~63비트: *Checksum*
 * 64~비트: *Data*

### TCP vs UDP

| 요소 | TCP | UDP |
|-|-|-|
| 프로토콜 지향성 | 연결지향 | 비연결지향 |
| 연결 방식 | Byte Stream | Message Stream |
| 혼잡제어 및 흐름제어 지원 | O | X |
| 순서 보장 여부 | O | X |
| 전송 속도 | 느림 | 빠름 |
| 데이터 안정성 및 신뢰도 | 높음 | 낮음 |
| 패킷 타입 | 세그먼트 | 데이터그램 |
| 사용처 | HTTP, SMTP, FTP | DNS, 스트리밍, 온라인 게임 |
