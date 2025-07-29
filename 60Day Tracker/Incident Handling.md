# 🛡️ Incident Handling Life Cycle (NIST SP 800-61)

The Incident Handling Life Cycle defines **6 structured stages** based on **NIST SP 800-61** guidelines. Mastering this process is crucial for any SOC Analyst.

---

## 📍 1. Preparation

> 🔑 "The best way to handle an incident is to be ready for it."

### 🎯 Goals:
- Build capability to respond efficiently.
- Ensure proactive defense.

### ✅ Key Activities:
- **Develop & document** an Incident Response Plan (IRP).
- Set up **security policies, procedures, and runbooks**.
- Maintain and test **incident response tools** (e.g., forensic kits, SIEM dashboards, packet sniffers).
- Define communication protocols — internal (CISO, Devs) & external (law enforcement, vendors).
- Train staff through **tabletop exercises**, drills, and phishing simulations.
- Regular **patching** and **vulnerability scanning**.

### 📌 Tools:
- SIEM (e.g., Splunk, QRadar)
- Ticketing systems (e.g., ServiceNow, JIRA)
- Endpoint Detection and Response (EDR) tools (e.g., CrowdStrike, SentinelOne)

---

## 🔍 2. Identification

> 🔍 "Is this normal activity, or is it an incident?"

### 🎯 Goals:
- Detect suspicious activity.
- Confirm if it's an actual incident.

### ✅ Key Activities:
- Monitor security alerts/logs via SIEM.
- Analyze anomalous events (e.g., failed logins, port scans, file changes).
- Correlate threat intel with internal alerts.
- Determine **type of incident** (Malware, Phishing, DDoS, Insider Threat).

### 🔔 Indicators of Compromise (IOCs):
- Unusual outbound traffic
- Multiple failed login attempts
- Disabled antivirus/firewall
- Unknown processes (e.g., `certutil.exe` misuse)

### 📌 Sources of Detection:
- IDS/IPS
- Antivirus
- Firewall logs
- Host logs
- User reports
- Threat Intel feeds (AlienVault OTX, VirusTotal)

---

## 🛑 3. Containment

> ⛔ "Limit the blast radius. Don't let it spread."

### 🎯 Goals:
- Isolate affected systems.
- Prevent lateral movement or data exfiltration.

### ✅ Key Activities:
- Short-term containment: Disconnect device from network.
- Long-term containment: Quarantine host, block IPs/domains, firewall rules.
- Preserve volatile data for forensics.
- Backup critical data.
- Monitor for reinfection attempts.

### 🧠 Considerations:
- Balance between operational continuity vs. damage control.
- Do not delete attacker artifacts yet — needed for forensics.

---

## 🧹 4. Eradication

> 🧽 "Remove the root cause."

### 🎯 Goals:
- Eliminate malware or attacker foothold.
- Fix exploited vulnerabilities.

### ✅ Key Activities:
- Patch exploited vulnerabilities.
- Remove malware, backdoors, rogue accounts.
- Change passwords or API keys.
- Harden misconfigured systems.
- Verify all attacker persistence mechanisms are removed.

### 📌 Tools:
- EDR cleanup
- AV scans
- Network re-imaging if needed

---

## 🔁 5. Recovery

> 🔁 "Bring back systems to production — securely."

### 🎯 Goals:
- Restore business operations without re-infection.
- Monitor for abnormal behavior post-restoration.

### ✅ Key Activities:
- Restore from clean backups.
- Monitor restored systems (24–72 hours).
- Resume normal operations in a phased manner.
- Test functionality before full user access.
- Conduct a risk acceptance review if needed.

---

## 📖 6. Lessons Learned

> 📚 "What did we learn? Let’s not make the same mistake again."

### 🎯 Goals:
- Improve processes and defense.
- Share knowledge internally and externally (if appropriate).

### ✅ Key Activities:
- Conduct post-incident review (PIR) or RCA meeting.
- Document incident timeline, impact, and root cause.
- Update IR plan and playbooks.
- Share threat intel with community (ISACs, CERT-In, etc.)
- Train employees if the attack vector was human-related (e.g., phishing).

### 📌 Deliverables:
- Incident Report
- Updated Runbooks
- Recommendations

