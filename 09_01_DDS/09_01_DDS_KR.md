**Volume 09 Robotics Communication**

# Chapter 1. DDS

## 1.1 OMG DDS Standard Overview

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

# 01_01_OMG_DDS_Standard_Overview (OMG DDS 표준 개요)

OMG(Object Management Group)의 DDS(Data Distribution Service) 표준은 현대 분산 실시간 시스템(Distributed Real-Time System)을 위한 가장 중요한 통신 기술 중 하나이다. DDS는 복잡한 사이버-물리 시스템(Cyber-Physical System)에서 확장 가능하고(Scalable), 신뢰성이 높으며(Reliable), 결정론적(Deterministic)이고 데이터 중심적인(Data-Centric) 통신을 제공하기 위해 개발되었다. 전통적인 클라이언트-서버(Client-Server) 통신 모델과 달리 DDS는 중앙 서버나 메시지 브로커(Message Broker)에 의존하지 않고 수많은 분산 컴퓨팅 노드(Distributed Computing Node)가 실시간으로 데이터를 교환할 수 있도록 설계되었다. 이러한 특성 덕분에 DDS는 로봇공학(Robotics), 자율주행 시스템(Autonomous System), 항공우주(Aerospace), 국방 시스템(Defense Platform), 산업 자동화(Industrial Automation), 스마트 제조(Smart Manufacturing), 철도 제어 시스템(Railway Control System), 의료기기(Medical Device), 그리고 미래의 피지컬 AI(Physical AI) 인프라에서 핵심 통신 기술로 자리 잡았다. 특히 DDS는 ROS2(Robot Operating System 2)의 기본 미들웨어(Middleware)로 채택되면서 현대 로봇 시스템에서 가장 널리 사용되는 통신 프레임워크 중 하나가 되었다.

DDS는 기존 통신 아키텍처의 한계를 극복하기 위해 등장하였다. 과거의 통신 시스템은 일반적으로 클라이언트와 서버가 직접 연결되는 구조를 사용하였다. 이러한 방식은 비즈니스 애플리케이션에서는 효과적이었지만, 실시간 분산 시스템에서는 여러 가지 문제를 야기하였다. 대규모 자율 시스템은 수백 또는 수천 개의 노드가 센서 데이터, 제어 명령, 상태 정보, 진단 데이터, 텔레메트리(Telemetry), 인공지능 추론 결과 등을 지속적으로 생성한다. 모든 장치가 서로 직접 연결되어야 한다면 시스템 복잡도는 기하급수적으로 증가하게 된다. DDS는 이러한 문제를 해결하기 위해 데이터 중심의 발행-구독(Publish-Subscribe) 모델을 도입하였다.

DDS의 핵심 철학은 "장치 간 연결(Connection)이 아니라 데이터(Data)가 통신의 중심이 되어야 한다"는 것이다. DDS 환경에서는 애플리케이션이 서로 직접 통신하지 않는다. 대신 글로벌 데이터 공간(Global Data Space)이라는 개념을 통해 정보를 공유한다. 발행자(Publisher)는 데이터를 생성하고, 구독자(Subscriber)는 특정 데이터에 관심을 등록한다. DDS 미들웨어는 자동으로 검색(Discovery), 매칭(Matching), 전송(Transport), 신뢰성(Reliability), 데이터 전달(Delivery)을 수행한다. 이러한 추상화는 시스템 설계를 단순화하면서도 높은 확장성을 제공한다.

DDS의 가장 기본적인 통신 모델은 발행-구독 모델이다. 발행자는 특정 토픽(Topic)에 대한 데이터를 생성한다. 구독자는 해당 토픽에 대한 관심을 등록하고, 데이터가 생성될 때마다 자동으로 수신한다. 발행자와 구독자는 서로의 존재를 알 필요가 없다. 이러한 느슨한 결합(Loose Coupling)은 시스템 유연성을 크게 향상시키며, 새로운 장치가 추가되더라도 기존 시스템을 수정할 필요가 없다.

토픽은 DDS에서 가장 중요한 개념 중 하나이다. 토픽은 시스템 전체에서 공유되는 특정 정보의 범주(Category)를 정의한다. 로봇 시스템에서는 로봇 위치(Robot Position), 배터리 상태(Battery Status), 모터 명령(Motor Command), 카메라 영상(Camera Image), LiDAR 포인트 클라우드(Point Cloud), 내비게이션 목표(Navigation Goal), 장애물 정보(Obstacle Information), 진단 정보(Diagnostic Report), AI 추론 결과(AI Inference Result) 등이 각각 토픽이 될 수 있다. 각 토픽은 데이터 타입(Data Type)을 가지며, DDS는 해당 데이터 구조를 이해하고 관리한다.

DDS는 여러 계층으로 구성된다. 가장 상위에는 사용자 애플리케이션(Application Layer)이 위치한다. 이 계층에는 로봇 제어기, 자율주행 알고리즘, 인지 시스템, 플릿 관리 시스템(Fleet Management System), 모니터링 소프트웨어 등이 포함된다. 그 아래에는 DDS 미들웨어 계층이 위치하며, 실제 데이터 교환을 담당한다. 최하위에는 Ethernet, Wi-Fi, TCP/IP, UDP/IP, 공유 메모리(Shared Memory), 실시간 네트워크(Real-Time Network)와 같은 전송 계층(Transport Layer)이 존재한다. 이러한 계층 구조는 애플리케이션이 네트워크 기술에 종속되지 않도록 해준다.

DDS의 가장 큰 특징 중 하나는 데이터 중심(Data-Centric) 설계이다. 기존 메시지 중심(Message-Centric) 시스템은 메시지 자체의 전달에 집중하지만, DDS는 데이터 객체(Data Object)의 구조, 생명주기(Lifecycle), 소유권(Ownership), 품질(Quality)을 이해한다. 이를 통해 DDS는 데이터 관리와 통신 최적화를 보다 효율적으로 수행할 수 있다.

자동 검색(Automatic Discovery)은 DDS를 강력하게 만드는 또 다른 기능이다. 기존 분산 시스템에서는 네트워크 주소, 포트 번호, 통신 상대를 수동으로 설정해야 했다. DDS는 이러한 과정을 자동화한다. 새로운 노드가 네트워크에 연결되면 자신의 존재를 알리고, 다른 참여자(Participant)를 자동으로 탐색한다. 발행자와 구독자는 자동으로 서로를 발견하고 연결된다. 이 기능은 대규모 시스템 구축 및 유지보수를 크게 단순화한다.

실시간 성능(Real-Time Performance)은 DDS가 로봇 및 자율 시스템에서 널리 사용되는 가장 큰 이유 중 하나이다. 로봇 모션 제어(Motion Control), 장애물 회피(Obstacle Avoidance), 센서 융합(Sensor Fusion), 자율주행(Autonomous Navigation), 안전 제어(Safety Control)는 모두 예측 가능한 지연시간(Predictable Latency)과 결정론적 통신을 필요로 한다. DDS는 이러한 요구를 만족하기 위해 다양한 QoS(Quality of Service) 정책을 제공한다.

QoS는 DDS의 가장 강력한 기능 중 하나이다. QoS를 통해 각 데이터 스트림(Data Stream)에 대해 통신 특성을 개별적으로 정의할 수 있다. 예를 들어 비상 정지(E-Stop) 명령은 매우 낮은 지연시간과 높은 신뢰성을 요구하지만, 진단 데이터는 일부 패킷 손실을 허용할 수 있다. DDS는 이러한 서로 다른 요구사항을 독립적으로 설정할 수 있도록 지원한다.

Reliability QoS는 데이터 전달 보장 여부를 정의한다. Reliable 모드는 메시지가 반드시 전달되도록 재전송을 수행한다. Best Effort 모드는 속도를 우선시하며 일부 패킷 손실을 허용한다. 로봇 시스템은 두 가지 방식을 상황에 따라 적절히 조합하여 사용한다.

Durability QoS는 데이터 지속성을 정의한다. 어떤 데이터는 현재 연결된 구독자에게만 전달되면 되지만, 어떤 데이터는 나중에 접속한 구독자도 받을 수 있어야 한다. DDS는 이러한 요구사항을 설정할 수 있도록 지원한다.

History QoS는 얼마나 많은 과거 데이터를 저장할 것인지 결정한다. 센서 데이터는 최신 정보만 필요할 수 있지만, 진단 및 분석 시스템은 과거 데이터를 저장해야 할 수 있다.

