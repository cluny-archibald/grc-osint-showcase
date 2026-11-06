# Sample ISO/IEC 27001:2022 Policy Set

A core set of information security policies for **Harbourline Logistics Pty Ltd**, the same fictional company used in the [sample assessment report](sample-report.md).

The policies are written in Markdown and kept under version control. Each change is a commit, each document carries its own document control table, and the [changelog](CHANGELOG.md) records what changed and why. This is how I'd manage a client's policy library so that an auditor can see who changed what, when and with whose approval.

> **Sample notice.** Harbourline Logistics is fictional. These policies are examples for portfolio purposes. Any organisation adopting a policy set needs it tailored to its own context, risks, legal obligations and Statement of Applicability.

## Policies

| ID | Policy | Main ISO/IEC 27001:2022 references | Owner |
|---|---|---|---|
| POL-01 | [Information Security Policy](POL-01-information-security-policy.md) | Clause 5.2, A.5.1, A.5.2, A.5.4 | Chief Executive Officer |
| POL-02 | [Access Control Policy](POL-02-access-control-policy.md) | A.5.15–A.5.18, A.8.2, A.8.5 | Head of IT Infrastructure |
| POL-03 | [Acceptable Use Policy](POL-03-acceptable-use-policy.md) | A.5.10, A.6.3, A.6.7, A.8.1 | Head of People and Culture |
| POL-04 | [Supplier Security Policy](POL-04-supplier-security-policy.md) | A.5.19–A.5.23 | Procurement Manager |
| POL-05 | [Incident Management Policy](POL-05-incident-management-policy.md) | A.5.24–A.5.28, A.6.8 | Head of IT Infrastructure |
| POL-06 | [Vulnerability and Patch Management Policy](POL-06-vulnerability-and-patch-management-policy.md) | A.8.8, A.8.9, A.8.19 | Head of IT Infrastructure |

## How the policies link to the assessment

Several findings in the sample assessment trace back to gaps these policies close.

| Finding | Policy that addresses it |
|---|---|
| HL-01 Remote-access portal from a product family with a KEV-listed vulnerability | POL-06 |
| HL-04 Staging login page exposed with an expired certificate | POL-02, POL-06 |
| HL-05 Public code repository may reference internal hostnames | POL-03 |
| HL-07 Third-party scripts on the quote page, including offshore processing | POL-04 |
| HL-09 Microsoft 365 tenant footprint visible | POL-02 |

## Document control conventions

- **Versions.** 0.x is a draft. 1.0 is the first approved version. Minor changes increase the second number (1.1). A full rewrite increases the first number (2.0).
- **Approval.** Every version above 1.0 needs the approver named in the document control table.
- **Review.** Each policy is reviewed at least once a year, and after a significant incident, audit finding or change to the business.
- **Exceptions.** Any exception to a policy is recorded in the risk register with an owner, a reason and an expiry date.
