**Volume 09 Robotics Communication**

# Chapter 3. MQTT

## 3.1 MQTT 3.1.1 and 5.0

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

MQTT, which stands for Message Queuing Telemetry Transport, is one of the most widely adopted lightweight communication protocols in modern Internet of Things systems, industrial automation platforms, cloud-connected robotics environments, autonomous vehicles, smart factories, and distributed AI infrastructures. Originally developed by IBM and Eurotech in the late 1990s for satellite communication systems and constrained remote telemetry networks, MQTT has evolved into a de facto standard for machine-to-machine communication due to its simplicity, scalability, low bandwidth requirements, and excellent support for unreliable network environments. Within the Hills Robotics communication architecture, MQTT occupies a critical role in fleet management systems, robot telemetry streaming, cloud integration, edge computing communication, digital twin synchronization, predictive maintenance infrastructure, and Physical AI deployment environments. The MQTT section appears within the Robotics Communication domain and specifically addresses how lightweight publish-subscribe communication can be utilized for large-scale robotic systems.

The fundamental philosophy behind MQTT differs significantly from traditional client-server communication models. Conventional protocols such as HTTP operate using direct request-response interactions between clients and servers. In contrast, MQTT utilizes a broker-centered publish-subscribe architecture. Rather than devices communicating directly with one another, all communication passes through a central broker that manages message routing, topic subscriptions, security policies, session management, and message delivery.

This architecture provides several advantages for distributed robotic systems. Robots do not need to know the location, network address, or availability of other devices. Instead, they simply publish information to specific topics and subscribe to topics containing information they require. The broker handles all routing responsibilities, significantly reducing coupling between software components and simplifying large-scale deployments.

In a robotic fleet environment, an individual robot may publish battery information, localization status, mission progress, sensor health metrics, network diagnostics, and fault reports. Simultaneously, fleet management servers, monitoring dashboards, predictive maintenance engines, digital twin platforms, and cloud AI systems may subscribe to these topics. The robot remains unaware of how many systems consume its data, allowing the overall architecture to scale efficiently.

MQTT communication revolves around several core entities. The first is the client. A client represents any device, software process, robot, sensor gateway, edge computer, cloud application, fleet manager, monitoring dashboard, or AI service participating in MQTT communication. Every participant in an MQTT system acts as a client.

The second entity is the broker. The broker functions as the central communication hub. It receives messages from publishers, determines which subscribers are interested in those messages, and distributes the information accordingly. Popular MQTT brokers include Eclipse Mosquitto, EMQX, HiveMQ, VerneMQ, and cloud-based services provided by AWS, Microsoft Azure, and Google Cloud.

The third entity is the topic. Topics represent hierarchical communication channels through which messages are exchanged. Topics are structured as text-based paths that organize information into logical categories. For example, a robotic fleet may utilize topics such as fleet/robot001/status, fleet/robot001/battery, fleet/robot001/location, fleet/robot001/mission, and fleet/robot001/faults. The hierarchical nature of topics simplifies organization and subscription management.

Publishers transmit information to topics without knowing which subscribers exist. Subscribers register interest in topics and automatically receive matching messages. This decoupled communication model significantly improves flexibility and scalability.

MQTT 3.1.1 became the most widely deployed version of the protocol and remains heavily used throughout industrial systems. The MQTT 3.1.1 specification standardized protocol behavior, clarified interoperability requirements, and established a stable foundation that enabled widespread adoption across IoT and robotics industries.

The MQTT 3.1.1 protocol is intentionally lightweight. Packet headers are extremely small, minimizing network bandwidth consumption. The protocol requires minimal computational resources and memory, making it suitable for embedded systems, battery-powered devices, low-bandwidth networks, and resource-constrained robotic platforms.

A typical MQTT connection begins when a client establishes a TCP connection with a broker. Following successful connection establishment, the client transmits a CONNECT packet containing authentication information and session parameters. The broker validates the request and responds with a CONNACK packet indicating acceptance or rejection.

Once connected, the client may publish messages, subscribe to topics, unsubscribe from topics, and maintain connection status through periodic keep-alive mechanisms. The simplicity of this workflow contributes significantly to MQTT\'s popularity.

MQTT 3.1.1 introduced three Quality of Service levels that balance reliability against communication overhead. QoS 0 provides at-most-once delivery, meaning messages are transmitted without acknowledgment. This approach minimizes latency and bandwidth consumption but does not guarantee delivery.

QoS 1 provides at-least-once delivery. Messages are acknowledged by recipients, ensuring delivery but potentially allowing duplicate messages under certain circumstances. This level is commonly used for telemetry and status reporting.

QoS 2 provides exactly-once delivery through a more sophisticated acknowledgment sequence. This mechanism eliminates duplicate delivery but increases protocol complexity and network overhead. QoS 2 is generally reserved for situations requiring maximum reliability.

MQTT 3.1.1 also introduced retained messages. A retained message remains stored by the broker and is automatically delivered to new subscribers upon subscription. This feature allows clients joining the system to immediately obtain the most recent state information without waiting for future updates.

For example, a fleet dashboard connecting to a robotic system can instantly receive the latest robot status, battery level, and mission state through retained messages. Without this mechanism, the dashboard would need to wait for the next telemetry transmission cycle.

Another important feature is the Last Will and Testament mechanism. When a client unexpectedly disconnects due to power failure, network loss, software crash, or hardware malfunction, the broker can automatically publish a predefined message indicating the client\'s disappearance.

This capability is extremely valuable in robotic systems. A robot may register a Last Will message stating that it has become unavailable. If communication is lost unexpectedly, fleet management systems immediately receive notification and can initiate recovery procedures.

Although MQTT 3.1.1 achieved tremendous success, evolving IoT and robotic applications demanded additional capabilities. These requirements ultimately led to the development of MQTT 5.0.

MQTT 5.0 represents a significant enhancement rather than a complete redesign. The core publish-subscribe architecture remains unchanged, preserving compatibility with existing MQTT concepts while introducing substantial improvements in scalability, diagnostics, metadata handling, error reporting, and enterprise integration.

One of the most important additions in MQTT 5.0 is enhanced reason codes. Previous protocol versions often provided limited information when operations failed. MQTT 5.0 introduces detailed reason codes that allow clients to understand exactly why a connection, subscription, publication, or authentication request was rejected.

This improvement simplifies troubleshooting and operational monitoring in large robotic deployments. Fleet operators can rapidly diagnose communication failures, permission issues, broker overload conditions, and network configuration problems.

MQTT 5.0 also introduces user properties. User properties allow arbitrary metadata to be attached to messages, subscriptions, and protocol operations. This feature enables richer communication semantics without modifying payload structures.

For example, a robotic telemetry message may include metadata describing software version, mission identifier, deployment region, customer identifier, AI model version, or operational mode. Such information can significantly improve monitoring and analytics capabilities.

