# Private Security Disclosure — Eneba Checkout Payment-Session Amount Integrity

Hello Eneba Security Team,

I would like to report a potential payment-integrity issue in the checkout GraphQL flow, involving the `Checkout_createPaymentSession` operation.

During testing, the operation accepted several client-supplied amount values and returned payment-session data whose represented amount matched the supplied value, including a zero-value test case. The cart payable amount was not changed between those test cases.

A separate final-checkout comparison using an invalid test payment token returned the same generic failure for both the normal-amount and zero-value session scenarios.

The evidence currently establishes the payment-session creation behavior, but does **not** establish a successful zero-value authorization/capture, completed order, or product delivery. I have deliberately avoided claiming a confirmed end-to-end payment bypass.

The attached report describes the observations, limitations, and recommended server-side controls. The evidence has been sanitized to remove session identifiers, payment secrets, raw token data, cookies, personal information, IP addresses, and cart/account identifiers.

I previously approached Eneba staff seeking compensation for reporting this issue, but my request was declined. I have therefore decided to publish this sanitized technical account on GitHub. I also report that the issue remains reproducible as of 2026-11-13; this is my current assessment and has not been independently verified by this repository update. The evidence still does not establish a successful underpayment, completed order, or product delivery.

Could your team review whether the amount used to create a payment session is derived from and bound to the authoritative cart payable amount, and whether the payment provider's authorized/captured amount is verified against the order before fulfillment?

I can provide additional non-secret correlation details through a secure channel if needed.

Regards,  
Security Researcher
