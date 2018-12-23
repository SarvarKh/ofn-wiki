Note: BROKEN here means "Email is sent in the instances default language instead of the users language".

This page lists all emails sent by the platform:
1. user signup with confirmation link
2. user welcome (after following confirmation link)
3. forgotten password (**BROKEN** not working in multi-language)
4. 1. enterprise welcome email - triggered after enterprise creation on admin/enterprises
   2. enterprise welcome email - triggered during self-registration in the frontoffice
5. 1. order confirmation - triggered during checkout
   2. order confirmation - triggered as resend in the backoffice
6. order confirmation for shop
7. order shipped (BROKEN not working in multi-language)
8. order cancelation email
9. order invoice email
10. 1. enterprise manager invite - new account - invitation email (**BROKEN** not working in multi-language)
    2. enterprise manager invite - new account - user signup with confirmation link (**BROKEN** not working in multi-language)
11. order cycle producers notification

Additionally, there are 6 emails related to subscriptions (**BROKEN** not working in multi-language):

12. subscription confirmation email
13. order placement email
14. empty order placement email
15. failed payment email
16. order placement summary email (to the shop owner)
17. confirmation summary email (to the shop owner)


NOTE: currently #5.1 #5.2 and #8 will not work in multi-language for orders placed by a GUEST user or by a customer created in the backoffice because it's difficult to persist guest user locale and there's no way to know what's the customer locale

NOTE: no email is sent when existing user is added as enterprise manager

[[Delayed job]] is used to send emails in OFN.