Message expiry intervals represent another important enhancement. In many robotic applications, information becomes obsolete rapidly. A robot position message generated several minutes ago may no longer be relevant. MQTT 5.0 allows publishers to specify message expiration times, ensuring outdated information is automatically discarded.

This capability improves data freshness and prevents stale information from affecting decision-making processes.

Subscription identifiers provide another valuable feature. Clients managing numerous subscriptions can assign identifiers to each subscription and subsequently determine which subscription caused a particular message to be delivered. This simplifies message processing and debugging within complex systems.

Shared subscriptions represent one of the most powerful scalability enhancements introduced in MQTT 5.0. Traditionally, every subscriber receives every message associated with a subscribed topic. Shared subscriptions allow multiple consumers to share message processing workloads.

For example, a large robotic fleet may generate millions of telemetry messages daily. Rather than processing all messages through a single analytics service, multiple backend processors can share the workload. The broker distributes messages among subscribers, improving scalability and fault tolerance.

Request-response communication is another area significantly improved in MQTT 5.0. While MQTT was originally designed primarily for asynchronous publish-subscribe communication, many applications require request-response interactions. MQTT 5.0 introduces response topics and correlation data that facilitate structured request-response workflows.

This capability allows robots to request information from cloud services while still leveraging MQTT infrastructure rather than introducing separate communication protocols.

Session management was also improved substantially. MQTT 5.0 provides greater control over session expiration and state persistence. Clients can maintain subscriptions and communication state across temporary disconnects, reducing reconnection overhead and improving reliability.

Flow control mechanisms introduced in MQTT 5.0 further enhance scalability. Clients and brokers can negotiate limits on message rates, outstanding requests, and resource utilization. These controls help prevent overload conditions and improve overall system stability.

Security remains a critical consideration for both MQTT 3.1.1 and MQTT 5.0. The protocol itself does not mandate encryption but commonly operates over TLS. Secure deployments typically incorporate TLS encryption, certificate-based authentication, access control lists, role-based permissions, and broker-level security policies.

In robotic environments, security becomes particularly important because MQTT often carries operational commands, telemetry information, maintenance data, and AI-related information. Unauthorized access could potentially compromise safety, privacy, or operational continuity.

Within industrial robotics, MQTT frequently complements rather than replaces other communication technologies. ROS 2 commonly manages real-time internal communication using DDS, while MQTT handles cloud connectivity, fleet management, telemetry streaming, remote monitoring, and cross-site communication. This layered architecture combines the strengths of both technologies.

For example, an autonomous mobile robot may use ROS 2 topics internally for sensor processing, localization, navigation, and control. Simultaneously, MQTT transmits battery status, mission updates, health diagnostics, operational statistics, and maintenance information to cloud services. This separation allows each protocol to operate within domains best suited to its characteristics.

Fleet management systems represent one of the most common MQTT use cases. Hundreds or thousands of robots can publish telemetry to centralized brokers while receiving mission assignments, software updates, operational policies, and maintenance instructions. MQTT\'s lightweight nature enables such deployments without excessive network overhead.

Cloud integration platforms also rely heavily on MQTT. AWS IoT Core, Azure IoT Hub, and many industrial IoT platforms provide native MQTT interfaces. Robotic systems can therefore integrate seamlessly with cloud analytics, machine learning pipelines, digital twins, predictive maintenance systems, and enterprise resource planning environments.

Physical AI systems further increase MQTT\'s importance. Future AI-native robots will continuously exchange telemetry, model updates, inference results, operational metrics, fleet coordination information, and cloud-generated intelligence. MQTT\'s scalable publish-subscribe architecture provides a natural foundation for these interactions.

For Hills Robotics platforms, including Indoor AMRs, Outdoor AMRs, Security Robots, Inspection Robots, Fleet Management Systems, Mobile Manipulators, GPR Inspection Vehicles, CAD2SCAN Systems, Quadruped Robots, Humanoid Robots, and future Cargo UAV platforms, MQTT serves as a critical communication mechanism connecting robots, edge infrastructure, cloud services, AI systems, monitoring dashboards, and operational management platforms. MQTT 3.1.1 provides a stable and proven foundation, while MQTT 5.0 introduces advanced capabilities necessary for next-generation autonomous and AI-driven robotic ecosystems.

Ultimately, MQTT 3.1.1 established the lightweight publish-subscribe paradigm that revolutionized IoT and robotic communication. MQTT 5.0 builds upon that foundation by introducing enhanced diagnostics, richer metadata support, improved scalability, advanced session management, shared subscriptions, message expiration, and enterprise-grade communication capabilities. Together these protocol versions form one of the most important communication technologies enabling modern connected robotics and future Physical AI infrastructures.

fleet/robot001/status

fleet/robot001/battery

fleet/robot001/location

fleet/robot001/mission

fleet/robot001/fault

## 3.2 QoS Level 0 1 2

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

Quality of Service (QoS) is one of the most important features of the MQTT protocol and serves as the foundation for controlling message delivery reliability in distributed communication systems. In modern robotics, Internet of Things infrastructures, industrial automation environments, cloud-connected autonomous systems, digital twins, and Physical AI platforms, communication reliability directly affects system safety, operational efficiency, and mission success. MQTT addresses these requirements through three distinct Quality of Service levels: QoS 0, QoS 1, and QoS 2. These levels allow system architects to balance communication reliability, network bandwidth consumption, latency, computational overhead, and storage requirements according to application-specific needs.

The concept of Quality of Service exists because not all data exchanged within a robotic system carries the same level of importance. Some information is transient and can tolerate occasional loss, while other information must be delivered reliably under all circumstances. For example, periodic battery updates may tolerate occasional packet loss without affecting overall operation. In contrast, emergency stop commands, critical alarms, safety-related notifications, or mission completion confirmations often require guaranteed delivery. By providing multiple QoS levels, MQTT enables developers to select the most appropriate reliability mechanism for each communication channel.

QoS in MQTT applies independently to individual messages and subscriptions. A publisher may transmit different message types using different QoS settings depending on the importance of the information being exchanged. Similarly, subscribers can negotiate desired QoS levels during topic subscriptions. The MQTT broker manages message delivery according to these policies, ensuring consistent behavior throughout the communication infrastructure.

QoS Level 0 is known as "At Most Once" delivery. This is the simplest and fastest message delivery mode supported by MQTT. In QoS 0 communication, the publisher transmits a message to the broker and immediately considers the transmission complete. No acknowledgment is required, no delivery confirmation is expected, and no retransmission mechanism exists. The message is effectively sent using a "fire-and-forget" strategy.

Because QoS 0 does not require acknowledgment packets, network overhead is minimized. Communication latency remains extremely low, making QoS 0 suitable for applications where performance and bandwidth efficiency are more important than guaranteed delivery. The protocol overhead associated with message tracking, retransmission management, and acknowledgment processing is completely eliminated.

