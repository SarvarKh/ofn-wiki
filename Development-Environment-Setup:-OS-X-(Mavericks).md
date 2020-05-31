You can follow the generic OSX setup guide [here](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Development-Environment-Setup%3A-OS-X). This page contains version specific setup details.

## Installing RVM in OSX v10.9 Mavericks

To install RVM and the latest version of ruby and rails, which is probably a good idea, use the following command. NOTE: if you REALLY don't want RVM to install anything expect RVM, remove the --rails flag.

    $ curl -L https://get.rvm.io | bash -s stable --rails --autolibs=enable

When that is done you should be able to restart your terminal and run:

    $ type rvm | head -1

Which should give you `rvm is a function` if everything went well.

## Manage postgres with Lunchy

You can manage postgres with [Lunchy](https://github.com/eddiezane/lunchy):

    $ gem install lunchy

When that is done, make sure you have a LaunchAgents folder:

    $ mkdir -p ~/Library/LaunchAgents

And then populate it with symbolic links back to the postgres config file(s) installed by homebrew:

    $ ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents

You should now be able to start the postgres server with:

    $ lunchy start postgres

Postgres can be stopped with: `$ lunchy stop postgres` but don't do this now.