# FAANG SOC Analyst Interview Master Q&A Bank

---

## 🔹 Foundations & Networking (Q1–Q25)

### 1. Explain the OSI model and its 7 layers.  
   The **OSI model** helps us understand **how data moves through a network**, broken into 7 layers:
   - **Physical** – Deals with **hardware**, cables, and how **bits** are transmitted.  
    _Example: Ethernet cables, fiber optics._
- **Data Link** – Handles **MAC addresses** and **frame delivery** within a local network.  
    _Example: Switches, ARP._
- **Network** – Responsible for **IP addressing** and **routing** packets across networks.  
    _Example: Routers, IP, ICMP._
- **Transport** – Ensures **reliable data transfer** with **TCP/UDP**.  
    _Example: Port numbers, TCP 3-way handshake._
- **Session** – Manages and maintains **connections** between applications.  
    _Example: Session tokens, authentication._
- **Presentation** – Translates data formats, handles **encryption/decryption**.  
    _Example: SSL/TLS, JPEG, ASCII._
- **Application** – Closest to the user; enables **network services**.  
    _Example: HTTP, DNS, SMTP._

- Helps trace **where an attack is happening** (e.g., Layer 3 for DDoS, Layer 7 for web attacks).
- Aids in **log analysis**, **packet inspection**, and **incident response**.
- Crucial for understanding **how malware communicates** over the network.

---
### 2. Map common attacks to OSI layers.  
#### 🧠 OSI Layers & Common Attacks

| **Layer** | **Name**     | **Common Attacks**                                                                            |
| --------- | ------------ | --------------------------------------------------------------------------------------------- |
| **7**     | Application  | - **Phishing**  <br>- **SQL Injection**  <br>- **XSS**  <br>- **Remote Code Execution (RCE)** |
| **6**     | Presentation | - **SSL/TLS Exploits**  <br>- **Man-in-the-Middle (MITM)** (e.g., weak encryption)            |
| **5**     | Session      | - **Session Hijacking**  <br>- **Session Fixation**                                           |
| **4**     | Transport    | - **Port Scanning**  <br>- **TCP SYN Flood**  <br>- **UDP Flood**                             |
| **3**     | Network      | - **IP Spoofing**  <br>- **DDoS (e.g., ICMP Flood)**  <br>- **Routing Attacks**               |
| **2**     | Data Link    | - **MAC Spoofing**  <br>- **ARP Spoofing/Poisoning**                                          |
| **1**     | Physical     | - **Cable Tapping**  <br>- **Hardware Keyloggers**  <br>- **Jamming (Wireless)**              |

---

### 3. Explain the TCP/IP model and its 4 layers.  
The **TCP/IP model** is a simplified version of the OSI model and is what the internet actually uses. It has **4 layers**, each responsible for a part of how data moves across networks.
#### 1. Network Access Layer

- This is where data **physically moves**—through cables, Wi-Fi, etc.
- It also deals with **MAC addresses** and how devices talk on a local network.
- Think: **Switches, Ethernet, ARP**.

🔍 **As a SOC Analyst**: You monitor this layer for things like **MAC spoofing** or unauthorized device connections.

#### 2. Internet Layer

- Handles **IP addresses** and **routing**.
- It figures out the **best path** for data to reach its destination across networks.
- Think: **IP, ICMP (ping), routers**.

🔍 Watch for: **IP spoofing**, **DDoS (e.g., ICMP flood)**, **routing anomalies**.

#### 3. Transport Layer

- Ensures that **data is delivered reliably** (TCP) or quickly (UDP).
- Manages **ports**, **connections**, and **error checking**.
- Think: **TCP, UDP, port numbers**.
    
🔍 Key for detecting: **Port scans**, **SYN floods**, and **abnormal port activity**.

#### 4. Application Layer

- This is where **users interact with the network** through apps.
- It includes things like **web browsing**, **email**, and **DNS lookups**.
- Think: **HTTP, DNS, SMTP**.
    
🔍 Common attacks: **Phishing, SQL injection, DNS tunneling, malware communications**.

---
#### 4. Explain TCP vs UDP and their security implications.
#### ✅ TCP vs UDP – At a Glance

| Feature         | **TCP (Transmission Control Protocol)**         | **UDP (User Datagram Protocol)**  |
| --------------- | ----------------------------------------------- | --------------------------------- |
| **Connection**  | **Connection-oriented** (requires handshake)    | **Connectionless** (no handshake) |
| **Reliability** | Ensures **data delivery**, order, and integrity | No guarantee of delivery or order |
| **Speed**       | Slower (due to checks and acknowledgments)      | Faster (less overhead)            |
| **Use Cases**   | Web browsing (HTTP/S), email, file transfer     | Streaming, VoIP, DNS, gaming      |
| **Overhead**    | Higher (due to connection management)           | Lower                             |
#### **TCP:**

- ✅ **Pros:**
    - Predictable state (due to handshake and session tracking).
    - Easier to detect anomalies like **TCP SYN floods**, **session hijacking**, or **retransmission attacks**.
        
- ❌ **Cons:**
    - **SYN Floods** can exhaust server resources (DoS).
    - **Session hijacking** possible if encryption isn't enforced.
        
#### **UDP:**

- ✅ **Pros:**
    - Minimal overhead; useful where speed is more important than reliability.

- ❌ **Cons:**
    - Easier to spoof (no handshake).
    - Harder to track or correlate.
    - Often used in **amplification attacks** (e.g., **DNS**, **NTP**, **SSDP** reflection).
    - **Data exfiltration** via UDP (e.g., DNS tunneling).

TCP is reliable and easier to monitor, which helps in detecting session-based attacks like hijacking or SYN floods. UDP is faster but riskier—it lacks connection state, making it a favorite for DDoS amplification and covert tunneling. As a SOC analyst, I closely monitor high-volume UDP traffic and failed TCP handshakes for potential threats.
    
---

#### 5. Walk me through the TCP 3-way handshake.  
It's the process that two computers use to **start a reliable connection** over the internet before they send data.

#### 🔁 The 3 Steps:

1. **SYN** –  
    The client (like your computer) wants to connect to a server.  
    It sends a **SYN** (short for "synchronize") message to say:  
    _“Hey, I want to start a connection.”_
    
2. **SYN-ACK** –  
    The server gets the SYN, and replies with **SYN-ACK**:  
    _“Got it. I'm ready too. Are you still there?”_
    
3. **ACK** –  
    The client replies back with **ACK**:  
    _“Yes, I’m here. Let’s start talking!”_  
    After this, the connection is open, and they can start sending real data.
    
##### 🧠 Why It Matters in Security (for a SOC Role):

- You can spot suspicious activity by looking at incomplete handshakes.  
    For example, **lots of SYNs but no ACKs** might mean someone is trying a **SYN flood attack** (a type of DoS attack).

---

#### 6. What happens during a TLS/SSL handshake?  
##### ✅ What Is the TLS/SSL Handshake?

It's the process that two systems (like your browser and a website) use to:
1. **Agree on how to talk securely**, and
2. **Make sure they’re talking to the right person**,  
    **before** they start sharing sensitive information (like passwords or payment info).
    
#### 🔐 Step-by-Step Breakdown:

