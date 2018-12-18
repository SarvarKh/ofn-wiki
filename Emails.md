Note: the X and "broken" here mean "Email is sent in the instances default language instead of the users language".

This page lists all emails sent by the platform:
- user signup with confirmation link
- user welcome (after following confirmation link)
- X - forgotten password (not working in multi-language)
- enterprise welcome email (triggered after creation on admin/enterprises and also after self-registration)
- X - order confirmation for customer as GUEST (not working in multi-language)
- order confirmation for customer as logged in user
- order confirmation for shop
- X - resend order confirmation (not working in multi-language) 
- ? - order shipped (I was not able to test this one)
- X - order cancelation email (not working in multi-language)
- ? - order invoice email (I was not able to test this one)
- enterprise manager invite - new account - invitation email
- X - enterprise manager invite - new account - user signup with confirmation link (not working in multi-language)
- X - enterprise manager invite - new account - user welcome after following signup email above (not working in multi-language)
- ? - enterprise manager invite - existing account - invitation email (I was not able to test this one)
- X - order cycle producers notification (not working in multi-language) 


Additionally, there are 6 emails related to subscriptions (not tested):
- subscription confirmation email
- order placement email
- empty order placement email
- failed payment email
- order placement summary email (to the shop owner)
- confirmation summary email (to the shop owner)

[Delayed job] is used to send emails in OFN.