**Volume 09 Robotics Communication**

# Chapter 3. MQTT

## 3.1 MQTT 3.1.1 and 5.0

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

# 03_01 MQTT 3.1.1과 5.0

MQTT(Message Queuing Telemetry Transport)는 현대 IoT(Internet of Things), 산업 자동화 시스템, 클라우드 기반 로봇 플랫폼, 자율주행 시스템, 스마트 팩토리, 분산 AI 인프라에서 가장 널리 사용되는 경량 통신 프로토콜 중 하나이다. MQTT는 원래 IBM과 Eurotech가 위성 통신 및 저대역폭 원격 모니터링 환경을 위해 개발하였으며, 단순성, 확장성, 낮은 네트워크 사용량, 불안정한 네트워크 환경에 대한 높은 적응력 덕분에 사실상 IoT와 로봇 분야의 표준 프로토콜로 자리 잡았다.

힐스로보틱스의 통신 아키텍처에서도 MQTT는 플릿(Fleet) 관리, 로봇 원격 모니터링, 클라우드 연결, Edge Computing, 디지털 트윈(Digital Twin), 예지보전(Predictive Maintenance), Physical AI 시스템 연결을 위한 핵심 프로토콜로 활용될 수 있다. MQTT는 Robotics Communication 영역의 중요한 구성 요소로서 로봇과 클라우드 간의 경량 메시징 구조를 제공한다.

MQTT의 기본 철학은 전통적인 클라이언트-서버 구조와 상당히 다르다. HTTP와 같은 일반적인 프로토콜은 클라이언트가 서버에 직접 요청(Request)을 보내고 응답(Response)을 받는 구조를 사용한다. 반면 MQTT는 Broker 중심의 Publish-Subscribe 구조를 사용한다.

MQTT 환경에서는 장치들이 서로 직접 통신하지 않는다. 모든 메시지는 중앙 Broker를 통과한다. 각 장치는 필요한 정보를 특정 Topic에 발행(Publish)하고, 필요한 정보를 가진 Topic을 구독(Subscribe)한다. Broker는 이러한 메시지를 적절한 Subscriber에게 전달한다.

이 구조는 대규모 로봇 시스템에서 매우 유리하다. 로봇은 다른 장치의 IP 주소나 위치를 알 필요가 없다. 단순히 Topic에 메시지를 보내기만 하면 되며, Broker가 자동으로 전달을 수행한다. 따라서 시스템 간 결합도가 낮아지고 확장성이 크게 향상된다.

예를 들어 하나의 AMR이 배터리 상태, 위치 정보, 임무 진행 상황, 센서 상태, 진단 정보 등을 Topic에 발행한다고 가정하자. Fleet Manager, Monitoring Dashboard, Digital Twin Server, AI 분석 시스템, 유지보수 플랫폼은 각각 해당 Topic을 구독하여 필요한 정보를 수신할 수 있다. 로봇은 누가 데이터를 사용하는지 전혀 알 필요가 없다.

MQTT의 핵심 구성 요소는 Client, Broker, Topic 세 가지이다.

Client는 MQTT 네트워크에 참여하는 모든 장치를 의미한다. 로봇, 센서 게이트웨이, Edge Computer, Cloud Application, Fleet Server, Dashboard, AI Service 모두 MQTT Client가 될 수 있다.

Broker는 중앙 메시지 허브 역할을 수행한다. Publisher가 보낸 메시지를 수신하고, 해당 Topic을 구독하는 Subscriber들에게 전달한다. 대표적인 Broker로는 Eclipse Mosquitto, EMQX, HiveMQ, VerneMQ 등이 있으며 AWS, Azure, Google Cloud 역시 MQTT 서비스를 제공한다.

Topic은 메시지가 흐르는 논리적 통신 채널이다. Topic은 계층적(Hierarchical) 구조를 가진 문자열로 구성된다.

예를 들면 다음과 같은 Topic 구조를 사용할 수 있다.

이러한 계층 구조는 수백 대 이상의 로봇을 관리하는 대규모 시스템에서도 매우 효율적인 데이터 분류를 가능하게 한다.

Publisher는 Topic에 데이터를 송신하며 Subscriber는 Topic을 구독하여 데이터를 수신한다. Publisher와 Subscriber는 서로를 직접 알지 못한다. 이러한 느슨한 결합(Loose Coupling)은 MQTT의 가장 큰 장점 중 하나이다.

MQTT 3.1.1은 MQTT 프로토콜의 가장 널리 사용된 버전으로 산업 현장에서 사실상의 표준이 되었다. MQTT 3.1.1은 프로토콜 동작을 표준화하고 상호운용성을 확보함으로써 IoT와 로봇 산업의 급속한 확산을 가능하게 만들었다.

MQTT 3.1.1의 가장 큰 특징은 매우 가볍다는 점이다. 패킷 헤더 크기가 매우 작아 네트워크 사용량이 적고, CPU 및 메모리 요구사항도 낮다. 따라서 임베디드 시스템, 배터리 기반 장치, 저속 무선망, 로봇 플랫폼 등에 적합하다.

MQTT 연결은 일반적으로 TCP 연결 위에서 동작한다. Client는 Broker에 CONNECT 패킷을 전송하고 Broker는 CONNACK 패킷으로 응답한다. 연결이 수립되면 Publish, Subscribe, Unsubscribe 등의 동작이 가능해진다.

MQTT 3.1.1은 세 가지 QoS(Quality of Service) 수준을 제공한다.

QoS 0은 At Most Once 방식이다. 메시지를 한 번 전송하지만 전달 여부를 확인하지 않는다. 가장 빠르고 네트워크 사용량이 적지만 메시지 손실 가능성이 존재한다.

QoS 1은 At Least Once 방식이다. 수신 확인(Acknowledgement)을 통해 최소 한 번은 전달되도록 보장한다. 단, 중복 메시지가 발생할 수 있다.

QoS 2는 Exactly Once 방식이다. 메시지가 정확히 한 번만 전달되도록 보장한다. 가장 신뢰성이 높지만 프로토콜 오버헤드도 가장 크다.

MQTT 3.1.1은 Retained Message 기능도 제공한다. Broker는 특정 Topic의 마지막 메시지를 저장하고 있다가 새로운 Subscriber가 접속하면 즉시 전달할 수 있다.

예를 들어 Fleet Dashboard가 새로 실행되었을 때 현재 로봇의 배터리 상태와 임무 상태를 즉시 확인할 수 있다. Retained Message가 없다면 다음 상태 업데이트가 올 때까지 기다려야 한다.

또 다른 중요한 기능은 Last Will and Testament(LWT)이다.

로봇이 갑자기 전원이 꺼지거나 네트워크가 끊어지는 경우 Broker는 미리 등록된 Will Message를 자동으로 발행할 수 있다.

예를 들어 로봇이 "robot001 offline" 메시지를 LWT로 등록해 두었다면 통신이 갑자기 끊겼을 때 Fleet Management 시스템은 즉시 해당 로봇이 비정상 종료되었음을 알 수 있다.

MQTT 3.1.1은 매우 성공적이었지만 IoT와 로봇 시스템이 더욱 복잡해지면서 추가적인 기능 요구가 발생하였다. 이를 해결하기 위해 MQTT 5.0이 등장하였다.

MQTT 5.0은 완전히 새로운 프로토콜이 아니라 MQTT 3.1.1을 확장한 버전이다. 기존 Publish-Subscribe 구조는 그대로 유지하면서 진단 기능, 메타데이터 처리, 오류 보고, 확장성, 대규모 시스템 운영 기능이 크게 향상되었다.

MQTT 5.0의 가장 중요한 개선점 중 하나는 Reason Code이다.

MQTT 3.1.1에서는 연결 실패나 인증 실패 시 원인을 정확히 파악하기 어려운 경우가 많았다. MQTT 5.0은 세부적인 오류 코드를 제공하여 운영자가 문제 원인을 쉽게 분석할 수 있도록 지원한다.

이는 대규모 로봇 플릿 운영 시 매우 유용하다. 인증 실패, 권한 문제, Broker 과부하, 네트워크 오류 등을 빠르게 파악할 수 있다.

