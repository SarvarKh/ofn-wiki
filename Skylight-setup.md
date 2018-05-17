What follows is a brief documenting on how to setup [Skylight](skylight.io) to monitor a particular OFN instance's performance.

## Create a Skylight OSS account

First, create an Skylight account for open source at
https://www.skylight.io/oss. In the signup form mention that we already have an
Open Food Network account for Barcelona and that we need one for your instance.

## Provision SKYLIGHT_AUTHENTICATE var

All configuration required in the app has already been introduced in https://github.com/openfoodfoundation/openfoodnetwork/pull/2070 and https://github.com/openfoodfoundation/openfoodnetwork/pull/2080. Furthermore, [ofn-install](https://github.com/openfoodfoundation/ofn-install) has also support for it, added in https://github.com/openfoodfoundation/ofn-install/pull/137 and https://github.com/openfoodfoundation/ofn-install/pull/153.

### Using ofn-install
All you need to do is to provide the `skylight_authentication` var as part of your secrets file. Take a look at [inventory/host_vars/_example.com/secrets.example.yml](https://github.com/openfoodfoundation/ofn-install/blob/f5213473a628769141184716481e59c93914698d/inventory/host_vars/_example.com/secrets.example.yml#L25).

Then, you can provision your instance's server running

```
$ ansible-playbook playbooks/provision.yml --limit=staging -e
<path_to_your_secrets_yml> --ask-vault-pass
```

You can check out the details about the provision process in
https://github.com/openfoodfoundation/ofn-install/wiki/Provisioning.

## Reboot the app server

Once the env var is in place, which you can check running `echo $SKYLIGHT_AUTHENTICATION`, restart the unicorn so that it reads this new variable as follows:

```
sudo systemctl restart unicorn_openfoodnetwork.service
```

Check the Rails' log file and you should see something like:

```
$ tail -f log/staging.log
...
[SKYLIGHT] [1.6.1] Skylight agent enabled
...
```

Skylight also has its own log file in `log/skylight.log`, which should only have
a line like:

```
# Logfile created on 2018-05-17 11:37:42 +0000 by logger.rb/44203
```

This one is your go-to file when debugging anything related to Skylight. All
messages are logged there.

## Troubleshooting

All this steps have proved working in Katuma but if you happen to experience any
problem make sure you run the latest version of OFN and that the
`SKYLIGHT_AUTHENTICATION` and `RAILS_ENV` env vars are present and accessible by
the unicorn process.

Keep in mind that a Unicorn reload is not enough for the process to pick up new
env vars so a full restart is mandatory.
