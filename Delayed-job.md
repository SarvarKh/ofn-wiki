[Delayed job](https://github.com/collectiveidea/delayed_job) is used for sending emails asynchronously and keeping the [products cache] up to date

* It is monitored and controlled by the delayed_job_openfoodnetwork systemd service.
* The service starts and stops delayed job via `script/delayed_job`.
* The service can be restarted with: `sudo systemctl restart delayed_job_openfoodnetwork.service` or you can check it's status with: `systemctl status delayed_job_openfoodnetwork.service`

Delayed job can be tested by running:

`Delayed::Job.enqueue ConfirmSignupJob.new Spree::User.find_by_email('admin@example.com')`

[products cache]: https://github.com/openfoodfoundation/openfoodnetwork/wiki/Products-cache
