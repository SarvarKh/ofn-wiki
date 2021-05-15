[Delayed job](https://github.com/collectiveidea/delayed_job) is a background job processing queue used for sending emails asynchronously (amongst other things).

* It is monitored and controlled by the delayed_job_openfoodnetwork systemd service.
* The service starts and stops delayed job via `script/delayed_job`.
* The service can be restarted with: `sudo systemctl restart delayed_job_openfoodnetwork.service` or you can check it's status with: `systemctl status delayed_job_openfoodnetwork.service`

Delayed job can be tested by running:

`Delayed::Job.enqueue ConfirmSignupJob.new Spree::User.find_by_email('admin@example.com')`