---

## 📌 Incident Handling Summary Table

| Stage         | Objective                               | Example Activity                           |
|---------------|-----------------------------------------|---------------------------------------------|
| Preparation   | Be ready before it hits                 | IR plans, playbooks, SIEM rules             |
| Identification| Spot the abnormal                       | SIEM alert for brute-force login            |
| Containment   | Stop spread                             | Isolate host, block IP, disable account     |
| Eradication   | Remove attacker completely              | Remove malware, patch vulnerability         |
| Recovery      | Resume business operations safely       | Restore from backup, monitor                |
| Lessons Learned| Prevent future occurrences              | PIR meeting, update playbooks               |

---

## 📘 Real-Life Story: The POS Terminal Breach — A Full Incident Handling Walkthrough

It was a quiet Tuesday night in the SOC, the kind where the hum of the AC and keyboard clicks are the only soundtrack. Around 3:07 AM, an alert pinged on the L1 analyst’s dashboard — an anomaly detected from one of the Point-of-Sale (POS) terminals in a retail store located in Pune. The rule had been tuned a week ago to detect abnormal outbound traffic from POS systems, and this one showed consistent HTTP POST requests to an Eastern European IP address over port 80 — something no legitimate payment terminal should ever be doing.

The analyst, curious but cautious, quickly pivoted to the SIEM logs. There it was: the IP belonged to a hosting service known to be associated with previous POS malware campaigns like *Backoff*. He correlated it with DNS logs, and sure enough, the domain was recently registered and flagged by VirusTotal just two days ago. Within minutes, he escalated the case to the L2 team, tagging the system hostname, user context, timestamps, and correlating it with EDR data showing the presence of an unknown executable running as a background task.

Now it was a race against time.

Knowing that POS terminals handle customer card data in real-time, the team moved into **containment mode**. The goal wasn’t just to block the malicious traffic, but to stop any chance of data exfiltration or internal spread. But disconnecting a POS terminal meant impacting sales at that location — a call that couldn’t be made lightly. The Incident Response Lead weighed the options quickly and decisively: better to lose a few hours of sales than breach customer trust. Using the network access controller, they isolated the POS terminal remotely from the central data center without involving store staff — precise and silent.

Meanwhile, forensic analysts pulled volatile memory snapshots and disk images from the isolated machine. They discovered the malware had achieved persistence through scheduled tasks and registry run keys. It disguised itself as a common Windows service, but the behavior stood out during memory analysis. It was scraping card data from RAM and attempting to exfiltrate it every 4 hours. It had likely been active for about 36 hours before being caught.

Next came **eradication**. The team removed the malware binaries, cleaned registry entries, deleted scheduled tasks, and patched the vulnerability in the OS that had likely been exploited — an unpatched RDP service that allowed brute-force entry. All local admin credentials were rotated. Endpoint protection was updated across all other POS systems chain-wide. The team even wrote a custom YARA rule to detect similar behavior in the future and pushed it to all retail branches.

By morning, it was time to **recover**. A clean image was pushed to the POS system, tested in a sandbox, and redeployed at the store. The firewall continued to monitor any attempt to connect to the previously contacted malicious IPs, but nothing showed up — they had caught it in time. For 48 hours, that terminal remained under watch, but everything returned to normal.

On Friday, the team gathered for the **Lessons Learned** session. It wasn’t just a routine review — it was an autopsy of what happened and why. They traced the incident to weak RDP passwords used during remote troubleshooting by third-party vendors. That led to policy changes, vendor training, and a mandatory switch to 2FA for all remote support. The detection logic in the SIEM was refined and tuned even tighter. A full incident report was sent to the compliance team and company leadership.

Most importantly, this one incident became a case study within the company — not just about how something went wrong, but how it was caught, stopped, and used as a stepping stone to strengthen the entire security posture. As the team wrapped up, one of the analysts wrote in the internal wiki: *“This wasn’t a fire drill. It was the real thing. And we passed.”*


---

## 🔁 STAR Format for Interview (Example)

> **Q:** Tell me about a time you handled a security incident.