In a robotic fleet environment, certain telemetry streams may be transmitted using QoS 0. Examples include continuously updated robot position estimates, wheel encoder values, camera status information, environmental sensor measurements, and periodic diagnostic statistics. If a single message is lost during transmission, the next update will typically arrive shortly thereafter, making recovery unnecessary.

Consider an Autonomous Mobile Robot publishing localization information ten times per second. If one position update is lost due to temporary network congestion, the next update arrives only one hundred milliseconds later. The system continues functioning normally because the information rapidly becomes obsolete. Retransmitting lost position updates would provide little value while consuming additional network resources.

QoS 0 therefore offers several advantages. Communication latency remains minimal. CPU utilization is reduced because acknowledgment processing is unnecessary. Network bandwidth consumption is minimized because only the original message is transmitted. System scalability improves because brokers can process larger message volumes without maintaining delivery state information.

However, these advantages come at the cost of reliability. Messages may be lost due to network failures, broker overload conditions, client disconnections, wireless interference, routing problems, or hardware faults. The sender receives no indication that a message was lost. Consequently, QoS 0 should only be used when occasional message loss is acceptable.

QoS Level 1 introduces a higher degree of reliability through an "At Least Once" delivery guarantee. Under QoS 1, the publisher transmits a message and expects an acknowledgment from the broker. Specifically, the broker responds with a PUBACK packet confirming successful receipt of the message.

If the publisher does not receive the acknowledgment within an expected time interval, it assumes the message may have been lost and retransmits it. This process continues until acknowledgment is successfully received or the communication session terminates.

The introduction of acknowledgment packets significantly improves reliability because message loss can be detected and corrected through retransmission. However, QoS 1 does not guarantee that a message will be delivered exactly once. Under certain circumstances, duplicate delivery may occur.

Consider a scenario in which the broker receives a message successfully but the acknowledgment packet is lost during transmission back to the publisher. From the publisher\'s perspective, the message appears undelivered because no acknowledgment was received. Consequently, the publisher retransmits the message. The broker then receives a second copy of the same message.

Because MQTT prioritizes guaranteed delivery under QoS 1, duplicate messages are considered acceptable. The protocol guarantees that the message will arrive at least once, even if multiple copies are occasionally delivered.

This behavior makes QoS 1 suitable for many industrial and robotic applications where reliable delivery is more important than strict uniqueness. Mission status updates, maintenance notifications, battery warnings, fault reports, work order assignments, inspection requests, and fleet coordination messages commonly utilize QoS 1 communication.

For example, a robot may report a low battery condition to a fleet management server. Missing this notification could result in mission failure or operational disruption. Receiving the notification twice is generally less problematic than not receiving it at all. Consequently, QoS 1 provides an appropriate balance between reliability and complexity.

QoS 1 introduces moderate communication overhead. Additional acknowledgment packets consume network bandwidth. Message tracking requires memory resources within both clients and brokers. Retransmission logic increases protocol complexity. Nevertheless, the reliability improvements often justify these costs.

In large robotic deployments, QoS 1 frequently represents the default communication mode because it offers strong reliability without excessive protocol overhead. Many MQTT-based fleet management systems rely heavily on QoS 1 for operational messaging.

QoS Level 2 provides the highest reliability available within MQTT and is known as "Exactly Once" delivery. The primary objective of QoS 2 is to ensure that each message reaches its destination precisely one time, eliminating both message loss and duplicate delivery.

Achieving this guarantee requires a more sophisticated communication sequence involving four distinct protocol exchanges. The publisher first transmits the message using a PUBLISH packet. The broker acknowledges receipt using a PUBREC packet. The publisher then sends a PUBREL packet indicating readiness to complete the transaction. Finally, the broker responds with a PUBCOMP packet confirming successful completion.

This four-step handshake creates a transactional communication process that allows both publisher and broker to coordinate message state accurately. Duplicate delivery can therefore be detected and prevented.

QoS 2 effectively combines the reliability of acknowledgment-based communication with mechanisms that eliminate duplicate processing. The result is the strongest delivery guarantee available within MQTT.

This capability becomes important when duplicate messages could create operational problems. Examples include financial transactions, inventory management operations, mission completion records, database updates, billing events, manufacturing execution commands, and safety-critical control actions.

Consider a warehouse robot reporting completion of a delivery mission. If duplicate completion messages are processed, inventory records could become inconsistent, logistics workflows could be disrupted, or downstream automation systems could perform redundant actions. QoS 2 prevents such issues by ensuring that each completion notification is processed exactly once.

Similarly, an autonomous inspection robot may upload defect reports to a maintenance management system. Duplicate reports could trigger unnecessary maintenance activities or create confusion among operators. QoS 2 eliminates this risk through transactional delivery control.

The enhanced reliability of QoS 2 comes at a cost. Communication latency increases because multiple acknowledgment exchanges are required. Network bandwidth consumption rises due to additional protocol packets. Memory requirements grow because message state must be maintained throughout the transaction. CPU utilization increases because more complex protocol logic is executed.

Consequently, QoS 2 should be reserved for situations where exact delivery semantics provide clear operational benefits. Using QoS 2 for high-frequency telemetry streams would unnecessarily increase resource consumption without significantly improving system performance.

Comparing the three QoS levels highlights their distinct tradeoffs. QoS 0 prioritizes speed and efficiency while accepting possible message loss. QoS 1 prioritizes reliable delivery while tolerating occasional duplicates. QoS 2 prioritizes both reliable delivery and uniqueness while accepting higher communication overhead.

The choice of QoS level should always be guided by application requirements rather than a desire for maximum reliability. Selecting the highest QoS level for every message often leads to unnecessary complexity and reduced system scalability.

In robotic systems, communication requirements vary widely across subsystems. High-frequency sensor streams generally favor QoS 0 because data rapidly becomes obsolete. Fleet coordination messages often utilize QoS 1 because reliable delivery is important while duplicate handling remains manageable. Mission accounting records, maintenance transactions, and operational state transitions may require QoS 2 because duplicate processing could produce undesirable consequences.

Within a typical Autonomous Mobile Robot architecture, localization updates, wheel odometry data, IMU measurements, environmental telemetry, and diagnostic statistics may operate using QoS 0. Battery alarms, navigation status updates, maintenance notifications, charging requests, and mission assignments often use QoS 1. Mission completion confirmations, inventory transactions, production reporting events, and audit records may utilize QoS 2.

The MQTT broker plays a central role in implementing QoS behavior. Brokers maintain message state information, manage acknowledgment processing, coordinate retransmissions, and ensure compliance with protocol requirements. As QoS levels increase, broker resource consumption also increases.

Large-scale robotic fleets may generate millions of messages per day. System architects must therefore consider broker scalability when selecting QoS policies. Excessive use of QoS 2 can significantly increase memory usage, CPU load, and storage requirements within broker infrastructure.

MQTT 5.0 further enhances QoS operation through improved diagnostics, reason codes, session management, and flow control mechanisms. Enhanced reason codes provide detailed information regarding acknowledgment failures and communication problems. Session management improvements support reliable message delivery across temporary disconnections. Flow control mechanisms help prevent overload conditions that could otherwise affect QoS performance.

