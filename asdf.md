### Internet Architecture

Protocol : 규칙, 메시지의 형식과 순서, 송*수신시 동작을 정의한다

Protocol Layering : 네트워킹을 부분 부분 나누어서 작은 일들을 처리하도록 나눔

- 프로토콜간 독립적으로 동작하도록 해서 유지 보수를 쉽도록함
- 프로토콜은 자기가 담당한 부분만하고 디테일한 부분은 하위 프로토콜에 맡김

Internet Architecture의 목표

1. 게이트웨이나 네트워크의 손실이 있어도 통신이 유지된다.
   - 패킷에 목적지 주소가 포함됨
   - 네트워크 장비에서는 스테이트 없이 포워딩만 해줌
   - Altanative : Circuit Switching (얘는 위의 특징을 못가짐. 그래서 인터넷은 패킷을 사용함)
     - Circuit : 통신 전에 연결을 세팅한 후에 그걸로만 통신함. 방해 X. 신뢰 ^, 빠름
     - Packet : 라우터에서 패킷을 큐에 갖고잇다가 포워딩해줌. 링크를 패킷들이 공유할 수 잇음. 느림. 신뢰 v
2. 다양한 형식의 통신 방식을 지원한다. (EX. TCP, UDP)
3. 여러 종류의 네트워크를 수용한다. (LTE, 3G, 유선 등 서로 연결될 수 있으며, 어플단에서 신경쓸 필요없도록 함)
4. 분산된 관리가 가능하다.
   - 네트워크 라우터, GW 들을 AS (Autonomous System)라는 독립적인 그룹으로 관리한다
   - BGP (Border Gateway Protocol)를 통해 AS들을 연결한다.
5. Cost-effective. but 프로토콜마다 잇는 패킷 헤더들이 비용(overhead)을 증가시킴. 애매함
6. 새로운 Host가 쉽게 추가될 수 있다. (IP는 꽂으면 연결되는 방식이라 간단함. 근데 Endpoint가 너무 많아져서 문제)
7. 자원 사용량의 모니터링이 가능하다.

---

### OSI 7 Layer

Protocol Stack : App > Presentation > Session > Transport > Network > Datalink > Physical

- Application Layer (패킷) : 사용자에게 나타나며, 네트워크를 통해 송*수신 할 수 있게 해줌. (EX. 브라우저, 이메일)
- Presentation Layer : 어플과 네트워크 사이의 데이터 형식 변환. (EX. Encoding, 암호화, 압축 등)
- Session Layer : 프로세스간 통신 세션을 관리함. (EX. SIP = Session Infliate Protocol (보이스톡 가튼거 할때))
  - 대화 (Dialog) 관리, 동기화, 오류 처리 등의 많은 기능이 있다.
- Transport Layer (세그먼트) : 네트워크 종류에 관계없이, 신뢰 가능한 통신하도록 함. 프로세스마다 포트를 부여, 포트를 통해 연결
  - TCP : Handshake 가튼거 하고, 신뢰 가능한 연결을 수립함. 받는 쪽에서 데이터를 다 받으면 재조립해서 상위 프로토콜로 올림. 문제가 잇으면 다시 보내라고 요청함. 느림
  - UDP : 연결 같은거 없고, 잘 받겟지 하고 그냥 보냄. 빠르지만 신뢰가 안됨
  - 주요 기능
    - 연결 관리 : 두 기기간 연결을 수립함 (TCP: 3-way Handshake를 통해 두 기기간 가상 회선을 통해 연결함)
    - Multiplexing : 포트를 통해 프로세스 별 연결을 관리한다. 소켓을 통해 App과 통신
    - Segmentation (쪼갬), Reliable/Unreliable 통신
    - Flow control : 송*수신자 사이의 처리 속도 차이 해결. 수신자의 처리 속도에 맞게 보내줌
    - Congestion Control : 네트워크 장비들의 처리 속도 차이, 트래픽 몰림 해결. 네트워크 상황에 맞게 보내줌
- Network Layer - IP (데이터그램) : Best effort delivery. 보내려고는 하겠는데 안되도 모름. 신뢰성 X
  - Internetworking : 다른 네트워크와 이어줌. (다른 프로토콜과 이어주진 않음 ex. Bluetooth <--x--> Wi-Fi)
  - Logical Addressing : 글로벌리 유니크한 논리적 주소를 통해 각 기기를 구별한다.
  - Routing : 각 라우터에서 그때그때 어떤 라우터로 보내는게 좋은지 찾아서 보냄
- Datalink Layer (프레임)  : 노드 간 신뢰 가능한 전달을 맡음. 패킷을 프레임으로 바꾸며 물리 주소와 쳌섬 등을 포함하여 전달함
  - Flow Control, Error Control 함
- Physical Layer : 전기 신호를 통해 실제로 데이터를 전송

OSI 7 Layer의 장점 : 각 프로토콜이 독립적으로 발전할 수 있고, 표준화가 가능하여 호환, 개발, 운영에 유리하다.

---

### IPv4

Class : 5개의 클래스로 나뉜다.  IP 할당이 너무 비효율적임, 라우팅도 너무 복잡함 -> CIDR 등장

|      A       |        B         |        C         |         D         |        E        |
| :----------: | :--------------: | :--------------: | :---------------: | :-------------: |
| 0~ (0 ~ 127) | 10~  (128 ~ 191) | 110~ (192 ~ 223) | 1110~ (224 ~ 239) | 1111~ (240~255) |
| 8비트 Prefix |  16비트 Prefix   |  24비트 Prefix   |   멀티캐스팅용    | For future use  |

