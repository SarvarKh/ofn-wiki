We are using [Bugsnag](https://www.bugsnag.com/) to track bugs and errors on particular OFN instance.

## Create a Bugsnag OSS account

First, create a Bugsnag account for open source at https://www.bugsnag.com. They are proposing a Community plan for free.
Please follow there [guide](https://docs.bugsnag.com/product/getting-started/) to setup your app on their side.

## Integrate Bugsnag on your instance

You can follow their [guide for Rails integration] (https://docs.bugsnag.com/platforms/ruby/rails).

### Using ofn-install

In case you are deploying with [ofn-install](https://github.com/openfoodfoundation/ofn-install), you will just have to setup the `bugsnag_key` on your secrets.yml file defining the host vars for your specific instance.