Security considerations also interact closely with QoS configuration. TLS encryption, certificate-based authentication, access control policies, and secure session management protect message integrity throughout transmission. Reliable delivery mechanisms become particularly valuable when operating across public networks, cloud infrastructures, and geographically distributed robotic fleets.

Cloud-connected robotic systems frequently combine multiple QoS levels simultaneously. An indoor AMR may publish telemetry using QoS 0, fleet status updates using QoS 1, and mission completion records using QoS 2. This layered approach optimizes resource utilization while maintaining appropriate reliability for each information category.

Outdoor autonomous vehicles, inspection robots, security robots, mobile manipulators, and future Physical AI systems similarly benefit from differentiated QoS strategies. Sensor-rich platforms generate enormous volumes of transient information that do not require guaranteed delivery, while operational coordination messages often demand stronger reliability guarantees.

For Hills Robotics platforms, including Indoor AMRs, Outdoor AMRs, Inspection Robots, Security Robots, Fleet Management Systems, Mobile Manipulators, CAD2SCAN Platforms, GPR Inspection Vehicles, Quadruped Robots, Humanoid Robots, and future Cargo UAV systems, careful QoS selection is essential for balancing performance, scalability, reliability, and operational efficiency. High-frequency telemetry can utilize QoS 0, fleet coordination may employ QoS 1, and mission-critical transactional events can leverage QoS 2.

Ultimately, MQTT QoS Levels 0, 1, and 2 provide a flexible framework for tailoring communication reliability to application requirements. QoS 0 delivers maximum efficiency through at-most-once delivery. QoS 1 delivers strong reliability through at-least-once delivery. QoS 2 delivers the highest assurance through exactly-once delivery. Together these mechanisms allow MQTT-based robotic systems to achieve an optimal balance between performance, scalability, bandwidth utilization, and operational reliability, making MQTT a foundational communication technology for modern robotics, industrial automation, cloud-connected infrastructure, and future Physical AI ecosystems.

# 03_02 QoS Level 0, 1, 2

## 3.3 Retain and Last Will Testament

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

Retain Messages and Last Will and Testament (LWT) are two of the most powerful and practical features provided by the MQTT protocol. While MQTT is fundamentally known for its lightweight publish-subscribe communication model, the protocol\'s true value in industrial automation, robotics, fleet management, cloud connectivity, and Physical AI systems becomes evident through these advanced operational mechanisms. Retain Messages ensure that newly connected subscribers immediately receive the most recent state information, while Last Will and Testament provides automatic failure notification when devices unexpectedly disconnect from the network. Together, these features significantly enhance reliability, observability, fault detection, system awareness, and operational continuity in distributed robotic systems.

Modern robotic systems operate within highly dynamic environments where devices continuously join and leave communication networks. Autonomous Mobile Robots, outdoor autonomous vehicles, mobile manipulators, inspection robots, security robots, warehouse automation systems, and future Physical AI platforms frequently communicate with cloud services, edge computing nodes, fleet management servers, monitoring dashboards, and digital twin infrastructures. In such environments, maintaining awareness of current system state becomes critically important.

Traditional publish-subscribe communication models only deliver messages to subscribers that are actively connected at the moment messages are published. If a subscriber joins the system after a message has already been transmitted, the information is typically lost. While this behavior may be acceptable for transient telemetry streams, it becomes problematic when newly connected systems require immediate access to the current operational state of devices.

Retain Messages were introduced to solve this challenge. A retained message is a normal MQTT message that includes a retain flag. When a publisher transmits a message with the retain flag enabled, the MQTT broker stores the message as the most recent value associated with that topic. Whenever a new subscriber subscribes to the topic, the broker immediately delivers the retained message without waiting for the publisher to transmit another update.

The retained message effectively acts as a snapshot of the latest known state. Instead of waiting for future updates, subscribers instantly receive the most current information available within the broker.

Consider a robotic fleet management system containing hundreds of autonomous robots operating across multiple facilities. Each robot periodically publishes battery status information to a topic such as fleet/robot001/battery. If these messages are transmitted as retained messages, a monitoring dashboard that connects later can immediately obtain the current battery level without waiting for the next telemetry update cycle.

Without retained messages, the dashboard might display incomplete information until the next battery update occurs. Depending on update frequency, this delay could range from several seconds to several minutes. Retained messages eliminate this uncertainty by providing immediate state visibility.

The usefulness of retained messages becomes even more apparent when examining system startup procedures. In large robotic deployments, multiple software services often start at different times. Cloud services may reboot, dashboards may reconnect, digital twin applications may restart, and maintenance tools may join the system after robots have already been operating for hours.

Retained messages ensure that newly connected services immediately acquire essential system state information. Robot availability, battery levels, operating modes, mission assignments, localization status, fault conditions, charging status, and software versions can all be communicated through retained topics.

Retained messages are particularly effective for state-oriented information. Data that represents the current condition of a system is often well suited for retention because subscribers typically need the latest value rather than a complete historical sequence.

Examples include operational status indicators, current location summaries, active mission identifiers, charging station assignments, fleet configuration parameters, network connectivity status, maintenance schedules, software deployment versions, and AI model identifiers.

However, not all data should be retained. High-frequency sensor streams such as camera images, LiDAR point clouds, IMU measurements, wheel encoder values, and raw telemetry generally do not benefit from retention. Such information rapidly becomes obsolete and storing the latest sample may provide little practical value.

Understanding how brokers manage retained messages is important. Each topic can maintain only a single retained message. Whenever a new retained message is published to a topic, it replaces the previous retained message. The broker therefore always stores the latest known value rather than maintaining a history of all values.

Deleting retained messages is also possible. A publisher can transmit a retained message containing an empty payload. Upon receiving this message, the broker removes the retained message associated with the topic. Future subscribers will therefore receive no retained information for that topic.

This capability allows dynamic management of retained state information. When a robot is permanently removed from service, its retained topics can be cleared to prevent outdated information from appearing in future system views.

Retained messages contribute significantly to digital twin architectures. Digital twins rely on accurate representations of current system state. When digital twin applications reconnect after maintenance or software upgrades, retained messages allow rapid reconstruction of robot status without requiring extensive synchronization procedures.

While retained messages address state persistence, Last Will and Testament addresses fault detection and unexpected disconnections.

In distributed robotic systems, network failures, power outages, software crashes, hardware malfunctions, and wireless communication disruptions are unavoidable realities. Detecting these failures rapidly is essential for maintaining operational awareness and ensuring safe system behavior.

Without explicit failure detection mechanisms, disconnected devices may simply disappear from the network without explanation. Other systems may continue assuming that disconnected devices remain operational. Such uncertainty can create significant operational risks.

Last Will and Testament provides an elegant solution to this problem. When an MQTT client initially connects to a broker, it may register a special message known as a Will Message. This message remains stored within the broker but is not immediately published.

