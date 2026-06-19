# Network Traffic Analysis & Protocol Identification

## 🎯 Project Objective
The goal of this project was to capture live network traffic from a local interface using Wireshark and analyze specific protocols. The focus was on isolating and analyzing Domain Name System (DNS) queries and Transport Layer Security (TLS) traffic to understand how data moves securely across a network.

---

## 🛠️ Tools & Environment
* **Network Analyzer:** Wireshark v4.x
* **Operating System:** Linux
* **Target Traffic:** Local Wi-Fi/Ethernet interface traffic

---

## 🚀 Step-by-Step Methodology & Findings

### Step 1: Traffic Capture Setup
I launched Wireshark, selected my active network interface, and began a live packet capture. To generate traffic for analysis, I performed standard web lookup activities by navigating to various domains in a web browser.

---

### Step 2: Isolating DNS Queries
To find the DNS traffic generated when my browser looked up domain names, I applied a protocol filter.

* **Wireshark Display Filter:** `dns`
* **Analysis:** 
  * I located the **Standard query** for the domain name `www.google.com`.
  * I analyzed the **Standard query response** returned from the local DNS server (`192.168.0.1`).
  * **Key Finding:** The response successfully mapped the requested domain name to its corresponding IP address.

---

### Step 3: Analyzing Encrypted Web Traffic (HTTPS/TLS)
Because modern websites automatically encrypt data, typing a standard `http` filter yielded 0 results. I adjusted my methodology to capture modern encrypted web traffic.

* **Wireshark Display Filter:** `tls`
* **Analysis:** 
  * **Handshake Identification:** I successfully isolated the **Client Hello** messages using TLSv1.3 and QUIC protocols to establish a secure connection with `www.google.com`.
  * **Data Confidentiality:** Subsequent packets are categorized as **Application Data**. The contents of the requests are completely encrypted, proving that an attacker sniffing this network segment cannot read the data passing through.

---

## 🛡️ Cybersecurity Observations & Security Takeaways
1. **The Danger of Plaintext:** While DNS queries reveal the domains a user visits, actual web traffic must be encrypted. If unencrypted HTTP traffic were present, it would expose user activity to eavesdropping (Sniffing attacks).
2. **Encrypted Alternatives:** Organizations must enforce **HTTPS** (via TLS/SSL) and consider **DNS over HTTPS (DoH)** or **DNS over TLS (DoT)** to prevent third parties from mapping user browsing habits or tampering with web requests.
3. **Baseline Monitoring:** Understanding what normal DNS and TLS traffic looks like is essential for a security analyst to spot anomalies, such as data exfiltration or command-and-control (C2) beacons.
