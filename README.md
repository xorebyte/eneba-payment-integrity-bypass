# Eneba Checkout — Payment-Session Amount Integrity

![Disclosure](https://img.shields.io/badge/status-private%20disclosure-yellow)
![Area](https://img.shields.io/badge/area-checkout%20%2F%20payments-blue)
![Evidence](https://img.shields.io/badge/evidence-redacted-success)

> **CONFIDENTIAL — RESPONSIBLE DISCLOSURE**
>
> This repository is a private technical disclosure workspace for the vendor and authorized reviewers. Do not redistribute or make it public before coordinated disclosure and remediation.

## Overview

Testing of Eneba's checkout GraphQL flow observed that the payment-session creation operation accepted client-supplied amount values and returned session data whose represented amount matched those values, including a zero-value test case.

This indicates a **payment-session amount integrity issue** that warrants server-side investigation. The available artifacts demonstrate session-creation behavior; they do **not** independently establish a successfully captured zero-value payment, a completed order, or product delivery without payment.

## Key observations

- Multiple test amount values were accepted by the payment-session creation operation.
- The returned session payload represented the client-supplied amount.
- A final-checkout differential using an invalid test payment token returned the same generic failure for the normal and zero-value session scenarios.
- No successful payment, order completion, or fulfillment was recorded in the supplied evidence.

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

## Disclosure status

- Report prepared: 2026-11-13
- Vendor notification: not confirmed
- Vendor response: not recorded
- Remediation verification: pending

**Handling:** Keep this repository private and share it only with authorized security contacts.