Deadline QoS는 데이터 갱신 주기를 정의한다. 발행자가 일정 시간 내에 데이터를 전송하지 않으면 DDS는 이를 통신 장애로 간주할 수 있다. 이는 고장 감지(Fault Detection) 및 시스템 모니터링에 활용된다.

Latency Budget QoS는 허용 가능한 지연시간을 정의한다. DDS는 이를 기반으로 네트워크 자원을 효율적으로 배분할 수 있다. 이 외에도 대역폭 관리(Bandwidth Management), 메시지 우선순위(Message Priority), 자원 제한(Resource Limit) 등 다양한 QoS 정책이 제공된다.

DDS는 중앙 브로커(Broker)를 사용하지 않는 P2P(Peer-to-Peer) 구조를 채택한다. MQTT와 같은 메시징 시스템은 중앙 브로커를 통해 모든 메시지를 전달한다. 반면 DDS는 가능한 경우 참여자 간 직접 통신을 수행한다. 이를 통해 병목(Bottleneck)을 줄이고 단일 장애점(Single Point of Failure)을 제거할 수 있다.

확장성(Scalability)은 DDS의 또 다른 강점이다. DDS는 소규모 로봇 시스템부터 수천 대의 장치가 연결된 대규모 자율 시스템까지 동일한 아키텍처를 사용할 수 있도록 설계되었다. 자동 검색, 분산 구조, 멀티캐스트(Multicast), QoS 정책은 DDS가 높은 확장성을 유지하도록 돕는다.

보안(Security)은 현대 DDS 환경에서 매우 중요한 요소이다. DDS Security 표준은 인증(Authentication), 권한 관리(Authorization), 암호화(Encryption), 접근 제어(Access Control), 무결성 검증(Integrity Verification), 보안 검색(Secure Discovery), 감사 로그(Audit Log) 기능을 제공한다.

DDS Security는 인증서를 이용하여 참여자의 신원을 확인한다. 접근 제어 정책은 어떤 참여자가 특정 토픽을 발행하거나 구독할 수 있는지 정의한다. 암호화는 데이터 기밀성(Confidentiality)을 보장하며, 무결성 검증은 데이터 변조를 방지한다.

ROS2는 DDS의 가장 대표적인 응용 사례이다. ROS1은 자체 통신 메커니즘을 사용했지만, ROS2는 DDS를 기본 미들웨어로 채택하였다. 이를 통해 자동 검색, QoS, 실시간 성능, 보안, 확장성이라는 장점을 얻었다. 오늘날 ROS2 기반 로봇 시스템은 DDS에 크게 의존하고 있다.

산업계에는 다양한 DDS 구현체가 존재한다. 대표적으로 Fast DDS, Cyclone DDS, RTI Connext DDS, OpenDDS, CoreDX DDS, Twin Oaks DDS 등이 있다. 이들은 모두 OMG DDS 표준을 준수하지만 성능, 기능, 라이선스 정책, 최적화 방식에서 차이를 가진다.

DDS는 자율주행 로봇 시스템에서 핵심적인 역할을 수행한다. 카메라, LiDAR, Radar, IMU, GNSS, 모터 제어기에서 생성되는 데이터를 효율적으로 분배해야 하기 때문이다. 인지 시스템은 객체 인식 결과를 생성하고, 계획 시스템은 경로를 계산하며, 제어 시스템은 모터 명령을 생성한다. DDS는 이러한 모든 기능 모듈을 연결하는 통신 기반을 제공한다.

산업 자동화에서도 DDS는 매우 중요하다. 스마트 팩토리(Smart Factory), 디지털 트윈(Digital Twin), 예지 정비(Predictive Maintenance), 산업용 IoT(Industrial IoT), 협동로봇(Cobot) 환경에서는 실시간 데이터 교환이 필수적이다. DDS는 이러한 요구사항을 충족하는 강력한 통신 프레임워크를 제공한다.

항공우주 및 국방 산업은 DDS의 초기 주요 적용 분야 중 하나였다. 레이더 시스템(Radar System), 무인기(UAV), 지휘통제 시스템(Command and Control System), 위성 운영(Satellite Operation), 함정 전투 시스템(Naval Combat System) 등은 높은 신뢰성과 실시간성을 요구한다. DDS는 이러한 요구사항을 충족하는 표준으로 널리 사용되고 있다.

피지컬 AI의 등장으로 DDS의 중요성은 더욱 커지고 있다. 미래의 지능형 시스템은 로봇, AI, 클라우드, 엣지 컴퓨팅, 자율 의사결정, 분산 센서 네트워크를 결합한 거대한 생태계를 형성할 것이다. DDS는 AI 추론 결과, 월드 모델(World Model), 행동 계획(Action Plan), 멀티모달 센서 데이터(Multimodal Sensor Data)를 효율적으로 교환하는 기반 기술이 될 것이다.

향후 DDS는 TSN(Time Sensitive Networking), 5G, 엣지 컴퓨팅, 클라우드 네이티브(Cloud-Native) 환경, AI 네이티브 미들웨어(AI-Native Middleware)와 더욱 긴밀하게 통합될 것으로 예상된다. 또한 적응형 QoS(Adaptive QoS), AI 기반 통신 최적화(AI-Assisted Communication Optimization), 지능형 라우팅(Intelligent Routing)과 같은 기술도 발전할 것으로 전망된다.

결론적으로 OMG DDS 표준은 단순한 통신 프로토콜이 아니라 분산 실시간 시스템을 위한 종합적인 데이터 분배 프레임워크(Data Distribution Framework)이다. DDS는 Publish-Subscribe 모델, 데이터 중심 설계, 자동 검색, QoS 관리, 분산 구조, 보안 기능, 확장성을 결합하여 현대 로봇공학, 자율 시스템, 산업 자동화, 그리고 미래의 피지컬 AI 생태계를 위한 핵심 통신 기반을 제공한다. ROS2 기반 로봇과 차세대 지능형 기계(Intelligent Machine)에서 DDS는 가장 중요한 핵심 기술 중 하나로 자리 잡고 있으며, 앞으로도 그 중요성은 계속 증가할 것으로 예상된다.

## 1.2 QoS Policy Design

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

QoS(Quality of Service) 정책 설계는 DDS(Data Distribution Service) 생태계에서 가장 중요한 개념 중 하나이며, 현대 로봇 시스템에서 결정론적(Deterministic), 신뢰성 높은, 확장 가능한 실시간 통신을 구현하기 위한 핵심 기반 기술이다. 기존의 통신 프레임워크가 네트워크의 동작 특성을 하부 인프라에 의존하는 경우가 많다면, DDS는 정책 기반 통신 아키텍처를 제공하여 개발자가 데이터 전달 방식, 저장 방식, 우선순위, 필터링 방법, 동기화 방법 등을 직접 정의할 수 있도록 한다. 이러한 특성 덕분에 DDS는 단순한 텔레메트리 시스템부터 안전성이 요구되는 자율주행 로봇, 산업용 모바일 로봇, 군집 로봇, 물리 AI 시스템에 이르기까지 매우 다양한 환경에서 활용될 수 있다.

로봇 시스템에서 모든 데이터가 동일한 중요도를 갖는 것은 아니다. 배터리 상태 정보는 일부 패킷 손실이 발생하더라도 큰 문제가 없을 수 있지만, 비상 정지(E-Stop) 명령은 반드시 정해진 시간 안에 전달되어야 한다. 고해상도 카메라 스트림은 높은 대역폭이 중요하며, 위치 추정 데이터는 낮은 지연 시간과 높은 일관성이 중요하다. DDS는 이러한 서로 다른 요구사항을 충족하기 위해 Topic, Publisher, Subscriber, DataWriter, DataReader 단위로 개별 QoS 정책을 설정할 수 있도록 설계되었다.

DDS QoS의 핵심 철학은 통신 특성을 명시적으로 정의한다는 점이다. 각 DDS 참여자는 자신이 원하는 서비스 수준을 QoS Profile로 선언하며, DDS Discovery 과정에서 Publisher와 Subscriber 간의 QoS 호환성을 자동으로 검사한다. 이를 Request-Offer 모델이라고 부른다. Subscriber는 특정 수준의 서비스를 요구(Request)하고 Publisher는 특정 수준의 서비스를 제공(Offer)한다. 양측이 호환될 경우에만 통신이 성립된다. 이러한 방식은 시스템 통합 시 발생할 수 있는 통신 오류를 사전에 방지하고 예측 가능한 동작을 가능하게 한다.

