
Flipper is a small tool with its own UI that we use to manage feature toggling and that can be configured at instance level. To know more about Flipper: https://github.com/jnunemaker/flipper

Flipper is our tool to manage feature toggling inside OFN application.

The purpose of this document is to present the tool, its functionalities and how to use it.

### Definitions
* **Feature**: A feature is a new behavior inside our application that is not totally released to all users (for many reasons to know more about the power of Feature Toggle concept and why it's powerful see: https://github.com/openfoodfoundation/openfoodnetwork/wiki/Feature-toggles). It is defined inside the OFN application. Creating feature inside Flipper UI can be seen as _link a feature that exists inside OFN application to Flipper UI in order to configure it_. View the list of features [here](#list-of-features-that-can-be-configured-within-the-flipper-user-interface).

* **Group**: A group is defined inside the OFN application: it is strictly linked to our code base. This can be seen as certain set of user that can share same characteristics. You can activate a feature to a group inside the Flipper UI. View the list of groups [here](#list-of-groups-that-can-be-assigned-to-a-feature).

* **Actor**: An actor is a User from a Flipper point of view. Activate a feature to an actor inside FlipperUI does activate this feature to the corresponding user inside the OFN application. 

### How to configure Flipper?

#### Create a feature inside Flipper UI
The UI is enabled for the admin users, and can be seen at this url: `/admin/feature-toggle`.
If this is the first time you access to this UI, no feature is present. You can then add a feature (see [List of Features](#list-of-features-that-can-be-configured-within-the-flipper-user-interface) in this document to know which features you can add) by clicking the 'Add Feature' blue button. Enter its name (NB: name must be strictly the same, case sensitive).

<div >
  <img src="https://user-images.githubusercontent.com/296452/114878834-ee4acc00-9e00-11eb-9b27-bca3c4369911.png">
</span>

Once you've created this feature inside the Flipper UI, it is linked from an already existing feature inside the OFN application (based on its name) and can be configured inside Flipper UI.

#### Configure toggling policies for a feature

A feature toggle can be enable at three level: actor, group and fully enabled. 

_NB_: this document doesn't address the configuration by percentage, which is possible with Flipper. 

##### Enable a feature for an actor
It is possible to enable a feature for one (or many) actor inside the Flipper UI. 

An actor is linked to a user by its `flipper id`: knowing the `id` of the user inside the database, the `flipper id` is therefor `Spree::User;[ID]` (for example `Spree::User;1`). To activate a feature to a specify user, simply add an actor by entering in the field its `flipper id`.

You can add as many actors as you want to activate them the feature.
<div>
  <img src="https://user-images.githubusercontent.com/296452/114878154-3ddcc800-9e00-11eb-86f9-900152025fbc.gif" />
</div>

##### Enable a feature for a group
You can know enable a feature to a group of users. Simply as hell: just select the group and add it.
<div>
<img src="https://user-images.githubusercontent.com/296452/114878164-3fa68b80-9e00-11eb-8950-3fd26028f49d.gif" />
<div>

##### Fully enable a feature
You can fully enable a feature by simply clicking the "Fully Enable" green button. It reset the previous configurations (actor, group) you've made.

##### Fully disable a feature
You can fully disable a feature by simply clicking the "Disable" green button. It reset the previous configurations (actor, group) you've made.


### List of features that can be configured within the Flipper user interface
As a feature is strongly linked to a concept that exists inside the codebase, notice that you must create/configure it inside the Flipper UI only feature that is actually existing inside the OFN application. 

At the moment, there is only on feature, which is: `unit_price`

### List of groups that can be assigned to a feature
* `admins`: all the users that are admin (ie. have access to the admin backoffice)




