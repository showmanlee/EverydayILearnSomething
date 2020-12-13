## IP(*Internet Protocol*)

OSI 참조 모델의 **네트워크 계층(*Network Layer*)** 에 위치한 프로토콜로, 다른 장치로 데이터를 직접 전송할 때 사용된다.

### IPv4

#### 헤더 구조

IP 헤더의 길이는 기본 20바이트로, 옵션에 따라 60바이트까지 늘어날 수 있다.

 * 0~3비트: *Version*
   * IP 헤더의 버전(4)
 * 4~7비트: *Header Length*
   * 헤더의 길이, 32비트 단위로 표기. 20~60바이트 범위
 * 8~15비트: *Type of Service*
   * 서비스의 우선 순위 및 품질 요구 사항
 * 16~31비트: *Total Packet Length*
   * 헤더와 데이터를 포함한 패킷 전체 길이, `2^16-1`바이트까지 가능
 * 32~47비트: *Fragment Identifier*
   * 단편화된 패킷인 경우 결합 과정에서 원래 데이터를 식별하는 코드
 * 48~50비트: *Fragmentation Flags*
   * 단편화 관련 플래그(IP 라우터에 의해 분열되는가, 마지막 조각인가)
 * 51~63비트: *Fragmentation Offset*
   * 원래 데이터의 어떤 범위를 저장한 조각인지 표시
 * 64~71비트: *Time to Live*
   * 패킷의 데이터가 유효한 기간
 * 72~79비트: *Protocol Identifier*
   * 상위 프로토콜이 무엇인지 표시
 * 80~95비트: *Header Checksum*
   * 헤더의 유효성 검증
 * 96~127비트: *Source IP Address*
   * 송신지의 IP 주소
 * 128~159비트: *Destination IP Address*
   * 수신지의 IP 주소
 * 160~비트: *Option & Data*

### IPv6

#### 헤더 구조

### 전송 방식

