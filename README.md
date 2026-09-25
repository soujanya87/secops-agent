# 🛡️ SecOps AI Agent — ServiceNow Connected

A browser-based Security Operations agent that lets any analyst report security incidents in plain English and instantly creates fully enriched Security Incident Records (SIRs) in ServiceNow — with MITRE ATT&CK mapping, IOC extraction, containment steps, and GRC notes.

**No installation. No Claude account. No API key. Just a browser.**

🔗 **Live Agent:** [https://soujanya87.github.io/secops-agent/](https://soujanya87.github.io/secops-agent/)

---

## 🚀 How to Use

1. Open the link above in any browser
2. Enter your ServiceNow instance URL: `https://dev423253.service-now.com`
3. Enter your ServiceNow username and password
4. Describe any security incident in plain English
5. A real SIR is created in ServiceNow instantly

> Your credentials are used only to connect to ServiceNow. They are never stored or sent anywhere else.

---

## 🎯 What It Does

### Create a SIR — just describe the incident
```
Ransomware detected on PROD-DB-01 — files encrypted with .lockbit extension, 
C2 traffic to 185.220.101.45, hash a1b2c3d4e5f6
```
```
Phishing email from cfo@micros0ft-helpdesk.com with link http://login-verify-365.xyz
— 3 users clicked and entered Office 365 credentials
```
```
Port scanning from 192.168.50.22 targeting our /24 subnet on ports 22, 80, 443, 3389
```
```
USB drive with confidential HR salary data found in the car park — no encryption,
belongs to an ex-employee
```

### Update an existing SIR
```
Close SIR0010025 — threat contained and resolved
Escalate SIR0010057 — needs CISO attention
Assign SIR0010057 to ibrahim.jabbar
Add notes to SIR0010025 — investigating the phishing email source
```

---

## 🔍 Supported Threat Types

| Threat | Category | Priority |
|--------|----------|----------|
| Ransomware / Malware | Malicious code activity | P1 Critical |
| Phishing | Phishing | P1 Critical |
| Unauthorized access / Credential theft | Unauthorized access | P1 Critical |
| Data breach / Exfiltration | Confidential personal identity data exposure | P1 Critical |
| DDoS / Service outage | Denial of Service | P1 Critical |
| Port scan / Reconnaissance | Un-patched vulnerability | P2 High |
| USB / Removable media | Confidential personal identity data exposure | P2 High |
| Unknown / Other | Unauthorized access | P2 High |

---

## 📋 ServiceNow Fields Populated

Every SIR created by the agent populates the following fields directly — not just work notes:

**Core fields**
- `short_description`, `description` (verbatim report)
- `category`, `priority`, `severity`, `urgency`, `business_criticality`
- `state`, `caller`, `opened_for`, `affected_user`
- `incident_detection_date`

**MITRE ATT&CK fields**
- `mitre_tactic`, `mitre_technique`, `mitre_platform`, `mitre_data_source`

**IOC fields** (extracted from description)
- `source_ip`, `dest_ip`, `malware_url`, `malware_hash`, `other_ioc`

**Impact fields**
- `functional_impact` — `low` / `medium` / `high`
- `information_impact` — `privacy_breach`, `proprietary_breach`, `integrity_loss`
- `recoverability` — `regular`, `supplemented`, `extended`, `not_recoverable`

**Reference fields**
- `attack_vector` — sys_id reference to `sn_si_attack_vector` table

**Phishing-specific**
- Creates a linked `sn_si_phishing_email` record with sender, URL, and body

---

## ⚡ Quick Prompts

The agent includes one-click quick prompts for the most common scenarios:

| Button | Action |
|--------|--------|
| 💻 Ransomware | Reports a malware/ransomware incident |
| 🎣 Phishing email | Reports a phishing attack |
| 🔐 Unauthorized access | Reports suspicious login or credential theft |
| 💾 Data breach | Reports data exfiltration or leak |
| ⚠️ Vulnerability | Reports a CVE or exploit |
| 🌐 DDoS attack | Reports a denial of service attack |

---

## 🛠️ Technical Details

**Architecture**
- Pure static HTML — no backend, no server, no build step
- Calls ServiceNow REST API directly from the browser
- Hosted on GitHub Pages

**ServiceNow API calls**
- `GET /api/now/table/sys_user` — authenticate and resolve caller
- `POST /api/now/table/sn_si_incident` — create SIR
- `PATCH /api/now/table/sn_si_incident/{sys_id}` — set attack_vector, affected_user
- `POST /api/now/table/sn_si_phishing_email` — create phishing record
- `PATCH /api/now/table/sn_si_incident/{sys_id}` — update / escalate / close

**CORS requirement**
ServiceNow must allow requests from this domain. Add a CORS rule in your instance:
- Navigate to: `System Web Services → REST → CORS Rules → New`
- Domain: `https://soujanya87.github.io`

---

## 🏗️ Built With

- **ServiceNow Security Incident Response (SIR)** — PDI instance
- **MITRE ATT&CK Framework** — threat classification
- **Claude.ai + sir-triage skill** — AI-powered triage (advanced mode)
- **GitHub Pages** — hosting

---

## 📁 Repository Structure

```
secops-agent/
├── index.html    ← Single-file agent (UI + logic)
├── skill.js      ← sir-triage skill constants (reference)
└── README.md     ← This file
```

---

## 🔐 Security Notes

- Credentials are stored only in browser memory (`sessionStorage`) for the duration of the session
- No credentials are logged, stored, or transmitted to any third party
- All API calls go directly from your browser to your ServiceNow instance
- Logging out clears all credentials from memory immediately

---

## 👤 Author

Built by the SecOps team using Claude.ai and ServiceNow MCP connector.

*For advanced AI-powered triage with correlation, parent-child incident linking, and bulk Draft sweep — use the Claude.ai SecOps agent with the `sir-triage` skill.*
