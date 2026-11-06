# External Exposure and GRC Assessment: Sample Report

| | |
|---|---|
| **Organisation** | Harbourline Logistics Pty Ltd *(fictional)* |
| **Sector** | Transport and logistics |
| **Domains in scope** | `harbourline.example`, `harbourline-freight.example` |
| **Assessment type** | Passive external assessment (public sources only) |
| **Assessment date** | 6 November 2026 |
| **Report status** | Sample, for demonstration only |

> **Sample notice.** Harbourline Logistics is a fictional company. All domains use the reserved `.example` top-level domain, and all IP addresses come from documentation ranges (RFC 5737). The findings are illustrative. They show the structure and reasoning of a real report, but they describe no real organisation.

---

## 1. Headline

**Overall rating: Elevated external exposure**

Harbourline's main website is well protected behind a content delivery network. Around it, though, several assets are exposed that an attacker's reconnaissance would find within hours. These are a remote-access portal from a product family with a known exploited vulnerability, an origin server that can be reached without the network's protection, a staging login page, and email settings that allow spoofing of the company's domain.

None of these items has been tested or exploited. They were observed from public sources. The three priority items below should be checked internally this month, and the privacy disclosure item has a fixed legislative date.

**Priority actions**

1. Confirm the version and patch status of the remote-access portal (HL-01).
2. Restrict direct access to the origin web server (HL-02).
3. Move the domain's DMARC policy from monitoring to enforcement (HL-03).
4. Update the privacy policy for automated decision-making before 10 December 2026 (HL-06).

## 2. Scope and method

The assessment used public sources only: DNS, public web responses, published certificates, app store listings, public code and package registries, vulnerability databases and the Australian Business Register.

**What was not done:** no port scanning, no login attempts, no vulnerability testing and no exploitation. Nothing was accessed that a normal visitor or public data source could not see.

Each finding is graded for evidence (Confirmed, Probable or Possible), basis (Observed or Inferred), severity and confidence. The grading scale is explained in the [evidence grading guide](../docs/evidence-grading.md).

## 3. Summary of findings

| ID | Finding | Severity | Evidence | Confidence | Priority |
|---|---|---|---|---|---|
| HL-01 | Remote-access portal from a product family with a KEV-listed vulnerability | High | Possible · Inferred | Medium | **P2** |
| HL-02 | Origin web server reachable directly, bypassing the CDN | Medium | Probable · Observed | High | **P2** |
| HL-03 | DMARC policy set to monitor only | Medium | Confirmed · Observed | High | **P2** |
| HL-04 | Staging login page exposed with an expired certificate | Medium | Confirmed · Observed | High | **P2** |
| HL-05 | Public code repository may reference internal hostnames | Medium | Possible · Observed | Low | **P3** |
| HL-06 | Privacy policy does not disclose automated decision-making | Medium | Probable · Inferred | Medium | **P3** |
| HL-07 | Third-party scripts on the quote page, including offshore processing | Low | Confirmed · Observed | High | **P4** |
| HL-08 | Mobile app privacy label not reflected in the privacy policy | Low | Probable · Observed | Medium | **P4** |
| HL-09 | Microsoft 365 tenant footprint visible | Informational | Confirmed · Observed | High | **P4** |

## 4. Risk register entries

### HL-01: Remote-access portal from a product family with a KEV-listed vulnerability

| | |
|---|---|
| **Severity** | High |
| **Evidence** | Possible · Inferred |
| **Confidence** | Medium |
| **Priority** | P2 |
| **Asset** | `vpn.harbourline.example` (198.51.100.17) |
| **Controls** | ISO/IEC 27001:2022 A.8.8 Management of technical vulnerabilities, A.8.20 Networks security |
| **Obligations** | Essential Eight: patch operating systems (internet-facing network devices) |
| **Suggested owner** | Head of IT Infrastructure |

**Observation.** The host serves a Palo Alto Networks GlobalProtect portal login page. This product family is affected by CVE-2024-3400, a command injection vulnerability in PAN-OS that is listed in the CISA Known Exploited Vulnerabilities catalogue.

**Why it's only Possible.** The PAN-OS version can't be seen from outside. The portal may already be patched. The finding is inferred from the product family, not from a confirmed vulnerable version.