MQTT 5.0은 User Property 기능도 제공한다.

User Property는 메시지에 추가적인 메타데이터를 부착할 수 있는 기능이다.

예를 들어 로봇 소프트웨어 버전, 고객 정보, AI 모델 버전, 임무 ID, 운영 지역 등의 정보를 메시지와 함께 전달할 수 있다.

Message Expiry Interval도 MQTT 5.0에서 새롭게 추가된 기능이다.

로봇 위치 정보나 센서 데이터는 일정 시간이 지나면 가치가 없어진다. MQTT 5.0은 메시지의 유효 시간을 지정할 수 있어 오래된 정보가 자동으로 폐기된다.

Subscription Identifier 기능은 여러 Subscription을 관리하는 Client에게 매우 유용하다. 어떤 Subscription에 의해 메시지가 전달되었는지 쉽게 식별할 수 있다.

MQTT 5.0의 가장 강력한 기능 중 하나는 Shared Subscription이다.

기존 MQTT에서는 모든 Subscriber가 모든 메시지를 수신한다. 그러나 Shared Subscription을 사용하면 여러 Subscriber가 작업을 분담하여 처리할 수 있다.

예를 들어 하루 수백만 건의 로봇 Telemetry 데이터를 처리해야 하는 경우 여러 Analytics Server가 부하를 나누어 처리할 수 있다. Broker는 메시지를 자동으로 분산시켜 준다.

Request-Response 패턴도 MQTT 5.0에서 크게 향상되었다.

원래 MQTT는 비동기 Publish-Subscribe 구조에 최적화되어 있었지만 MQTT 5.0은 Response Topic과 Correlation Data를 지원하여 요청-응답 방식의 통신도 가능하게 만들었다.

이를 통해 로봇이 클라우드 서비스에 특정 정보를 요청하고 응답을 받는 구조를 MQTT만으로 구현할 수 있다.

Session Management도 개선되었다.

MQTT 5.0은 Session Expiry 기능을 제공하여 일시적인 연결 끊김 상황에서도 Subscription 상태와 세션 정보를 유지할 수 있다.

Flow Control 기능 역시 추가되었다.

Broker와 Client는 메시지 처리량과 자원 사용량을 조정할 수 있으며, 이를 통해 과부하 상황을 방지할 수 있다.

보안(Security)은 MQTT 3.1.1과 MQTT 5.0 모두에서 매우 중요한 요소이다.

MQTT 자체는 암호화를 강제하지 않지만 일반적으로 TLS 위에서 동작한다. 산업용 시스템에서는 TLS 암호화, 인증서 기반 인증, ACL(Access Control List), 역할 기반 권한 관리 등이 함께 사용된다.

로봇 시스템에서는 MQTT를 통해 임무 명령, 상태 정보, 유지보수 데이터, AI 분석 결과 등이 전달될 수 있기 때문에 보안은 필수적이다.

실제 로봇 시스템에서는 MQTT가 DDS나 ROS 2를 대체하는 것이 아니라 보완하는 역할을 수행한다.

예를 들어 ROS 2는 로봇 내부에서 실시간 센서 처리와 제어를 담당한다. 반면 MQTT는 클라우드 연결, Fleet Management, Telemetry Streaming, Remote Monitoring을 담당한다.

실내 AMR은 ROS 2 Topic을 통해 내부 센서 데이터를 처리하면서 동시에 MQTT를 통해 배터리 상태, 위치 정보, 임무 진행 상황, 진단 정보를 클라우드로 전송할 수 있다.

플릿 관리 시스템은 MQTT의 대표적인 응용 사례이다. 수백 대 또는 수천 대의 로봇이 하나의 Broker에 연결되어 상태를 보고하고 임무를 수신할 수 있다.

AWS IoT Core, Azure IoT Hub와 같은 클라우드 플랫폼도 MQTT를 기본적으로 지원한다. 따라서 로봇은 클라우드 AI, 디지털 트윈, 예지보전, ERP 시스템과 쉽게 연동될 수 있다.

Physical AI 시대에는 MQTT의 중요성이 더욱 커질 것으로 예상된다. 미래의 AI 기반 로봇은 Telemetry, AI 모델 업데이트, 추론 결과, 플릿 협업 정보, 클라우드 지능 서비스를 지속적으로 교환해야 한다.

MQTT의 경량 Publish-Subscribe 구조는 이러한 대규모 분산 AI 환경에 매우 적합하다.

힐스로보틱스의 Indoor AMR, Outdoor AMR, Security Robot, Inspection Robot, Fleet Management Platform, Mobile Manipulator, GPR Inspection Vehicle, CAD2SCAN Platform, Quadruped Robot, Humanoid Robot, 그리고 미래의 Cargo UAV 플랫폼에서도 MQTT는 로봇과 클라우드, Edge Server, AI 플랫폼, 운영 대시보드를 연결하는 핵심 통신 기술이 될 수 있다.

결론적으로 MQTT 3.1.1은 경량 Publish-Subscribe 통신 모델을 확립하여 IoT와 로봇 산업의 폭발적인 성장을 이끌었다. MQTT 5.0은 여기에 고급 진단 기능, 메타데이터 지원, 확장성 향상, 세션 관리, Shared Subscription, Message Expiry, 엔터프라이즈급 기능을 추가함으로써 차세대 자율주행 로봇과 Physical AI 플랫폼을 위한 강력한 통신 기반을 제공한다. MQTT는 앞으로도 클라우드 연결형 로봇과 대규모 AI 기반 로봇 생태계의 핵심 통신 프로토콜로 지속적으로 활용될 것이다.

## 3.2 QoS Level 0 1 2

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

QoS(Quality of Service)는 MQTT 프로토콜의 가장 중요한 기능 중 하나로, 분산 통신 시스템에서 메시지 전달의 신뢰성을 제어하는 핵심 메커니즘이다. 현대의 로봇 시스템, IoT 인프라, 산업 자동화 환경, 클라우드 기반 자율주행 플랫폼, 디지털 트윈 시스템, 그리고 Physical AI 환경에서는 통신 신뢰성이 시스템의 안전성, 운영 효율성, 그리고 임무 성공 여부에 직접적인 영향을 미친다. MQTT는 이러한 요구사항을 만족하기 위해 QoS 0, QoS 1, QoS 2라는 세 가지 서비스 품질 수준을 제공한다. 이를 통해 개발자는 네트워크 대역폭, 지연시간, CPU 부하, 메모리 사용량, 그리고 전달 신뢰성 사이에서 적절한 균형을 선택할 수 있다.

QoS가 필요한 이유는 로봇 시스템에서 교환되는 모든 데이터의 중요도가 동일하지 않기 때문이다. 어떤 데이터는 일부 손실이 발생해도 문제가 없지만, 어떤 데이터는 반드시 전달되어야 한다. 예를 들어 배터리 상태나 위치 정보와 같은 주기적인 상태 데이터는 일부 메시지가 손실되어도 다음 데이터가 곧 도착하기 때문에 큰 문제가 없다. 반면 비상 정지(E-Stop) 명령, 치명적인 장애 알람, 임무 완료 보고와 같은 정보는 반드시 전달되어야 한다. MQTT의 QoS는 이러한 다양한 요구사항을 지원하기 위해 설계되었다.

QoS는 메시지 단위로 적용된다. Publisher는 메시지를 전송할 때 QoS 수준을 지정할 수 있으며, Subscriber 역시 구독 시 원하는 QoS 수준을 설정할 수 있다. Broker는 이 설정에 따라 메시지를 전달하고 신뢰성을 보장한다.

QoS Level 0은 "At Most Once" 방식으로 불린다. 이는 MQTT에서 가장 단순하고 빠른 메시지 전달 방식이다. Publisher는 메시지를 Broker로 전송한 후 전달 여부를 확인하지 않는다. Broker 역시 수신 확인 응답을 보내지 않는다. 메시지는 단순히 한 번 전송되고 그 과정이 종료된다.

QoS 0은 흔히 "Fire and Forget" 방식이라고도 불린다. 메시지를 보내고 잊어버리는 구조이다. 따라서 가장 적은 네트워크 자원을 사용하며 가장 낮은 지연시간을 제공한다.

