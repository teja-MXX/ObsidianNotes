## 🧾 Identifying Hosts in Network Investigations

### 🎯 Why Host Identification Matters

- Helps determine **where to start an investigation**.
- Allows mapping of **malicious activity** to specific users or hosts.
- Aids in building a **network inventory** during incident response.
    
### 🖥️ Host and User Naming Patterns

- **Enterprise networks** often use **structured naming conventions** for:
    - Usernames (e.g., `john.doe`, `jdoe01`)
    - Hostnames (e.g., `HR-LAPTOP-01`, `SRV-DC01`)
        
#### ✅ Pros:
- Easy recognition of device roles or user ownership.
- Simplifies asset management and tracking.

#### ❌ Cons:
- Predictable naming conventions can be **exploited by attackers** to blend in or impersonate assets.
    
### 🧪 Protocols Useful for Host & User Identification

|Protocol|Use|
|---|---|
|**DHCP**|Maps IP addresses to MAC addresses and sometimes hostnames.|
|**NBNS (NetBIOS)**|Reveals hostname and user information during name resolution.|
|**Kerberos**|Shows authentication traffic with usernames and service access patterns.|

### 📖 Story Summary: Finding the Infected Host

Imagine you're a SOC analyst named **Ravi**, and your SIEM just flagged **suspicious DNS traffic** from an internal IP: `192.168.10.45`.

To trace it:

1. You check **DHCP logs** and discover:
    - IP `192.168.10.45` is assigned to MAC `00:1A:2B:3C:4D:5E`
    - Hostname: `FINANCE-PC02`    
2. You sniff **NBNS traffic** and confirm the hostname `FINANCE-PC02` sends out name queries, confirming the machine’s identity.
3. Looking at **Kerberos logs**, you observe that user `nisha.k` has authenticated from this host recently.
    
Now you know:
- The suspicious activity is tied to **user Nisha** on **FINANCE-PC02**.
- You can check her login patterns, investigate her recent activity, and isolate the machine for forensic analysis.
    
Because you used DHCP, NBNS, and Kerberos logs, you’ve confidently **mapped an IP to a host and user**, giving you a strong lead to start remediation.
