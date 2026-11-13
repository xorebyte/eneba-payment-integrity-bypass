# Evidence index (sanitized)

All files in this directory are minimized summaries of the supplied test artifacts. Session IDs, payment-session secrets, raw token blobs, public integration identifiers, cookies, authorization data, customer details, IP addresses, cart/order identifiers, and account identifiers have been excluded.

| File | Contents |
|---|---|
| [createpaymentsession_live_matrix.json](createpaymentsession_live_matrix.json) | Sanitized authenticated-session amount matrix |
| [createpaymentsession_amount_matrix.txt](createpaymentsession_amount_matrix.txt) | Sanitized amount matrix for a separate test cart |
| [checkout_finalstep_differential_live.json](checkout_finalstep_differential_live.json) | Sanitized record of an inconclusive final-step test using an invalid test token |

These summaries preserve the reported HTTP status and amount relationship, but intentionally do not include reusable payment-session material.

**Scope note:** The final-step test used an invalid test payment token and therefore did not validate the successful authorization or fulfillment path. The primary finding is the client-controlled amount accepted and reflected during payment-session creation.
