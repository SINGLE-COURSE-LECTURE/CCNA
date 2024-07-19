# NAT

|참고|
|-|
|[바로가기](https://velog.io/@wlsdnjs156/CCNA-NAT)|

```
1. NAT (Network Address Translation)

  ㅇ IP 주소를 변환하는 기술
     - 1개의 실제 공인 IP 주소에, 다량의 가상 사설 IP 주소를 할당 및 매핑하는,
     - 1:1 또는 1:多 주소 변환(Address Translation) 방식

  ※ NAT 및 DHCP 비교
     - NAT  : 사설 IP 주소공간을 별도로 정하고는, IP 주소변환에 중점을 둠
        . 기본적으로, 인터넷 상에 동일 IP 주소를 재사용할 수 있게 하는 것
     - DHCP : 기확보된 공인 IP 주소들을 Pool화하여, 집단 공유하며 사용후 반납
        . 기본적으로, 제한된 IP 주소를 여럿이 공유케 하여 절약하려는 것


2. NAT의 사용 이유 및 유의점

  ㅇ IPv4의 `IPv4 주소 고갈` 및 `라우팅 테이블 대형화 (Routing Scalability)`에 대한 해소책

  ㅇ 공인 IP 주소의 효율적 공유 및 절약
     - 공인 IP 주소 사용시 ISP社를 바꿀 때 마다, 모든 컴퓨터 주소를 바꿔야하는 등의 단점 해소

  ㅇ 또한, 주소변환을 통해 내부 사설망 보안 및 Load Balancing 등도 가능
     - 통상, 방화벽 등과 결합되어 함께 기능 수행
        . 내/외 주소변환 규칙을 외부에서 알 수 없으므로, 
        . 내부 망에 대한 정보(내부망 주소,숫자 설정 등)가 외부에 노출 안됨 (프라이버시 보호,은닉)
     - 주로, 외부망과 사설망 간에 연결점에 있는 라우터에서 수행됨

  ㅇ 그러나, NAT은, IP 주소 변환 뿐만 아니라,
     - IP 주소, TCP Checksum, UDP Checksum 등 서로 관련된 많은 부분도 함께 변경해야 하므로,
        . 매 연결 단위로 연결 상태 정보를 필요로 하고, 
        . 다수의 프로토콜 계층에 걸쳐 동작해야 할 필요가 있는 등
     - FTP, SIP 등에서 문제 발생

  ※ [참고] 
     - NAT 문제 발생 유형들  ☞ RFC 3027 (Protocol Complications with the IP Network Address Translator)
     - IP 주소의 동적 설정 방식  ☞ 유동IP
     - 사적인 용도로 임의 사용하는 IP 주소 범위 ☞ 사설IP
     - NAT 사용 장비 例)  ☞ 인터넷공유기(IP 공유기) 등
 

3. NAT의 주요 기능

  ㅇ IP Masquerading (IP 위장, IP 마스커레이드)
     - 외부망에서 볼 때 내부망 여러 호스트들이 단일 NAT 서버 하나로 만 보임
        . (외부) ↔ (NAT 1대) ↔ (多 호스트들)
     - 단 하나의 공인 IP로 내부 모든 PC가 대표 IP 주소를 공유 사용
     - IP 계층,전송 계층 모두에서 변환(맵핑)으로, 마치 가면(위장,Masquerading) 효과 있음

  ㅇ NAT Traversal (NAT 투과/초월)
     - (내외부 간 호스트끼리 직접 통신 연결이 가능토록 하는 기능)
        . 인터넷을 통한 양자간 직접 음성 및 영상 통화(VoIP 등) 등에 필요
     - Port Forwarding (포트 포워딩)
        . 트랜스포트계층에서, 내부망 여러 호스트들이 외부망과 각각 별도로 연결짓게 할 수 있음
           .. 서비스 포트 번호 별로 내부의 사설 IP로 지정된 호스트로 전달
           .. 주로, 내부 서버를 외부 인터넷에 공개할 때 등에 사용 

  ㅇ Load Balancing
     - 각 포트별로 트래픽을 균형있게 함

  ㅇ 패킷 필터링
     - IP주소,포트번호를 기반으로 접근제어


4. NAT의 방식 구분

  ㅇ Basic NAT 방식 : (IP 주소의 변환 만 수행)
     - 정적 NAT 방식 (static NAT 방식, 정적인 1:1 NAT 방식)
        . 수동으로, 외부 공인 IP와 내부 사설 IP를 매핑 (1:1)
     - 동적 NAT 방식 (dynamic NAT 방식)
        . 사설 IP 주소를 풀(Pool)화하여, 소수의 공인 주소들에 동적으로 자동 매핑 (多:1)

  ㅇ NAPT 또는 PAT 방식
     - IP 주소 뿐만 아니라 포트 번호 등까지도 포함시켜 내부 호스트를 구분
        . TCP,UDP에서는, 포트번호
        . ICMP에서는, 질의식별자(에코 요청시 ID 필드, Query Identifier)

  ※ [참고] ☞ RFC 3022 : Traditional IP Network Address Translator (Traditional NAT)

  ㅇ Twice NAT 방식
     - 출발지,목적지 모두 주소변환이 동시에 필요한 방식
        . 例) 동일 회사 내 멀리 떨어진 두 지사 간에 인터넷 경유 연결 등

  ㅇ CGNAT (Carrier Grade NAT), LSNAT (Large Scale NAT)
     - 기존 NAPT를 통신사업자 또는 ISP에서 대규모로 사용 가능토록 확장한 방식


5. NAPT (Network Address Port Translation) 또는 PAT (Port Address Translation), 그리고 문제점

  ㅇ 원래는, (Basic NAT)
     - IP 주소 변환(공인 IP 주소 → 사설 IP 주소) 만을 이용하는 basic NAT 방식 이었음
        . 즉, IP 헤더의 목적지와 출발지 주소 만 변경하고, IP 패킷 내부에 다른 어떤 처리도 안함

  ㅇ 그러다가, (NAPT 또는 PAT)
     - TCP,UDP 헤더 내 포트 번호 변환을 통해 (IP 주소 및 포트 번호를 모두 함께 결합시킴),
     - 하나의 IP 주소 및 여러 포트 번호에 의해, 
     - 여러 내부 호스트 주소 및 포트를 연결이 가능해짐 (IP Masquerading)

  ㅇ 이에따라, 구현이 복잡해지는 등 문제점 발생
     - 프로토콜,응용 마다 NAT 구현이 달라짐
     - NAT 장비 자신이 연결 마다 내부 상태 정보(내부 호스트 IP 주소,포트 번호 등)를 보존 필요 
     - 例) SIP를 이용한 어플리케이션 등에 문제 발생 가능성
```