**Treatment.** Confirm the PAN-OS version against the vendor advisory and apply fixes if needed. Check that the device is in the vulnerability management scope with a defined patch timeframe for internet-facing devices. If it was ever unpatched while exposed, consider a compromise check in line with the vendor's guidance.

---

### HL-02: Origin web server reachable directly, bypassing the CDN

| | |
|---|---|
| **Severity** | Medium |
| **Evidence** | Probable · Observed |
| **Confidence** | High |
| **Priority** | P2 |
| **Asset** | 203.0.113.24 (origin for `www.harbourline.example`) |
| **Controls** | A.8.20 Networks security, A.8.9 Configuration management |
| **Suggested owner** | Web Platform Manager |

**Observation.** The main website sits behind a CDN and web application firewall. Historical DNS records point to 203.0.113.24, which presents a certificate for `www.harbourline.example` and serves the same site content.

**Why it's Probable.** The certificate and content match, but it can't be confirmed from outside whether the origin accepts all traffic or only some.

**Treatment.** Restrict the origin to accept traffic only from the CDN's published address ranges, or use an authenticated origin connection. Rotate the origin's IP address if the old one has been public.

---

### HL-03: DMARC policy set to monitor only

| | |
|---|---|
| **Severity** | Medium |
| **Evidence** | Confirmed · Observed |
| **Confidence** | High |
| **Priority** | P2 |
| **Asset** | `harbourline.example` (DNS) |
| **Controls** | A.5.14 Information transfer, A.8.9 Configuration management |
| **Suggested owner** | Messaging and Collaboration Lead |

**Observation.** The DMARC record is set to `p=none`, and the SPF record ends in `~all` (soft fail). Mail that fails authentication is still delivered.

**Why it matters.** Anyone can send email that appears to come from `@harbourline.example`. For a freight business that issues invoices and delivery notices, that supports invoice fraud and phishing aimed at customers.

**Treatment.** Review DMARC aggregate reports to confirm all legitimate senders are aligned. Then move to `p=quarantine` and then `p=reject`, in line with ASD guidance on email authentication.

---

### HL-04: Staging login page exposed with an expired certificate

| | |
|---|---|
| **Severity** | Medium |
| **Evidence** | Confirmed · Observed |
| **Confidence** | High |
| **Priority** | P2 |
| **Asset** | `staging-portal.harbourline.example` |
| **Controls** | A.8.31 Separation of development, test and production environments, A.5.9 Inventory of information and other associated assets |
| **Suggested owner** | Head of Digital |

**Observation.** A staging copy of the customer portal is publicly reachable and shows a login page. Its TLS certificate expired more than four months ago, which suggests nobody is maintaining it.

**Why it matters.** Staging systems often have weaker controls, test accounts or copies of real data. An unmaintained one is unlikely to be monitored or patched.

**Treatment.** Confirm whether the environment is still needed. If it is, put it behind the VPN or an access restriction and add it to the asset inventory. If it isn't, decommission it and remove the DNS record.

---

### HL-05: Public code repository may reference internal hostnames

| | |
|---|---|
| **Severity** | Medium |
| **Evidence** | Possible · Observed |
| **Confidence** | Low |
| **Priority** | P3 |
| **Asset** | Public repository under an account named `harbourline-dev` |
| **Controls** | A.8.4 Access to source code, A.8.12 Data leakage prevention |
| **Suggested owner** | Head of Digital |

**Observation.** A public repository contains a configuration file that references `db01.int.harbourline.example`. No passwords or keys were observed.

**Why it's only Possible.** It isn't confirmed that the account belongs to Harbourline or one of its developers. It could be a former contractor or an unrelated party.

**Treatment.** Confirm ownership. If it's Harbourline's, make it private or remove it. Check its history for credentials, and rotate any found. Set a policy for publishing code to public platforms.

---

### HL-06: Privacy policy does not disclose automated decision-making

| | |
|---|---|
| **Severity** | Medium |
| **Evidence** | Probable · Inferred |
| **Confidence** | Medium |
| **Priority** | P3 |
| **Asset** | `harbourline.example/privacy` and the online quote tool |
| **Controls** | A.5.34 Privacy and protection of PII |
| **Obligations** | Privacy Act 1988, APP 1.7–1.9 (automated decision-making transparency, from 10 December 2026) |
| **Suggested owner** | Privacy Officer |

**Observation.** The privacy policy was last updated in 2021 and doesn't mention automated decision-making. The online quote tool returns instant pricing and account-terms decisions, and it loads a third-party pricing service.