QoS 0은 확인 응답이 필요하지 않기 때문에 네트워크 트래픽이 최소화된다. CPU 부하도 매우 낮으며 Broker 역시 메시지 상태를 추적할 필요가 없다. 따라서 대량의 데이터가 지속적으로 발생하는 환경에 적합하다.

예를 들어 AMR이 초당 10회 위치 정보를 전송한다고 가정하자. 만약 한 번의 위치 데이터가 손실되더라도 100ms 후에 새로운 위치 데이터가 전송된다. 이 경우 이전 데이터는 이미 가치가 없기 때문에 재전송이 필요하지 않다.

이러한 이유로 위치 정보, IMU 데이터, Wheel Encoder 데이터, 환경 센서 값, 온도 데이터, CPU 사용률, 네트워크 상태 정보와 같은 고주기 Telemetry 데이터는 일반적으로 QoS 0을 사용한다.

QoS 0의 가장 큰 장점은 매우 낮은 지연시간과 높은 처리량이다. 또한 Broker가 관리해야 하는 상태 정보가 없기 때문에 수천 대 이상의 로봇이 연결되는 대규모 시스템에서도 효율적으로 동작할 수 있다.

반면 QoS 0은 신뢰성을 보장하지 않는다. 네트워크 장애, Broker 과부하, 무선 간섭, 클라이언트 연결 끊김 등의 상황이 발생하면 메시지가 손실될 수 있다. 또한 Publisher는 메시지가 실제로 전달되었는지 확인할 방법이 없다.

따라서 QoS 0은 일부 메시지 손실이 허용되는 경우에만 사용하는 것이 바람직하다.

QoS Level 1은 "At Least Once" 전달 방식을 제공한다. 이 방식에서는 Publisher가 메시지를 전송한 후 Broker로부터 PUBACK(Packet Acknowledgement)를 수신해야 한다.

Broker가 메시지를 정상적으로 수신하면 PUBACK 패킷을 Publisher에게 전송한다. Publisher는 PUBACK를 수신한 시점에 메시지 전달이 완료되었다고 판단한다.

만약 PUBACK가 일정 시간 안에 도착하지 않으면 Publisher는 메시지가 손실되었다고 가정하고 동일한 메시지를 다시 전송한다.

이러한 구조는 메시지 손실을 방지하는 효과가 있다. 하지만 QoS 1은 "최소 한 번 전달"을 보장할 뿐 "정확히 한 번 전달"을 보장하지는 않는다.

예를 들어 Broker는 메시지를 정상적으로 수신했지만 PUBACK 패킷이 네트워크 장애로 인해 손실되었다고 가정해 보자. Publisher는 PUBACK를 받지 못했기 때문에 메시지를 다시 전송한다. 결과적으로 Broker는 동일한 메시지를 두 번 받게 된다.

즉 QoS 1에서는 중복 메시지(Duplicate Message)가 발생할 수 있다.

그러나 MQTT는 QoS 1에서 메시지 유실을 방지하는 것을 더 중요하게 생각한다. 따라서 중복은 허용하되 전달 실패는 허용하지 않는 방식이다.

이러한 특성 때문에 QoS 1은 산업용 로봇 시스템에서 가장 널리 사용되는 QoS 수준이다.

예를 들어 배터리 부족 경고, 임무 상태 업데이트, 유지보수 요청, 오류 보고, Fleet 명령, 검사 결과 알림 등의 데이터는 반드시 전달되어야 하지만 중복으로 수신되어도 큰 문제가 되지 않는다.

배터리 부족 경고를 두 번 받는 것은 크게 문제가 되지 않지만, 경고를 전혀 받지 못하는 것은 심각한 문제를 일으킬 수 있다. 이러한 경우 QoS 1이 적합하다.

QoS 1은 QoS 0보다 약간 더 많은 네트워크 대역폭과 메모리를 사용한다. ACK 패킷을 주고받아야 하며 Broker와 Client가 메시지 상태를 관리해야 하기 때문이다.

그럼에도 불구하고 신뢰성과 효율성의 균형이 우수하기 때문에 실제 산업용 IoT와 로봇 시스템에서는 가장 많이 사용되는 QoS 레벨이라고 할 수 있다.

QoS Level 2는 MQTT에서 제공하는 가장 높은 수준의 신뢰성이다. QoS 2는 "Exactly Once" 전달을 보장한다.

즉 메시지가 반드시 전달되며 동시에 중복도 발생하지 않는다.

이를 위해 QoS 2는 보다 복잡한 4단계 핸드셰이크(Handshake)를 사용한다.

먼저 Publisher가 PUBLISH 패킷을 전송한다. Broker는 PUBREC(Packet Received)을 반환한다. Publisher는 PUBREL(Packet Release)을 전송한다. 마지막으로 Broker는 PUBCOMP(Packet Complete)를 반환한다.

이 네 단계의 절차를 통해 Publisher와 Broker는 메시지 상태를 정확하게 추적할 수 있으며 중복 전송 여부를 관리할 수 있다.

결과적으로 QoS 2는 메시지가 정확히 한 번만 처리되도록 보장한다.

이러한 특성은 중복 메시지가 심각한 문제를 일으킬 수 있는 경우에 매우 중요하다.

예를 들어 창고 로봇이 물품 배송 완료를 보고한다고 가정하자. 동일한 완료 메시지가 두 번 처리되면 재고 관리 시스템에 오류가 발생할 수 있다.

또 다른 예로 검사 로봇이 결함 보고서를 업로드하는 경우를 생각해 볼 수 있다. 동일한 결함 보고가 두 번 처리되면 불필요한 유지보수 작업이 생성될 수 있다.

금융 거래, 생산 실적 보고, ERP 업데이트, MES 연동, 유지보수 작업 생성, 임무 완료 기록과 같은 데이터는 QoS 2가 적합한 대표적인 사례이다.

QoS 2는 가장 강력한 신뢰성을 제공하지만 그만큼 비용도 크다.

4단계 핸드셰이크로 인해 지연시간이 증가한다. 추가 패킷 전송 때문에 네트워크 사용량도 증가한다. Broker와 Client는 더 많은 상태 정보를 저장해야 하므로 메모리 사용량도 증가한다. CPU 부하 역시 높아진다.

따라서 QoS 2는 반드시 필요한 경우에만 사용하는 것이 바람직하다.

고주기 센서 데이터에 QoS 2를 사용하는 것은 일반적으로 비효율적이다. 위치 정보나 카메라 상태 정보가 정확히 한 번만 전달될 필요는 없기 때문이다.

세 가지 QoS를 비교해 보면 각기 다른 특성을 가진다.

QoS 0은 속도와 효율성을 최우선으로 하며 메시지 손실을 허용한다.

QoS 1은 신뢰성을 우선하며 중복을 허용한다.

QoS 2는 신뢰성과 중복 방지를 모두 제공하지만 가장 높은 오버헤드를 가진다.

따라서 항상 가장 높은 QoS를 사용하는 것이 최선은 아니다. 데이터의 중요도에 따라 적절한 QoS를 선택하는 것이 중요하다.

일반적인 AMR 시스템에서는 Localization 데이터, IMU 데이터, Wheel Odometry, Telemetry 데이터는 QoS 0을 사용한다.

배터리 경고, Fleet 명령, 충전 요청, 유지보수 알림, 장애 보고는 QoS 1을 사용한다.

임무 완료 보고, ERP 연동 데이터, 생산 실적 기록, 재고 관리 데이터, 감사(Audit) 기록은 QoS 2를 사용하는 것이 일반적이다.

MQTT Broker는 QoS 구현의 핵심 역할을 담당한다. Broker는 메시지 상태를 저장하고 ACK를 처리하며 재전송을 관리한다. QoS 수준이 높아질수록 Broker가 관리해야 하는 정보도 증가한다.

수천 대의 로봇이 연결된 Fleet 환경에서는 하루 수백만 건의 메시지가 발생할 수 있다. 따라서 모든 데이터를 QoS 2로 처리하는 것은 비효율적이며 시스템 확장성을 저하시킬 수 있다.