가장 많이 사용되는 QoS 정책 중 하나는 Reliability 정책이다. Reliability는 데이터 전달 과정에서 패킷 손실을 허용할 것인지, 아니면 반드시 전달을 보장할 것인지를 결정한다. Best Effort 모드는 빠른 전송과 낮은 오버헤드를 목표로 하며 네트워크 혼잡 상황에서 일부 데이터 손실을 허용한다. LiDAR 포인트 클라우드, 카메라 영상, 상태 모니터링 데이터처럼 최신 데이터가 중요하고 일부 데이터 손실이 허용되는 경우에 적합하다. 반면 Reliable 모드는 ACK와 재전송 메커니즘을 통해 데이터 전달을 보장한다. 명령 메시지, 안전 관련 신호, 작업 지시, 상태 동기화 정보와 같이 데이터 손실이 허용되지 않는 경우에 사용된다.

Durability 정책은 과거 데이터의 유지 방식을 정의한다. Volatile 모드에서는 Subscriber가 연결된 이후의 데이터만 수신할 수 있다. DDS는 과거 데이터를 저장하지 않는다. 반면 Transient Local 모드에서는 Publisher가 최근 데이터를 저장하고 있으며, 새로운 Subscriber가 연결되더라도 마지막 상태 정보를 즉시 전달할 수 있다. 예를 들어 로봇이 이미 지도를 생성한 상태에서 새로운 시각화 프로그램이 연결되면, 기존 지도 정보를 즉시 수신할 수 있다.

History 정책은 DDS가 몇 개의 데이터 샘플을 저장할 것인지를 결정한다. Keep Last 모드는 최근 N개의 데이터만 유지하며, Keep All 모드는 가능한 모든 데이터를 저장한다. 대부분의 로봇 시스템에서는 메모리 사용량을 예측하기 쉬운 Keep Last 방식이 사용된다. 예를 들어 위치 추정 데이터의 최근 10개 샘플만 유지하여 일시적인 통신 지연 상황에서도 데이터 복구가 가능하도록 설계할 수 있다.

Depth 정책은 Keep Last와 함께 사용되며 유지할 샘플 수를 정의한다. Depth가 1이면 항상 최신 데이터만 유지된다. Depth가 커질수록 Subscriber는 더 많은 과거 데이터를 받을 수 있지만 메모리 사용량과 잠재적 지연 시간도 증가한다. 따라서 데이터 특성과 실시간 요구사항을 고려하여 적절한 값을 설정해야 한다.

Deadline 정책은 데이터가 일정 시간 내에 반드시 갱신되어야 함을 정의한다. Publisher는 데이터 갱신 주기를 선언하고 Subscriber는 이를 모니터링한다. 만약 Deadline이 초과되면 DDS는 이벤트를 발생시켜 잠재적인 통신 장애나 소프트웨어 오류를 감지할 수 있도록 한다. 예를 들어 위치 추정 노드가 50ms마다 데이터를 제공해야 하는데 이 조건을 만족하지 못하면 시스템은 센서 장애 또는 네트워크 문제를 감지할 수 있다.

Latency Budget 정책은 데이터가 어느 정도의 지연 시간을 허용하는지를 DDS Middleware에 전달한다. 실시간 제어 시스템은 매우 낮은 지연 시간을 요구하지만, 로그 수집 시스템은 수백 밀리초 수준의 지연도 허용할 수 있다. DDS 구현체는 이 정보를 활용하여 내부 스케줄링과 자원 관리를 최적화할 수 있다.

Liveliness 정책은 통신 상대가 살아있는지를 확인하는 기능을 제공한다. 분산 로봇 시스템에서는 노드 장애를 빠르게 감지하는 것이 매우 중요하다. DDS는 Automatic, Manual by Participant, Manual by Topic 방식의 Liveliness 검사를 지원한다. 일정 시간 동안 Liveliness가 갱신되지 않으면 Subscriber는 Publisher가 더 이상 동작하지 않는 것으로 판단할 수 있다. 이는 장애 감지, 이중화 시스템, Failover 설계의 핵심 요소가 된다.

Lifespan 정책은 데이터의 유효 기간을 정의한다. 지정된 시간이 지나면 DDS는 해당 데이터를 자동으로 폐기한다. 예를 들어 몇 초 전의 장애물 정보는 현재 환경을 정확히 반영하지 못할 수 있으므로 자동으로 제거되어야 한다. 이를 통해 오래된 데이터가 의사결정 과정에 영향을 미치는 것을 방지할 수 있다.

Ownership 정책은 여러 Publisher가 동일한 Topic에 데이터를 송신할 때 데이터 소유권을 결정한다. Shared Ownership은 모든 Publisher의 데이터를 허용하며, Exclusive Ownership은 가장 높은 우선순위를 가진 Publisher만 유효한 데이터 소스로 인정한다. 이 기능은 이중화 제어 시스템에서 매우 유용하다.

Ownership Strength는 Exclusive Ownership 환경에서 Publisher의 우선순위를 정의한다. 주 제어기가 장애를 일으키면 DDS는 자동으로 백업 제어기로 소유권을 이전할 수 있다. 이러한 기능은 자율주행 차량, 산업용 로봇, UAV와 같은 고신뢰성 시스템에서 매우 중요하다.

Destination Order 정책은 데이터 정렬 기준을 정의한다. 메시지를 송신 시각 기준으로 정렬할 것인지, 수신 시각 기준으로 정렬할 것인지를 선택할 수 있다. 분산 센서 환경에서는 네트워크 지연이 서로 다를 수 있기 때문에 Source Timestamp 기반 정렬이 센서 융합 정확도를 높이는 데 도움이 된다.

Transport Priority 정책은 특정 데이터 스트림에 우선순위를 부여하는 기능이다. 비상 정지 명령, 충돌 회피 메시지, 제어 명령은 일반 로그 데이터보다 높은 우선순위를 가질 수 있다. 실제 구현 수준은 DDS 벤더와 운영체제에 따라 다르지만, 실시간 시스템 설계에서 중요한 고려 요소이다.

Resource Limits 정책은 DDS가 사용할 수 있는 메모리와 버퍼 크기를 제한한다. 이를 통해 과도한 데이터 발생 시 메모리 부족 현상을 방지하고 시스템의 예측 가능성을 향상시킬 수 있다. 특히 MCU 기반 제어기나 Jetson 기반 임베디드 플랫폼에서는 매우 중요한 설정이다.

Presentation 정책은 여러 데이터 샘플을 논리적으로 하나의 그룹으로 묶어 전달하는 기능을 제공한다. 예를 들어 위치, 속도, 가속도 정보를 하나의 일관된 데이터 세트로 전달할 수 있으며, Subscriber는 이를 동기화된 상태로 수신할 수 있다.

Time-Based Filter 정책은 Subscriber가 수신하고자 하는 최소 데이터 주기를 지정한다. Publisher가 100Hz로 데이터를 송신하더라도 모니터링 화면은 1Hz만 필요할 수 있다. 이 경우 DDS가 중간 데이터를 자동으로 필터링하여 네트워크와 CPU 자원을 절약할 수 있다.

Partition 정책은 논리적 네트워크 분리를 제공한다. 동일한 DDS Domain 내에서도 여러 로봇 그룹, 시뮬레이션 환경, 테스트 환경, 실제 운영 환경을 서로 분리하여 운영할 수 있다. 이는 대규모 Fleet 시스템에서 매우 유용하다.

실제 시스템에서는 QoS 상호 운용성도 중요한 이슈이다. Fast DDS, Cyclone DDS, Connext DDS 등 DDS 구현체마다 일부 정책 지원 수준이 다를 수 있다. 따라서 시스템 통합 단계에서 QoS 호환성 검증이 반드시 수행되어야 한다. ROS 2 역시 DDS 위에서 동작하기 때문에 ROS 2 QoS 설정을 이해하려면 DDS QoS 정책에 대한 이해가 필수적이다.

자율주행 AMR에서는 QoS 정책 설계가 시스템 성능을 직접 결정한다. LiDAR 데이터는 일반적으로 Best Effort와 작은 History Depth를 사용하여 지연 시간을 최소화한다. Localization 데이터는 Reliable 통신과 적절한 History를 사용하여 안정성을 확보한다. Safety Topic은 Reliable, Deadline, High Priority 설정을 활용하여 높은 신뢰성을 제공한다. Fleet Management 시스템은 DDS와 MQTT, REST API, gRPC 등을 함께 사용하여 실시간 제어와 클라우드 통합을 동시에 달성한다.

