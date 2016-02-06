[Delayed job](https://github.com/collectiveidea/delayed_job) is used for sending emails asynchronously.

* It is monitored by the [monit](https://mmonit.com/) daemon.
* The [monit configuration](https://github.com/openfoodfoundation/ofn_deployment/blob/master/roles/common/templates/monit.j2) is setup by `ofn_deployment`.
* It starts delayed job by running `script/delayed_job`.

Delayed job can be tested by running:

`Delayed::Job.enqueue ConfirmSignupJob.new Spree::User.find_by_email('admin@example.com')`