MQTT 5.0은 QoS 기능을 더욱 강화하였다. 향상된 Reason Code를 통해 ACK 실패 원인을 정확히 분석할 수 있으며 Session Management 개선을 통해 일시적인 네트워크 끊김 이후에도 안정적으로 메시지 전달을 유지할 수 있다. 또한 Flow Control 기능을 통해 Broker 과부하를 방지할 수 있다.

보안 역시 QoS와 밀접하게 관련된다. TLS 암호화, 인증서 기반 인증, ACL(Access Control List), 사용자 권한 관리 등을 함께 적용하면 메시지의 신뢰성과 무결성을 더욱 강화할 수 있다.

실제 로봇 시스템에서는 여러 QoS를 동시에 사용하는 경우가 대부분이다. 실내 AMR은 위치 정보와 Telemetry는 QoS 0으로 전송하고, Fleet 상태 보고는 QoS 1로 처리하며, 임무 완료 기록은 QoS 2로 전송할 수 있다.

실외 자율주행 차량, 검사 로봇, 보안 로봇, 모바일 매니퓰레이터, 그리고 미래의 Physical AI 플랫폼 역시 동일한 접근 방식을 사용하게 된다. 대용량 센서 데이터는 QoS 0을 사용하고 운영 정보는 QoS 1을 사용하며, 거래성(Transaction) 데이터는 QoS 2를 사용하는 방식이다.

힐스로보틱스의 Indoor AMR, Outdoor AMR, Inspection Robot, Security Robot, Fleet Management System, Mobile Manipulator, CAD2SCAN Platform, GPR Inspection Vehicle, Quadruped Robot, Humanoid Robot, 그리고 미래의 Cargo UAV 플랫폼에서도 QoS 선택은 통신 아키텍처 설계의 핵심 요소가 된다. 고주기 Telemetry는 QoS 0, Fleet 운영 데이터는 QoS 1, 임무 완료 및 ERP 연계 데이터는 QoS 2를 사용하는 방식이 일반적인 설계 방향이 될 수 있다.

결론적으로 MQTT의 QoS 0, QoS 1, QoS 2는 통신 신뢰성을 상황에 맞게 조절할 수 있는 매우 강력한 메커니즘이다. QoS 0은 최대 성능을 제공하는 At Most Once 방식이며, QoS 1은 신뢰성을 보장하는 At Least Once 방식이다. QoS 2는 가장 높은 수준의 안정성을 제공하는 Exactly Once 방식이다. 이 세 가지 QoS 수준은 성능, 확장성, 네트워크 사용량, 신뢰성 사이에서 최적의 균형을 제공하며, 현대 로봇 시스템과 미래 Physical AI 플랫폼의 핵심 통신 기술로 활용되고 있다.

## 3.3 Retain and Last Will Testament

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

# 03_03 Retain Message와 Last Will Testament

Retain Message와 Last Will Testament(LWT)는 MQTT 프로토콜이 제공하는 가장 강력하고 실용적인 기능 중 두 가지이다. MQTT는 일반적으로 경량 Publish-Subscribe 통신 프로토콜로 알려져 있지만, 실제 산업 자동화, 로봇 시스템, 플릿 관리, 클라우드 연결, 디지털 트윈, 그리고 Physical AI 플랫폼에서 MQTT의 진정한 가치는 이러한 고급 운영 기능을 통해 나타난다. Retain Message는 새롭게 연결된 Subscriber가 즉시 최신 상태 정보를 받을 수 있도록 지원하며, Last Will Testament는 장치가 예기치 않게 네트워크에서 사라졌을 때 자동으로 장애 알림을 생성한다. 이 두 기능은 분산 로봇 시스템의 신뢰성, 가시성(Visibility), 장애 감지 능력, 운영 연속성, 그리고 시스템 인지 능력을 크게 향상시킨다.

현대 로봇 시스템은 매우 동적인 환경에서 운영된다. AMR, 실외 자율주행 차량, 모바일 매니퓰레이터, 검사 로봇, 보안 로봇, 창고 자동화 시스템, 그리고 미래의 Physical AI 플랫폼은 클라우드 서비스, Edge Computer, Fleet Management Server, Monitoring Dashboard, Digital Twin 플랫폼과 지속적으로 연결되고 분리된다. 이러한 환경에서는 현재 시스템 상태를 정확하게 파악하는 것이 매우 중요하다.

일반적인 Publish-Subscribe 구조에서는 Subscriber가 메시지가 발행되는 순간 연결되어 있어야만 해당 메시지를 수신할 수 있다. Subscriber가 나중에 접속하면 과거에 발행된 정보는 받을 수 없다. 실시간 Telemetry 데이터에서는 이러한 동작이 문제가 되지 않을 수 있지만, 현재 상태를 즉시 알아야 하는 시스템에서는 상당한 제약이 된다.

Retain Message는 이러한 문제를 해결하기 위해 만들어졌다. Retain Message는 일반 MQTT 메시지와 동일하지만 Retain Flag가 설정되어 있다는 점이 다르다. Publisher가 Retain Flag를 활성화하여 메시지를 전송하면 Broker는 해당 Topic의 최신 메시지를 저장한다. 이후 새로운 Subscriber가 해당 Topic을 구독하면 Broker는 즉시 저장된 메시지를 전달한다.

즉 Retain Message는 Topic의 최신 상태 스냅샷 역할을 수행한다. Subscriber는 다음 업데이트를 기다릴 필요 없이 현재 상태를 즉시 확인할 수 있다.

예를 들어 수백 대의 AMR을 운영하는 Fleet Management 시스템을 생각해 보자. 각 로봇은 배터리 상태를 fleet/robot001/battery와 같은 Topic에 발행한다. 만약 이 메시지가 Retain Message로 저장되어 있다면 새롭게 접속한 Dashboard는 즉시 현재 배터리 상태를 확인할 수 있다.

Retain Message가 없다면 Dashboard는 다음 배터리 업데이트가 발생할 때까지 기다려야 한다. 업데이트 주기가 길다면 수 초 또는 수 분 동안 상태를 확인하지 못할 수도 있다. Retain Message는 이러한 문제를 제거한다.

Retain Message의 효과는 시스템 시작 과정에서 더욱 명확하게 나타난다. 대규모 로봇 시스템에서는 모든 서비스가 동시에 시작되지 않는다. Cloud Service가 재부팅될 수도 있고, Dashboard가 재접속할 수도 있으며, Digital Twin 시스템이 유지보수 후 다시 시작될 수도 있다.

이러한 상황에서 Retain Message는 새롭게 연결된 서비스가 즉시 현재 상태를 파악할 수 있도록 해준다. 로봇의 가용성, 배터리 상태, 운영 모드, 현재 임무, 위치 추정 상태, 장애 정보, 충전 상태, 소프트웨어 버전 등의 정보를 즉시 획득할 수 있다.

Retain Message는 상태(State)를 표현하는 정보에 매우 적합하다. Subscriber는 일반적으로 과거의 모든 상태 변화 기록보다는 현재 상태를 알고 싶어 하기 때문이다.

예를 들어 운영 상태, 현재 위치, 진행 중인 임무 ID, 충전소 할당 정보, 플릿 설정 정보, 네트워크 연결 상태, 유지보수 스케줄, 소프트웨어 버전, AI 모델 버전 등은 Retain Message로 관리하기에 적합하다.

반면 모든 데이터가 Retain Message에 적합한 것은 아니다. 카메라 영상, LiDAR Point Cloud, IMU 데이터, Wheel Encoder 데이터, 고주기 Telemetry와 같은 정보는 매우 빠르게 가치가 사라진다. 이러한 데이터를 Retain Message로 저장하는 것은 일반적으로 의미가 없다.

Broker는 Topic당 하나의 Retain Message만 저장한다. 새로운 Retain Message가 발행되면 기존 Retain Message는 자동으로 교체된다. 따라서 Broker는 항상 최신 상태만 보관하게 된다.

Retain Message는 삭제도 가능하다. Publisher가 빈 Payload를 가진 Retain Message를 전송하면 Broker는 해당 Topic에 저장된 Retain Message를 삭제한다.

이는 로봇이 영구적으로 제거되거나 서비스가 종료될 때 오래된 상태 정보가 남아 있는 것을 방지하는 데 유용하다.