향후 Physical AI, Foundation Model, VLA(Vision-Language-Action), Embodied AI 기반 로봇이 확대되면서 QoS의 중요성은 더욱 커질 것이다. 미래의 DDS QoS는 단순한 메시지 전송을 넘어 AI 추론 결과 동기화, Action Token 전달, 분산 월드 모델 공유, 다중 로봇 협업 추론과 같은 새로운 요구사항을 지원하게 될 것이다.

결국 QoS 정책 설계는 단순한 통신 설정 작업이 아니라 로봇 시스템 전체의 안정성, 실시간성, 확장성, 안전성을 결정하는 핵심 아키텍처 기술이다. 실내 AMR, 실외 자율주행 플랫폼, 모바일 매니퓰레이터, Cargo UAV, 사족보행 로봇, 휴머노이드 로봇 등 모든 차세대 자율 시스템에서 DDS QoS 엔지니어링은 통신 아키텍처의 중심 역할을 수행하게 되며, 분산 지능형 로봇 시스템의 성공 여부를 좌우하는 핵심 요소가 될 것이다.

## 1.3 Topic Publisher Subscriber

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

Topic--Publisher--Subscriber 모델은 DDS(Data Distribution Service) 표준의 핵심 통신 패러다임으로서, 현대 로봇 시스템, 자율주행 플랫폼, 분산 제어 시스템, 그리고 Physical AI 아키텍처에서 확장 가능하고 느슨하게 결합된 실시간 데이터 교환을 가능하게 하는 기본 구조이다. 전통적인 클라이언트-서버 방식에서는 통신하는 양쪽이 서로의 위치와 주소를 알고 있어야 하지만, DDS는 데이터 중심(Data-Centric)의 Publish-Subscribe 구조를 사용하여 데이터 생산자와 데이터 소비자를 분리한다. 이러한 구조는 모듈성, 확장성, 장애 허용성, 유지보수성, 그리고 시스템 유연성을 크게 향상시키며, AMR, 실외 자율주행 차량, 모바일 매니퓰레이터, 사족보행 로봇, 휴머노이드, Cargo UAV와 같은 대규모 로봇 시스템에 매우 적합하다.

DDS 통신의 중심에는 Topic이라는 개념이 존재한다. Topic은 데이터가 전달되는 논리적인 통신 채널을 의미한다. Topic은 누가 통신하는지를 정의하는 것이 아니라 어떤 정보를 전달하는지를 정의한다. 따라서 Topic은 시스템 내 데이터의 의미를 표현하는 공통 인터페이스 역할을 수행한다. 모든 Topic은 특정 데이터 타입과 연결되며, 데이터 타입은 전송되는 정보의 구조와 의미를 정의한다. 이러한 데이터 중심 구조 덕분에 Publisher와 Subscriber는 서로를 알 필요 없이 동일한 데이터 정의만 공유하면 통신할 수 있다.

로봇 시스템에서는 Topic이 일반적으로 의미 있는 정보 흐름을 표현한다. LiDAR 센서는 "/lidar/points"라는 Topic으로 포인트 클라우드를 전송할 수 있으며, Localization 시스템은 "/robot/pose" Topic으로 로봇 위치를 전달할 수 있다. Navigation Planner는 "/planner/path"를 통해 경로를 제공하고, Motor Controller는 "/cmd_vel" Topic을 통해 속도 명령을 수신할 수 있다. Topic은 물리적인 통신 경로가 아닌 논리적인 데이터 채널이기 때문에 시스템은 매우 유연하고 확장 가능한 구조를 갖게 된다.

Publisher는 Topic에 데이터를 생성하고 전송하는 DDS 엔티티이다. Publisher는 하나 이상의 DataWriter를 생성하며, 각 DataWriter는 특정 Topic에 연결된다. 실제 데이터는 DataWriter를 통해 DDS Middleware로 전달된다. Publisher는 데이터 생성 주기, QoS 정책, 신뢰성 수준, 내구성 정책, 타이밍 조건 등을 정의한다. 다시 말해 Publisher는 DDS 생태계에서 정보의 공급자 역할을 수행한다.

실제 로봇 시스템에서 Publisher는 센서 드라이버, 제어기, 인식 알고리즘, 위치추정 모듈, 경로계획기, 진단 시스템, Fleet Management 서버 등에 해당한다. LiDAR 드라이버는 지속적으로 포인트 클라우드를 발행하고, IMU 드라이버는 관성 데이터를 발행한다. GNSS 모듈은 위치 정보를 제공하며, 배터리 관리 시스템은 배터리 상태를 전송한다. Fleet 서버는 작업 지시와 교통 제어 명령을 발행할 수 있다. 데이터를 생성하는 모든 구성 요소는 DDS 관점에서 Publisher가 된다.

Subscriber는 Topic으로부터 데이터를 수신하는 DDS 엔티티이다. Subscriber는 DataReader를 생성하여 특정 Topic에 대한 데이터를 읽는다. 전통적인 통신 시스템처럼 특정 송신자를 직접 지정하는 것이 아니라, 원하는 Topic에 대한 관심만 표현하면 된다. DDS는 자동으로 적절한 Publisher를 찾아 연결을 생성한다. 이 과정은 완전히 동적으로 이루어지며 사용자가 네트워크 설정을 직접 수행할 필요가 없다.

Subscriber 역시 다양한 형태로 존재한다. Navigation Controller는 위치 정보와 장애물 정보를 구독할 수 있다. 모니터링 화면은 진단 정보와 배터리 상태를 수신할 수 있다. 인식 알고리즘은 카메라 영상과 LiDAR 데이터를 동시에 구독할 수 있으며, Fleet Management 시스템은 수십 대 또는 수백 대의 로봇 상태 정보를 동시에 수신할 수 있다. DDS는 One-to-Many, Many-to-One, Many-to-Many 통신을 자연스럽게 지원하기 때문에 동일한 Topic을 여러 Subscriber가 동시에 사용할 수 있다.

Topic--Publisher--Subscriber 모델의 가장 큰 특징은 느슨한 결합(Loose Coupling)이다. Publisher는 누가 데이터를 읽는지 알 필요가 없다. Subscriber는 누가 데이터를 보내는지 알 필요가 없다. 양측 모두 IP 주소, 프로세스 위치, 하드웨어 구성, 배포 환경 등에 대한 정보를 알지 못해도 된다. 오직 Topic과 데이터 정의만 공유하면 된다. 이러한 구조는 유지보수성을 크게 향상시키며 대규모 시스템 확장을 용이하게 한다.

예를 들어 창고형 AMR에서 LiDAR 센서가 장애물 데이터를 발행하고 있다고 가정해보자. 처음에는 충돌 회피 알고리즘만 이 데이터를 사용한다. 이후 시각화 도구, 데이터 기록 시스템, AI 기반 객체 인식 시스템을 추가하더라도 LiDAR Publisher는 전혀 수정할 필요가 없다. 새로운 Subscriber들이 동일한 Topic을 구독하기만 하면 된다. 이러한 특성은 통합 비용을 크게 줄이고 시스템 진화를 쉽게 만든다.

DDS 통신은 기본적으로 비동기(Asynchronous) 방식으로 동작한다. Publisher는 데이터가 준비되면 즉시 전송하고, Subscriber는 자신의 처리 속도에 맞추어 데이터를 수신한다. 느린 Subscriber가 있다고 해서 Publisher가 멈추지 않는다. 이러한 비동기 구조는 실시간 로봇 시스템에서 매우 중요하다. 센서 처리, 제어, 인공지능 추론 등이 서로 독립적으로 실행될 수 있기 때문이다.

Topic 구조는 강력한 타입 안정성(Type Safety)도 제공한다. 모든 Topic은 IDL(Interface Definition Language)을 기반으로 데이터 구조를 정의한다. DDS는 Discovery 단계에서 Publisher와 Subscriber의 데이터 타입을 비교하여 호환성을 검증한다. 데이터 구조가 다르면 통신 자체를 허용하지 않는다. 이를 통해 분산 시스템에서 발생할 수 있는 데이터 손상과 통합 오류를 예방할 수 있다.

