### Before you begin
This is a walkthrough for setting up Stripe on an OFN instance. At present, to complete this process you will need someone who is comfortable logging into the server via a secure shell, making changes to files and restarting the server. It is assumed that you already have a server running before starting this guide.

### Intro to Stripe Connect
The OFN has been set up to use Stripe Connect, which is an offering from Stripe designed for marketplaces with multiple vendors connecting with customers through a central platform (sounds like the OFN!). The basic concept is that a central 'platform' is registered with Stripe, and then other Stripe users can 'connect' their accounts to the platform, thereby authorising the 'platform' to charge customers on their behalf. From the customer's perspective, most of this detail is hidden, and they experience Stripe Connect as a fairly generic-looking credit card form in the checkout. They absolutely do not need to comprehend any of the complexity of the links between the platform and the vendors.

There is a fairly extensive discussion about the pros-and-cons of this approach on the [community forum](https://community.openfoodnetwork.org/t/integrating-stripe-into-ofn/664). In short, Stripe Connect allows us to do three really useful things:
1. Charge customers on behalf of vendors without requiring access to the vendors' Stripe API keys
2. Store customer card details without having those details ever pass through our servers
3. Charge each customer on behalf of multiple vendors, without the customer ever having to re-enter their card details

There is a more in-depth technical explanation of the process used to charge customers [here](SOME LINK) if you are interested

## Instructions
### Step 1. Create a Stripe Account
Before you do anything, you will need to create a Stripe account (at [stripe.com](https://stripe.com)) that you will use to administer your 'platform'. It is a good idea to create a brand new dedicated account for this purpose. 

### Step 2. Register your platform
To add Connect functionality to your account, you will need to register your platform with Stripe. This entails providing details about your OFN instance, which Stripe requires in order to verify the authenticity of the platform, and to authorise you to create charges on behalf of other Stripe accounts. Follow the first step `Step 1: Register your platform
` in Connect's [quickstart guide](https://stripe.com/docs/connect/quickstart#register-platform). That is, you register your Stripe account to use Stripe Connect. 

### Step 3. Set up your Connect callbacks
When users of your OFN instance want to connect their Stripe accounts, they will be redirected from the OFN to Stripe and back again. You will need to tell Stripe where you would like users to be redirected to after they have finished creating and connecting their accounts. There are very specific URIs that should be used here.

To enter them, go to your Stripe Dashboard, then select `Settings` from the side menu. Then, if you scroll you will find the `Stripe apps` section. Click on `Connect settings`. If you scroll to the bottom, you should be able to enter `Redirect URIs` for your Development and Production environments. Check the video below for details.

The URI you need to enter for each needs to follow this format:

````
https://[YOUR OFN DOMAIN]/stripe/callbacks
````

**You should also take note of the `client_id` provided in this section. You will need to use it to configure your OFN instance (Step 5).**

![](https://github.com/openfoodfoundation/openfoodnetwork/wiki/stripe_client_id_and_redirects.gif)

### Step 4. Set up your Connect webhooks
Stripe can communicate important information about events that happen to connected accounts outside of the OFN via webhooks. At the moment, the OFN codebase has been configured to only listen for the most important webhook (deauthorisation of the platform by a connected account). This will allow the OFN to know about a disconnection request that is initiated from Stripe (rather than via the OFN).

To set up this webhook, head to your Stripe Dashboard, click `Developers` from the side menu, and then click `Webhooks` from menu that appears beneath it. In the section titled `Endpoints receiving events from Connect applications`, click the `+ Add Endpoint` button. In the dialog that appears, enter the following for `URL to be called`:

````
https://[YOUR OFN DOMAIN]/stripe/webhooks
````

Then select `Select types to send` and check `account.application.deauthorized`, which should be second from the top. Then click `Add Endpoint` to save.

By clicking on the endpoint you just created you will see all its details. **Now, click on it take note of the `Signing secret`**. You will need to use it to configure your OFN instance (Step 5).

![](https://github.com/openfoodfoundation/openfoodnetwork/wiki/stripe_webhooks.gif)

### Step 5. Configure your OFN instance
Most of the configuration of the OFN will be done through use of the `application.yml` file. This is the same file used to configure the language and currency information.

To use the OFN with Stripe, you will need a `client_id` (from Step 3), a webhook `Signing Secret` (from step 4) the `Publishable key` and `Secret key`. The last tow can be found by going to your Stripe Dashboard, navigating to `Developers` from the side menu and clicking on `API keys`. API keys can be either `test` keys or `live` keys. You can make your `test` keys visible by clicking the 'Viewing test data' switch, the last item in the side menu. For staging you will need these, while in production you need the `live` ones.

![](https://github.com/openfoodfoundation/openfoodnetwork/wiki/stripe_keys.gif)

You can then use the values you have just looked up to set the following values in the `config/application.yml` file on your OFN server. Please note that if you are using a development `client_id`, then you must use `test` API keys. If you are using a production `client_id` you must use `live` API keys.

````yml
STRIPE_CLIENT_ID: "ca_xxx" # This can be a development ID or a production ID
STRIPE_INSTANCE_SECRET_KEY: "sk_test_xxx" # This can be a test key or a live key
STRIPE_INSTANCE_PUBLISHABLE_KEY: "pk_test_xxx" # This can be a test key or a live key
STRIPE_ENDPOINT_SECRET: "whsec_xxx"
````

### Step 6. Restart your server
How this is done may vary depending on the server, but it will be necessary to ensure your OFN instance is running with the new configuration you have just set up. Note this might mean restarting the unicorn after the provisioning.

### Step 7. Verify your Stripe configuration
You can check that the configuration settings you have just entered are valid from the super-admin configuration section. Log in as a super admin user, navigate to 'Configuration' and select 'Stripe Connect' (probably somewhere down near the bottom). If your settings are valid, you should see a green box that says your Stripe Connect status is 'OK'.

If you see an error message instead, your configuration is not valid. You may not have restarted your server properly, or you may have entered invalid information into your `application.yml`.

### Step 8. Enable Stripe Connect
There is a feature toggle that enabled/disables Stripe across your whole OFN instance. Stripe is disabled by default, so you will need to enable it if you want to use it. You can do this from the same page used in Step 7. Simply check the box that says 'Enable shops to accept payments using Stripe Connect?' in the top section of the 'Stripe Connect' configuration interface. You can always disable Stripe again at any point in time, and Stripe will no longer be available as a payment method.

### Step 9. Connect an enterprise
You should now be able to navigate to the edit page for an enterprise and find a button to 'Connect with Stripe' under the 'Payment Methods' section. Clicking on the button will redirect you to Stripe where you can connect an existing Stripe account to the enterprise, or create a new one. IMPORTANT: do not use the platform's Stripe account here (even if you are only testing). This would entail connecting the platform to itself, and can result in unexpected behaviour. If you are just testing (ie. using a development `client_id` with `test` API keys), you should see a option to 'Skip this account form', which will allow you to complete the connection without actually requiring a Stripe account.

Once you have completed this process, you should be able to create a new Stripe-based payment method for the enterprise you just connected.

### Step 10. Add a Stripe payment method
Clicking the `CREATE NEW PAYMENT METHOD +` button will allow you to create a new payment method using Stripe. You will need to select 'Stripe' from the `Provider` dropdown, and then specify the enterprise you just connected from the `STRIPE ACCOUNT OWNER` dropdown. Once you have selected the stripe account owner, you should see a status message indicating whether the selected account is ready to use.

### Step 11. Place an order
Open an order cycle and place an order. If you are in testing mode, you will need to use the test card numbers [provided by Stripe](https://stripe.com/docs/testing#cards), real card numbers will not work.

That's it!
