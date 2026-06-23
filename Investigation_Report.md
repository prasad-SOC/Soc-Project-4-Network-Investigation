# Investigation Report

## Incident Information

| Field               | Value                    |
| ------------------- | ------------------------ |
| Investigation Title | Network Investigation    |
| Alert ID            | SOC-2026-104             |
| Severity            | Medium                   |
| Investigation Type  | Network Traffic Analysis |
| Analyst             | SOC Analyst              |
| Status              | Closed                   |
| Affected Asset      | WS-023                   |
| Source IP           | 192.168.1.23             |

---

# Executive Summary

The Security Operations Center (SOC) received an alert regarding unusual outbound network activity originating from workstation WS-023.

A packet capture (PCAP) containing 1,200 packets was analyzed using Wireshark to determine the nature of the communications and identify any potential security concerns.

The investigation identified repeated HTTP communications between the internal workstation and an unfamiliar external host. While no direct evidence of malware execution, credential theft, or data exfiltration was observed within the available network traffic, the communication pattern appeared unusual and warranted additional review.

---

# Investigation Scope

The purpose of this investigation was to:

* Identify the source host responsible for the traffic
* Review network protocols in use
* Analyze DNS activity
* Investigate external communications
* Identify indicators of interest
* Assess potential risk

---

# Evidence Reviewed

## Network Capture

| Evidence                                | Description                      |
| --------------------------------------- | -------------------------------- |
| network_investigation_1200_packets.pcap | Packet capture used for analysis |

## Analysis Tool

| Tool      | Purpose                                   |
| --------- | ----------------------------------------- |
| Wireshark | Packet analysis and traffic investigation |

---

# Initial Traffic Analysis

Protocol hierarchy statistics were reviewed to understand the overall traffic composition.

## Protocol Summary

| Protocol | Packets |
| -------- | ------- |
| IPv4     | 1200    |
| TCP      | 650     |
| UDP      | 550     |
| DNS      | 300     |
| NTP      | 250     |
| HTTP     | 120     |

### Observation

The traffic primarily consisted of DNS, TCP, HTTP, and NTP communications commonly observed on a workstation connected to the internet.

---

# Source Host Identification

Traffic analysis identified a single internal workstation generating the captured communications.

| Asset      | Value        |
| ---------- | ------------ |
| Hostname   | WS-023       |
| IP Address | 192.168.1.23 |
| Department | Finance      |

### Observation

The workstation was responsible for all observed outbound communications within the capture.

---

# DNS Analysis

DNS traffic was reviewed to identify domains contacted by the workstation.

## Observed Domains

* [www.microsoft.com](http://www.microsoft.com)
* [www.google.com](http://www.google.com)
* [www.github.com](http://www.github.com)
* time.windows.com
* cdn-update-check.com

### Observation

Most domains appeared consistent with legitimate workstation activity. However, the domain **cdn-update-check.com** was identified as requiring additional review due to its unfamiliar naming convention.

---

# External Communication Analysis

Conversation statistics were reviewed to identify the external hosts contacted by WS-023.

## External Destinations

| IP Address      | Purpose                    |
| --------------- | -------------------------- |
| 8.8.8.8         | DNS Resolution             |
| 20.112.52.29    | Microsoft Services         |
| 142.250.193.132 | Google Services            |
| 140.82.121.4    | GitHub Services            |
| 13.86.101.172   | Windows Time Service       |
| 129.6.15.28     | NTP Service                |
| 45.77.188.99    | Unidentified External Host |

### Observation

The destination **45.77.188.99** generated repeated communications and became the primary focus of the investigation.

---

# HTTP Traffic Analysis

Traffic associated with the external host 45.77.188.99 was analyzed.

## Example Request

GET /checkin?id=WS023&seq=0 HTTP/1.1

Host: cdn-update-check.com

### Observation

Multiple HTTP GET requests were observed between the workstation and the external host.

The communication pattern appeared repetitive and automated due to the incremental sequence values present in the requests.

Examples included:

* /checkin?id=WS023&seq=0
* /checkin?id=WS023&seq=1
* /checkin?id=WS023&seq=2

---

# Indicators of Interest

## Internal Asset

| Type       | Value        |
| ---------- | ------------ |
| Hostname   | WS-023       |
| IP Address | 192.168.1.23 |

## External Indicators

| Type       | Value                |
| ---------- | -------------------- |
| Domain     | cdn-update-check.com |
| IP Address | 45.77.188.99         |

---

# Risk Assessment

## Risk Level

Medium

## Justification

The workstation repeatedly communicated with an unfamiliar external destination using HTTP requests.

Although the communication pattern appeared unusual, the packet capture did not provide sufficient evidence to confirm:

* Malware execution
* Command and control activity
* Data exfiltration
* Credential theft

Further investigation would be required to validate the legitimacy of the destination and determine whether the activity is authorized.

---

# Conclusion

Analysis of the packet capture identified recurring outbound communications between workstation WS-023 (192.168.1.23) and external host 45.77.188.99 associated with the domain cdn-update-check.com.

The observed communication pattern was repetitive and appeared automated. However, no direct indicators of compromise were identified within the available network evidence.

Based on the collected evidence, the activity was assessed as suspicious but not conclusively malicious.

---

# Recommendations

1. Review endpoint activity on WS-023.
2. Validate whether communication with cdn-update-check.com is authorized.
3. Monitor the host for continued outbound communications.
4. Search for the identified indicators across the environment.
5. Continue monitoring for similar network behavior.

---

# Final Assessment

**Status:** Closed

**Risk Rating:** Medium

**Verdict:** Suspicious Network Communication Observed – Additional Monitoring Recommended
