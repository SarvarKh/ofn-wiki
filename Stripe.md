## Stripe Integration with OFN

#### Stripe Connect

OFN uses Stripe Connect to allow shops to easily use Stripe without having to do anything with publishable/secret keys. The instance manager [sets up a Stripe account](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Setting-up-Stripe-on-an-OFN-instance) that shop managers then authorize to accept cards on their behalf. Transactions appear in the shop manager's Stripe account. Saved credit cards, however, are saved on the platform account.

#### Saving cards

Since cards are saved on the platform account, in order to use the [Payment Intents API](https://stripe.com/docs/payments/payment-intents), in particular when [setting up an Intent](https://stripe.com/docs/api/setup_intents), we need to [clone the customer's card to the connected account](https://stripe.com/docs/connect/cloning-saved-payment-methods).

To avoid creating a new clone of the card/customer each time the card is charged or authorized (e.g. for SCA), we attach metadata `{ clone: true }` to the card the first time we clone it and look for a card with the same fingerprint (hash of the card number) and that metadata key to avoid cloning it multiple times. This is done in the `Stripe::CreditCardCloner` module.

#### SCA

OFN has a Payment Method that uses Stripe's Payment Intents and Setup Intents API's. These API's allow for the case where a bank requests an additional authorization step for a payment, typically entering a code sent to their phone or other app. This can happen for payments that are made during the checkout process (i.e. when the customer is present and can immediately authorize), or for payments that are run offline (e.g. subscriptions). 

In order to run a charge offline, we use the [Setup Intent API](https://stripe.com/docs/payments/setup-intents) to authorize the card for offline use. The bank may ask for SCA authorization at this point. We use the [Stripe.js](https://stripe.com/docs/js/setup_intents/confirm_card_setup) library here to confirm the card setup. 

Once the card is set up for offline payments, the card issuer still may require the additional authorization. If this happens, we send the customer an email requesting that they visit an OFN url, which then redirects them to the Stripe-provided authorization url for the payment. In the case of a subscription, we also send the hub manager an email letting them know that the payment requires additional authorization.
In the case of the shop manager adding the payment themselves, we only email the customer. 
 