Retain Message는 Digital Twin 시스템에서도 매우 중요한 역할을 한다. Digital Twin은 실제 시스템의 현재 상태를 정확하게 반영해야 한다. 재시작 후에도 Retain Message를 통해 현재 상태를 빠르게 복원할 수 있기 때문이다.

Retain Message가 현재 상태를 유지하는 기능이라면 Last Will Testament는 장애 감지(Failure Detection)를 담당한다.

분산 로봇 시스템에서는 네트워크 장애, 전원 차단, 소프트웨어 충돌, 하드웨어 고장, 무선 통신 장애가 언제든지 발생할 수 있다. 이러한 장애를 빠르게 감지하는 것은 안전한 운영을 위해 매우 중요하다.

명확한 장애 감지 메커니즘이 없다면 장치는 단순히 네트워크에서 사라질 뿐이다. 다른 시스템은 해당 장치가 아직 정상적으로 동작하고 있다고 오해할 수 있으며, 이는 운영상의 위험을 초래할 수 있다.

Last Will Testament는 이러한 문제를 매우 우아하게 해결한다.

MQTT Client가 Broker에 연결할 때 Will Message를 등록할 수 있다. 이 메시지는 Broker 내부에 저장되지만 즉시 발행되지는 않는다.

정상적인 경우 Client는 MQTT DISCONNECT 패킷을 전송한 후 연결을 종료한다. 이 경우 Broker는 Will Message를 삭제하고 아무런 메시지도 발행하지 않는다.

그러나 전원 장애, 네트워크 단절, 소프트웨어 충돌, 운영체제 오류, 통신 타임아웃 등으로 인해 Client가 비정상적으로 사라지면 Broker는 자동으로 Will Message를 발행한다.

즉 LWT는 자동 장애 알림 시스템이라고 볼 수 있다.

예를 들어 창고에서 운행 중인 AMR이 연결 시점에 fleet/robot001/status Topic에 "robot001 offline"이라는 Will Message를 등록했다고 가정하자.

만약 로봇이 갑자기 전원을 잃고 통신이 끊어진다면 Broker는 자동으로 해당 메시지를 발행한다.

Fleet Management System, Dashboard, Digital Twin, Maintenance Platform, Monitoring System은 즉시 해당 로봇이 오프라인 상태가 되었음을 인지할 수 있다.

LWT의 핵심은 정상 종료와 비정상 종료를 구분한다는 점이다.

정상적으로 종료되면 DISCONNECT 패킷이 전송되므로 Will Message는 발행되지 않는다.

반면 예기치 않은 장애가 발생하면 Broker가 Will Message를 발행한다.

이 구조는 불필요한 오탐(False Alarm)을 줄이면서 실제 장애를 신속하게 감지할 수 있도록 해준다.

LWT의 가치는 시스템 규모가 커질수록 더욱 증가한다. 수백 대 또는 수천 대의 로봇을 운영하는 환경에서는 모든 연결 상태를 수동으로 확인하는 것이 사실상 불가능하다.

LWT는 중앙 모니터링 시스템이 전체 플릿 상태를 자동으로 관리할 수 있도록 해준다.

또한 LWT는 계층형 모니터링 구조를 구성할 수도 있다. 개별 로봇은 자신의 상태를 보고하고, Edge Gateway는 네트워크 상태를 보고하며, Fleet Server는 서비스 가용성을 보고할 수 있다.

이를 통해 전체 로봇 생태계에 대한 종합적인 운영 가시성을 확보할 수 있다.

실제 시스템에서는 Retain Message와 LWT를 함께 사용하는 경우가 많다.

예를 들어 로봇이 연결될 때 "online" 상태를 Retain Message로 발행하고, 동시에 LWT로 "offline" 메시지를 등록할 수 있다.

새로운 Subscriber는 즉시 Retain Message를 받아 로봇이 온라인 상태임을 확인할 수 있다.

이후 로봇이 예기치 않게 연결이 끊어지면 Broker가 자동으로 Offline 메시지를 발행한다.

결과적으로 모든 시스템은 별도의 Heartbeat 프로토콜 없이도 로봇의 연결 상태를 정확하게 파악할 수 있다.

Broker는 Will Message를 일반 MQTT 메시지와 동일하게 처리한다. 따라서 QoS 0, QoS 1, QoS 2를 사용할 수 있으며 필요하다면 Retain Message로도 발행할 수 있다.

이를 통해 안전성이 중요한 환경에서는 높은 QoS를 사용하여 장애 알림의 신뢰성을 보장할 수 있다.

산업 자동화 시스템에서는 LWT가 Supervisory Monitoring의 핵심 기능으로 사용된다. 생산 설비, 컨베이어 시스템, 검사 장비, 자율주행 차량, Edge Computer 등이 모두 LWT를 활용하여 장애 상태를 보고할 수 있다.

실외 자율주행 차량이나 원격 검사 로봇과 같이 접근이 어려운 환경에서는 LWT의 가치가 더욱 커진다. 운영자는 현장에 가지 않고도 시스템 상태를 즉시 확인할 수 있다.

클라우드 기반 로봇 시스템에서도 Retain Message와 LWT는 함께 사용된다. Retain Message는 현재 상태를 유지하고, LWT는 장애를 보고한다. 이 조합은 매우 강력한 상태 동기화 메커니즘을 제공한다.

MQTT 5.0에서는 이러한 기능들이 더욱 강화되었다. Session Management, Message Expiry, 향상된 진단 기능, 추가 메타데이터 지원 등이 제공되어 Retain Message와 LWT를 보다 효율적으로 관리할 수 있다.

예를 들어 Message Expiry 기능을 사용하면 오래된 Retain Message가 무한정 남아 있는 것을 방지할 수 있다.

보안 역시 매우 중요하다. Retain Message와 LWT에는 배터리 상태, 장애 정보, 운영 위치, 임무 상태, 소프트웨어 버전과 같은 민감한 운영 정보가 포함될 수 있다.

따라서 TLS 암호화, 인증서 기반 인증, ACL, 역할 기반 권한 관리 등을 통해 이러한 정보가 보호되어야 한다.

ROS 2 기반 로봇 시스템에서는 MQTT의 Retain Message와 LWT가 DDS 기반 실시간 통신을 보완하는 역할을 수행한다. DDS는 로봇 내부 통신을 담당하고 MQTT는 플릿 수준의 상태 관리와 클라우드 연동을 담당한다.

힐스로보틱스의 Indoor AMR, Outdoor AMR, Security Robot, Inspection Robot, Fleet Management System, Mobile Manipulator, CAD2SCAN Platform, GPR Inspection Vehicle, Quadruped Robot, Humanoid Robot, 그리고 미래의 Cargo UAV 플랫폼에서도 Retain Message와 LWT는 매우 중요한 운영 기능이 된다. Retain Message는 배터리 상태, 임무 상태, 충전 상태, 소프트웨어 버전 등의 최신 정보를 즉시 제공하며, LWT는 전원 장애, 네트워크 단절, 시스템 오류를 자동으로 감지하고 보고할 수 있다.

결론적으로 Retain Message와 Last Will Testament는 MQTT를 단순한 메시징 프로토콜에서 강력한 운영 관리 플랫폼으로 발전시키는 핵심 기능이다. Retain Message는 현재 상태를 보존하고 새로운 Subscriber에게 즉시 제공하며, LWT는 예상치 못한 장애를 자동으로 감지하여 시스템 전체에 알린다. 이 두 기능은 플릿 관리, 디지털 트윈, 클라우드 통합, 예지보전, 자율운영 시스템, 그리고 미래의 Physical AI 인프라를 구성하는 핵심 기반 기술로 계속 활용될 것이다.

## 3.4 MQTT Broker Design

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

MQTT Broker 설계는 현대 분산 통신 시스템에서 가장 중요한 아키텍처 주제 중 하나이다. Broker는 MQTT 생태계 전체의 중앙 메시지 허브이자 통신 제어 중심 역할을 수행하기 때문이다. 로봇, 센서, 게이트웨이, 클라우드 서비스, 모바일 애플리케이션, 디지털 트윈, 플릿 관리 시스템, AI 플랫폼과 같은 MQTT Client들은 데이터를 생성하고 소비하지만, 실제 메시지 교환, 연결 상태 관리, 구독 관리, 보안 정책, 세션 유지, 신뢰성 보장, 통신 흐름 제어는 모두 Broker가 담당한다. 대규모 로봇 시스템에서는 Broker 아키텍처의 품질이 전체 시스템의 확장성, 신뢰성, 응답성, 유지보수성, 장기 운영 안정성을 결정하는 핵심 요소가 된다.

