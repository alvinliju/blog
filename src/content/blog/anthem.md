---
title: "Introduction to SIEM and IR"
author: "Alvin Liju"
pubDate: "2024-7-10"
description: "Introduction to SEIM and Incident Response"
---


## Introduction to SIEM

SIEM (Security Information and Event Management system) is a powerful tool designed to collect data from various endpoints and network devices, store it centrally, and perform correlation analysis. This overview covers the essential concepts needed to understand SIEM and its operation.

### Why Do We Need a SIEM?

Imagine managing a network with multiple Linux systems. As a cybersecurity professional, you need to monitor these systems' activities and their interactions outside the network. We can categorize these logs into two types:

**Host-Centric Logs:**
- File access by users
- File access timestamps
- Authentication attempts
- Process executions
- PowerShell executions

**Network-Centric Logs:**
- SSH connections
- FTP connections
- Web traffic

Network devices generate hundreds of logs per second, making manual analysis overwhelming, especially during an incident. SIEM consolidates all these logs into a single platform, allowing for correlation, real-time alerts, 24/7 monitoring, and data visualization.

### Log Ingestion

![Log Ingestion in SIEM](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/593abd2bfd9fb31329bd1a6a80bf5ee0.png)

Logs provide valuable information for identifying security issues. SIEM solutions use various methods to ingest logs, including:

1. **Agent/Forwarder:** A lightweight tool installed on endpoints that captures and sends important logs to the SIEM server. Splunk refers to this as a forwarder.
2. **Syslog:** A widely used protocol for real-time data collection from systems like web servers and databases, sending it to a centralized destination.
3. **Manual Upload:** Some SIEM solutions, like Splunk and ELK, allow users to ingest offline data for quick analysis. Once ingested, the data is normalized for analysis.
4. **Port-Forwarding:** SIEM solutions can be configured to listen on specific ports, with endpoints forwarding data to the SIEM instance on these ports.

### Components of Splunk

Splunk has three basic components:

1. **Forwarder:** A lightweight agent/program installed on endpoints to collect and forward necessary system logs to the main Splunk server.
    - Examples: Web servers, Windows machines (generating Event Logs, PowerShell, Sysmon data), Linux hosts, databases (generating DB connection requests, responses, and errors).

2. **Indexer:** Organizes and normalizes data, storing it in specific fields for easy retrieval, making it simple to search and analyze.

3. **Search Head:** An input bar for searching indexed logs using Splunk's Search Processing Language (SPL).

### SIEM in Incident Response (IR)

SIEM is crucial in the Incident Response (IR) process due to its comprehensive capabilities:

1. **Real-Time Monitoring and Alerts:** SIEM continuously monitors network activity, generating real-time alerts for suspicious activities. This immediate notification allows for swift incident detection.

2. **Data Correlation:** By correlating logs from different sources, SIEM can identify patterns and link related events, providing a broader understanding of incidents.

3. **Forensic Analysis:** SIEM stores historical data, enabling detailed forensic analysis to trace the root cause of an incident and understand its impact.

4. **Automated Response:** Some SIEM solutions integrate with other security tools to automate responses to certain types of incidents, enhancing the efficiency of the IR process.

5. **Reporting and Compliance:** SIEM generates reports that help in compliance with regulatory requirements and provide valuable insights for post-incident reviews.

### Incident Handling Lifecycle

**1. Preparation:**
   - Documenting requirements
   - Defining policies
   - Incorporating security controls (e.g., EDR, SIEM, IDS, IPS)
   - Hiring and training staff

**2. Detection and Analysis:**
   - Detecting incidents
   - Analyzing incidents
   - Investigating alerts from security controls
   - Threat hunting

**3. Containment, Eradication, and Recovery:**
   - Preventing incident spread
   - Securing the network
   - Isolating infected hosts
   - Removing infection traces
   - Regaining control

**4. Post-Incident Activity/Lessons Learned:**
   - Identifying security weaknesses
   - Improving security measures
   - Updating detection rules
   - Training staff


### We investigate logs with SEIM according to the Cyber-Kill-Chain 

### Cyber-Kill Chain

1. Reconnaissance
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command & Control
7. Actions on Objectives

### Conclusion

SIEM is an indispensable tool in cybersecurity, especially in Incident Response. It enables real-time monitoring, efficient data correlation, detailed forensic analysis, automated responses, and comprehensive reporting. By integrating SIEM into your cybersecurity strategy, you can enhance your organization's ability to detect, analyze, and respond to security incidents effectively, ensuring a robust defense against cyber threats.