As long as the client disconnects normally using the MQTT DISCONNECT procedure, the Will Message is discarded and never transmitted. However, if the client unexpectedly disappears due to power failure, software crash, network interruption, hardware malfunction, operating system failure, or communication timeout, the broker automatically publishes the Will Message on behalf of the disconnected client.

This mechanism effectively serves as an automatic failure notification system.

Consider an autonomous mobile robot operating within a warehouse. During connection establishment, the robot registers a Will Message indicating "robot001 offline" on a topic such as fleet/robot001/status. If the robot experiences a sudden power loss and communication ceases unexpectedly, the broker automatically publishes the offline notification.

Fleet management systems, monitoring dashboards, maintenance platforms, digital twin services, and supervisory control systems immediately receive notification that the robot has become unavailable. Operators can respond appropriately without waiting for manual diagnosis.

The distinction between normal disconnection and abnormal disconnection is central to LWT functionality. If the robot intentionally shuts down and sends a DISCONNECT message, the broker understands that the departure was expected and suppresses the Will Message. Only unexpected failures trigger automatic publication.

This behavior significantly reduces false alarms while ensuring rapid fault detection.

The operational value of LWT increases dramatically as robotic systems scale. In a fleet containing hundreds or thousands of robots, manually monitoring connectivity becomes impractical. Automatic failure reporting enables centralized monitoring systems to maintain accurate awareness of fleet health.

LWT also supports hierarchical monitoring architectures. Individual robots may publish offline notifications. Edge gateways may publish connectivity alerts. Regional fleet managers may publish service availability information. Cloud infrastructure components may register failure notifications as well.

The result is a comprehensive operational awareness framework spanning the entire robotic ecosystem.

One common deployment pattern involves combining retained messages and LWT within the same topic structure. A robot may publish "online" status messages as retained messages while simultaneously registering an LWT message that publishes "offline" upon unexpected disconnection.

This arrangement creates a highly effective presence detection mechanism. When the robot connects successfully, it publishes a retained status message indicating availability. New subscribers immediately see the robot as online. If the robot subsequently disconnects unexpectedly, the broker automatically publishes the offline notification.

Consequently, all system participants maintain an accurate view of robot availability without requiring specialized heartbeat protocols.

MQTT brokers treat Will Messages similarly to ordinary published messages. The Will Message can utilize QoS 0, QoS 1, or QoS 2 reliability levels. It can also be published as a retained message if desired.

This flexibility allows system architects to tailor failure notification behavior according to application requirements. Safety-critical environments may utilize higher QoS levels to ensure reliable fault reporting, while large-scale telemetry environments may prioritize efficiency.

In industrial automation systems, LWT frequently serves as a foundation for supervisory monitoring. Production systems, robotic workcells, conveyor controllers, inspection stations, autonomous vehicles, and edge computing nodes all register Will Messages that enable rapid detection of abnormal conditions.

For autonomous robots operating outdoors, LWT becomes even more valuable because physical access to devices may be limited. Network operations centers can immediately detect communication failures and initiate recovery procedures.

Inspection robots operating in hazardous environments similarly benefit from automatic fault reporting. Operators gain visibility into system health without requiring direct interaction with deployed equipment.

Cloud-connected robotic systems frequently use retained messages and LWT together to synchronize operational state across distributed infrastructure. Cloud services can immediately reconstruct system state through retained messages while simultaneously receiving fault notifications through LWT mechanisms.

The introduction of MQTT 5.0 further enhanced these capabilities. MQTT 5.0 provides improved session management, richer metadata support, enhanced diagnostics, message expiry intervals, and more sophisticated operational control mechanisms. These enhancements improve both retained message management and Will Message processing.

Message expiry intervals, for example, can prevent stale retained information from persisting indefinitely. Session management improvements help maintain state consistency during temporary network disruptions. Enhanced diagnostics simplify troubleshooting when connectivity issues occur.

Security considerations are equally important. Retained messages and LWT often contain operationally sensitive information. Battery levels, robot availability, fault conditions, maintenance status, deployment locations, software versions, and mission assignments may all represent valuable operational data.

Secure MQTT deployments therefore utilize TLS encryption, certificate-based authentication, role-based authorization, access control lists, and broker security policies to protect retained state information and failure notifications from unauthorized access.

Within ROS 2-based robotic systems, MQTT retained messages and LWT frequently complement DDS communication. DDS handles real-time internal communication while MQTT provides fleet-level state awareness and cloud integration. This hybrid architecture combines deterministic local communication with scalable distributed monitoring.

For Hills Robotics platforms, including Indoor AMRs, Outdoor AMRs, Security Robots, Inspection Robots, Fleet Management Systems, Mobile Manipulators, CAD2SCAN Platforms, GPR Inspection Vehicles, Quadruped Robots, Humanoid Robots, and future Cargo UAV systems, Retain Messages and Last Will and Testament provide essential operational capabilities. Retained messages ensure immediate visibility of robot status, battery condition, mission state, charging availability, and software configuration. Last Will and Testament enables automatic detection of failures, communication loss, power interruptions, and unexpected system shutdowns.

Ultimately, Retain Messages and Last Will and Testament transform MQTT from a simple messaging protocol into a powerful operational management platform. Retained messages preserve current system state for future subscribers, while LWT ensures rapid awareness of unexpected failures. Together they provide the foundation for reliable fleet monitoring, digital twin synchronization, cloud integration, predictive maintenance, autonomous operations management, and future Physical AI communication infrastructures. As robotic systems continue expanding in scale and complexity, these MQTT capabilities will remain fundamental building blocks for resilient and intelligent distributed communication architectures.

## 3.4 MQTT Broker Design

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

MQTT Broker Design is one of the most important architectural subjects in modern distributed communication systems because the broker serves as the central intelligence and message-routing hub of the entire MQTT ecosystem. While MQTT clients such as robots, sensors, gateways, cloud services, mobile applications, digital twins, fleet management systems, and AI platforms generate and consume data, the broker is responsible for managing all message exchanges, connection states, subscriptions, security policies, session persistence, reliability mechanisms, and overall communication orchestration. In large-scale robotic deployments, the quality of broker architecture often determines the scalability, reliability, responsiveness, maintainability, and long-term operational success of the entire system.

The MQTT protocol follows a broker-centric publish-subscribe architecture. Unlike peer-to-peer communication systems where devices communicate directly with each other, MQTT clients never exchange messages directly. Instead, every message passes through the broker. Publishers send messages to the broker, and subscribers receive messages from the broker. This architectural separation significantly simplifies communication patterns and allows distributed systems to scale efficiently.

The broker functions as an intermediary that decouples message producers from message consumers. Publishers do not need to know who is receiving their messages, and subscribers do not need to know where messages originate. This loose coupling dramatically improves flexibility and enables highly dynamic system architectures.