MQTT는 Broker 중심의 Publish-Subscribe 구조를 사용한다. Peer-to-Peer 통신 방식과 달리 MQTT Client들은 서로 직접 통신하지 않는다. 모든 메시지는 Broker를 통해 전달된다. Publisher는 Broker에 메시지를 전송하고 Subscriber는 Broker로부터 메시지를 수신한다. 이러한 구조는 시스템 간 결합도를 크게 낮추고 대규모 확장을 가능하게 한다.

Broker의 가장 기본적인 역할은 메시지 라우팅(Message Routing)이다. MQTT 메시지는 Topic을 기반으로 전달된다. Broker는 어떤 Subscriber가 어떤 Topic을 구독하고 있는지 관리하며, 메시지가 도착하면 적절한 Subscriber에게 전달한다.

소규모 시스템에서는 수백 개 수준의 Topic만 존재할 수 있지만, 대규모 산업용 로봇 시스템에서는 수십만 개에서 수백만 개 이상의 Topic이 존재할 수 있다. 따라서 Broker는 Topic 검색과 구독 매칭을 매우 효율적으로 수행해야 한다.

현대 MQTT Broker는 Trie Tree, Topic Tree, Hierarchical Index, Memory Optimized Lookup Table과 같은 고성능 자료구조를 사용하여 Topic 검색을 최적화한다. 이를 통해 초당 수백만 건 이상의 메시지를 처리하면서도 낮은 지연시간을 유지할 수 있다.

Broker의 또 다른 중요한 역할은 Connection Management이다. 모든 MQTT Client는 Broker에 연결되며, Broker는 연결 상태를 지속적으로 관리한다.

Broker는 Client ID, 인증 정보, 연결 상태, Keep Alive 타이머, Subscription 정보, Session 상태, QoS 상태 등을 유지한다.

예를 들어 10,000대 규모의 AMR 플릿을 운영한다면 Broker는 10,000개의 동시 연결을 관리해야 한다. 따라서 효율적인 Connection Management 알고리즘은 확장성 확보에 매우 중요하다.

Session Persistence 역시 Broker 설계의 핵심 요소이다. MQTT는 Client가 일시적으로 네트워크 연결을 잃더라도 Subscription 정보와 세션 상태를 유지할 수 있도록 지원한다.

실외 자율주행 차량, 창고 로봇, 검사 로봇과 같이 이동 중인 장치는 일시적으로 무선 통신이 불안정해질 수 있다. Session Persistence는 이러한 환경에서도 통신 연속성을 보장한다.

Broker는 다양한 종류의 데이터를 저장해야 한다. 대표적으로 Retained Message, Persistent Session, Offline Message Queue, QoS 상태 정보, Client Metadata 등이 있다.

Retained Message 기능을 사용할 경우 Broker는 각 Topic의 최신 상태를 저장한다. 새로운 Subscriber가 연결되면 Broker는 즉시 해당 상태를 전달할 수 있다.

플릿 관리 시스템에서는 수천 대의 로봇이 상태 정보를 지속적으로 발행하기 때문에 Retained Message 저장 구조의 효율성이 매우 중요하다.

QoS 구현은 MQTT Broker 설계에서 가장 복잡한 기능 중 하나이다.

QoS 0은 별도의 상태 관리가 필요하지 않다.

QoS 1은 ACK(PUBACK)를 관리해야 하며 재전송 기능도 필요하다.

QoS 2는 PUBREC, PUBREL, PUBCOMP까지 포함하는 트랜잭션 상태를 관리해야 한다.

QoS 수준이 높아질수록 Broker는 더 많은 상태 정보를 저장해야 하며 메모리 사용량과 CPU 부하도 증가한다.

따라서 Broker 설계 시 QoS 1과 QoS 2 트래픽을 얼마나 처리할 것인지에 대한 고려가 매우 중요하다.

Broker는 Last Will and Testament(LWT) 기능도 담당한다.

Client가 연결될 때 Will Message를 등록하면 Broker는 해당 정보를 저장한다. 이후 Client가 정상적으로 DISCONNECT 하지 않고 갑자기 사라지면 Broker는 자동으로 Will Message를 발행한다.

이를 통해 Broker는 단순한 메시지 전달 장치를 넘어 분산 장애 감지 시스템 역할까지 수행하게 된다.

플릿 관리 시스템은 Broker를 통해 로봇 장애, 게이트웨이 장애, 클라우드 연결 장애 등을 즉시 감지할 수 있다.

보안(Security)은 MQTT Broker 설계에서 가장 중요한 요소 중 하나이다.

Broker는 모든 통신의 중심이기 때문에 공격자의 주요 목표가 된다. 만약 Broker가 침해되면 플릿 전체의 상태 정보가 노출될 수 있으며 악의적인 명령이 전달될 수도 있다.

따라서 강력한 인증(Authentication) 메커니즘이 필요하다.

일반적으로 Username/Password 인증, Certificate 기반 인증, Token 인증, OAuth, Enterprise Identity System, Hardware Security Module(HSM) 등이 사용된다.

Authorization은 인증 이후 어떤 Topic에 접근할 수 있는지를 제어한다.

예를 들어 로봇은 자신의 상태 Topic에만 Publish할 수 있어야 하며, Fleet Manager는 상태 Topic을 읽을 수 있어야 하지만 관리용 Topic만 수정할 수 있도록 제한할 수 있다.

이러한 세밀한 접근 제어는 시스템 보안성을 크게 향상시킨다.

암호화 역시 필수 요소이다.

MQTT는 일반적으로 TLS 위에서 동작하며 데이터의 기밀성, 무결성, 인증을 보장한다.

현대 산업용 MQTT Broker는 대부분 TLS를 기본적으로 지원하며 외부 연결 시 TLS 사용을 강제하는 경우가 많다.

확장성(Scalability)은 성공적인 MQTT Broker 설계의 핵심 목표 중 하나이다.

소규모 환경에서는 단일 Broker로 충분할 수 있지만, 대규모 로봇 시스템에서는 수만 대 이상의 장치가 동시에 연결될 수 있다.

이 경우 Broker Clustering이 사용된다.

Broker Cluster는 여러 Broker 서버가 하나의 논리적 Broker처럼 동작하는 구조이다.

메시지, Subscription, Session 정보, Retained Message가 Cluster 전체에 분산되어 저장된다.

Clustering은 확장성뿐 아니라 장애 대응 능력도 향상시킨다.

하나의 Broker 서버가 장애를 일으켜도 다른 Broker가 서비스를 계속 제공할 수 있기 때문이다.

Load Balancer는 신규 연결을 여러 Broker 노드에 분산시켜 병목 현상을 방지한다.

고가용성(High Availability) 역시 중요한 설계 요소이다.

산업용 MQTT 환경에서는 이중 네트워크, 이중 저장장치, 다중 Broker Cluster, 자동 Failover, Backup System, Disaster Recovery 체계가 함께 구축되는 경우가 많다.

이러한 구조는 24시간 운영되는 산업 현장에서 매우 중요하다.

성능 최적화 역시 Broker 설계의 중요한 부분이다.

Broker는 메모리 사용량, CPU 사용량, 네트워크 대역폭, 디스크 I/O, 메시지 지연시간을 균형 있게 관리해야 한다.

메모리 관리 측면에서는 Session 정보, Subscription, Retained Message, QoS 상태 정보 등이 주요 자원 소비 요소이다.

CPU 사용량은 Topic Matching, 암호화 처리, 메시지 라우팅, 인증 처리 등에 의해 결정된다.

특히 수천 대 이상의 로봇이 초당 Telemetry 데이터를 전송하는 경우 Broker의 처리 성능이 매우 중요해진다.

Monitoring과 Observability도 현대 Broker 아키텍처의 필수 요소이다.

