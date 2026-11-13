# Private Security Disclosure — Eneba Checkout Payment-Session Amount Integrity

Hello Eneba Security Team,

I would like to report a potential payment-integrity issue in the checkout GraphQL flow, involving the `Checkout_createPaymentSession` operation.

During testing, the operation accepted several client-supplied amount values and returned payment-session data whose represented amount matched the supplied value, including a zero-value test case. The cart payable amount was not changed between those test cases.

The confirmed finding is that payment-session creation accepted multiple client-supplied amounts, including zero, and returned session data reflecting those values while the cart payable amount remained unchanged. The report documents the payment-session amount-integrity behavior observed during testing.

The attached report describes the observations, limitations, and recommended server-side controls. The evidence has been sanitized to remove session identifiers, payment secrets, raw token data, cookies, personal information, IP addresses, and cart/account identifiers.

I previously approached Eneba staff seeking compensation for reporting this issue, but my request was declined. I have therefore decided to publish this sanitized technical account on GitHub. I also report that the issue remains reproducible as of 2026-11-13; this is my current assessment and has not been independently verified by this repository update. The report presents the observed payment-session behavior and supporting sanitized evidence.

Could your team review whether the amount used to create a payment session is derived from and bound to the authoritative cart payable amount, and whether the payment provider's authorized/captured amount is verified against the order before fulfillment?

I can provide additional non-secret correlation details through a secure channel if needed.

Regards,  
Security Researcher
