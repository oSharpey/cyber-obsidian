# Security Operations

## The Attack Landscape
- "There are only two types of companies: those that have been hacked, and those that will be." - Robert Mueller (former FBI Director)
- Attackers are numerous and evolving
- No system is completely secure
- Many attacks are unpredictable (file-less, malware-less, complex anti-detection/anti-forensics, multi-stage)
#### Cyber Kill Chain (Lockheed Martin)
1. Reconnaissance
2. Weaponisation
3. Delivery
4. Exploitation
5. Installation
6. Command and Control
	1. Privilege Escalation
	2. Lateral Movement
7. Actions and Objectives
#### MITRE Frameworks
- ATT&CK: adversary tactics and techniques
- D3FEND: countermeasure knowledge graph
- ATLAS: TTPs for AI-based systems
## Security Operations Center (SOC)
#### Implementation Options
- In-house SOC
- Virtual SOC
- Managed Security Service Provider (MSSP)
#### Roles and Responsibilities
- Monitoring (SIEM)
- Detection engineering
- Threat intelligence
- Security incident response
- Information risk management
- Information assurance (IA)
- Information security compliance
- Security governance

#### SOC Team Structure
1. Tier 1 Security Analyst
   - Manages monitoring tools, handles alerts, and performs initial triage
   - Conducts vulnerability management and forwards complex issues to Tier 2

2. Tier 2 Security Analyst (Incident Responder)
   - Investigates escalated alerts and leverages threat intelligence
   - Directs remediation and recovery efforts

3. Tier 3 Expert Security Analyst (Threat Hunter)
   - Conducts proactive threat hunting and attack surface management
   - Performs penetration tests and optimizes security tools

4. Tier 4 SOC Manager
   - Oversees SOC operations, manages escalations, and communicates with stakeholders
   - Ensures compliance, measures performance, and develops SOC strategy

## SOC Process

1. Asset Discovery
2. Implement security controls
3. Event Classification & Triage
4. Prioritization & Analysis
5. Remediation & Recovery
6. Assessment & Audit

## Defence-in-depth

- Layered security approach
- Tools: Vulnerability scanner, Behavioral monitoring, IDS/IPS, EDR/XDR, SIEM, SOAR

## SOC Monitoring Infrastructure

- Cover network traffic and logs/alerts at network devices
- Monitor events/alerts at endpoints
- Centralized collection, correlation, and analysis
- Informed by threat modeling techniques (STRIDE+DREAD, PASTA, VAST, Trike, Attack trees)