##### 1. **Client Hello**
- Your computer says:
    > “Hi, I want to connect securely. Here are the encryption methods I support.”
    
##### 2. **Server Hello**
- The server responds:
    > “Cool, let’s use this encryption method. Here’s my certificate to prove who I am.” 
- That **certificate** includes the server's **public key** and is usually signed by a **trusted Certificate Authority (CA)**.
    
##### 3. **Certificate Check**
- Your computer checks:
    > “Is this server's certificate valid and trusted?”  
    > (If it’s not, you’ll see a warning like "Your connection is not private.")
    
##### 4. **Key Exchange**
- Your computer and the server agree on a **shared secret key** using the server’s public key.  
    This key will be used to **encrypt the actual data**.
    
##### 5. **Finished**
- Both sides say:
    > “I’ve set up encryption and I’m ready.”
- From here, they start **secure communication** — like loading the webpage, logging in, etc.
    
#### 🔎 Why It Matters for a SOC Analyst:

- Attackers might try to intercept or fake certificates (**man-in-the-middle attacks**).
- You should monitor for:
    - **Expired or self-signed certificates**
    - **Weak cipher suites**
    - **Unusual certificate authorities**
        
- Also, TLS versions matter: older versions (like SSL or TLS 1.0/1.1) are **no longer secure** and should be flagged.
   
---

#### 7. How do SSL/TLS certificates work?  

SSL/TLS certificates are used to **establish a secure, encrypted connection** between a client (like a browser) and a server. Here's a step-by-step breakdown:

#### 🧩 1. **Purpose of SSL/TLS**

