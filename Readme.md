# Network Investigation

## Project Overview

This project demonstrates a network traffic investigation conducted using Wireshark to analyze unusual outbound communications from a workstation. The objective was to identify network activity, review communication patterns, extract indicators of interest, and assess potential security risks.

---

## Scenario

The Security Operations Center (SOC) received an alert indicating unusual outbound connections from a workstation in the Finance department.

The affected system, identified as **WS-023**, was observed communicating with multiple external hosts. During the investigation, one external destination appeared unusual and required further analysis.

The goal of this investigation was to determine whether the observed network activity represented normal business communication or a potential security concern.

---

## Investigation Objectives

* Identify the source host responsible for the traffic
* Review network protocols in use
* Analyze DNS activity
* Examine external communications
* Investigate HTTP requests
* Extract indicators of interest
* Assess overall risk
* Provide recommendations

---

## Tools Used

* Wireshark
* TCP/IP Analysis
* DNS Analysis
* HTTP Traffic Analysis

---

## Evidence

* Network Packet Capture (PCAP)
* Network Alert Information

---

## Investigation Methodology

### 1. Initial Traffic Review

The packet capture was loaded into Wireshark and reviewed using:

* Protocol Hierarchy
* Endpoints
* Conversations

This provided visibility into protocol usage and communication patterns.

---

### 2. Source Host Identification

The source of the traffic was identified as:

| Asset      | Value        |
| ---------- | ------------ |
| Hostname   | WS-023       |
| IP Address | 192.168.1.23 |
| Department | Finance      |

---

### 3. DNS Analysis

DNS traffic was reviewed to identify domains queried by the workstation.

Observed domains included:

* [www.microsoft.com](http://www.microsoft.com)
* [www.google.com](http://www.google.com)
* [www.github.com](http://www.github.com)
* time.windows.com
* cdn-update-check.com

Most domains appeared legitimate; however, **cdn-update-check.com** warranted further review.

---

### 4. External Communication Analysis

Conversation statistics identified communication with several external hosts.

Common destinations included:

* Microsoft Services
* Google Services
* GitHub Services
* Public NTP Servers

One external host generated additional interest:

**45.77.188.99**

---

### 5. HTTP Traffic Analysis

Filtering traffic associated with the external IP revealed repeated HTTP requests.

Example:

GET /checkin?id=WS023&seq=0

Host:

cdn-update-check.com

The workstation repeatedly contacted the same external destination throughout the capture.

---

## Key Findings

### Normal Activity

The workstation generated traffic associated with:

* DNS resolution
* NTP synchronization
* HTTPS communications
* Access to common internet services

### Unusual Activity

Repeated outbound HTTP communications were observed between:

* Internal Host: 192.168.1.23
* External Host: 45.77.188.99

Associated Domain:

* cdn-update-check.com

The communication pattern appeared automated due to the repeated check-in requests.

---

## Indicators of Interest

| Type          | Value                |
| ------------- | -------------------- |
| Internal Host | 192.168.1.23         |
| Hostname      | WS-023               |
| Domain        | cdn-update-check.com |
| External IP   | 45.77.188.99         |

---

## Risk Assessment

**Risk Level: Medium**

### Reason

The workstation repeatedly communicated with an unfamiliar external destination. While the traffic pattern appeared unusual, no direct evidence of malware execution, credential theft, or data exfiltration was identified within the available packet capture.

---

## Conclusion

The investigation identified recurring outbound HTTP communications from workstation **WS-023** to external host **45.77.188.99**, associated with the domain **cdn-update-check.com**.

Although the available network evidence did not conclusively indicate system compromise, the communication pattern appeared unusual and warranted further monitoring and validation.

---

## Recommendations

* Review endpoint activity on WS-023
* Validate the legitimacy of the external destination
* Monitor for additional communications
* Investigate related indicators across the environment
* Continue network monitoring for similar activity

---

## Skills Demonstrated

* Network Traffic Analysis
* Wireshark Investigation
* DNS Analysis
* HTTP Analysis
* IOC Identification
* Incident Documentation
* Risk Assessment
* SOC Analyst Investigation Workflow