**Why it's Probable.** The quote tool appears to make decisions automatically using personal information. How much those decisions use personal information can't be confirmed from outside.

**Treatment.** Confirm which tools make or substantially assist decisions about individuals using personal information. Update the privacy policy with the disclosures APP 1.7–1.9 require before 10 December 2026.

---

### HL-07: Third-party scripts on the quote page, including offshore processing

| | |
|---|---|
| **Severity** | Low |
| **Evidence** | Confirmed · Observed |
| **Confidence** | High |
| **Priority** | P4 |
| **Asset** | `harbourline.example/quote` |
| **Controls** | A.5.19 Information security in supplier relationships, A.5.21 Managing information security in the ICT supply chain |
| **Obligations** | Privacy Act 1988, APP 8 (cross-border disclosure) |
| **Suggested owner** | Procurement Manager, with the Privacy Officer |

**Observation.** The quote page loads scripts from eleven third-party domains, including analytics, a chat widget and a pricing service. The chat vendor's published terms show processing outside Australia.

**Treatment.** Check that each supplier is in the supplier register with a due diligence record covering ownership, jurisdiction, data residency and sub-processors. Remove scripts that are no longer needed, and confirm the privacy policy covers cross-border disclosure.

---

### HL-08: Mobile app privacy label not reflected in the privacy policy

| | |
|---|---|
| **Severity** | Low |
| **Evidence** | Probable · Observed |
| **Confidence** | Medium |
| **Priority** | P4 |
| **Asset** | *Harbourline Driver* app (iOS and Android listings) |
| **Controls** | A.5.34 Privacy and protection of PII |
| **Suggested owner** | Privacy Officer |

**Observation.** The app store privacy label says precise location is collected and linked to the user's identity. The privacy policy doesn't mention location data.

**Treatment.** Align the privacy policy with what the app actually collects. Confirm how long location data is kept and who can access it.

---

### HL-09: Microsoft 365 tenant footprint visible

| | |
|---|---|
| **Severity** | Informational |
| **Evidence** | Confirmed · Observed |
| **Confidence** | High |
| **Priority** | P4 |
| **Asset** | Microsoft 365 / Entra ID tenant for `harbourline.example` |
| **Controls** | A.5.15 Access control, A.8.5 Secure authentication |
| **Suggested owner** | Identity and Access Lead |

**Observation.** Public lookups confirm a Microsoft 365 tenant with managed authentication. This is normal and can't be hidden. It does tell an attacker where to aim password-spraying and phishing.

**Treatment.** No change needed to the footprint. Confirm that phishing-resistant multi-factor authentication covers all users, and that legacy authentication is blocked.

## 5. Not assessed

| Check | Reason |
|---|---|
| Passive DNS history for `harbourline-freight.example` | The free data source was rate-limited during the assessment and returned no data. |

This check is reported as **Not assessed**, not as "no issue found". It can be re-run on request.

## 6. Framework coverage

| ISO/IEC 27001:2022 control | Findings |
|---|---|
| A.5.9 Inventory of information and other associated assets | HL-04 |
| A.5.14 Information transfer | HL-03 |
| A.5.15 Access control | HL-09 |
| A.5.19 Information security in supplier relationships | HL-07 |
| A.5.21 Managing information security in the ICT supply chain | HL-07 |
| A.5.34 Privacy and protection of PII | HL-06, HL-08 |
| A.8.4 Access to source code | HL-05 |
| A.8.5 Secure authentication | HL-09 |
| A.8.8 Management of technical vulnerabilities | HL-01 |
| A.8.9 Configuration management | HL-02, HL-03 |
| A.8.12 Data leakage prevention | HL-05 |
| A.8.20 Networks security | HL-01, HL-02 |
| A.8.31 Separation of development, test and production environments | HL-04 |

## 7. Limitations

This is an external view only. It can't see internal controls, patch levels, configurations or monitoring. Findings graded Possible or Inferred need internal checking before anyone acts on them as fact. A clean result in any area means nothing was observed from outside. It doesn't mean the control is effective.

## 8. Next steps

Remediation belongs to Harbourline. Each register entry names a suggested owner and a treatment. The full register is provided as a separate file so it can be merged into Harbourline's enterprise risk register.

A follow-up assessment can be run on request to confirm which items have been closed.