Hierarchy : 주소 관리을 위해 계층을 둠. 라우팅에도 도움.  IANA > 지역 > 국가 (NIR) or 로컬 (LIR) > 기관 or ISP > 개별 장비

Special Address : 예약된 IP 주소

- 0.0.0.0 : IP 주소를 할당 받기 전에 Src 주소로 사용됨 (0.{Host번호} : 해당 네트워크 안에서 같은 목적의 Src로 사용됨)
- 255.255.255.255 : Broadcast. ({Network Prefix}.111... : 해당 네트워크 안에서 Broadcast)
- 127.~ : Loopback 주소
- 10.~ / 172.16.~ / 192.168.~ : Private Address. 소속된 네트워크 안에서만 유효한 주소

Packet forwarding method : Unicast, Broadcast, Multicast

CIDR : Class 안에서 subnet으로 또 쪼갬. IP주소/마스크 비트 수로 표기함.

- Subnet mask를 통해 Subnet으로 쪼개어지며, Network / Subnet / Host로 주소가 분리됨
- 마찬가지로 첫 주소와 마지막 주소(Network.Subnet.0, 11...)는 각각 대표 주소와 Broadcast 주소로 사용됨
- Subnet - 주소 관리가 효율적, 라우팅 테이블 엔트리 수도 줄어듦
- Supernet : C 클래스들을 묶어서 한 네트워크로 보이게 함. 마찬가지로 CIDR 표기로 표시함

---

### IPv6

형식 : 128비트. 16비트씩 끊어서 :로 구분함. ::는 0들을 묶은거

- 네트워크 인터페이스마다 할당될 수 있음. 한 인터페이스가 여러 주소를 가질 수 있음.
- 한 링크에 여러가지 subnet prefix가 할당될 수 있음

Packet forwarding method : Unicast, Anycast, Multicast (IPv4는 애니 대신 브로드)

- **Anycast** : 같은 Anycast 주소를 가진 노드 중 가장 가까운 친구에게 전달되도록 한다. Src로 사용 불가

  - 장점 : 트래픽 로드 밸런싱, 한 친구가 죽어도 다른 친구가 받아서 처리해 줌
  - Subnet-Router Anycast Address : 서브넷 라우터에 애니캐스트 주소를 두어서 가장 가까운 라우터를 통해서 전달되도록 함

- Unicast : 범위에 따라 분류가 나뉨.  Link-local (라우터를 안넘어가는 범위) < Unique-local (특정 지역, 사설망) < Global

  - Local-use Address : Link-local, Site-local
    - Link-local : FE80::{Modified EUI-64 Format} 
    - Site-local : IPv4의 Private 주소랑 같음. 잘안씀. FEC0:{54bit SubnetID}:{Modified EUI-64 Format}

  - Global : Public IPv4 주소처럼 글로벌에서 라우팅 가능한 주소
    - 보통, 앞 64비트 네트워크 (뒷 16 = Subnet)  / 64비트 Host 로 나뉨
    - IP 할당 : Network는 라우터가 보내주는 Network Prefix 사용 / Host는 Modified EUI-64 Format로 구성
  - Special Address
    - 0 : 아직 주소가 없을 때 Src로 사용
    - 0::1 : Loopback

- Multicast : 해당 주소의 모든 인터페이스에게 전달. Src로 사용 불가. **Broadcast는 Bandwidth을 낭비**하지만 Multi는 X

  - 노드들은 여러개의 Multicast 주소를 가질 수 있고, 언제든 그룹에 들어가거나 나올 수 있음
  - 주소 형식 : FF로 시작. 각 4비트의 Flag와 Scope 필드. 이후 112비트의 Multicast Group ID
    - Flag : 0 / Rendezvous Point Address / Prefix / Transient 순서
      - Prefix = 해당 주소가 Unicast 주소 기반 / Transient  = 임시 (IANA가 할당하지 않은) 주소인지
    - Scope : 해당 Mulicast 트래픽이 포함되는 범위 (EX. 1 = Interface-local / 2 = Link-local / 5 = Site-local)
      - Assigned Address : ~~ ::1 = 해당 범위 내의 모든 노드 / ~~::2 = 해당 범위 내의 모든 라우터
      - Interface-local : 루프백 (프로세스간 통신)

**Modified EUI-64 Format** : IP 할당을 위해 Host 부분을 자신의 MAC 주소로 구성함

- 48비트인 MAC 주소의 가운데에 FF FE를 넣어서 64비트로 바꿈
- 일반적으로 (Unique한 경우) 왼쪽에서 7번째 비트를 1로 바꿔줌

IPv4-mapped IPv6 address : IPv4 -> IPv6. 0::FF FF:{IPv4 주소}

**Solicited-Node Multicast Address** : 다음 홉의 IPv6 Unicast 주소를 통해 Layer 2 주소를 생성하는 방식

- IPv4에서는 Broadcast ARP를 통해 다음홉의 MAC 주소를 알아낸다.

- IPv6에서는 NDP (Neighbor Discovery Protocol)를 사용하여 알아낸다.

  - IPv6 Unicast 주소 뒤 24비트를 떼서 Multicast 주소를 생성하고, 이를 통해 다시 Layer 2 주소를 생성한다.

  - |         |    Unicast     |     Multicast     |
    | :-----: | :------------: | :---------------: |
    | Layer 3 | 2001:DB8::AB:2 |  FF02::1:FFAB:2   |
    | Layer 2 |                | 33-33-FF-AB-00-02 |

---

### Transport Layer











---

### IP

Identity : 기기 자체 / Identifier : 기기를 구별할 수 있게 하는 요소들. 기종, 물리주소 등