In a robotic fleet environment, hundreds or even thousands of robots may simultaneously publish telemetry information, battery status, localization data, diagnostic information, mission progress updates, sensor health reports, AI inference results, and maintenance alerts. At the same time, fleet management servers, monitoring dashboards, predictive maintenance engines, cloud analytics systems, digital twins, and enterprise software platforms subscribe to various subsets of this information. The broker acts as the communication coordinator that manages all these interactions.

The fundamental responsibility of an MQTT broker is message routing. Every incoming message contains a topic name that identifies its destination category. The broker maintains subscription tables that map topic patterns to interested subscribers. When a message arrives, the broker determines which subscribers match the topic and forwards the message accordingly.

Topic routing efficiency becomes increasingly important as system scale grows. Small MQTT deployments may involve only a few hundred topics, but large industrial robotics environments can contain hundreds of thousands or even millions of active topics. Efficient topic indexing and subscription matching algorithms therefore become critical design considerations.

Modern MQTT brokers typically utilize optimized topic trees, hierarchical indexing structures, trie-based matching algorithms, and memory-efficient lookup tables to ensure high-performance routing. These techniques allow brokers to process millions of messages per second while maintaining low latency.

Connection management represents another major responsibility of the broker. Every MQTT client establishes a connection with the broker and maintains session information throughout its operational lifetime. The broker tracks client identities, authentication credentials, connection status, keep-alive timers, subscription lists, session state, QoS information, and retained message associations.

In large robotic fleets, connection management can become a significant engineering challenge. A broker supporting ten thousand robots must maintain thousands of simultaneous network connections while continuously monitoring connectivity health. Efficient connection management algorithms are therefore essential for scalability.

Session persistence is closely related to connection management. MQTT allows clients to maintain communication state across temporary disconnections. If a robot temporarily loses network connectivity, its subscriptions, queued messages, and session information can be preserved within the broker until reconnection occurs.

Persistent sessions are particularly valuable in robotic environments where wireless communication may be intermittent. Outdoor autonomous vehicles, warehouse robots, inspection robots, and mobile service robots frequently move through areas with varying network quality. Session persistence ensures that communication continuity is maintained despite temporary disruptions.

Message storage represents another important aspect of broker design. The broker may need to store retained messages, persistent session data, offline messages, QoS tracking information, and operational metadata. Storage architecture therefore directly affects system reliability and performance.

Retained messages require the broker to maintain the most recent message associated with each retained topic. When new subscribers join the system, retained messages are immediately delivered to provide current state awareness. Efficient retained message management is especially important in fleet management systems where thousands of robots continuously publish status information.

QoS implementation is one of the most technically demanding responsibilities of the MQTT broker. QoS 0 messages require minimal state management because no acknowledgments are necessary. QoS 1 messages require acknowledgment tracking and retransmission management. QoS 2 messages require transactional state tracking across multiple protocol exchanges.

As QoS levels increase, broker complexity also increases. The broker must maintain message identifiers, delivery state information, acknowledgment queues, retransmission timers, and transaction histories. Careful broker design is therefore necessary to ensure scalability when supporting high volumes of QoS 1 and QoS 2 traffic.

The broker also plays a central role in implementing Last Will and Testament functionality. During connection establishment, clients may register Will Messages that should be published if unexpected disconnection occurs. The broker monitors client connectivity and automatically publishes these messages when failures are detected.

This capability transforms the broker into a distributed fault detection system. Fleet management platforms can immediately detect robot failures, gateway outages, cloud service interruptions, and infrastructure problems without requiring additional monitoring protocols.

Security architecture is one of the most critical components of MQTT broker design. Because the broker acts as the central communication hub, it becomes a primary target for cyberattacks. Unauthorized access could potentially expose sensitive operational information, disrupt fleet operations, compromise safety systems, or enable malicious command injection.

Authentication mechanisms verify client identities before allowing access to broker services. Common authentication methods include username-password authentication, certificate-based authentication, token-based authentication, OAuth integration, enterprise identity management systems, and hardware security modules.

Authorization mechanisms determine which topics each client may publish or subscribe to. Fine-grained access control policies help prevent unauthorized information access and reduce the impact of compromised devices.

For example, a robot may be permitted to publish telemetry information only within its designated topic hierarchy. Fleet operators may have read access to operational topics but not administrative configuration channels. Maintenance personnel may access diagnostic topics while remaining restricted from mission control topics.

Encryption is another essential security feature. MQTT commonly operates over TLS, ensuring confidentiality, integrity, and authenticity of communications. Modern broker deployments frequently enforce TLS encryption for all external connections.

Scalability is one of the defining characteristics of successful MQTT broker architectures. Small deployments may operate effectively with a single broker instance. However, enterprise robotic ecosystems often require significantly greater capacity.

Horizontal scaling techniques distribute workloads across multiple broker instances. Broker clustering allows several servers to operate collectively as a unified communication platform. Messages, subscriptions, session data, retained messages, and client connections may be distributed across the cluster.

Clustering improves fault tolerance as well as scalability. If one broker instance fails, other cluster members continue operating. This redundancy is particularly important in mission-critical robotic systems where communication downtime can directly affect operational safety.

Load balancing is commonly integrated into clustered broker deployments. Incoming client connections are distributed across multiple broker nodes, preventing resource bottlenecks and improving overall system performance.

High availability design extends beyond clustering. Enterprise-grade MQTT infrastructures often incorporate redundant network interfaces, redundant storage systems, geographically distributed broker clusters, automated failover mechanisms, backup services, and disaster recovery procedures.

These capabilities are especially valuable for large-scale industrial robotics deployments operating continuously across multiple facilities.

Performance optimization represents another important area of broker design. MQTT brokers must balance memory consumption, CPU utilization, network bandwidth usage, disk I/O activity, and message latency.

Memory management becomes increasingly important as client counts grow. Session data, subscriptions, retained messages, QoS tracking information, and offline message queues all consume memory resources. Efficient data structures and storage strategies help maximize broker capacity.

CPU utilization is heavily influenced by topic matching algorithms, security processing, message serialization, encryption operations, and routing activities. Optimized implementations minimize processing overhead while maintaining reliability.

Network performance also plays a major role. High-frequency telemetry streams generated by robotic fleets can produce enormous message volumes. Brokers must process these streams efficiently without introducing excessive latency.

Monitoring and observability are essential components of modern broker architecture. Operators require visibility into connection counts, message throughput, subscription activity, memory utilization, CPU usage, network traffic, storage consumption, error rates, authentication failures, and latency metrics.

Most enterprise MQTT brokers expose monitoring interfaces compatible with Prometheus, Grafana, OpenTelemetry, Elastic Stack, and cloud-native observability platforms. These tools provide real-time insight into broker health and operational performance.

Cloud integration has become increasingly important in MQTT broker design. Modern robotic systems frequently connect to cloud platforms for analytics, machine learning, digital twins, fleet coordination, predictive maintenance, and enterprise integration.