운영자는 현재 연결 수, 메시지 처리량, Subscription 수, 메모리 사용량, CPU 사용량, 네트워크 트래픽, 저장소 사용량, 오류율, 인증 실패 횟수 등을 실시간으로 모니터링할 수 있어야 한다.

대부분의 엔터프라이즈 MQTT Broker는 Prometheus, Grafana, OpenTelemetry, Elastic Stack과 연동되어 이러한 정보를 제공한다.

클라우드 통합은 최근 MQTT Broker 설계에서 더욱 중요해지고 있다.

현대 로봇 시스템은 AI 분석, 디지털 트윈, 예지보전, Fleet Coordination, ERP 통합 등을 위해 클라우드와 지속적으로 연결된다.

이를 위해 Docker, Kubernetes 기반의 Cloud-Native MQTT Broker 아키텍처가 널리 사용된다.

Edge Computing 환경에서는 Edge Broker 개념도 중요하다.

공장, 병원, 물류센터, 공항, 항만, 광산 내부에는 Edge MQTT Broker를 배치하여 저지연 통신을 제공하고, 필요한 정보만 중앙 Cloud Broker로 전달하는 구조가 사용된다.

이를 계층형 Broker Architecture라고 부른다.

Edge Broker는 현장 통신을 담당하고 Cloud Broker는 기업 전체의 통합 관리를 담당한다.

이 구조는 네트워크 의존성을 줄이면서도 전체 시스템의 가시성을 유지할 수 있다.

디지털 트윈 시스템 역시 MQTT Broker에 크게 의존한다.

실시간 상태 동기화, 운영 모니터링, AI 분석, 유지보수 예측, 시뮬레이션 업데이트, 플릿 시각화 모두 Broker를 통해 수행된다.

Broker는 실제 로봇과 디지털 세계를 연결하는 신경망 역할을 수행한다고 볼 수 있다.

Physical AI 시대에는 Broker의 중요성이 더욱 증가할 것이다.

미래의 AI 기반 로봇은 Telemetry, AI 모델 업데이트, 추론 결과, 계획 정보, 멀티모달 센서 데이터, 플릿 협업 정보 등을 지속적으로 교환해야 한다.

MQTT Broker는 이러한 방대한 데이터 흐름을 안정적으로 처리할 수 있어야 한다.

ROS 2 환경에서는 MQTT Broker가 DDS를 보완하는 역할을 수행한다.

DDS는 로봇 내부의 실시간 통신을 담당하고, MQTT Broker는 클라우드 연결, 플릿 관리, 원격 모니터링, 다중 사이트 통합을 담당한다.

이러한 계층형 통신 구조는 각 기술의 장점을 최대한 활용할 수 있게 해준다.

힐스로보틱스의 Indoor AMR, Outdoor AMR, Inspection Robot, Security Robot, Mobile Manipulator, Fleet Management System, CAD2SCAN Platform, GPR Inspection Vehicle, Quadruped Robot, Humanoid Robot, 그리고 향후 Cargo UAV 플랫폼에서는 MQTT Broker가 플릿 수준의 통신 인프라 중심 역할을 수행하게 된다.

일반적인 힐스로보틱스 아키텍처에서는 각 공장이나 물류센터에 Edge MQTT Broker를 배치하고, 이를 중앙 Cloud Broker와 연결하는 계층형 구조를 사용할 수 있다.

배터리 상태, 임무 진행 상황, AI 분석 결과, 유지보수 알람, 소프트웨어 업데이트, 운영 통계 등이 이 Broker 계층 구조를 통해 전달된다.

결론적으로 MQTT Broker는 단순한 메시지 전달 서버가 아니다. Broker는 통신 제어기, 세션 관리자, 보안 게이트웨이, 신뢰성 엔진, 장애 감지 시스템, 확장성 기반 플랫폼, 모니터링 허브, 그리고 분산 로봇 생태계의 통합 플랫폼 역할을 수행한다. 로봇 플릿과 Physical AI 시스템이 더욱 대규모화되고 지능화될수록 MQTT Broker는 현대 자율주행 및 로봇 인프라의 핵심 기반 기술로서 더욱 중요한 위치를 차지하게 될 것이다.

## 3.5 MQTT in AMR Telemetry

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

# 03_05 AMR Telemetry에서의 MQTT 활용

MQTT는 현대 자율주행 이동로봇(AMR, Autonomous Mobile Robot) 시스템에서 가장 중요한 통신 기술 중 하나로 자리 잡고 있다. MQTT는 로봇, 플릿 관리 시스템, 클라우드 서비스, 디지털 트윈, 유지보수 플랫폼, AI 분석 시스템 간의 Telemetry 데이터를 가볍고 효율적이며 확장성 있게 전달할 수 있는 구조를 제공한다. AMR이 단순한 개별 장비에서 수백 대, 수천 대 규모의 플릿(Fleet)으로 확대되면서 Telemetry 통신은 전체 시스템 운영의 핵심 요소가 되었다. MQTT는 이러한 대규모 로봇 운영 환경에서 데이터를 수집하고 분배하며 분석하는 데 최적화된 통신 방식이다.

Telemetry란 시스템의 상태, 성능, 위치, 건강 상태, 임무 진행 상황, 환경 정보 등을 자동으로 측정하고 전송하는 기술을 의미한다. AMR에서는 Telemetry가 로봇의 현재 상태를 외부 시스템에 전달하는 주요 수단이 된다.

AMR은 운행 중 지속적으로 방대한 양의 데이터를 생성한다. 배터리 상태, 충전 상태, 모터 온도, 휠 엔코더 값, 위치 정보, 내비게이션 상태, 장애물 감지 정보, 네트워크 품질, CPU 사용률, 메모리 사용량, 센서 상태, 장애 코드, 임무 진행률, 안전 시스템 상태 등이 모두 Telemetry 데이터에 해당한다.

MQTT가 AMR Telemetry에 적합한 이유는 여러 가지가 있다. 첫째, MQTT는 매우 가벼운 프로토콜이므로 Wi-Fi, LTE, 5G, 사설망과 같은 제한된 네트워크 환경에서도 효율적으로 동작한다. 둘째, 수백 대 이상의 로봇이 동시에 데이터를 전송해도 확장성이 우수하다. 셋째, QoS 기능을 통해 필요한 수준의 신뢰성을 제공할 수 있다. 넷째, 클라우드, 디지털 트윈, AI 플랫폼과 자연스럽게 통합할 수 있다.

HTTP와 같은 전통적인 Request-Response 방식은 Telemetry 전송에 적합하지 않다. Telemetry 데이터는 지속적으로 생성되며 여러 시스템이 동시에 소비하기 때문이다. MQTT의 Publish-Subscribe 구조는 이러한 요구사항을 매우 효율적으로 지원한다.

MQTT 기반 AMR 구조에서 로봇은 Publisher 역할을 수행한다. 로봇은 다양한 Telemetry 데이터를 MQTT Broker에 게시(Publish)한다. Fleet Manager, Dashboard, Cloud Analytics, AI Engine, Digital Twin, ERP 시스템 등은 Subscriber로 동작하며 필요한 Topic을 구독한다.

이 구조의 가장 큰 장점은 로봇이 데이터를 누가 사용하는지 알 필요가 없다는 점이다. 로봇은 단순히 Topic에 데이터를 발행하기만 하면 되며, Broker가 필요한 Subscriber에게 자동으로 전달한다.

일반적으로 AMR Telemetry는 로봇 고유 ID를 기준으로 구성된다. 예를 들어 robot001이라는 AMR이 있다면 다음과 같은 Topic 구조를 사용할 수 있다.

이러한 구조는 Subscriber가 원하는 범위의 데이터를 쉽게 수집할 수 있도록 해준다. Fleet Dashboard는 fleet/+/battery를 구독하여 모든 로봇의 배터리 상태를 모니터링할 수 있다. Digital Twin은 fleet/#를 구독하여 전체 데이터를 수집할 수 있다.

배터리 Telemetry는 가장 중요한 데이터 중 하나이다. 배터리 상태는 임무 계획, 충전 스케줄, 유지보수 계획, 플릿 가용성에 직접적인 영향을 미친다.

