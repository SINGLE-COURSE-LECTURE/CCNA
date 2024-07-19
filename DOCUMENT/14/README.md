#ACL

|참고|
|-|
|[바로가기](https://velog.io/@wlsdnjs156/CCNA-ACL1)|

```
1. Access Control List

  ㅇ [일반]
     - 각각의 엔트리(개별항목)에 대한 접근 권한(누구에게 어떤 권한을 주는 등)을 설정하는 것

  ㅇ [네트워크]
     - 라우터 등의 장비에서 `패킷 필터링`,`패킷 분류`를 결정짓는 일련의 규칙(Rules) 목록들
        . 주로, 4 계층까지 제어 가능


2. [네트워크]  접근제어의 대상이 되는 주요 파라미터들

  ㅇ 송신지 주소, 목적지 주소 (IP 주소)
  ㅇ 프로토콜의 종류
  ㅇ 포트 번호
  ㅇ 기타 파라미터 등


3. [네트워크]  Cisco社의 경우
 
  ㅇ Cisco社는, Access List를 매우 일반화시켜 다양한 경우에 사용하고 있음
   
  ㅇ Access List 사용 목적
     - 트래픽의 제어용
       . 가장 일반적인 용도로써, 어떤 인터페이스를 통과(in,out)하는, 패킷 Traffic을 제어
     - 보안용
     - 패킷의 식별용 등

  ㅇ Access List 적용 대상
     - Interface 에 적용  -> Access Group
        . 이 경우의 Access List는 입력 또는 출력 인터페이스에 적용
     - Protocol 에 적용   -> Distribute List

     * 입력/출력(inbound/outbound :적용방향)에 따라,
        . 인터페이스 및 프로토콜 각각에, 하나의 ACL 만 적용 가능

  ㅇ 특징
     - 규칙 집합(차단,허용 정책)에 의해 접근제어
        . 허용 : 차단,허용 동시에 있을 때, 우선순위가 차단 보다 높음
        . 차단 : 따라서, 필요시 우선적으로 차단 적용이 필요함
     - 통상, 허용(permit) 정책을 적용하면, 그외 미 언급된 모든 패킷이 거부됨이 원칙
        . 이때, 필요 부분은 반드시 허용 처리해야 함

  ㅇ Basic Access List

     - stanadrd ACL (표준 엑세스 리스트)
        . 출발지 IP 주소 만 기반으로 검사
           .. IP 주소 및 네트워크 주소로  필터링 수행
        . 표준 ACL 번호
           .. 1~99 또는 1300~1999 (IP),  300~399 (DECnet), 800~899 (IPX),
              1000~1099 (IPX SAP) 등
        . 사용 문법
           .. access-list access-list-number [permit|deny] source-address [widcard-mask]

     - extended ACL (확장 엑세스 리스트)
        . 출발지 및 목적지의 IP주소,프로토콜,포트번호 등 모두를 검사 (폭넓은 확장 가능)
           .. IP 주소 뿐만아니라 TCP 및 UDP의 포트 번호도 검사하여 제어
           .. ICMP code 및 type 등
        . 확장 ACL 번호 
           .. 100~199 또는 2000~2699 (IP), 900~999 (IPX) 등

     * 사용 例) ☞ 아래 5.항 참조

  ㅇ 기타 유용한 Access List들
     - Prefix List
     - AS-Path List
     - Community List


4. [네트워크]  시스코社 ACL 명령어 

  ㅇ 주요 명령어
     - ACL 설정 명령어  :  access-list
     - ACL 활성화 명령  :  access-group, access-class 
     - ACL를 설정한 내용을 보고 싶을 때의 명령어  :  show ip access-list

  ㅇ 명령어 적용 순서
     - ① ACL 설정
        . access-list 번호를 정하고, 프로토콜별,IP주소별로,포트번호별로, 세부내역 정의 생성
     - ② 정의된 ACL를 적용할 인터페이스를 선택
     - ③ 선택한 해당 인터페이스에서, 정의된 ACL 번호에 대해, 적용방향을 정하고 활성화

  ㅇ 와일드카드 마스크
     - `0` : 대응 비트 값을 반드시 검사 (반드시 일치)
     - `1` : 대응 비트 값을 무시 (상관없음, don't care)
     * 例) 
        . host 1.1.1.1 은, 1.1.1.1 0.0.0.0 와 같은 표현
        . 0.0.0.0 255.255.255.255 은, any 와 같은 표현


5. [네트워크]  시스코社 ACL 명령어 사용 例)

  ㅇ stanadrd ACL 사용 순서 例)
     - ① (ACL 설정 정의 명령어) access-list 1 permit 192.168.1.0 0.0.0.255
        . 매칭 기준 : 출발지 IP주소(192.168.1.0), 와일드카드마스크(0.0.0.255)
           .. 와일드카드마스크에서, `0` : 정확히 일치, `1` : 상관 없음
        . 오로지, 192.168.1.0 /24에서 출발한 패킷 만 허용되고, 그외 모든 패킷이 거부됨
     - ② (특정 인터페이스 설정 모드로 진입) interface fastethernet 0/0
     - ③ (특정 인터페이스 적용) ip access-group 1 out
        . fastethernet 인터페이스 0/0 으로부터 나갈 수 있는 패킷은, 192.168.1.0 /24 뿐 임

  ㅇ extended ACL 사용 순서 例)
     - ① (ACL 설정 정의 명령어) access-list 101 permit icmp any host 192.168.153.102
     - ② (특정 인터페이스 설정 모드로 진입) interface fastethernet 0/0
     - ③ (특정 인터페이스 적용) ip access-group 101 in
```
