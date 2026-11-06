# Evidence grading

External observation can't see everything, so every finding states how strong its evidence is. That lets a risk owner tell the difference between something proven and something worth checking.

## Evidence grade

| Grade | Meaning |
|---|---|
| **Confirmed** | Directly verified from a public source. Anyone could reproduce it. |
| **Probable** | Strong indicators point to it, but one element can't be verified from outside. |
| **Possible** | Consistent with the observation, but it needs internal checking to confirm or rule out. |

## Basis

| Basis | Meaning |
|---|---|
| **Observed** | Seen directly in a response, record or published document. |
| **Inferred** | Concluded from observed facts, for example a product family linked to a known vulnerability where the version can't be seen. |

## Severity

Severity reflects the likely impact if the finding is real and exploited, adjusted for exploitation context such as KEV listing.

| Severity | Meaning |
|---|---|
| **High** | Could lead directly to compromise or a reportable breach. |
| **Medium** | Weakens a control or widens the attack surface in a way an attacker could use. |
| **Low** | A hygiene or disclosure issue with limited direct impact. |
| **Informational** | Context the client should know, with no action required. |

The platform does not use a "critical" rating. Passive assessment sees from outside, and a rating that strong needs an inside view to justify it.

## Confidence

High, Medium or Low. This shows how sure the assessment is that the finding belongs to the organisation and is current.

## Priority

Priority combines severity, evidence grade and confidence into an order for treatment:

| Priority | Typical combination |
|---|---|
| **P1** | High severity with Confirmed or Probable evidence |
| **P2** | High severity with Possible evidence, or Medium severity with Confirmed evidence, or Probable evidence at High confidence |
| **P3** | Medium severity with Probable or Possible evidence |
| **P4** | Low severity or Informational |

## Not assessed

When a data source is rate-limited, unavailable or returns nothing, the check is reported as **Not assessed**. It is never reported as "no issue found". A gap in coverage is not an all-clear.
