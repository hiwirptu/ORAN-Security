**Security in O-RAN**
![image](https://github.com/hiwirptu/ORAN-Security/assets/164016759/e5e80c9d-ce67-4bff-8769-b873bebdc53f)

**Architecture-related Risks for 5G/6G Security**

The architecture components and associated risks in 5G (and prospective 6G) are as follows:

1. **Service-Based Architecture (SBA):** Decomposed, virtualized, and distributed network functions pose challenges for network security as the independence of network functions from infrastructure can lead to vulnerabilities.

2. **Application Programming Interfaces (API):** Poorly encrypted or inadequately secured APIs can jeopardize network resources, making them susceptible to attacks.

3. **Private and Corporate 5G Networks:** If not adequately protected, these networks can become sources of attacks for the network segments they are connected to, compromising overall network security.

4. **Multi-Access Edge Computing (MEC):** Security management becomes challenging in decentralized information processing, such as edge computing, as significant network parts can be vulnerable to attacks from various locations.

5. **Radio Access Network (RAN) and Open-RAN:** The inherent exposure of the radio segment to attacks related to the transmission medium poses significant security challenges. Additionally, the open specification of the radio interface (O-RAN) introduced in the 5G network raises concerns about the security of O-RAN applications, particularly in the physical or MAC layer.

The first three issues are akin to general software architecture and application security problems, while the last two specifically address RAN and its novel features, including openness and intelligence.

**Opportunities**

The O-RAN architecture facilitates running specialized programming modules/applications (xApps) in Near-Real-Time RAN Intelligent Controller (Near-RT RIC). These xApps can continuously monitor and analyze security threats, safeguarding RAN from malicious and illegal access to network segments. They enable faster threat detection, reducing network operation disruption. Importantly, xApps can target specific types of threats in a given network. The distributed architecture of the 5G/6G network and the use of MEC modules allow for threat detection closer to their occurrence, minimizing delay and control data volume.

**Radio Segment Threats**

- **Jamming:** Various types including normal, delusive (such as Primary User Emulation - PUE), random, responsive, go-next, and control-channel jamming can inject unwanted signals into communication or control channels.
  
- **Denial of Service (DoS) and Distributed DoS (DDoS):** Exhausting network resources, causing traffic congestion, or wasting network resources, making it impossible to serve users.
  
- **Signaling Storm:** Seizing resources for the transmission of signaling information in the control plane.
  
- **Eavesdropping and Traffic Analysis:** Intercepting users’ messages, analyzing them, or changing their content, such as by creating a fake base station.
  
- **Man in the Middle (MITM) Attacks:** Where adversaries intercept users’ messages, analyze, or alter their content.
  
- **Free-riding (Spoofing) Attacks:** To conserve local computing resources, make up for lack of necessary data, or avoid violating data privacy laws.

**AI/ML-related Threats**

- **Poisoning Attacks**
- **Evasion Attacks**
- **Inference Attacks**

![image](https://github.com/hiwirptu/ORAN-Security/assets/164016759/c12e0795-8dd9-49b6-8dfe-5c68d8abcd0b)

**AI for O-RAN Security**

The O-RAN architecture enables the deployment of xApps in Near-RT RIC, which can continuously monitor and analyze security threats, safeguarding RAN from malicious and illegal access to network segments.

This setup facilitates swift threat detection, preempting their impact on the entire network. xApps can be tailored to address specific threat types, enhancing detection capabilities closer to their origin.

xApps developed for RAN and O-RAN security should adhere to best practices for network cybersecurity. These practices encompass a zero-trust approach, continuous monitoring, security tests, data protection, privacy measures, and defensive mechanisms.

**Jamming**

Jamming poses challenges, particularly for Ultra-Reliable, Low Latency Communication (URLLC) traffic, where delay can be unacceptable. Moreover, the simplicity and low cost of executing jamming attacks increase their likelihood.

*Solution:* A two-dimensional distribution of Channel Quality Indicator (CQI) and Reference Signals Received Power (RSRP) values can help identify anomalies indicative of jamming. Reports of CQI and RSRP are collected for a designated cell, with consideration for a specified memory size reflecting the valid time horizon. This ensures the rejection of older reports collected under varying propagation conditions or interference levels. If a report is not categorized as collected under jamming, it is incorporated into the current CQI-RSRP distribution estimate. Each new report is compared against the current distribution, assuming a local Gaussian approximation with a predefined false alarm probability. If a report deviates significantly from the distribution beyond the constant false alarm threshold, it indicates the presence of jamming.
![image](https://github.com/hiwirptu/ORAN-Security/assets/164016759/a5fc7b45-60c9-4501-aea3-d7c3bf0950cb)

It is highly effective in the presence of highly damaging broadband jamming. The probability of false jamming detection is relatively low. 

The other issue is the mitigation of the detected jamming effects. Multiple solutions are possible here, from major base station reconfigurations, such as carrier frequency change, to Modulation and Coding Schemes (MCS) adaptations. 

Jammers should be automatically reported to the security monitoring entity and responsible authorities.

**Signaling Storm**

When executing this type of attack, devices, authorized or not, repeatedly attempt to connect to the network, congesting the Random Access Channel (RACH). Additionally, due to the multi-stage nature of the handshaking procedure, redundant control information is generated in both the access and core network.

The key to combating a Signaling Storm attack lies in promptly locating the source of the threat and deciding to reject connection requests at an early stage of registration.

In detecting a Signaling Storm attack, Key Performance Indicator (KPI) profiles play a vital role. These profiles are generated in Near-RT RIC and dynamically updated to capture statistics of network access requests and Timing Advance (TA) parameters of each User Equipment (UE), especially relevant for IoT devices. Anomaly values are computed based on these profiles and the observed number of access requests, utilizing a weighted version of the DBSCAN grouping algorithm. This algorithm determines the presence or absence of a Signaling Storm attack and estimates the adversary's proximity to the gNodeB (E2 Node). An increased number of Msg2: Random Access Response messages containing the same TA parameter measured by the gNodeB (E2 Node) indicates adversary UEs. Connection requests associated with a specific TA may be rejected.

The detection process consists of two phases: the training phase and the inference phase. During the training phase, KPI profiles are constructed using Random Access responses forwarded to Near-RT RIC from the gNodeB via the E2 interface. Parameters of the detection algorithm, such as the anomaly metric threshold, are fine-tuned in this phase. In the inference phase, detection takes place at the access network level, necessitating the collection of Msg2 messages for a period aligned with the validity of the KPI profile. If adversary activity is detected, the xApp leverages the E2 interface to inform the gNodeB (E2 Node) to reject connection requests from the identified UE, thereby halting UE authentication within the core network.

![image](https://github.com/hiwirptu/ORAN-Security/assets/164016759/cad5e5ba-b4dc-4cfc-8ba6-4c3a0a694878)