- SSL (Secure Sockets Layer) and TLS (Transport Layer Security) are cryptographic protocols designed to:
    - Ensure **confidentiality** (data is encrypted),
    - **Integrity** (data is not tampered with), and
    - **Authentication** (you know you're talking to the right server).
        
#### 🔑 2. **How the Certificate Works**

An SSL/TLS certificate is a **digital certificate** issued by a trusted Certificate Authority (CA). It includes:

- The domain name,
- The public key,
- The certificate's expiration date,
- And the digital signature of the CA.
    
#### 🔄 3. **TLS Handshake (Simplified Flow)**

When a client connects to a secure website:
1. **Client Hello**: The client sends a request to the server with supported TLS versions and cipher suites.
2. **Server Hello**: The server responds with:
    - Its SSL/TLS certificate,
    - Selected cipher suite,
    - (Optionally) a request for client certificate.    
3. **Certificate Validation**:
    - The client checks that the certificate is:
        - Issued by a **trusted CA**,
        - Not expired,
        - Matches the domain,
        - Not revoked (via OCSP or CRL).        
4. **Key Exchange**:
    - Using asymmetric encryption (RSA, ECDHE, etc.), the client and server **negotiate a session key** (symmetric key).
5. **Session Established**:
    - Once verified, both parties use the **shared symmetric session key** for fast encrypted communication.
        
#### 🛡️ 4. **Security Relevance for SOC**

As a SOC Analyst, here's what I care about:
- **Certificate Expiry Monitoring**: Expired certs can lead to failed HTTPS connections and security alerts.
- **Self-Signed or Untrusted Certs**: Could indicate man-in-the-middle (MitM) attacks.
- **Weak Ciphers / Protocols**: Outdated SSL versions (like SSLv3) are insecure and must be flagged.
- **Cert Transparency Logs**: Help detect rogue/malicious certificates.
   
---
#### 8. Explain the difference between symmetric and asymmetric encryption.  
##### 🔐 Difference Between Symmetric and Asymmetric Encryption

|Feature|Symmetric Encryption|Asymmetric Encryption|
|---|---|---|
|🔑 Keys|Uses **one key** for both encryption and decryption|Uses a **key pair**: public key (encrypt) and private key (decrypt)|
|🔄 Speed|**Faster** – suitable for bulk data encryption|**Slower** – computationally heavy|
|📦 Use Case|Encrypting large amounts of data|Secure key exchange, digital signatures|
|🔓 Key Sharing|Key must be **securely shared** between parties|No need to share private key; only public key is distributed|
|🔧 Algorithms|AES, DES, ChaCha20|RSA, ECC, DSA|
##### ✅ Symmetric Encryption (One Key)

- **Same key** is used to encrypt and decrypt.
- Example: `AES-256`
- 🔒 Must be kept **secret** and shared securely.
- 🔄 Used after handshake in **TLS sessions** for fast data encryption.
    
##### ✅ Asymmetric Encryption (Key Pair)

- Involves a **public key** (shared) and a **private key** (kept secret).
- Example: `RSA`, `Elliptic Curve (ECDSA/ECDHE)`
- 🔐 Used to:
    - Encrypt data (e.g., session keys),
    - Verify digital signatures,
    - Perform key exchange in TLS handshakes.

---
#### 9. What is hashing vs encryption vs encoding?  

##### 🔍 Hashing vs Encryption vs Encoding

|Feature|**Hashing**|**Encryption**|**Encoding**|
|---|---|---|---|
|**Purpose**|Ensure **data integrity**, fingerprinting|**Confidentiality** – protect data|**Data transformation** for transport/storage|
|**Reversible?**|❌ **No** – One-way|✅ **Yes** – Two-way (with key)|✅ **Yes** – Always reversible|
|**Key Required?**|❌ No|✅ Yes (symmetric or asymmetric)|❌ No|
|**Output Type**|Fixed-size hash (e.g., 256 bits)|Encrypted binary or text|Readable text (Base64, etc.)|
|**Examples**|SHA-256, MD5, SHA-1|AES, RSA, ChaCha20|Base64, ASCII, URL encoding|
##### 🔐 1. **Hashing**

- A **one-way function** that converts input data into a **fixed-length digest**.
- **Cannot be reversed**.
- Used to:
    - Store passwords securely (with salt),
    - Verify integrity (e.g., file hashes, digital signatures),
    - Generate checksums.
        
✅ **Same input always gives same output**  
❌ You can’t get the original data from the hash.

🧠 _SOC relevance_: Watch for hash leaks in logs or memory (e.g., password hash dumps), and validate file integrity.

##### 🔒 2. **Encryption**

- A **two-way** process to **protect confidentiality** of data.
- Requires a **key**:
    - **Symmetric**: Same key to encrypt/decrypt (e.g., AES),
    - **Asymmetric**: Public key to encrypt, private key to decrypt (e.g., RSA).
- Only someone with the right key can decrypt the data.
    
🧠 _SOC relevance_: Detect use of **weak encryption**, **unencrypted sensitive data**, or **malicious encryption (ransomware)**.

##### 🔤 3. **Encoding**

- Converts data into a **different format** for compatibility or safe transmission (e.g., over the web or in email).
- **Not secure** — it’s just a format change.
- Anyone can **decode** it — no key needed.
    
Examples:
- **Base64**: Common for email or JSON transport.
- **URL encoding**: Replaces spaces and special chars (`%20`).
    
🧠 _SOC relevance_: Base64 encoding is often used to **obfuscate malware payloads or commands**. Not always malicious — context matters.

---
#### 10. Explain how digital signatures work.  

##### 🔐 What Is a Digital Signature?

A **digital signature** is a cryptographic mechanism that provides.

|Property|Purpose|
|---|---|
|✅ **Authentication**|Proves who sent the message|
|✅ **Integrity**|Proves message wasn’t altered|
|✅ **Non-repudiation**|Sender **cannot deny** sending it|

It’s the digital equivalent of a **handwritten signature**, but way more secure and verifiable.
#### ⚙️ How Digital Signatures Work (Step-by-Step)

##### ✍️ 1. **Signing (Sender Side)**
1. **Hash the Message**:
    - Create a digest using a hash function like `SHA-256`.
    - This ensures the signature is tied to the exact content.
        
2. **Encrypt the Hash with Private Key**:
    - The sender uses their **private key** to encrypt the hash.
    - This encrypted hash is the **digital signature**.
        
3. **Send**:
    - The sender sends:
        `[ Original Message + Digital Signature ]`
        

#### 🔍 2. **Verification (Receiver Side)**
1. **Hash the Received Message**:
    - The receiver hashes the original message using the same algorithm.
        
2. **Decrypt the Signature Using Sender’s Public Key**:
    - This gives the original hash (created by the sender).
        
3. **Compare the Two Hashes**:
    - If they match → ✅ Signature is valid.
    - If they don’t → ❌ Data was tampered with, or sender isn't authentic.
        

---
#### 11. Explain ARP and ARP spoofing.  

#### ✅ What is ARP? Address Resolution Protocol

**Purpose**:  
ARP maps an **IP address** (Layer 3) to a **MAC address** (Layer 2) on a local network (LAN).

#### 🧠 Why it's needed:

- When a device wants to send a packet to another device on the **same subnet**, it needs the **MAC address**.
- The device knows the IP (e.g., 192.168.1.5), but not the MAC (e.g., `00:0a:95:9d:68:16`).
    

#### 🔍 How ARP Works:

1. Device A wants to talk to IP `192.168.1.5`
2. Sends a **broadcast**:  
    `Who has 192.168.1.5? Tell me!`
3. Device B replies with:  
    `192.168.1.5 is at 00:0a:95:9d:68:16`
4. Device A caches this in its **ARP table**
    

#### 🚨 What is ARP Spoofing?

##### 🎭 **ARP Spoofing (or ARP Poisoning)** is a **Man-in-the-Middle (MitM)** attack.

**Goal**: Trick devices into sending traffic to the attacker by sending **fake ARP replies**.

##### 🔥 How it works:
1. Attacker sends **forged ARP replies** to devices on the network:
    - Claims:  
        `I am 192.168.1.1 (the gateway) → my MAC is 11:22:33:44:55:66` 
2. Now:
    - Victim sends all traffic **intended for the gateway** to the attacker’s MAC.
    - Attacker can:
        - Intercept
        - Modify
        - Drop
        - Forward (to stay stealthy)
            

#### 🎯 Real-World Attack Example:

- Attacker uses tools like:
    - `arpspoof` (part of dsniff)
    - `ettercap`
    - `Bettercap`
        
- Can sniff:
    
    - Credentials (if not encrypted)
    - Session cookies
    - DNS requests (for DNS spoofing)
        

#### 🛡️ SOC Analyst Perspective

##### 🔎 Detection Techniques:

|Method|What to Look For|
|---|---|
|🧪 **ARP Table Anomalies**|Multiple IPs pointing to **same MAC**|
|📡 **Frequent unsolicited ARP replies**|Attacker spamming fake updates|
|🛠️ **ARP Watch tools**|Monitor MAC-IP bindings|
|📊 **SIEM/Logs**|Unusual network flows or duplicate MACs|
|📉 **Connectivity Drops**|Caused by conflicting ARP entries|
|🔐 **MITM alerts**|From IDS/IPS or EDR (e.g., CrowdStrike, Suricata)|

#### 🔐 Mitigations

| Strategy                                    | Description                                    |
| ------------------------------------------- | ---------------------------------------------- |
| ✅ **Static ARP entries**                    | Hard-code trusted mappings on critical devices |
| 🔒 **Use HTTPS/TLS**                        | So intercepted traffic is useless              |
| 🧱 **Network Segmentation**                 | Reduce attack surface on VLANs                 |
| 🔍 **Monitor ARP traffic**                  | Use tools like `arpwatch`, `Wireshark`, or EDR |
| 🛡️ **Enable dynamic ARP inspection (DAI)** | On enterprise switches (Cisco, etc.)           |

---
#### 12. What is DHCP, and how can it be abused?  
    
##### ✅ **What is DHCP, and How Can It Be Abused?**

**DHCP (Dynamic Host Configuration Protocol)** is a **network protocol** used to automatically assign **IP addresses and other configuration parameters** (like DNS servers, gateway, subnet mask) to devices on a network.

In enterprise environments, DHCP simplifies network management and ensures devices receive proper configurations dynamically as they connect.


##### 🚨 **How DHCP Can Be Abused (from a Security Perspective):**

As a SOC Analyst, it’s important to recognize how DHCP can be a vector for **network-based attacks**. Common abuses include:

##### 1. **Rogue DHCP Server Attack**

- **Threat Actor** introduces an unauthorized DHCP server on the network.
- The rogue server **responds faster than the legitimate server** and assigns malicious configuration:
    - Incorrect default gateway (MITM)
    - Malicious DNS server (DNS spoofing)
        
- This can lead to **traffic interception**, **data exfiltration**, or **phishing**.

> 🧠 _Impact: Users unknowingly route traffic through attacker-controlled systems._

##### 2. **DHCP Starvation Attack**

- Attacker floods the DHCP server with **fake DHCP requests**, exhausting the available IP address pool.
- Legitimate users cannot obtain IP addresses → **Denial of Service (DoS)**.
- Often used as a precursor to **rogue DHCP server attacks**.
    
> 🧠 _Impact: Disrupts network availability and opens the door for rogue DHCP injection._

##### 3. **DHCP Information Gathering**

- DHCP packets are unauthenticated and in plaintext.
- Attackers can **sniff DHCP traffic** to identify active hosts, MAC addresses, operating systems, etc.
- Useful in **reconnaissance** during the early stages of an attack.
    

#### 🔐 **Mitigations (What SOC Analysts Should Watch For):**

1. **DHCP Snooping** (enabled on switches) 
    - Allows only trusted ports to respond to DHCP requests.
    - Blocks rogue DHCP servers at Layer 2.
        
2. **Port Security / NAC**
    - Controls which devices can connect and get IPs.
        
3. **Monitoring Logs & Alerts**
    - Detect unusual DHCP activity (e.g., many lease requests, unknown DHCP servers).
    - Set up alerts for new DHCP servers or rapid lease consumption.
        
4. **Segmentation**
    - Use VLANs to isolate critical systems from end-user subnets.
        

#### 🧠 Bonus: Behavioral Indicators (For SIEM/SOC Monitoring)

- Sudden spikes in DHCP lease requests
- Multiple DHCPDISCOVER packets from a single MAC/IP
- Duplicate DHCP Offer packets from different MAC addresses
- Mismatched DHCP and ARP info (e.g., MITM attempts)

---
#### 13. Explain DNS and common attacks (DNS tunneling, poisoning, amplification).  

“DNS, or Domain Name System, resolves domain names to IP addresses and is essential to network operations. It’s also a frequent attack vector. Three common DNS-based attacks include:

First, **DNS Tunneling**, where attackers use DNS queries to exfiltrate data or establish command-and-control — it’s hard to detect because DNS is typically trusted.

Second, **DNS Poisoning**, where fake DNS entries are injected into a resolver’s cache, redirecting users to malicious IPs — this enables phishing and man-in-the-middle attacks.

Third, **DNS Amplification**, a type of DDoS attack where spoofed requests are sent to open resolvers, amplifying traffic to flood a target.

As a SOC analyst, I’d monitor DNS traffic for anomalies, apply DNSSEC where possible, and ensure proper logging and alerting on unusual patterns like high-entropy domain queries or unexpected resolution changes.”

---

#### 14. What is a MITM attack? Give real examples.  

> A **Man-in-the-Middle (MITM)** attack occurs when an attacker **secretly intercepts, relays, or alters communication** between two parties who **believe they are directly communicating** with each other.

- The attacker positions themselves **between the victim and the intended service** (e.g., website, server, network gateway).
    
- The goal may be to **steal credentials**, **inject malicious content**, **eavesdrop**, or **manipulate data**.
    
##### 🧠 **Types of MITM Attacks**

|Type|Description|
|---|---|
|🔌 **Network-based MITM**|Intercepting traffic on unsecured Wi-Fi or LAN using ARP spoofing or DNS spoofing.|
|🧾 **SSL Stripping**|Downgrading HTTPS to HTTP to capture credentials.|

---
#### 15. Explain ICMP and how attackers might use it.  

“ICMP is a network-layer protocol used for diagnostics and error reporting — tools like `ping` and `traceroute` rely on it. However, attackers can abuse ICMP in several ways. They often use it for **reconnaissance**, like ping sweeps and mapping networks. More advanced threats include **ICMP tunneling**, where data is covertly sent through Echo packets to bypass firewalls. It can also be used in **DDoS attacks**, such as ICMP floods or legacy smurf attacks. As a SOC analyst, I would monitor for abnormal ICMP traffic, apply rate limits, and inspect payloads to detect tunneling or scanning activity.”

> **ICMP (Internet Control Message Protocol)** is a **network-layer protocol** used for **diagnostic and error reporting**.

It’s used by tools like:

- `ping` – to check if a host is reachable
- `traceroute` – to map the route packets take
    
🔧 It’s not used for transferring data — but rather for **network troubleshooting and control**.

ICMP messages are encapsulated in IP packets and include types like:
- Type 0: Echo Reply
- Type 8: Echo Request
- Type 3: Destination Unreachable
- Type 11: Time Exceeded
    
#### 🚨 **How Attackers Might Use ICMP**

##### 1. 🕵️‍♂️ **Reconnaissance / Scanning**

- Attackers use ICMP to **discover live hosts and network topology**.
- `ping sweeps` (ICMP Echo Requests) can identify which IPs are active.
- `traceroute` can help map network paths and firewalls.

> 🧠 This is often the **first phase** of an attack — **information gathering**.
#### 🔍 Detection:

- Unusual ICMP Echo traffic from unknown sources
- High-frequency ICMP across many IPs (sweeps)
    
##### 2. 🐍 **ICMP Tunneling**

- Attackers **tunnel data or commands** inside ICMP packets to **bypass firewalls** and **exfiltrate data**.
- Tools like `icmpsh` or `Ptunnel` use Echo Request/Reply to carry **malicious payloads**.
- Since ICMP is often **allowed through firewalls**, this can **evade traditional security controls**.
    

> 🎯 This is a form of **covert channel or C2 (Command & Control)**.

#### 🔍 Detection:

- Large ICMP payloads
- ICMP traffic from endpoints that don’t normally use it
- ICMP patterns outside of normal diagnostic behavior
    
##### 3. 💣 **ICMP Flood / DDoS**

- Attackers send massive numbers of ICMP Echo Requests to a target (e.g., `ping -f`) to **overwhelm the system**.
- Can cause **resource exhaustion** (CPU/network stack), leading to DoS.
    
> 🧠 Older systems (esp. Windows 95 era) were vulnerable to **Ping of Death** (malformed ICMP packets).

#### 🔍 Detection:

- Sudden spike in ICMP traffic to a single host
- High packet rate from multiple sources (DDoS patterns)
    
##### 4. 🎭 **Smurf Attack** (legacy but important to know)

- Attacker sends ICMP Echo Requests with the victim’s IP as **spoofed source** to a **broadcast address**.
- All hosts on the subnet **reply to the victim**, overwhelming it.

> ⚠️ Rare today due to better broadcast handling, but still a textbook ICMP attack.

#### 🛡️ **Mitigations (SOC Analyst Perspective)**

|Threat|Mitigation|
|---|---|
|Recon|Rate-limit ICMP, block from untrusted zones|
|Tunneling|Deep packet inspection (DPI), monitor payload sizes|
|DDoS|ICMP rate limiting, anomaly detection in NIDS/NIPS|
|Smurf|Disable IP-directed broadcasts, ingress/egress filtering|

---
#### 16. What is NAT, and how does it impact detection?  

NAT, or Network Address Translation, is used to map private internal IP addresses to public ones, allowing multiple devices to share a single IP for internet access. While NAT is essential for scalability and security, it creates challenges for detection. Specifically, it **masks the original source IP**, which makes it difficult for SOC analysts to attribute alerts or traffic to specific hosts. In logs, multiple users behind a NAT may appear as one IP, complicating threat hunting, correlation, and forensics. To mitigate this, I’d ensure the NAT device logs mappings, integrate that with SIEM, and combine IP data with user identity logs to regain visibility.

---
#### 17. Explain VLANs and their role in network segmentation.  

A **VLAN (Virtual Local Area Network)** is a logical segmentation of a physical network. It allows network administrators to partition a single physical switch (or network) into multiple isolated networks at **Layer 2** of the OSI model — without requiring separate physical infrastructure.

VLANs isolate traffic between different departments or roles — e.g., engineering, HR, finance — even if they share the same physical infrastructure. 

This reduces lateral movement in case of a breach. For example, if a device in VLAN_A is compromised, it **cannot directly access** resources in VLAN_B unless explicitly allowed.

Security policies (e.g., ACLs or firewall rules) are often applied **per VLAN**.
This allows for **role-based access control (RBAC)** at the network layer, complementing IAM systems at the application level.


---
#### 18. What is a VPN, and how does it work at protocol level?  

A VPN, or Virtual Private Network, creates a secure, encrypted tunnel between a user's device and a remote network over the internet. At the protocol level, VPNs use tunneling protocols like IPSec, SSL/TLS, or WireGuard to encapsulate and encrypt data packets.


---
#### 19. Explain how proxies differ from VPNs.  

Proxies and VPNs both route traffic through an intermediary server, but they differ in function, scope, and security. A proxy operates at the application layer — it only handles specific traffic like HTTP or SOCKS — and typically doesn’t encrypt data. It’s mainly used for content filtering, bypassing geo-restrictions, or controlling web access. In contrast, a VPN works at the network layer, encrypting all traffic between the client and the VPN server, providing secure, private communication over untrusted networks. From a SOC analyst's perspective, VPNs are more secure but can also mask malicious behavior, while proxies are easier to detect but more limited in protection.

---
#### 20. What is port scanning, and how do you detect it?  

**Port scanning** is when an attacker sends packets to a range of ports on a host to **identify which ports are open, closed, or filtered**. The goal is to discover **services** running on the target system that could be vulnerable.

> 🧠 It’s like knocking on every door in a building to see which ones are unlocked.

---
#### 21. What’s the difference between IPv4 and IPv6 from a SOC perspective?

From a SOC perspective, IPv6 introduces a much larger address space and native IPsec, which changes how attackers scan and move within networks. The lack of NAT means more direct host exposure. SOC teams must adapt tools and detection strategies to handle IPv6’s unique traffic patterns and new attack vectors.

---
#### 22. Explain common HTTP methods and possible abuse.  
Common HTTP methods like GET and POST are used for retrieving and submitting data, while PUT and DELETE allow modifying or deleting resources. Attackers can abuse methods like PUT to upload malicious files or DELETE to remove data. From a SOC perspective, monitoring and restricting these methods helps prevent unauthorized access and abuse.


---
#### 23. What are WebSockets, and what risks do they introduce?  

**WebSockets** are a protocol that allows a **persistent, full-duplex (two-way)** connection between a **client (like a browser)** and a **server** over a **single TCP connection**.

> 🧠 Unlike HTTP (which is request-response), WebSockets stay open — both sides can send data anytime.

---
#### 24. Explain how cookies, sessions, and JWTs can be abused.  

#### 🍪🔐 **Cookies, Sessions, and JWTs — and How They’re Abused**

These are all methods of **tracking and authenticating users** on web applications.

##### 1. 🍪 **Cookies**
Cookies are small pieces of data stored in the browser and sent with every HTTP request to a domain.

##### ✅ Used For:

- Session tracking
- Authentication tokens
- Preferences (language, theme, etc.)
    
##### ⚠️ Abuses:

|Attack Type|Description|
|---|---|
|**Session hijacking**|If cookies aren't marked **Secure** or **HttpOnly**, attackers can steal them via **XSS** or network sniffing (if not using HTTPS).|
|**Cross-Site Request Forgery (CSRF)**|Browser auto-sends cookies with requests → attacker tricks user into making a request with their valid session.|
|**Cookie poisoning**|Attacker manipulates cookie values to **elevate privileges** or access unauthorized data.|

##### 2. 🪪 **Sessions**

A session is server-side state stored for a logged-in user, typically tracked using a **session ID** stored in a cookie.

##### ✅ Used For:

- Keeping users logged in
- Managing temporary server-side user data
    
##### ⚠️ Abuses:

|Attack Type|Description|
|---|---|
|**Session fixation**|Attacker sets a known session ID and tricks victim into using it. Once victim logs in, attacker hijacks session.|
|**Session prediction**|Poor session ID entropy allows attacker to guess valid session IDs.|
|**Session timeout abuse**|Long-lived sessions make it easier for stolen sessions to remain valid.|

##### 3. 🔑 **JWTs (JSON Web Tokens)**
JWTs are **self-contained tokens** used to authenticate users. They're usually stored in **cookies** or **localStorage** and include data + a signature.

##### ✅ Used For:

- Stateless authentication
- API security
- Mobile and SPA (Single Page App) login flows
    
##### ⚠️ Abuses:

|Attack Type|Description|
|---|---|
|**Token theft**|Stored in `localStorage` → vulnerable to **XSS**. Once stolen, attacker has full access.|
|**Weak signing algorithm**|If `alg: none` or weak key is allowed, attacker can **forge tokens**.|
|**Token replay**|If tokens don’t expire quickly or have no IP/device binding, stolen tokens can be reused.|
|**No revocation**|JWTs are stateless. If stolen, there's often **no way to revoke** them unless implemented separately.|


---

## 🔹 Security Fundamentals & Cryptography (Q26–Q45)
#### 25. Explain the CIA Triad.  

The CIA Triad stands for Confidentiality, Integrity, and Availability. Confidentiality ensures sensitive data is only accessible to authorized users — typically enforced through encryption and access controls. Integrity means the data remains accurate and unaltered, using mechanisms like hashing and digital signatures. Availability ensures systems and data are accessible when needed, often supported by redundancy and protections against threats like DDoS. These three principles form the backbone of all security decisions

---
#### 26. What’s the difference between confidentiality and integrity violations?  

Confidentiality violations occur when unauthorized users gain access to sensitive data, leading to data leaks or exposure. Integrity violations happen when data is altered or tampered with by unauthorized parties, compromising its accuracy and trustworthiness. In short, confidentiality breaches affect who can _see_ the data, while integrity breaches affect whether the data can be _trusted_.

----
#### 27. What is the principle of least privilege?  

The principle of least privilege means giving users, systems, or processes the minimum level of access needed to perform their tasks — and nothing more. This reduces the attack surface by limiting what an attacker could do if access is compromised. It’s a core concept in securing identities, APIs, and infrastructure


---
#### 28. What’s the difference between authentication, authorization, and accounting?  

Authentication is verifying who a user or system is — like logging in with a password or MFA. Authorization determines what that authenticated user is allowed to do — like read vs. write access. Accounting, or auditing, is tracking what actions were performed, when, and by whom. Together, they form the AAA security model: Authenticate the user, Authorize their actions, and Account for their activity.

---
#### 29. Explain single sign-on (SSO) and its risks.  

Single Sign-On (SSO) allows users to authenticate once and access multiple applications without logging in again. It improves user experience and reduces password fatigue. However, it introduces risks — if the SSO account is compromised, it can grant access to all linked systems. That’s why securing the SSO entry point with strong authentication methods like MFA, strict session controls, and monitoring is critical.

---
30. What’s Kerberos authentication and how does it work?  
31. What is NTLM, and how can it be attacked?  
32. What are password salts and why are they important?  
33. Explain brute force vs dictionary vs rainbow table attacks.  
34. What is PKI, and why is it critical for SOC monitoring?  
35. What is certificate pinning?  
36. Explain HMAC and its use cases.  
37. What is zero trust architecture?  
38. Explain what defense-in-depth means.  
39. What are security controls: preventive, detective, corrective?  
40. What is an attack surface?  
41. What is threat modeling?  
42. Explain security by design vs security by obscurity.  
43. What is data in transit vs data at rest vs data in use?  
44. Explain hashing algorithms like MD5, SHA-256, and why some are weak.  

---

## 🔹 SOC Operations & Tools (Q46–Q70)
#### 45. What is a SIEM and how does it work?  

A SIEM — Security Information and Event Management — collects and aggregates logs from across an organization’s infrastructure: endpoints, servers, network devices, cloud services, and applications. It normalizes the data, applies correlation rules, and generates alerts for suspicious or malicious activity. Analysts use SIEMs to detect threats, investigate incidents, and support compliance by centralizing visibility and providing real-time monitoring and historical analysis.

---
#### 46. What are log sources typically ingested into a SIEM?  

A SIEM typically ingests logs from a wide range of sources to provide complete visibility across the environment. Common log sources include endpoint security tools (like EDRs), firewalls and IDS/IPS, authentication systems (like Active Directory), operating systems (Windows Event Logs, Linux syslogs), cloud platforms (AWS CloudTrail, GCP Audit Logs), web servers, VPNs, and applications. Ingesting diverse logs allows the SIEM to correlate events and detect threats across users, devices, and networks.

---
#### 47. Explain correlation rules in SIEM.  

Correlation rules in a SIEM are logic-based rules that connect events from different log sources to detect suspicious or malicious activity. Instead of looking at isolated events, the SIEM identifies patterns — like multiple failed logins followed by a successful one from the same IP, or a VPN login immediately followed by privileged actions. These rules help detect complex threats like brute-force attacks, lateral movement, or insider abuse by analyzing the relationship between events over time.

---
#### 48. What are notable events in Splunk?  

"In Splunk, a notable event is an alert that has been generated based on correlation rules in Splunk Enterprise Security. These events highlight potentially suspicious or risky activity — like multiple failed logins, unusual data transfers, or malware detections. Notable events are sent to the Incident Review dashboard, where analysts can triage, investigate, and assign them based on severity and urgency. They're a core part of Splunk’s use case-driven detection approach.

---
#### 49. Explain UBA (User Behavior Analytics).  

**User Behavior Analytics (UBA)** is a technique that uses machine learning and statistical models to detect abnormal behavior by users or entities — like employees, accounts, or service identities. Instead of relying solely on known threat signatures or rules, UBA builds a baseline of ‘normal’ activity and flags deviations that may indicate insider threats, compromised accounts, or lateral movement.

---
#### 50. What is SOAR, and how is it used in SOCs?  

SOAR stands for Security Orchestration, Automation, and Response. It's a platform that helps SOCs automate repetitive tasks, integrate tools, and streamline incident response workflows. Instead of manually gathering data, enriching alerts, or blocking IPs, SOAR playbooks can do it automatically — saving time and reducing analyst fatigue. It also standardizes response actions, improves consistency, and helps teams respond faster to real threats.
##### **Examples of SOAR in action:**

- Automatically enrich an alert with threat intel and user details
- Quarantine a compromised endpoint via EDR
- Block a malicious IP in the firewall
- Open or close a ticket in Jira or ServiceNow
- Send alerts to Slack or email for review
  
---
#### 51. What is log normalization and parsing?  

Log parsing is the process of extracting key fields — like IPs, usernames, timestamps — from raw log data. Normalization goes a step further by converting those fields into a consistent format across all log sources. This makes it easier to search, correlate, and apply detection rules across different systems in tools like SIEMs. Without normalization and parsing, the same type of event from different sources would be too inconsistent to analyze effectively.

---
#### 52. How do you validate a false positive vs true positive alert?  

First, I review the alert context — what rule triggered it, what log source, and what activity was detected. Then I investigate the entities involved: user, IP, host, and time of event. I correlate this with other logs — like authentication, EDR, network traffic, or threat intel. If the behavior is expected or benign — like an admin action or a known script — it’s a false positive. But if it shows malicious intent, unauthorized access, or matches threat patterns, I escalate it as a true positive.

---
#### 53. Explain EDR vs NDR vs XDR.  

**EDR (Endpoint Detection and Response)** focuses on detecting and responding to threats on endpoints like laptops and servers — using telemetry like processes, file activity, and network connections.

**NDR (Network Detection and Response)** monitors network traffic for anomalies, lateral movement, or malicious communication — especially useful for spotting threats that bypass the endpoint.

**XDR (Extended Detection and Response)** combines data from EDR, NDR, email, cloud, and other sources into a unified platform. It correlates and enriches alerts to improve detection accuracy and streamline response across the environment.**

|Feature|EDR|NDR|XDR|
|---|---|---|---|
|Focus|Endpoints (devices)|Network traffic|Multiple data sources (EDR, NDR, cloud, email)|
|Data type|Processes, files, memory|Packets, flows, DNS, traffic|Aggregated, correlated telemetry|
|Visibility|Host-level|Network-level|End-to-end, cross-layer|
|Example Tools|CrowdStrike, SentinelOne|Darktrace, Vectra, ExtraHop|Microsoft Defender XDR, Palo Alto Cortex XDR|

___
#### 54. What are IOC, IOA, and TTP?  

**"IOCs (Indicators of Compromise)** are pieces of forensic evidence that confirm a system has been breached — like malicious IPs, file hashes, or domains.

**IOAs (Indicators of Attack)** focus on detecting attacker behavior or intent before damage is done — like unusual PowerShell usage or credential dumping.

**TTPs (Tactics, Techniques, and Procedures)** describe how attackers operate, based on frameworks like MITRE ATT&CK — for example, using phishing (technique) for initial access (tactic), and then using Mimikatz (procedure) to dump credentials."**

---
#### 55. Explain threat intelligence feeds.  

Threat intelligence feeds provide up-to-date information on known malicious indicators — such as IP addresses, domains, URLs, file hashes, and attacker TTPs. These feeds help security tools like SIEMs, firewalls, and EDRs detect and block threats by comparing activity in your environment against known bad actors.

---
#### 56. Explain Sysmon and why it’s important.  

**Sysmon (System Monitor)** is a Windows system service from Microsoft Sysinternals that logs detailed system activity into the Windows Event Log. It provides rich telemetry like process creation, command-line arguments, network connections, file changes, and more — far beyond what standard Windows logs capture.

##### **Example Use Case:**

- An attacker uses a base64-encoded PowerShell command to download a payload.
- Standard logs might miss it.
- **Sysmon logs the full command line**, process path, parent process, and network connection.
- The SOC can detect and investigate this as an **IOA (Indicator of Attack)**.

##### 📁 **Key Sysmon Event IDs to Know:**

| Event ID | Description                                        |
| -------- | -------------------------------------------------- |
| 1        | Process creation (with command line, hashes, etc.) |
| 3        | Network connection                                 |
| 7        | Image loaded                                       |
| 11       | File created                                       |
| 13       | Registry value set                                 |
| 22       | DNS query                                          |
| 23       | File delete detected (recent version)              |

---
#### 57. What are Windows Event IDs critical to SOC monitoring?  

| Event ID            | Description                                                           | Why It Matters                                   |
| ------------------- | --------------------------------------------------------------------- | ------------------------------------------------ |
| **4624**            | Successful logon                                                      | Detect account access; important for correlation |
| **4625**            | Failed logon                                                          | Bruteforce attempts, login anomalies             |
| 4688                | Process Creation                                                      |                                                  |
| 4697                | Service Installed                                                     |                                                  |
| **4723** / **4724** | Password change/reset                                                 | Can indicate account compromise                  |
| 5140                | Network share was accessed                                            |                                                  |
| 5145                | Detailed share access                                                 |                                                  |
| 5156                | Windows Filtering Platform: Allow network connection (firewall allow) |                                                  |
| 5157                | Windows Filtering Platform: Block network connection                  |                                                  |

---
#### 58. Explain PowerShell logging and why it matters.  

**PowerShell logging** refers to enabling logs that capture PowerShell activity, including executed commands, scripts, and behavior — crucial for detecting malicious use of PowerShell by attackers.

---
#### 59. Explain how you’d use Wireshark in an investigation.  

In an investigation, I’d use Wireshark primarily to analyze packet captures and validate suspicious network activity. For example, if I saw an alert about unusual outbound traffic from a host, I’d load the pcap in Wireshark and apply filters like `ip.addr == <suspicious IP>` or `http` to isolate relevant flows.

I’d check for abnormal DNS queries, suspicious domains, or repeated failed connections that might indicate beaconing. I’d also look at the TCP streams to reassemble conversations — that’s useful for spotting things like cleartext credentials or malicious payloads being transferred.

In short, I use Wireshark to go deeper than the SIEM/IDS alert: it helps me validate whether the alert is real, understand the scope of activity, and extract indicators such as IPs, domains, or file transfers, which I can then pivot on in other tools.

---
#### 60. What is NetFlow and how does it help SOC monitoring?  

**NetFlow** is a **network protocol** developed by **Cisco** that collects and monitors **IP network traffic flow data** as it enters or exits an interface.

It captures **metadata** about traffic — not full packet contents — making it efficient for high-volume environments.

##### 📦 NetFlow Data Includes:

|Field|Description|
|---|---|
|Source & destination IP|Who is talking to whom|
|Source & destination port|What service/application|
|Protocol|e.g., TCP, UDP, ICMP|
|Bytes & packets|Amount of data transferred|
|Timestamps|Start and end times of the flow|
|Interface info|Where traffic came in or out|

---
#### 61. Explain firewall logs vs proxy logs vs DNS logs in investigations.  

In investigations, I use firewall logs to identify allowed or blocked connections at the network layer, proxy logs to see detailed web activity and possible exfiltration or C2 attempts, and DNS logs to detect suspicious domain lookups like DGAs or beaconing. Correlating all three helps build a complete picture of attacker behavior from resolution to access to data movement.

### 🔥 1. **Firewall Logs**

- **What they log**:
    
    - Source/destination IP
    - Port and protocol
    - Action (allow/deny)
    - Interface or rule triggered
        
- **Good for**:
    
    - **Perimeter defense** (blocked inbound RDP, port scans)
    - Detecting **data exfiltration attempts**
    - Identifying **lateral movement** across internal segments
    - Correlating with NetFlow for **traffic patterns**
        
### 🌐 2. **Proxy Logs**

- **What they log**:
    
    - User or host making the request
    - Full URL (including path and query string)
    - HTTP methods (GET, POST)
    - Response codes (e.g., 200, 404, 403)
    - Timestamp, bytes sent/received
        
- **Good for**:
    
    - **Malicious URL access** (e.g., phishing, exploit kits)
    - **C2 communication** detection (e.g., unusual web beacons)
    - **User behavior analysis** (non-business-related browsing, TOR proxies)
    - **Data exfiltration** via HTTP POST
        
### 🧠 3. **DNS Logs**

- **What they log**:
    
    - Query timestamp
    - Queried domain (FQDN)
    - Source IP/host
    - DNS server used
    - Response IPs (A, AAAA records)
        
- **Good for**:
    
    - **Detecting DGA (Domain Generation Algorithm)** usage
    - **C2 beaconing** using unique subdomains (e.g., `abc123.attacker.com`)
    - **DNS tunneling** (high volume of small TXT queries)
    - **Initial compromise indicators** (user resolving known malicious domains)
        
---
#### 62. What are common Splunk search commands a SOC analyst should know?  

##### 🔍 **Core Search Commands**

|Command|Purpose|Example|Use Case|
|---|---|---|---|
|`index=`|Specifies the data source|`index=firewall`|Start your search with the right dataset|
|`source=`, `sourcetype=`|Narrow by log format or file|`sourcetype=wineventlog:security`|Filter Windows Event Logs|
|`host=`|Filter by host|`host=web01`|Investigate specific machine|
|`search`|Explicitly filters results|`|search status=500`|
##### 📊 **Stats & Aggregation Commands**

|Command|Purpose|Example|Use Case|
|---|---|---|---|
|`stats`|Aggregate and group data|`|stats count by src_ip`|
|`top`|Most frequent values|`|top user`|
|`rare`|Least frequent values|`|rare dest_ip`|
|`timechart`|Time-based trends|`|timechart count by status`|
|`eventstats`|Add aggregate info to each row|`|eventstats avg(bytes) as avg_bytes`|
##### 🧹 **Filtering & Transformation Commands**

|Command|Purpose|Example|Use Case|
|---|---|---|---|
|`where`|Filter using conditions|`|where bytes > 100000`|
|`eval`|Create or transform fields|`|eval size_mb=bytes/1024/1024`|
|`rename`|Rename fields|`|rename src_ip as Source_IP`|
|`fields`|Select specific fields|`|fields user, src_ip`|
|`table`|Format results in a table|`|table user, action, time`|
|`dedup`|Remove duplicate events|`|dedup user`|
##### 🔁 **Joining & Correlation Commands**

|Command|Purpose|Example|Use Case|
|---|---|---|---|
|`join`|Join two datasets on a field|`|join user [search index=auth_failures]`|
|`append`|Combine results from multiple searches|`|append [search index=proxy]`|
|`transaction`|Group events by session ID, user, etc.|`|transaction user startswith="login" endswith="logout"`|

---
#### 63. Explain the MITRE ATT&CK framework and how SOCs use it.  

The MITRE ATT&CK framework provides a detailed map of how adversaries operate — from initial access to data exfiltration. In the SOC, we use ATT&CK to guide detection engineering, threat hunting, incident response, and reporting. It gives us a common language to prioritize alerts, map gaps, and simulate attacks in purple teaming exercises. It’s essential for building a threat-informed defense.

---
#### 64. How does the Cyber Kill Chain map to SOC investigations?  

In the SOC, I use the Cyber Kill Chain to frame investigations — it helps me understand where the attacker is in their lifecycle and prioritize response. For example, if I detect C2 activity, I know the attacker has already exploited the system, so I need to check for persistence and data access. I then layer in MITRE ATT&CK to identify the exact techniques used and guide deeper hunts or detections. Together, they help turn raw alerts into actionable, contextual investigations.

---
#### 65. Explain the Diamond Model of Intrusion Analysis.  

The Diamond Model helps SOC teams frame an intrusion as a relationship between four elements: adversary, capability, infrastructure, and victim. For example, if we detect traffic to a known C2 domain (infrastructure), we can pivot to see what malware was used (capability), who was targeted (victim), and potentially tie it to a threat group (adversary). This structured approach enhances investigation, attribution, and reporting, especially when combined with frameworks like MITRE ATT&CK.


----
#### 66. How do you prioritize alerts in a SOC?  

I prioritize alerts based on a combination of severity, asset criticality, user context, correlation with other events, and threat intelligence. I also map alerts to the MITRE ATT&CK framework to understand where in the attack chain the activity falls. A high-fidelity alert on a privileged system during off-hours would take precedence over a low-severity alert on a non-critical asset. This risk-based approach ensures that we focus on what's most likely to impact the organization.

---

## 🔹 Incident Response & Detection (Q71–Q100)
#### 67. Walk me through the incident response lifecycle.  

The Incident Response Lifecycle includes four phases: **Preparation**, where we build and test our detection and response capabilities; **Detection & Analysis**, where we triage alerts and confirm the incident; **Containment, Eradication, and Recovery**, where we isolate, clean, and restore systems; and **Post-Incident Activity**, where we document lessons learned and improve defenses. Throughout this process, I use frameworks like MITRE ATT&CK to map attacker behaviors and guide investigation.

---
#### 68. What is containment vs eradication?  

Containment is about limiting the attacker’s ability to do more damage — like isolating a host or blocking traffic. Eradication is about fully removing the threat — deleting malware, closing backdoors, and patching vulnerabilities. Containment buys us time; eradication ensures the threat is eliminated.

---
#### 69. How do you handle malware detection on a critical server?  

When malware is detected on a critical server, my first step is to verify the alert and assess the potential impact. I would immediately isolate the server to contain the threat and prevent lateral movement. Then, I’d collect forensic data to understand the malware’s behavior, followed by eradication efforts including malware removal and patching. After that, I would coordinate recovery, ensuring the server is clean before bringing it back online. Finally, I’d conduct a post-incident review, update detection rules, and recommend security improvements. Throughout the process, I’d map the attack to MITRE ATT&CK to better understand attacker tactics and improve future detection.

---
#### 70. Explain phishing detection and response workflow.  

Phishing detection starts with both user reports and automated email security tools scanning for malicious content. Once detected, I analyze the email for legitimacy, extract IOCs, and assess the impact. Containment involves blocking malicious URLs and quarantining emails. If credentials are compromised, I coordinate resets and enforce MFA. Post-incident, I update detection rules, share intelligence, and conduct user training to reduce future risks.

---
#### 71. How do you investigate a suspicious login attempt?  

I begin by validating the login alert — checking the source IP, geolocation, and whether MFA was used. I compare the login to the user’s typical behavior — time, location, device. If the IP is from a suspicious ASN or there’s impossible travel, I check post-login activity for data access or lateral movement. I correlate this with other logs and enrich the IP with tools like ipinfo and GreyNoise. Based on risk, I may reset credentials or disable the account. Everything is documented, and I ensure detections are updated if needed.

---
#### 72. How would you detect brute-force login attempts?  

 To detect brute-force attempts, I analyze authentication logs in the SIEM, looking for patterns like multiple failed logins from a single IP or across many accounts. I enrich IPs with threat intel and check for related successful logins. I tune thresholds to avoid false positives from legitimate mistyped passwords. In some cases, we use SOAR automation to lock accounts or block IPs in real time

---
#### 73. How do you investigate privilege escalation?  

“In Windows, I investigate any process or service running as **SYSTEM or a high-privilege user** that is **unusual or not part of the standard baseline** for that endpoint. These could indicate privilege escalation — especially if initiated by a non-admin user or tied to a suspicious binary path, scheduled task, or token manipulation.


---
#### 74. How do you detect persistence mechanisms on Windows?  

To detect persistence, I monitor changes to startup entries like Run keys, services, scheduled tasks, and WMI filters. Using Sysmon and EDR telemetry, I look for unauthorized binaries registered to auto-start — especially those in user-writable paths or launched with SYSTEM privileges. I also use autoruns and threat hunting tools to compare against a known-good baseline.

---
#### 75. What’s the difference between ransomware and wiper malware? 

**Ransomware** encrypts data and demands a ransom for recovery.  
**Wiper malware** is designed to **permanently destroy** data with **no intent to recover** — it's purely destructive.

----
#### 76. Explain lateral movement and how to detect it.  

**Lateral movement** is when an attacker, after gaining initial access to a machine, tries to move across the network to access other systems — especially to find privileged accounts, sensitive data, or domain controllers.

Lateral movement is how attackers pivot through internal systems after gaining initial access. I detect it by correlating Windows logon events, remote execution tool usage, abnormal network connections, and behavior analytics. Tools like PsExec, WMI, RDP, or token theft are commonly used, and detection focuses on unusual access patterns, privilege escalation, and remote service creation.

---
#### 77. What is credential dumping and which tools are used?  

**Credential dumping** is the process of extracting account credentials (like passwords, hashes, or Kerberos tickets) from memory, registry, or files on a compromised system.  
Attackers use it to escalate privileges and move laterally within a network.

##### 🛠️ Common Tools Used in Credential Dumping

|Tool|Use|
|---|---|
|**Mimikatz**|Extracts creds from LSASS, tickets, SAM|

---
#### 78. How do you detect Mimikatz in logs?  




---
79. How do you investigate suspicious PowerShell activity?  
80. What are LOLBins, and how do you detect their abuse?  
81. How would you detect C2 communication in logs?  
82. Explain beaconing behavior in network traffic.  
83. What is DNS tunneling and how do you detect it?  
84. How do you handle a suspected insider threat?  
85. How do you contain a DDoS attack at SOC level?  
86. How do you handle supply chain compromise detection?  
87. How do you respond to alerts outside business hours?  
88. What escalation steps do you take after triage?  
89. How do you document and report an incident?  
90. What KPIs do SOCs use to measure effectiveness?  
91. How do you handle false positives at scale?  
92. How do you use playbooks in incident response?  
93. Explain threat hunting vs incident response.  
94. How do you conduct a root cause analysis?  
95. How do you communicate incident findings to non-technical stakeholders?  

---

## 🔹 Advanced / FAANG-Level Scenarios (Q101–Q130+)
101. Walk me through investigating an alert where a process launched `powershell.exe -EncodedCommand`.  
102. How would you detect Golden Ticket attacks?  
103. Explain Silver Ticket vs Golden Ticket.  
104. How would you investigate abnormal Kerberos activity?  
105. How do you detect pass-the-hash attacks?  
106. Explain OAuth abuse in cloud environments.  
107. How do you monitor for suspicious AWS IAM activity?  
108. How do you detect data exfiltration over HTTPS?  
109. How do you differentiate malicious TOR traffic from normal VPN traffic?  
110. How would you investigate suspicious scheduled tasks?  
111. How do you investigate parent-child process anomalies?  
112. Explain detecting malicious macros in Office documents.  
113. How do you handle alerts from EDR about LSASS access?  
114. Explain registry persistence detection.  
115. How do you investigate SMB traffic anomalies?  
116. What’s Kerberoasting and how do you detect it?  
117. Explain how to detect living-off-the-land techniques.  
118. How do you detect obfuscated scripts in logs?  
119. How do you investigate cloud-based phishing campaigns?  
120. How do you detect API key leakage?  
121. How would you investigate suspicious container activity?  
122. Explain Kubernetes security monitoring.  
123. How do you detect supply chain compromises like 3CX/SolarWinds?  
124. How do you monitor for data integrity issues in GitHub repos?  
125. How would you handle zero-day detections in SOC?  
126. Explain how machine learning is applied in UBA for anomaly detection.  
127. How do you differentiate nation-state attacks from cybercrime?  
128. How do you detect insider misuse of SaaS apps like GDrive or Slack?  
129. How do you investigate suspicious OAuth consent grants?  
130. How do you perform proactive threat hunting with MITRE ATT&CK?  