Cloud-native MQTT architectures often utilize containerization technologies such as Docker and Kubernetes. These platforms simplify deployment, scaling, monitoring, maintenance, and software upgrades while supporting highly elastic resource allocation.

Edge computing environments introduce additional design considerations. Edge brokers may operate locally within factories, warehouses, hospitals, airports, ports, mines, or manufacturing facilities. These brokers provide low-latency communication even when cloud connectivity is limited or unavailable.

A hierarchical broker architecture is frequently used in large robotic deployments. Local edge brokers handle facility-level communication while synchronizing selected information with centralized cloud brokers. This architecture reduces network dependency while preserving enterprise-wide visibility.

Digital twin systems rely heavily on MQTT broker infrastructure. Real-time state synchronization, operational monitoring, AI-driven analytics, maintenance prediction, simulation updates, and fleet visualization all depend on reliable message distribution. The broker effectively becomes the nervous system connecting physical robots to their digital representations.

Physical AI platforms further increase the importance of sophisticated broker design. Future AI-native robots will continuously exchange telemetry, AI model updates, inference outputs, planning information, multimodal perception data, fleet coordination messages, and cloud-generated intelligence. MQTT brokers must support these increasingly demanding workloads while maintaining low latency and high reliability.

Within ROS 2 ecosystems, MQTT brokers often complement DDS-based communication. DDS provides deterministic real-time communication inside robotic platforms, while MQTT brokers handle cloud integration, fleet management, enterprise connectivity, remote monitoring, and cross-site coordination. This layered architecture combines the strengths of both communication paradigms.

For Hills Robotics platforms, including Indoor AMRs, Outdoor AMRs, Inspection Robots, Security Robots, Mobile Manipulators, Fleet Management Systems, CAD2SCAN Platforms, GPR Inspection Vehicles, Quadruped Robots, Humanoid Robots, and future Cargo UAV systems, MQTT Broker Design forms the foundation of fleet-level communication infrastructure. The broker connects robots with cloud services, AI platforms, maintenance systems, monitoring dashboards, digital twins, ERP systems, manufacturing execution systems, and enterprise analytics environments.

A typical Hills Robotics deployment may utilize edge MQTT brokers within individual facilities to manage low-latency robot communication while synchronizing selected information with centralized cloud brokers. Fleet status, battery health, mission progress, AI analytics, maintenance alerts, software updates, and operational metrics can all flow through this hierarchical communication architecture.

Ultimately, MQTT Broker Design extends far beyond simple message forwarding. The broker serves as a communication coordinator, session manager, security gateway, reliability engine, fault detection platform, scalability foundation, monitoring hub, and integration layer for distributed robotic ecosystems. As robotic fleets continue expanding and Physical AI systems become increasingly connected, MQTT brokers will remain one of the most important infrastructure components enabling reliable, scalable, secure, and intelligent communication across modern autonomous systems.

# 03_04 MQTT Broker Design

## 3.5 MQTT in AMR Telemetry

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

MQTT has become one of the most important communication technologies in modern Autonomous Mobile Robot (AMR) systems because it provides a lightweight, scalable, and reliable mechanism for transmitting telemetry information between robots, fleet management platforms, cloud services, digital twin systems, maintenance infrastructure, and enterprise software environments. As AMR deployments continue to expand from individual robots to large fleets operating across warehouses, factories, hospitals, airports, ports, distribution centers, and smart cities, telemetry communication becomes a critical component of overall system architecture. MQTT offers an efficient solution for collecting, distributing, analyzing, and managing operational data generated by autonomous robots in real time.

Telemetry refers to the automatic measurement, collection, transmission, and analysis of operational data generated by a system. In the context of AMRs, telemetry includes all information that describes the current state, performance, health, location, mission progress, environmental conditions, and operational status of the robot. Telemetry serves as the primary source of situational awareness for operators, maintenance personnel, fleet managers, AI systems, digital twins, and enterprise management platforms.

An AMR continuously generates large amounts of telemetry data during operation. Battery status, charging conditions, motor temperatures, wheel encoder values, localization information, navigation status, obstacle detection events, network quality measurements, CPU utilization, memory consumption, sensor health, fault codes, mission progress indicators, and safety system status all represent telemetry information that may be transmitted through MQTT.

The use of MQTT in AMR telemetry is driven by several important requirements. First, telemetry communication must be lightweight because robots often operate over wireless networks with limited bandwidth. Second, telemetry systems must be scalable because a fleet may contain hundreds or thousands of robots simultaneously publishing information. Third, communication must be reliable enough to support operational monitoring and maintenance activities. Fourth, telemetry architectures must integrate seamlessly with cloud platforms, edge computing infrastructure, AI analytics systems, and enterprise software.

Traditional request-response communication models such as HTTP are not ideally suited for telemetry transmission because telemetry data is continuously generated and distributed to multiple consumers simultaneously. MQTT\'s publish-subscribe architecture provides a much more efficient mechanism for this type of communication.

In an MQTT-based AMR architecture, the robot acts as a publisher. The robot continuously publishes telemetry messages to various topics managed by an MQTT broker. Multiple subscribers may simultaneously consume these messages. Fleet management systems, cloud analytics platforms, maintenance systems, monitoring dashboards, digital twins, AI services, and enterprise resource planning systems can all receive telemetry data without requiring direct communication with the robot.

This architecture significantly reduces communication complexity. The robot does not need to know which systems consume its telemetry information. It simply publishes data to predefined topics. The broker handles message distribution automatically.

A typical AMR telemetry hierarchy begins with robot identification. Each robot is assigned a unique identifier that forms the basis of its MQTT topic structure. For example, an AMR identified as robot001 may publish telemetry using topic hierarchies such as:

fleet/robot001/status

fleet/robot001/battery

fleet/robot001/location

fleet/robot001/navigation

fleet/robot001/mission

fleet/robot001/faults

fleet/robot001/diagnostics

fleet/robot001/performance

This hierarchical structure allows subscribers to consume information at different levels of granularity. A fleet dashboard may subscribe to fleet/+/battery to monitor battery levels across all robots. A maintenance system may subscribe to fleet/+/diagnostics. A digital twin platform may subscribe to fleet/# to obtain comprehensive operational data.

Battery telemetry is among the most important information transmitted by AMRs. Battery state directly affects mission planning, charging operations, fleet availability, maintenance scheduling, and operational efficiency.

Battery telemetry typically includes state of charge, voltage, current, temperature, charge cycles, estimated remaining runtime, charging status, battery health indicators, and energy consumption rates. MQTT enables this information to be continuously streamed to fleet management systems where charging schedules can be optimized automatically.

Localization telemetry represents another major category of AMR data. Modern AMRs continuously estimate their position within operational environments using SLAM algorithms, LiDAR systems, cameras, IMUs, wheel encoders, GNSS receivers, and sensor fusion techniques.

Localization telemetry commonly includes position coordinates, orientation angles, localization confidence metrics, map identifiers, navigation zones, floor levels, velocity estimates, acceleration measurements, and route progress information.