DDS Discovery는 Topic--Publisher--Subscriber 구조를 가능하게 하는 핵심 기능이다. DDS Participant가 네트워크에 참여하면 자신의 존재를 자동으로 알리고 다른 Participant를 탐색한다. Publisher는 자신이 제공하는 Topic과 QoS 정보를 광고하고, Subscriber는 자신이 필요로 하는 Topic과 QoS 요구사항을 광고한다. DDS Middleware는 이를 자동으로 매칭하여 통신 경로를 생성한다. 중앙 서버나 수동 설정이 필요하지 않기 때문에 진정한 Plug-and-Play 통신이 가능하다.

이러한 Discovery 메커니즘은 로봇 시스템의 유연성을 크게 향상시킨다. 새로운 센서를 연결하면 즉시 기존 시스템이 이를 사용할 수 있다. 진단 도구는 실행 중인 시스템에 접속하여 데이터를 수신할 수 있으며, 시뮬레이션 환경과 실제 로봇도 동일한 DDS Domain 내에서 쉽게 통합될 수 있다.

Topic--Publisher--Subscriber 구조는 다양한 통신 패턴을 지원한다. 하나의 Publisher가 수십 개 Subscriber에게 데이터를 전송할 수 있고, 여러 Publisher가 동일 Topic에 데이터를 기록할 수도 있다. 또한 여러 Subscriber가 동시에 여러 Topic을 수신할 수 있다. 노드가 실행 중에 생성되거나 종료되더라도 시스템 전체가 중단되지 않는다.

Fleet Robotics 환경에서는 이러한 확장성이 특히 중요하다. 수백 대의 로봇이 상태 정보, 위치 정보, 진단 데이터, 작업 진행 상황, 환경 인식 정보를 발행할 수 있다. Fleet Management 서버는 이 데이터를 모두 수신하면서 동시에 작업 명령과 운영 지시를 발행한다. DDS는 이러한 대규모 데이터 흐름을 효율적으로 처리하면서도 실시간성을 유지할 수 있다.

Topic 설계 자체도 중요한 엔지니어링 영역이다. 좋은 Topic 구조는 시스템의 이해도와 유지보수성을 향상시킨다. 일반적으로 "/robot/pose", "/robot/velocity", "/sensor/lidar/front", "/sensor/camera/left", "/navigation/path", "/diagnostics/system"과 같은 계층적 네이밍 구조가 사용된다. 일관된 Topic 설계는 상호 운용성을 향상시키고 통합 작업을 단순화한다.

QoS 정책은 Topic--Publisher--Subscriber 통신의 동작 방식을 결정한다. Publisher는 특정 QoS를 제공하고 Subscriber는 원하는 QoS를 요청한다. DDS는 두 설정이 호환되는 경우에만 연결을 허용한다. Reliability, Durability, History, Deadline, Liveliness, Latency Budget, Ownership, Lifespan 등의 정책은 통신의 신뢰성, 지연 시간, 저장 방식, 장애 감지 방식을 결정한다.

ROS 2는 DDS를 기반으로 동작하기 때문에 Topic--Publisher--Subscriber 모델을 그대로 활용한다. ROS 2 Node들은 대부분 Topic을 통해 데이터를 교환한다. Perception Node는 센서 데이터를 발행하고, Localization Node는 위치 정보를 제공하며, Planning Node는 경로를 생성하고, Control Node는 명령을 수신한다. DDS는 이 모든 통신을 지원하는 실질적인 Middleware 계층 역할을 수행한다.

보안 역시 DDS Topic 기반 통신에 통합될 수 있다. DDS Security는 인증(Authentication), 권한 관리(Authorization), 암호화(Encryption), Governance 정책을 제공한다. 특정 Publisher나 Subscriber만 Topic에 접근하도록 제한할 수 있으며, 네트워크 상의 데이터는 암호화되어 보호될 수 있다. 이는 공장, 물류센터, 스마트시티, 군사용 로봇과 같은 환경에서 매우 중요하다.

최근 Physical AI 시스템은 Topic--Publisher--Subscriber 구조에 더욱 의존하고 있다. 대규모 AI 모델은 인식, 추론, 계획, 행동 생성 과정에서 막대한 양의 데이터를 교환한다. 센서 융합 엔진은 여러 Publisher의 데이터를 동시에 소비하고, 계획 시스템은 Localization, Perception, Semantic Understanding, Fleet Coordination 시스템으로부터 정보를 수신한다. DDS Topic은 이러한 복잡한 정보 흐름을 체계적으로 관리할 수 있는 구조를 제공한다.

다중 로봇 환경에서는 Topic 기반 통신을 통해 협업 지능(Collaborative Intelligence)이 가능해진다. 로봇들은 지도, 위치 정보, 객체 정보, 작업 상태, 환경 정보를 서로 공유할 수 있다. 결과적으로 각각의 로봇은 독립적인 기계가 아니라 하나의 거대한 정보 네트워크를 구성하는 지능형 노드가 된다.

향후 AI Native Robotics 시대에는 Topic--Publisher--Subscriber 구조의 중요성이 더욱 증가할 것이다. 미래 로봇은 단순한 센서 데이터와 제어 명령뿐 아니라 월드 모델(World Model), 추론 결과, Foundation Model 출력, VLA Action Token, Task Plan 등을 공유하게 된다. DDS Topic은 이러한 차세대 데이터 흐름을 지원할 수 있는 강력한 추상화 계층으로 발전할 것이다.

결론적으로 Topic--Publisher--Subscriber 모델은 단순한 통신 기법이 아니라 현대 로봇 소프트웨어 아키텍처의 핵심 설계 철학이다. 데이터 생산자와 소비자를 분리하고, Topic 중심으로 시스템을 구성함으로써 DDS는 높은 모듈성, 확장성, 유연성, 신뢰성, 유지보수성을 제공한다. AMR, 실외 자율주행 플랫폼, 모바일 매니퓰레이터, Cargo UAV, 사족보행 로봇, 휴머노이드, 그리고 미래의 Physical AI 시스템에 이르기까지 Topic--Publisher--Subscriber 구조는 분산 지능형 로봇 시스템의 가장 중요한 통신 기반으로 계속 활용될 것이며, 차세대 자율 시스템을 가능하게 하는 핵심 아키텍처 원칙으로 자리잡게 될 것이다.

## 1.4 DDS Security Plugin

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

DDS Security Plugin은 분산 시스템에서 DDS(Data Distribution Service) 통신을 보호하기 위해 OMG(Object Management Group)가 정의한 표준 보안 프레임워크이다. DDS가 현대 로봇, 자율주행 차량, 산업 자동화 시스템, 항공우주 플랫폼, 국방 시스템, 그리고 Physical AI 아키텍처의 핵심 미들웨어로 자리 잡으면서 강력한 사이버보안 체계의 필요성이 급격히 증가하였다. 초기 DDS 시스템은 성능, 확장성, 결정론적 통신에 중점을 두었지만, 최근에는 로봇과 자율 시스템이 기업 네트워크, 클라우드 환경, 무선 통신망, 다중 제조사 생태계와 연결되면서 보안이 필수적인 요구사항이 되었다. DDS Security는 이러한 문제를 해결하기 위해 인증(Authentication), 권한 관리(Authorization), 암호화(Encryption), 접근 제어(Access Control), 감사(Auditing) 기능을 제공하면서도 DDS의 실시간성과 고성능 특성을 유지하도록 설계되었다.

DDS Security는 DDS의 기본 통신 구조를 변경하지 않고 보안 기능을 추가하는 확장 구조를 채택한다. 보안 기능은 플러그인 형태로 구현되므로 기존 DDS 애플리케이션을 수정하지 않고도 보안 기능을 적용할 수 있다. 이러한 구조는 기존 DDS 시스템과의 호환성을 유지하면서 조직별 요구사항에 맞는 보안 정책을 적용할 수 있게 해준다. 결과적으로 보안 기능은 각 애플리케이션에서 개별적으로 구현되는 것이 아니라 DDS Middleware 계층에 통합되어 전체 시스템에서 일관된 보안 정책을 제공하게 된다.

