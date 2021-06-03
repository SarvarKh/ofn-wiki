Note: BROKEN here means "Email is sent in the instances default language instead of the users language". Epic representing the broken cases https://github.com/openfoodfoundation/openfoodnetwork/issues/3243

This page lists all emails sent by the platform:
1. user signup with confirmation link
2. user welcome (after following confirmation link)
3. forgotten password (**BROKEN** not working in multi-language)
4. 1. enterprise welcome email - triggered after enterprise creation on admin/enterprises
   2. enterprise welcome email - triggered during self-registration in the frontoffice
5. 1. order confirmation - triggered during checkout
   2. order confirmation - triggered as resend in the backoffice
6. order confirmation for shop
7. order shipped (**BROKEN** not working in multi-language)
8. order cancellation email
9. order cancellation email for shop
10. order invoice email
11. 1. enterprise manager invite - new account - invitation email (**BROKEN** not working in multi-language)
    2. enterprise manager invite - new account - user signup with confirmation link (**BROKEN** not working in multi-language)
12. order cycle producers notification

Additionally, there are 6 emails related to subscriptions (**BROKEN** not working in multi-language):

13. subscription confirmation email
14. order placement email
15. empty order placement email
16. failed payment email
17. order placement summary email (to the shop owner)
18. confirmation summary email (to the shop owner)


NOTE: currently #5.1 #5.2 and #8 will not work in multi-language for orders placed by a GUEST user or by a customer created in the backoffice because it's difficult to persist guest user locale and there's no way to know what's the customer locale

NOTE: related to case 11 above, no email is sent when an existing user is added as an manager of an enterprise

Sidekiq is used to send emails in OFN.