Fleet managers utilize this information to monitor robot locations in real time. Digital twins use localization telemetry to synchronize virtual representations with physical robots. AI systems analyze localization patterns to optimize traffic flow and operational efficiency.

Mission telemetry provides visibility into task execution. AMRs typically receive assignments such as material transport, inventory movement, inspection activities, cleaning operations, delivery services, or collaborative manufacturing support.

Mission telemetry may include mission identifiers, task priorities, estimated completion times, current mission phase, pickup locations, destination coordinates, operational status, task completion percentages, and mission outcome indicators.

Real-time mission visibility enables operators to monitor productivity, identify delays, optimize workflows, and improve resource utilization across large robotic fleets.

Navigation telemetry provides detailed information about autonomous movement behavior. Navigation subsystems continuously generate data related to path planning, obstacle avoidance, route selection, traffic management, localization quality, and motion execution.

Typical navigation telemetry includes planned routes, active waypoints, obstacle encounters, route deviations, navigation confidence levels, path replanning events, traffic congestion indicators, and travel efficiency metrics.

This information helps engineers evaluate navigation performance and identify opportunities for optimization.

Safety telemetry is particularly important in industrial AMR environments. Autonomous robots operate around people, equipment, vehicles, and critical infrastructure. Continuous monitoring of safety-related systems is therefore essential.

Safety telemetry may include emergency stop status, bumper activation events, safety LiDAR states, collision warnings, obstacle detection events, reduced speed zones, safety controller diagnostics, safety relay conditions, and compliance monitoring indicators.

Fleet management systems can use safety telemetry to identify hazardous conditions and initiate corrective actions when necessary.

Mechanical health telemetry supports predictive maintenance strategies. Modern AMRs contain motors, gearboxes, bearings, wheels, brakes, suspension systems, actuators, cooling systems, and power electronics that experience wear over time.

Mechanical telemetry may include motor temperatures, vibration measurements, current consumption, wheel slip indicators, gearbox efficiency metrics, bearing condition estimates, brake wear indicators, and actuator performance statistics.

By continuously monitoring these parameters, maintenance systems can identify emerging failures before catastrophic breakdowns occur.

Computing telemetry has become increasingly important as AMRs evolve into sophisticated AI-enabled platforms. Modern robots frequently include edge computers, GPUs, AI accelerators, embedded controllers, and distributed software systems.

Computing telemetry may include CPU utilization, memory consumption, GPU load, storage capacity, network bandwidth usage, container status, software version information, ROS node health, DDS performance metrics, and AI inference statistics.

This information allows operators to maintain software reliability and optimize computational resource allocation.

Network telemetry is particularly important for connected robotic systems. AMRs often depend on Wi-Fi, private 5G, LTE, Ethernet, mesh networking, or industrial wireless communication technologies.

Network telemetry commonly includes signal strength, packet loss rates, latency measurements, bandwidth utilization, roaming events, connection quality indicators, broker connectivity status, and communication reliability metrics.

Network health directly affects operational reliability and therefore requires continuous monitoring.

MQTT Quality of Service levels play an important role in AMR telemetry design. Different categories of telemetry require different levels of reliability.

High-frequency telemetry streams such as localization updates, IMU measurements, and environmental sensor data often utilize QoS 0. These messages are transmitted with minimal overhead because occasional message loss is acceptable.

Operational notifications such as battery warnings, maintenance alerts, fault reports, and mission status updates commonly utilize QoS 1. Reliable delivery is important, but duplicate messages can generally be tolerated.

Critical operational records such as mission completion confirmations, maintenance transactions, or audit-related information may utilize QoS 2 to guarantee exact delivery.

Retained messages are also widely used in AMR telemetry systems. Battery status, robot availability, software versions, mission state, and operational modes can be published as retained messages. New subscribers immediately receive the most recent information upon connection.

Last Will and Testament functionality further enhances telemetry reliability. Each robot can register an offline notification that is automatically published if communication is unexpectedly lost. Fleet managers immediately become aware of connectivity problems without requiring additional monitoring infrastructure.

Cloud integration represents one of the primary motivations for using MQTT in AMR systems. Major cloud platforms such as AWS IoT Core, Microsoft Azure IoT Hub, Google Cloud IoT services, and enterprise IoT platforms provide native MQTT support.

Telemetry data can therefore flow directly from robots into cloud analytics environments where machine learning algorithms, predictive maintenance engines, digital twins, business intelligence systems, and AI applications process operational information.

Edge computing architectures frequently incorporate MQTT brokers deployed within operational facilities. Local edge brokers provide low-latency telemetry processing while synchronizing selected information with cloud infrastructure. This hybrid architecture balances responsiveness, scalability, and bandwidth efficiency.

Digital twin systems depend heavily on MQTT telemetry. The virtual representation of an AMR must continuously receive updates regarding position, mission status, battery condition, sensor state, operational health, and environmental context. MQTT provides an efficient mechanism for maintaining synchronization between physical and virtual systems.

Physical AI systems will further increase telemetry requirements. Future AMRs will integrate large language models, vision-language-action systems, multimodal perception engines, world models, autonomous reasoning frameworks, and advanced decision-making architectures.

These systems will generate new categories of telemetry including AI confidence scores, reasoning outcomes, model performance metrics, semantic environment understanding, policy execution status, and collaborative intelligence indicators. MQTT\'s scalable architecture provides an effective foundation for transmitting and managing these data streams.

For Hills Robotics platforms, including Indoor AMRs, Outdoor AMRs, Security Robots, Inspection Robots, Mobile Manipulators, CAD2SCAN Systems, GPR Inspection Vehicles, Quadruped Robots, Humanoid Robots, and future Cargo UAV platforms, MQTT telemetry serves as a critical communication layer connecting robots to fleet management infrastructure, cloud services, digital twins, AI analytics platforms, maintenance systems, and enterprise operations centers.

A typical Hills Robotics deployment may utilize ROS 2 and DDS for internal real-time communication while MQTT handles telemetry distribution to external systems. This layered architecture combines deterministic robot control with scalable fleet-level monitoring and cloud integration.

Ultimately, MQTT in AMR telemetry provides the communication foundation necessary for large-scale autonomous robot operations. By enabling lightweight publish-subscribe communication, efficient data distribution, scalable fleet monitoring, predictive maintenance, cloud connectivity, digital twin synchronization, and AI-driven analytics, MQTT transforms telemetry from simple status reporting into a strategic operational intelligence platform. As robotic fleets continue to expand and Physical AI systems become increasingly sophisticated, MQTT-based telemetry architectures will remain a fundamental component of modern autonomous robotic ecosystems.

fleet/robot001/status

fleet/robot001/battery

fleet/robot001/location

fleet/robot001/navigation

fleet/robot001/mission

fleet/robot001/faults

fleet/robot001/diagnostics

fleet/robot001/performance