DDS Security는 몇 가지 핵심 원칙을 기반으로 설계된다. 첫 번째는 신원 확인이다. DDS Domain에 참여하는 모든 노드는 자신의 신원을 증명해야 한다. 두 번째는 권한 관리이다. 인증된 노드라 하더라도 허가된 데이터와 서비스에만 접근할 수 있어야 한다. 세 번째는 기밀성이다. 민감한 데이터는 허가되지 않은 사용자가 볼 수 없어야 한다. 네 번째는 무결성이다. 데이터는 전송 과정에서 변조되지 않아야 한다. 다섯 번째는 추적 가능성이다. 모든 보안 이벤트는 기록되어 감사와 사고 분석이 가능해야 한다. 이러한 원칙은 분산 로봇 시스템을 위한 종합적인 사이버보안 체계를 형성한다.

DDS Security는 크게 Authentication Plugin, Access Control Plugin, Cryptographic Plugin, Logging Plugin, Data Tagging Plugin의 다섯 가지 핵심 플러그인으로 구성된다. 각각의 플러그인은 특정 보안 기능을 담당하며 표준 인터페이스를 통해 서로 협력한다. 이러한 모듈 구조 덕분에 DDS 공급업체들은 서로 다른 구현 방식을 사용하더라도 상호 운용성을 유지할 수 있다.

Authentication Plugin은 DDS Participant의 신원을 검증하는 역할을 수행한다. DDS Domain에 참여하려는 노드는 먼저 자신의 자격 증명을 제시해야 한다. 일반적으로 DDS Security는 X.509 인증서 기반의 PKI(Public Key Infrastructure)를 사용한다. 각 Participant는 신뢰된 인증기관(Certificate Authority)이 발급한 디지털 인증서를 보유한다. 인증 과정에서 DDS Participant들은 인증서를 교환하고 암호화된 핸드셰이크를 수행하여 상호 신뢰를 구축한다. 인증에 성공한 노드만이 DDS 네트워크에 참여할 수 있다.

이 기능은 특히 대규모 로봇 시스템에서 매우 중요하다. 예를 들어 수백 대의 AMR이 운영되는 물류센터에서는 악성 장치가 Fleet Server나 Robot Controller를 가장하여 네트워크에 침입하려 할 수 있다. DDS Authentication은 인증된 장치만 통신에 참여하도록 함으로써 이러한 위협을 차단한다. 인증 과정은 Discovery 단계에서 자동으로 수행되며 애플리케이션 개발자가 직접 처리할 필요가 없다.

인증이 완료되면 Access Control Plugin이 권한을 관리한다. 인증이 "누구인가"를 확인하는 과정이라면 권한 관리는 "무엇을 할 수 있는가"를 결정하는 과정이다. DDS Access Control은 Governance Document와 Permissions Document를 기반으로 정책을 적용한다. 이를 통해 특정 Participant가 어떤 Topic을 Publish할 수 있는지, 어떤 Topic을 Subscribe할 수 있는지, 어떤 Domain에 접근할 수 있는지를 세밀하게 정의할 수 있다.

예를 들어 Localization Node는 위치 정보를 Publish할 수 있지만 Motor Control 명령은 Publish할 수 없도록 설정할 수 있다. Monitoring Dashboard는 Diagnostics Topic을 Subscribe할 수 있지만 Safety 관련 Topic에는 접근하지 못하도록 제한할 수 있다. Fleet Management Server는 Mission Assignment Topic을 Publish할 수 있지만 일반 로봇 노드는 이를 수신만 할 수 있다. 이러한 세밀한 권한 제어는 시스템의 공격 표면을 크게 줄여준다.

Governance Document는 Domain 수준의 보안 정책을 정의한다. 어떤 Topic에 암호화를 적용할지, 어떤 통신에 인증이 필요한지, 보안 정책을 어떻게 적용할지를 정의한다. Permissions Document는 개별 Participant의 권한을 정의한다. 두 문서가 결합되어 DDS 네트워크 전체의 보안 정책을 구성한다.

Cryptographic Plugin은 DDS 통신의 기밀성과 무결성을 보장한다. 인증과 권한 검사가 완료되면 DDS는 암호화된 통신 채널을 생성할 수 있다. DDS Security는 AES(Advanced Encryption Standard)와 같은 현대적인 암호화 알고리즘을 지원하며 안전한 키 교환 메커니즘을 제공한다. 암호화는 네트워크 트래픽이 도청되더라도 내용을 이해할 수 없도록 보호한다.

이 기능은 산업용 로봇, 군사용 로봇, 자율주행 차량, UAV와 같은 시스템에서 특히 중요하다. 예를 들어 발전소 점검 로봇은 민감한 시설 정보를 전송할 수 있으며, Cargo UAV는 비행 제어 정보를 주고받을 수 있다. 이러한 데이터가 외부에 노출되지 않도록 보호하는 것이 암호화의 핵심 목적이다.

무결성 보호 역시 매우 중요하다. DDS Security는 디지털 서명과 메시지 인증 코드를 이용하여 데이터가 전송 중에 변경되지 않았음을 보장한다. 수신자는 데이터가 원본 그대로인지 검증할 수 있으며, 변조가 감지되면 데이터를 거부할 수 있다. 이는 안전 관련 명령이나 제어 데이터가 악의적으로 조작되는 것을 방지한다.

DDS Security는 다양한 수준의 암호화를 지원한다. 전체 Participant 간 통신을 암호화할 수도 있고 특정 Topic만 암호화할 수도 있다. 일부 데이터는 강력한 보호를 적용하고 일부 데이터는 최소한의 보호만 적용하는 것도 가능하다. 이를 통해 보안 수준과 시스템 성능 사이의 균형을 맞출 수 있다.

Logging Plugin은 보안 관련 이벤트를 기록하는 기능을 제공한다. 인증 실패, 권한 위반, 정책 위반, 인증서 만료, 암호화 오류와 같은 이벤트를 저장하여 나중에 분석할 수 있도록 한다. 이러한 로그는 보안 사고 조사, 규제 준수 검증, 시스템 감사에 매우 중요한 역할을 수행한다.

특히 산업 자동화, 항공우주, 의료 로봇, 국가 기반 시설과 같이 규제가 엄격한 분야에서는 보안 로그가 필수적이다. 로그를 통해 시스템이 정상적으로 보안 정책을 준수하고 있는지 확인할 수 있으며, 사고 발생 시 원인을 추적할 수 있다.

Data Tagging Plugin은 데이터에 보안 메타데이터를 추가할 수 있도록 한다. 예를 들어 데이터의 보안 등급, 민감도 수준, 소유 조직, 정책 정보 등을 태그 형태로 부착할 수 있다. Subscriber는 이러한 태그를 기반으로 추가적인 접근 제어 정책을 적용할 수 있다. 이는 여러 기관이 협업하는 환경에서 특히 유용하다.

DDS Security는 Discovery 과정과 긴밀하게 통합되어 있다. DDS Participant가 네트워크에 참여하면 먼저 인증과 보안 협상이 수행된다. 인증서 교환, 신뢰 검증, 암호화 파라미터 협상, 보안 컨텍스트 생성이 완료된 이후에만 실제 DDS 통신이 시작된다. 이를 통해 처음부터 안전한 통신 환경을 구축할 수 있다.

DDS Security의 가장 큰 장점은 보안 정책이 Middleware 수준에서 강제된다는 점이다. 개발자는 개별 애플리케이션마다 인증, 암호화, 권한 관리 기능을 구현할 필요가 없다. DDS가 이를 공통 인프라 서비스로 제공하기 때문에 구현 오류를 줄이고 전체 시스템에 일관된 보안 정책을 적용할 수 있다.

ROS 2 환경에서도 DDS Security는 핵심 보안 기술로 활용된다. ROS 2는 DDS를 기반으로 동작하기 때문에 DDS Security 기능을 그대로 사용할 수 있다. ROS 2 Node는 DDS Security를 통해 상호 인증을 수행하고, Topic 통신을 암호화하며, Topic 접근 권한을 제어할 수 있다. 이는 공공장소, 스마트시티, 공장, 물류센터와 같이 외부 네트워크와 연결된 로봇 시스템에서 매우 중요하다.

Fleet Robotics 환경에서는 보안의 중요성이 더욱 커진다. 수백 또는 수천 대의 로봇이 무선 네트워크를 통해 통신하는 상황에서는 명령 위조, 데이터 탈취, 스푸핑 공격, 서비스 거부 공격 등이 발생할 수 있다. DDS Security는 이러한 위협을 줄이고 Fleet 전체의 신뢰성을 향상시킨다.