배터리 Telemetry에는 State of Charge(SOC), 전압, 전류, 온도, 충전 횟수, 예상 잔여 운행 시간, 충전 상태, 배터리 건강도(SOH), 에너지 소비율 등이 포함될 수 있다.

Fleet Management System은 이 데이터를 활용하여 자동 충전 계획을 수립하고 배터리 교체 시기를 예측할 수 있다.

위치(Localization) Telemetry 역시 매우 중요하다.

현대 AMR은 LiDAR, Camera, IMU, Wheel Encoder, GNSS, SLAM 알고리즘을 이용하여 자신의 위치를 지속적으로 추정한다.

위치 Telemetry에는 X, Y, Z 좌표, 방향각(Heading), Localization Confidence, Map ID, 현재 Zone, 이동 속도, 가속도 등이 포함될 수 있다.

Fleet Manager는 이를 통해 로봇 위치를 실시간으로 확인할 수 있으며 Digital Twin은 가상 환경과 실제 환경을 동기화할 수 있다.

Mission Telemetry는 로봇이 수행 중인 작업의 상태를 나타낸다.

AMR은 물류 운반, 재고 이동, 검사 작업, 청소 작업, 배송 작업 등 다양한 임무를 수행한다.

Mission Telemetry에는 Mission ID, 우선순위, 예상 완료 시간, 현재 단계, 출발지, 목적지, 완료율, 임무 상태 등이 포함된다.

운영자는 이를 통해 생산성을 모니터링하고 병목 현상을 분석할 수 있다.

Navigation Telemetry는 자율주행 성능을 평가하는 데 사용된다.

경로 계획 결과, 현재 Waypoint, 장애물 회피 상태, 재경로 계획(Replanning), 경로 이탈 여부, 이동 효율성 등의 정보를 제공한다.

이 정보는 자율주행 알고리즘의 성능 개선에 활용될 수 있다.

Safety Telemetry는 산업 현장에서 매우 중요하다.

AMR은 사람, 차량, 설비와 함께 동작하기 때문에 안전 시스템 상태를 지속적으로 모니터링해야 한다.

Safety Telemetry에는 E-Stop 상태, Bumper 상태, Safety LiDAR 상태, 충돌 경고, 위험 구역 진입 여부, Safety Controller 상태 등이 포함될 수 있다.

Fleet Manager는 이러한 데이터를 활용하여 위험 상황을 즉시 감지할 수 있다.

Mechanical Health Telemetry는 예지보전(Predictive Maintenance)의 핵심 데이터이다.

AMR 내부에는 모터, 감속기, 베어링, 휠, 브레이크, 액추에이터 등이 존재하며 시간이 지남에 따라 마모가 발생한다.

Telemetry에는 모터 온도, 진동 값, 전류 소비량, Wheel Slip, 감속기 효율, 베어링 상태, 브레이크 마모 상태 등이 포함될 수 있다.

AI 기반 유지보수 시스템은 이러한 데이터를 분석하여 고장을 사전에 예측할 수 있다.

최근에는 Computing Telemetry의 중요성도 증가하고 있다.

현대 AMR은 Edge Computer, GPU, AI Accelerator, Embedded Controller 등을 포함하는 복잡한 컴퓨팅 플랫폼으로 발전하고 있다.

CPU 사용률, 메모리 사용량, GPU 부하, 디스크 사용량, 네트워크 사용량, 컨테이너 상태, ROS Node 상태, DDS 성능 정보 등이 모두 Telemetry로 수집될 수 있다.

이 정보는 시스템 안정성 확보와 성능 최적화에 매우 유용하다.

Network Telemetry 역시 중요하다.

AMR은 Wi-Fi, Private 5G, LTE, Ethernet, Mesh Network 등을 통해 통신한다.

Telemetry에는 RSSI, Packet Loss, Latency, Throughput, Roaming Event, Broker 연결 상태 등이 포함될 수 있다.

네트워크 품질은 자율주행과 Fleet Operation의 안정성에 직접적인 영향을 준다.

MQTT QoS는 Telemetry 설계에서 중요한 역할을 한다.

위치 정보, IMU 데이터, 환경 센서 데이터와 같은 고주기 데이터는 일반적으로 QoS 0을 사용한다. 일부 데이터 손실이 발생하더라도 다음 데이터가 곧 도착하기 때문이다.

배터리 경고, 유지보수 알람, 장애 보고, 임무 상태 업데이트와 같은 운영 정보는 QoS 1을 사용하는 경우가 많다.

임무 완료 보고, ERP 연동 데이터, 감사 로그(Audit Log)와 같은 중요한 데이터는 QoS 2를 사용할 수 있다.

Retained Message 기능도 Telemetry 시스템에서 널리 사용된다.

배터리 상태, 로봇 온라인 상태, 소프트웨어 버전, 현재 임무 상태 등을 Retain Message로 저장하면 새롭게 연결된 Subscriber가 즉시 최신 상태를 확인할 수 있다.

Last Will and Testament(LWT)는 Telemetry 신뢰성을 더욱 향상시킨다.

각 로봇은 연결 시 Offline 메시지를 Will Message로 등록할 수 있다. 로봇이 예기치 않게 네트워크에서 사라지면 Broker는 자동으로 Offline 메시지를 발행한다.

Fleet Manager는 즉시 해당 로봇의 연결 장애를 감지할 수 있다.

클라우드 연동은 MQTT를 사용하는 가장 큰 이유 중 하나이다.

AWS IoT Core, Microsoft Azure IoT Hub, Google Cloud IoT와 같은 플랫폼은 MQTT를 기본적으로 지원한다.

따라서 Telemetry 데이터는 클라우드 AI, 디지털 트윈, 예지보전, BI(Business Intelligence), 데이터 분석 시스템으로 쉽게 전달될 수 있다.

Edge Computing 환경에서는 공장 내부에 Edge MQTT Broker를 배치하는 경우가 많다.

Edge Broker는 실시간 Telemetry 처리를 수행하며, 중요한 데이터만 Cloud Broker로 전달한다.

이러한 구조는 네트워크 사용량을 줄이고 지연시간을 최소화한다.

Digital Twin 역시 MQTT Telemetry에 크게 의존한다.

가상 로봇은 실제 로봇의 위치, 상태, 임무, 배터리 정보, 센서 정보 등을 지속적으로 받아야 한다.

MQTT는 물리적 로봇과 가상 로봇 간 상태 동기화를 수행하는 핵심 통신 수단이 된다.

Physical AI 시대에는 Telemetry 데이터의 종류와 양이 더욱 증가할 것으로 예상된다.

미래의 AMR은 LLM, Vision-Language-Action 모델, World Model, Multimodal AI를 탑재하게 된다.

이 경우 AI Confidence Score, 추론 결과, 행동 계획 상태, Semantic Map 정보, 정책 실행 결과 등 새로운 형태의 Telemetry가 생성될 것이다.

MQTT는 이러한 대규모 AI Telemetry를 전달하는 핵심 인프라 역할을 수행할 수 있다.

힐스로보틱스의 Indoor AMR, Outdoor AMR, Inspection Robot, Security Robot, Mobile Manipulator, CAD2SCAN Platform, GPR Inspection Vehicle, Quadruped Robot, Humanoid Robot, 그리고 향후 Cargo UAV 플랫폼에서도 MQTT Telemetry는 핵심 통신 계층으로 사용될 수 있다.

일반적인 구조에서는 ROS 2와 DDS가 로봇 내부의 실시간 제어 및 센서 통신을 담당하고, MQTT는 Fleet Management, Cloud Integration, Digital Twin, Predictive Maintenance, AI Analytics를 담당한다.

결론적으로 MQTT 기반 AMR Telemetry는 단순한 상태 보고 기능을 넘어 플릿 운영, 예지보전, 디지털 트윈, AI 분석, 클라우드 통합을 가능하게 하는 핵심 운영 인텔리전스 플랫폼이다. MQTT는 경량성, 확장성, 신뢰성, 유연성을 제공하며, 미래의 대규모 자율주행 로봇 및 Physical AI 생태계를 연결하는 핵심 통신 기술로 지속적으로 활용될 것이다.
