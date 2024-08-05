---
title: "POC of SSL vuln in google API"
author: "Alvin Liju"
pubDate: "2024-8-5"
description: "Found out that there is a possiblity of SSL degradation attack in googleapi subdomain"
---


#### Title: Vulnerability Analysis of `jnn-pa.googleapis.com`

**Target:** `jnn-pa.googleapis.com`

#### Overview

This PoC demonstrates the discovery and analysis of security vulnerabilities in the subdomain `jnn-pa.googleapis.com`, specifically focusing on deprecated TLS protocols that pose potential risks to secure communications.

#### Tools Used

- **Sublist3r**: For subdomain enumeration
- **HTTPX**: To verify the status and availability of the domain
- **SSL Labs**: For comprehensive SSL/TLS vulnerability assessment

#### Findings

1. **Subdomain Enumeration**:
    
    - Using Sublist3r, the subdomain `jnn-pa.googleapis.com` was identified.
2. **Domain Availability**:
    
    - Verified the domain's availability using HTTPX with a response status code `200`, indicating that the site is up and running.
3. **SSL/TLS Analysis**:
    
    - SSL Labs test results revealed that the server supports deprecated TLS versions, including:
        - **TLS 1.0**
        - **TLS 1.1**
    
   [SSL scan report](https://i.imgur.com/bJCxWMa.png)
    
    - **Security Grade:** B 
#### Vulnerabilities

1. **Deprecated TLS Versions**:
    
    - **TLS 1.0 and 1.1** are no longer considered secure and are vulnerable to various attacks, including BEAST and POODLE.
    - Continued support for these protocols increases the risk of exploitation by attackers who can intercept or manipulate data.
2. **Potential Attacks**:
    
    - **Man-in-the-Middle (MitM) Attacks**: Older TLS versions are susceptible to interception and eavesdropping.
    - **Downgrade Attacks**: Attackers can force the server to use a less secure protocol version.

#### Mitigations

To address these vulnerabilities and enhance the security posture of `jnn-pa.googleapis.com`, the following mitigations are recommended:

1. **Disable Deprecated TLS Versions**:
    
    - Configure the server to support only modern, secure versions of TLS (e.g., TLS 1.2 and TLS 1.3).
    - Update server configuration files to exclude TLS 1.0 and TLS 1.1 support.
2. **Implement Strong Cipher Suites**:
    
    - Use strong cipher suites that provide forward secrecy and resistance against known cryptographic attacks.
    - Preferred cipher suites include:
        - `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
        - `TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256`
3. **Regularly Update SSL/TLS Libraries**:
    
    - Ensure that SSL/TLS libraries and server software are kept up to date with the latest security patches.
    - Monitor for new vulnerabilities and apply patches promptly.
4. **Conduct Regular Security Audits**:
    
    - Perform regular security assessments to identify and mitigate potential vulnerabilities.
    - Utilize automated scanning tools to detect insecure configurations.
5. **Implement HSTS (HTTP Strict Transport Security)**:
    
    - Enforce HTTPS connections to prevent protocol downgrades and MitM attacks.
    - Set the HSTS header to ensure browsers always connect over HTTPS.

