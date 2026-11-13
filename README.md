# Eneba Checkout — Payment-Session Amount Integrity

![Disclosure](https://img.shields.io/badge/status-private%20disclosure-yellow)
![Area](https://img.shields.io/badge/area-checkout%20%2F%20payments-blue)
![Evidence](https://img.shields.io/badge/evidence-redacted-success)

> **PUBLIC SECURITY DISCLOSURE**
>
> This repository documents a security issue identified in Eneba's checkout payment-session flow.
>
> Following discussions with Eneba staff, my request for compensation for reporting this issue was declined. I therefore decided to publish this sanitized report to make the findings publicly accessible.
>
> Sensitive credentials, session data, and personal information have been removed. All technical claims are limited to the available evidence. The issue is reported to remain reproducible as of November 13, 2026.

## Overview

Testing of Eneba's checkout GraphQL flow observed that the payment-session creation operation accepted client-supplied amount values and returned session data whose represented amount matched those values, including a zero-value test case.

This indicates a **payment-session amount integrity issue** that warrants server-side investigation. The available artifacts demonstrate session-creation behavior; they do **not** independently establish a successfully captured zero-value payment, a completed order, or product delivery without payment.

## Key observations

- Multiple test amount values were accepted by the payment-session creation operation.
- The returned session payload represented the client-supplied amount.
- A final-checkout differential using an invalid test payment token returned the same generic failure for the normal and zero-value session scenarios.

## Contents

| Path | Description |
|---|---|
| [Technical report](SECURITY_REPORT_ENEBA.md) | Scope, observed behavior, limitations, impact considerations, and remediation |
| [Vendor submission](SUBMISSION.md) | Concise responsible-disclosure message |
| [Evidence index](evidence/README.md) | Redacted evidence and interpretation |

## Security and privacy

The repository intentionally excludes session IDs, payment-session secrets, raw token payloads, cookies, authorization headers, personal email addresses, IP addresses, cart/order identifiers, account identifiers, and test-environment script paths. Evidence has been minimized to the observations needed for triage.

## Recommended remediation

1. Derive payment-session amounts exclusively from authoritative server-side cart payable totals.
2. Reject mismatches between cart payable amount, currency, and payment-session amount.
3. Bind each payment session to the corresponding cart/order and verify the gateway-side amount before authorization, capture, and fulfillment.
4. Invalidate stale sessions whenever cart contents, currency, fees, or payable totals change.
5. Log and alert on payment-session/cart amount mismatches.

## Researcher statement and disclosure status

The researcher states that, after approaching Eneba staff seeking compensation for reporting the issue, the staff declined to pay. The researcher says this response was a factor in deciding to place this sanitized report on GitHub.

The researcher also reports that the issue is still working/reproducible as of 2026-11-13. This is the researcher's statement, not an independent retest or vendor confirmation; no new production testing is represented by this repository update. The technical evidence and limitations described above remain unchanged: the supplied artifacts do not establish a successful underpayment, completed order, or product delivery.

- Report prepared: 2026-11-13
- Compensation request: researcher reports it was declined by Eneba staff
- Current reproducibility: researcher-reported; not independently verified in this update
- Vendor remediation: not confirmed

**Handling:** This repository remains private. Share it only with authorized security contacts or other recipients you intentionally choose.