자율주행 차량 역시 중요한 적용 분야이다. 자율주행 플랫폼은 Perception, Localization, Planning, Control, Cloud Service 간의 분산 통신에 의존한다. 통신 보안이 실패할 경우 심각한 안전 문제로 이어질 수 있다. DDS Security는 이러한 통신이 신뢰 가능한 상태를 유지하도록 지원한다.

산업 자동화 환경에서도 DDS Security는 중요한 역할을 한다. 현대 스마트팩토리는 로봇, PLC, Edge Computer, AI 서버, 검사 시스템, 클라우드 플랫폼이 모두 연결되어 있다. DDS는 이러한 시스템의 통신 백본 역할을 수행하며, Security Plugin은 사이버 공격으로부터 운영 네트워크를 보호한다.

Physical AI 시스템이 발전함에 따라 DDS Security의 중요성은 더욱 커질 것이다. 미래의 AI 기반 로봇은 단순한 센서 데이터뿐 아니라 Semantic World Model, 추론 결과, 학습된 행동 모델, Foundation Model 출력, 협업 계획 정보 등을 공유하게 된다. 이러한 데이터는 매우 높은 경제적 가치와 전략적 가치를 가질 수 있으며, 강력한 보호가 요구된다.

클라우드와 Edge Computing이 결합된 환경에서도 DDS Security는 핵심 역할을 수행한다. Edge Device, On-Premise GPU Server, Cloud AI Platform, Fleet Management System이 함께 동작하는 하이브리드 구조에서는 복잡한 신뢰 관계를 관리해야 한다. DDS Security는 이러한 환경에서도 안전한 통신을 보장한다.

물론 보안 기능은 일정 수준의 계산 오버헤드를 발생시킨다. 암호화와 인증 과정은 CPU 사용량과 지연 시간을 증가시킬 수 있다. 따라서 시스템 설계 단계에서 보안과 실시간 성능 사이의 균형을 고려해야 한다. TPM(Trusted Platform Module), HSM(Hardware Security Module), Secure Boot, 암호화 가속기와 같은 기술을 활용하면 이러한 영향을 줄일 수 있다.

DDS Security를 적용할 때는 보안 테스트도 반드시 수행해야 한다. 인증 실패 처리, 인증서 갱신 절차, 키 교체 메커니즘, 권한 정책 검증, 침투 테스트, 취약점 분석, 사고 대응 계획 등을 체계적으로 검증해야 한다. 보안은 일회성 기능이 아니라 시스템 전체 생명주기 동안 지속적으로 관리되어야 하는 요소이다.

결론적으로 DDS Security Plugin은 DDS를 단순한 고성능 통신 미들웨어에서 완전한 보안 통신 플랫폼으로 확장시켜 주는 핵심 기술이다. 인증, 권한 관리, 암호화, 무결성 검증, 감사 로그, 정책 기반 접근 제어를 Middleware 수준에서 제공함으로써 DDS Security는 현대 자율 시스템이 필요로 하는 신뢰 기반을 구축한다. 향후 AMR, 실외 자율주행 차량, 모바일 매니퓰레이터, Fleet 시스템, Cargo UAV, 사족보행 로봇, 휴머노이드, 그리고 AI Native Robotics 환경이 더욱 확대될수록 DDS Security는 분산 지능형 시스템의 필수 아키텍처 구성 요소로 자리 잡게 될 것이며, 증가하는 사이버 위협으로부터 안전한 통신을 보장하는 핵심 기술로 활용될 것이다.

## 1.5 FastDDS vs CycloneDDS

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

Fast DDS와 Cyclone DDS는 현대 로보틱스와 자율 시스템 분야에서 가장 널리 사용되는 오픈소스 DDS(Data Distribution Service) 구현체이다. DDS가 ROS 2와 다양한 분산 실시간 시스템의 핵심 통신 미들웨어로 자리 잡으면서, 어떤 DDS 구현체를 선택할 것인가는 중요한 시스템 아키텍처 결정 요소가 되었다. 두 솔루션 모두 OMG DDS 표준을 준수하며 RTPS(Real-Time Publish Subscribe) 프로토콜을 기반으로 상호 운용성을 제공한다. 그러나 내부 아키텍처, 성능 특성, 확장성, 메모리 관리 방식, 설정 구조, 디버깅 기능, 생태계 성숙도 및 최적화 방향에는 상당한 차이가 존재한다. 따라서 AMR, 실외 자율주행 차량, 모바일 매니퓰레이터, Fleet Management 시스템, Physical AI 플랫폼, Cargo UAV, 사족보행 로봇, 휴머노이드 로봇을 설계하는 엔지니어에게 DDS 선택은 매우 중요한 문제이다.

DDS는 기본적으로 RTPS 프로토콜, QoS 정책, Topic 기반 데이터 교환, Discovery 메커니즘, Security 확장 기능, 데이터 직렬화 규칙 등을 정의하는 표준이다. Fast DDS와 Cyclone DDS는 동일한 DDS 표준을 구현하지만 내부 구현 방식은 서로 다르다. Fast DDS는 eProsima에서 개발하고 유지보수하며, Cyclone DDS는 Eclipse Foundation 생태계에서 주도적으로 개발되고 있다. 두 솔루션 모두 ROS 2 환경에서 널리 사용되며 산업 현장에서 충분한 신뢰성을 인정받고 있다.

Fast DDS는 대규모 분산 시스템을 지원하기 위해 개발되었다. 설계 초기부터 높은 확장성과 풍부한 설정 기능을 제공하는 것을 목표로 하였으며, ROS 2의 여러 배포판에서 기본 DDS 구현체로 채택되었다. DDS Security 지원, 다양한 운영체제 지원, 광범위한 툴 체인, 산업 현장 적용 사례 등이 풍부하여 엔터프라이즈급 로봇 시스템에서 자주 사용된다.

반면 Cyclone DDS는 단순성, 예측 가능성, 결정론적 동작, 낮은 지연 시간, 효율적인 자원 사용을 우선 목표로 개발되었다. 설정 복잡도를 최소화하고 안정성을 높이는 방향으로 설계되었기 때문에 연구용 로봇, 실시간 제어 시스템, 개발 환경에서 높은 평가를 받고 있다.

가장 큰 차이점 중 하나는 Discovery 메커니즘이다. DDS에서는 Participant가 통신하기 전에 서로를 발견해야 한다. 시스템 규모가 커질수록 Discovery 트래픽은 전체 성능에 큰 영향을 미친다.

Fast DDS는 Simple Discovery뿐만 아니라 Discovery Server 구조도 제공한다. Discovery Server를 사용하면 수백\~수천 개 노드가 존재하는 대규모 시스템에서도 멀티캐스트 트래픽을 줄이고 Discovery 효율을 크게 향상시킬 수 있다. 이러한 기능은 대규모 AMR Fleet, 스마트팩토리, 클라우드 기반 로봇 플랫폼에 매우 유리하다.

Cyclone DDS는 비교적 단순한 Peer-to-Peer Discovery 구조를 사용한다. 구현이 가볍고 효율적이며 일반적인 ROS 2 시스템에서는 매우 빠른 노드 발견 성능을 보여준다. 개발 환경이나 중소 규모 로봇 시스템에서는 설정 없이도 우수한 성능을 얻을 수 있다는 장점이 있다.

지연 시간(Latency)은 DDS 비교에서 가장 자주 언급되는 항목 중 하나이다. 지연 시간은 Publisher에서 Subscriber까지 메시지가 전달되는 데 걸리는 시간을 의미한다. 이는 자율주행, 충돌 회피, 실시간 제어, 센서 융합 시스템에서 매우 중요하다.

전통적으로 Cyclone DDS는 많은 ROS 2 벤치마크 환경에서 매우 낮은 지연 시간을 보여주었다. 내부 구조가 단순하고 불필요한 처리 경로가 적기 때문이다. 따라서 실시간 제어 성능이 중요한 시스템에서 선호되는 경우가 많다.

하지만 최근 Fast DDS는 지속적인 최적화를 통해 지연 시간을 크게 개선하였다. 최신 버전에서는 Cyclone DDS와의 성능 차이가 상당히 줄어들었으며 일부 환경에서는 비슷한 수준의 응답성을 제공한다.

처리량(Throughput) 역시 중요한 비교 항목이다. 처리량은 일정 시간 동안 전송 가능한 데이터 양을 의미한다. 고해상도 카메라, LiDAR, AI 데이터 파이프라인, 지도 생성 시스템에서는 매우 중요한 요소이다.

