This page describes how to configure the integration between OFN and Matomo.

### Basic Matomo Configuration
See https://matomo.org/docs/manage-websites/#add-a-website

### OFN Configuration
In your OFN instance, go to Admin > Configuration > Matomo Settings.
In this screen you will be able to integrate your instance with matomo.

You can also configure your OFN instance to display the matomo section of the cookies policy page, you can do this in the Legal Settings section of Admin > General Configuration.

If this and the integration with matomo are enabled, the cookies policy page will also display the Matomo optout checkbox that allows users to optout of being tracked.

### Privacy Matomo configuration
It is recommended you use your Matomo dashboard to:
- Anonymize user data
- Delete Old Visitors Logs
- Enable respect for the DoNotTrack preference

To do that, you can follow [this guide](https://matomo.org/docs/privacy/).

### More details
For more details about Matomo you can check the initial research done in [#2404](https://github.com/openfoodfoundation/openfoodnetwork/pull/2404).