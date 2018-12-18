Note: the X and "broken" here mean "Email is sent in the instances default language instead of the users language".

This page lists all emails sent by the platform:
1. user signup with confirmation link
2. user welcome (after following confirmation link)
3. **BROKEN** forgotten password (not working in multi-language)
4. enterprise welcome email (triggered after creation on admin/enterprises and also after self-registration)
5. 1. 1. order confirmation to logged in user
      2. resend order confirmation to logged in user from backoffice
   2. 1. **BROKEN** order confirmation to GUEST user (not working in multi-language)
      2. **BROKEN** resend order confirmation to GUEST user from backoffice (not working in multi-language)
6. order confirmation for shop
7. **NOT TESTED** order shipped (I was not able to test this one)
8. **BROKEN** - order cancelation email (not working in multi-language)
9. **NOT TESTED** - order invoice email (I was not able to test this one)
10. 1. enterprise manager invite - new account - invitation email
    2. **BROKEN** - enterprise manager invite - new account - user signup with confirmation link (not working in multi-language)
    3. **BROKEN** - enterprise manager invite - new account - user welcome after following signup email above (not working in multi-language)
    4. **NOT TESTED** - enterprise manager invite - existing account - invitation email (I was not able to test this one)
11. **BROKEN** - order cycle producers notification (not working in multi-language) 


Additionally, there are 6 emails related to subscriptions (**NOT TESTED**):

12. subscription confirmation email
13. order placement email
14. empty order placement email
15. failed payment email
16. order placement summary email (to the shop owner)
17. confirmation summary email (to the shop owner)

NOTE: re point 5. order confirmation emails to customers created in the backoffice will be in default language as there is no way to define customer language currently

[[Delayed job]] is used to send emails in OFN.