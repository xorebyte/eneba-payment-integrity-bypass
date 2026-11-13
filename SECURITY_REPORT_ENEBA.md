# Security Vulnerability Report — Eneba Checkout

**Report ID:** ENB-2026-001  
**Title:** Client-Controlled Amount in Payment-Session Creation  
**Affected area:** Checkout / payment-session integrity  
**Reported:** 2026-11-13  
**Disclosure status:** Researcher reports compensation was declined; current reproducibility is researcher-reported and not independently verified

---

## 1. Executive summary

Testing of Eneba's checkout GraphQL flow observed that the payment-session creation operation accepted a client-supplied `amount` value and returned a payment-session payload representing that supplied value. This behavior was observed across several test values, including zero, while the test cart's payable amount remained unchanged.

These observations establish a **payment-session amount integrity weakness**: the session-creation behavior did not visibly demonstrate reconciliation of the supplied amount against the authoritative cart payable amount.

**Scope of the finding:** The confirmed behavior is the acceptance and reflection of manipulated amounts at payment-session creation. The supplied artifacts do not independently verify the subsequent authorization, capture, order-completion, or fulfillment stages. Those downstream controls require vendor-side validation.

## 2. Affected component

| Field | Value |
|---|---|
| Platform | Eneba (eneba.com) |
| Interface | GraphQL checkout API |
| Operation | `Checkout_createPaymentSession` |
| Payment flow | Checkout.com / Payrails integration |
| Currency observed | EUR |

Exact session identifiers, persisted-query hashes, public integration identifiers, raw payment tokens, and customer/cart metadata have been intentionally omitted from this copy. They can be supplied privately to the vendor if required for correlation.

## 3. Technical observation

The payment-session creation operation accepted multiple client-supplied amount values. The returned session payload's amount field reflected the submitted value, including the zero-value test case.

The observed behavior is inconsistent with a robust server-authoritative payment model if the client-provided amount is trusted without comparison to the current cart payable amount.

The relevant trust boundary is between:
- the authoritative cart/order payable amount maintained by the merchant backend; and
- the amount represented by the payment session created for the payment provider.

The backend should establish and enforce the relationship between these values rather than relying on client-supplied payment amounts.

## 4. Evidence summary

### 4.1 Payment-session amount matrix

The supplied test artifacts record successful HTTP responses for several requested amounts. The corresponding amount represented in each returned session payload matched the requested amount.

The sanitized evidence index records only the tested amount values, response status, and represented amount. All session identifiers and raw token material have been removed.

### 4.2 Final-checkout test scope

A separate final-checkout differential was attempted using a deliberately invalid test payment token. Because the test credential was invalid, this test did not validate the successful authorization and fulfillment path. It is therefore not used as evidence for or against downstream payment acceptance.

### 4.3 What was and was not demonstrated

**Demonstrated by the supplied artifacts**
- The payment-session creation operation accepted the tested client-supplied amount values.
- The amount represented in the returned session payload matched the supplied value.

**Not demonstrated**
- Successful authorization or capture at a zero or reduced amount.
- Successful order completion at the full cart value after a zero/reduced payment.
- Product/key delivery without full payment.
- The precise gateway-side settlement amount for a valid card transaction.

## 5. Impact considerations

If the merchant backend or payment integration relies on the client-controlled session amount without independently reconciling it against the authoritative cart payable amount, a discrepancy could permit underpayment or inconsistent billing/order state.

The current evidence establishes a session-creation integrity weakness. The vendor should assess downstream exposure by validating payment authorization, capture, and fulfillment against the authoritative order total.

## 6. Recommended remediation

1. **Server-authoritative amount:** Derive the payment-session amount from the current server-side cart payable total. Do not trust a client-supplied amount as authoritative.
2. **Strict reconciliation:** Reject session creation if amount or currency differs from the cart's payable amount.
3. **Session-to-order binding:** Bind the payment session to the relevant cart/order and customer context using server-controlled identifiers.
4. **Final verification:** Before accepting payment or fulfilling an order, verify with the payment provider that the authorized/captured amount and currency match the current order payable.
5. **Session lifecycle:** Expire or invalidate sessions when cart contents, quantities, fees, discounts, taxes, or currency change; prevent stale or replayed sessions from being reused.
6. **Monitoring:** Record and alert on payment-session/cart amount mismatches and anomalous zero/reduced-amount session creation.

## 7. Testing and safety notes

- The supplied material describes black-box testing of the production checkout flow.
- The evidence in this repository is scoped to payment-session creation; it does not independently document the later authorization, capture, or fulfillment stages.
- Raw session credentials, tokens, cookies, personal data, IP addresses, cart identifiers, and account identifiers have been excluded from this repository.
- No further testing should be performed against production payment flows without explicit authorization and an agreed test procedure.

## 8. Researcher statement and disclosure timeline

The researcher states that they approached Eneba staff seeking compensation for reporting this issue and that the staff declined to pay. The researcher says this refusal was a reason they decided to put the sanitized report on GitHub. This is the researcher's account; correspondence or other independent confirmation of the compensation discussion is not included in this repository.

The researcher further reports that the issue is still working/reproducible as of 2026-11-13. This status has not been independently retested as part of this repository update and should not be read as a new production test or vendor confirmation. The finding remains scoped to the payment-session creation behavior described in this report; the later authorization, capture, and fulfillment stages have not been independently verified.

| Date | Event |
|---|---|
| 2026-09-07 | Additional validation described in source material |
| 2026-11-13 | Sanitized report prepared; researcher reports compensation request was declined and issue remains reproducible |
| Pending | Independent validation / vendor response |
| Pending | Vendor verification and remediation |

---

**Researcher contact:** To be provided directly to the vendor through an appropriate private disclosure channel.