**S** - Detected a brute-force attack via SIEM logs.  
**T** - My task was to investigate and contain the threat quickly.  
**A** - I identified the affected user account, disabled it, traced source IPs, and blocked them via firewall.  
**R** - We successfully contained it within 15 minutes with no data breach. Post-analysis led to improved alert tuning.

---

## 🧠 Quick Mnemonic for Stages:  
**P-I-C-E-R-L**  
📌 *Please Investigate Cyber Events Rapidly, Logically*

---
## 🎴 Flashcards: Incident Handling Life Cycle (NIST SP 800-61)

### 🔹 Basics

What is the full name of the standard that defines the Incident Handling Life Cycle?::NIST SP 800-61

How many stages are there in the Incident Handling Life Cycle?::6 stages

What are the 6 stages of Incident Handling?::Preparation, Identification, Containment, Eradication, Recovery, Lessons Learned

---

### 🔹 Preparation

What is the goal of the Preparation phase?::To build capabilities to respond to incidents effectively.

What are some key activities during the Preparation stage?::Develop IR plan, set up policies/playbooks, configure tools, train staff, run simulations.

Which tools are typically used in the Preparation stage?::SIEM, EDR tools, forensic kits, ticketing systems

---

### 🔹 Identification

What is the main goal of the Identification phase?::To detect and confirm a security incident.

What are Indicators of Compromise (IOCs)?::Signs that suggest a system might be compromised (e.g., abnormal traffic, unknown processes)

What tools or data sources help in identifying incidents?::SIEM, IDS/IPS, AV logs, firewall logs, threat intel feeds

What’s the purpose of correlation in this stage?::To confirm if anomalous events are part of an actual security incident

---

### 🔹 Containment

What is the main goal of the Containment phase?::To stop the spread of the threat and limit damage.

What is the difference between short-term and long-term containment?::Short-term is immediate (e.g., isolate host); long-term includes durable controls (e.g., firewall rules, patching).

Why should evidence not be deleted during containment?::It may be needed for forensic investigation or legal proceedings.

---

### 🔹 Eradication

What is the main goal of the Eradication phase?::To completely remove the attacker’s presence from the environment.

What are common eradication activities?::Remove malware/backdoors, patch systems, disable rogue accounts, change credentials.
<!--SR:!2025-07-30,1,230-->

---

### 🔹 Recovery

What is the main goal of the Recovery phase?::To safely restore systems and resume normal business operations.

What steps are involved in the Recovery phase?::Restore from backups, monitor systems, test before full reintegration.

Why should restored systems be monitored after recovery?::To ensure the threat doesn’t resurface and no artifacts remain.

---

### 🔹 Lessons Learned

What is the purpose of the Lessons Learned phase?::To analyze the incident and improve future detection/response.

What deliverables are typically created during this phase?::Post-incident report, updated playbooks, process improvements

Why is it important to conduct a Lessons Learned meeting?::To prevent recurrence, capture insights, and enhance team readiness.

---

### 🔹 General Concepts

What’s a good mnemonic to remember the 6 stages of incident handling?::P-I-C-E-R-L (Preparation, Identification, Containment, Eradication, Recovery, Lessons Learned)
<!--SR:!2025-07-30,1,230-->

What’s an example of a detection tool used in both Identification and Eradication?::EDR (Endpoint Detection and Response)

What should be done before restoring systems in the Recovery phase?::Ensure threat is fully eradicated and backups are clean.

---

### 🔹 Cloze (Fill in the blanks)

The goal of the {{c1::Preparation}} phase is to build capability and readiness for incident response.

{{c1::Identification}} involves detecting and confirming whether an event is a real security incident.

{{c1::Containment}} aims to limit the scope and impact of an incident before it spreads.

{{c1::Eradication}} ensures complete removal of attacker tools, access, and malware.

{{c1::Recovery}} is focused on restoring operations without reintroducing threats.

{{c1::Lessons Learned}} helps improve response strategy and reduce future risk.

---
# ✅ Final Tip

Practice answering:
- "What are the 6 stages of incident handling?"
- "What tools do you use in each stage?"
- "How would you handle a ransomware incident?"

Make your answers clear, structured, and always mention **tools + team collaboration + documentation**.


Tags - #flashcards 