Fast DDS는 전통적으로 높은 처리량 성능을 보여준다. 대용량 데이터 전송을 위한 최적화 기능이 풍부하며 Shared Memory Transport, 효율적인 직렬화 구조, 전송 계층 설정 기능 등을 제공한다. 다수의 카메라 스트림이나 고밀도 LiDAR 데이터를 처리하는 환경에서는 매우 우수한 성능을 발휘한다.

Cyclone DDS 역시 높은 처리량을 제공하지만 최대 성능보다는 안정성과 예측 가능성을 우선시한다. 일반적인 로봇 시스템에서는 차이를 체감하기 어렵지만, 초고대역폭 환경에서는 Fast DDS가 더 유리한 경우가 많다.

메모리 관리 방식도 중요한 차이점이다. 임베디드 로봇 시스템에서는 메모리 사용량이 시스템 안정성에 직접적인 영향을 미친다.

Cyclone DDS는 상대적으로 작은 메모리 사용량과 예측 가능한 자원 소비로 유명하다. 불필요한 메모리 할당을 최소화하고 결정론적 동작을 유지하도록 설계되었다. 따라서 Jetson, MCU 기반 제어기, 저전력 임베디드 플랫폼에서 선호되는 경우가 많다.

Fast DDS는 다양한 메모리 관리 옵션을 제공한다. 기본 설정에서는 Cyclone DDS보다 더 많은 자원을 사용할 수 있지만, 적절한 튜닝을 수행하면 매우 높은 효율성을 얻을 수 있다. 특히 대규모 시스템에서는 이러한 유연성이 큰 장점이 된다.

ROS 2와의 통합성은 실질적인 선택 기준 중 하나이다. ROS 2는 RMW(ROS Middleware) 계층을 통해 DDS 구현체를 추상화한다. 따라서 애플리케이션 코드를 수정하지 않고 DDS 구현체를 변경할 수 있다.

Fast DDS는 오랫동안 ROS 2의 기본 DDS로 사용되었기 때문에 많은 예제, 튜토리얼, 문서, 커뮤니티 자료가 Fast DDS를 기준으로 작성되었다. ROS 2를 처음 접하는 개발자들이 가장 먼저 만나게 되는 DDS 구현체인 경우가 많다.

Cyclone DDS는 최근 몇 년 동안 ROS 2 커뮤니티에서 매우 빠르게 성장하였다. 많은 개발자들이 별도 설정 없이도 안정적이고 빠른 성능을 얻을 수 있다고 평가하고 있으며, 특히 개발 환경에서 높은 만족도를 보인다.

네트워크 스트레스 상황에서의 안정성 역시 중요한 비교 요소이다. 실제 로봇 시스템은 무선 네트워크, 패킷 손실, 전파 간섭, 네트워크 지연 등의 문제에 노출된다.

Fast DDS는 QoS 설정과 Discovery Server 구조를 활용하여 다양한 네트워크 환경에 대응할 수 있다. 네트워크 품질이 낮은 환경에서도 적절한 설정을 통해 높은 안정성을 확보할 수 있다.

Cyclone DDS는 상대적으로 단순한 구조 덕분에 네트워크 품질이 변화하더라도 예측 가능한 동작을 보여주는 경우가 많다. 야외 자율주행 로봇이나 현장형 AMR 프로젝트에서 이러한 특성이 장점으로 평가된다.

Shared Memory 통신도 최근 중요한 요소가 되었다. 카메라, LiDAR, AI 추론 데이터는 매우 크기 때문에 동일한 컴퓨터 내부에서 네트워크 스택을 거치지 않고 직접 데이터를 공유하는 것이 유리하다.

Fast DDS는 Shared Memory Transport를 매우 적극적으로 활용한다. 대용량 센서 데이터를 다루는 AI 기반 로봇 시스템에서는 상당한 성능 향상을 제공할 수 있다.

Cyclone DDS 역시 로컬 통신 최적화를 지원하지만 구현 방식은 다르다. 양쪽 모두 지속적으로 성능 개선이 이루어지고 있다.

보안 기능도 중요한 비교 항목이다. DDS Security는 인증, 권한 관리, 암호화, Governance 정책, 로깅 기능 등을 제공한다.

Fast DDS는 DDS Security 지원이 매우 성숙한 편이며, 엔터프라이즈 환경에서 요구되는 다양한 보안 정책을 지원한다. 따라서 보안 요구사항이 높은 산업 환경에서 선호되는 경우가 많다.

Cyclone DDS 역시 DDS Security를 지원하며 지속적으로 기능이 향상되고 있다. 일반적인 로봇 시스템에서는 충분한 보안 기능을 제공하지만, 복잡한 엔터프라이즈 보안 정책을 적용하는 경우에는 Fast DDS가 더 많은 기능을 제공하는 경우가 있다.

확장성(Scalability)은 미래의 Physical AI 시스템에서 더욱 중요해지고 있다. 물류창고에는 수백 대의 AMR이 존재할 수 있으며, 스마트시티에는 수천 개의 자율 장치가 연결될 수 있다. AI 기반 로봇은 클라우드, Edge Server, GPU Cluster, Fleet Management 플랫폼과 동시에 통신해야 한다.

Fast DDS는 Discovery Server, Transport Layer 최적화, Memory Pool 설정, 엔터프라이즈 아키텍처 지원 등을 통해 대규모 확장성을 제공한다. 따라서 대규모 Fleet 시스템에서는 Fast DDS가 유리한 경우가 많다.

Cyclone DDS 역시 상당한 규모까지 안정적으로 동작하지만, 매우 큰 규모의 엔터프라이즈 환경에서는 Fast DDS가 제공하는 고급 기능들이 장점으로 작용할 수 있다.

디버깅과 모니터링 측면에서도 차이가 존재한다. Fast DDS는 다양한 로깅 기능, 상태 모니터링 기능, 설정 분석 도구를 제공하여 대규모 시스템 운영 시 문제 분석이 용이하다.

Cyclone DDS는 구조 자체가 단순하기 때문에 상대적으로 문제 발생 가능성이 적고 디버깅이 쉬운 편이다. 최근에는 관련 도구들도 지속적으로 개선되고 있다.

Physical AI 관점에서 보면 두 DDS 모두 매우 중요한 역할을 수행하게 될 것이다. 미래의 로봇은 단순히 센서 데이터와 제어 명령만 교환하는 것이 아니라 World Model, Foundation Model 출력, 추론 결과, Semantic Map, Action Token, 협업 계획 등을 실시간으로 공유하게 된다. 따라서 DDS 미들웨어는 더 높은 성능과 더 높은 신뢰성을 요구받게 된다.

힐스로보틱스의 실내 AMR, 실외 자율주행 플랫폼, 모바일 매니퓰레이터, Fleet Management 시스템, Cargo UAV, 사족보행 로봇, 휴머노이드 로봇과 같은 아키텍처를 기준으로 보면 선택 기준은 비교적 명확하다.

대규모 Fleet, 클라우드 연동, 복잡한 보안 정책, 엔터프라이즈 환경, 높은 확장성이 중요하다면 Fast DDS가 적합하다.

낮은 지연 시간, 단순한 설정, 예측 가능한 성능, 효율적인 자원 사용이 중요하다면 Cyclone DDS가 매우 좋은 선택이 될 수 있다.

실제 ROS 2 기반 개발에서는 두 DDS 모두 충분히 산업용 시스템을 구축할 수 있는 수준에 도달해 있다. 따라서 어느 하나가 절대적으로 우수하다고 보기보다는 프로젝트의 특성과 목표에 따라 선택하는 것이 바람직하다. Fast DDS는 유연성, 확장성, 엔터프라이즈 기능을 강조하는 철학을 가지고 있으며, Cyclone DDS는 단순성, 효율성, 결정론적 성능을 강조하는 철학을 가지고 있다.

향후 Physical AI와 대규모 분산 로봇 시스템이 확대될수록 Fast DDS와 Cyclone DDS는 모두 핵심 인프라 기술로 자리 잡게 될 것이다. 두 솔루션은 분산 지능, 실시간 통신, 협업 로보틱스, 차세대 자율 시스템을 지원하는 중요한 기반 기술로 계속 발전할 것이며, DDS는 앞으로도 현대 로보틱스에서 가장 강력한 통신 표준 중 하나로 유지될 것이다.
