:warning:  Warning: This page is under active development.

## Intro
This guide will help you set up a development environment for OFN on Fedora. 

## Install supporting packages

```bash
sudo dnf install -y git-core git curl zlib-devel openssl-libs openssl-devel readline-devel libyaml-devel sqlite-devel sqlite libxml2-devel  postgresql postgresql-server postgresql-contrib libpq-devel epel-release nodejs apg

sudo postgresql-setup --initdb --unit postgresql # if not already initialized
sudo systemctl start postgresql # launch PostgreSQL right now

# Create database user "ofn". The default password in the codebase is "f00d".
# If you set something different, update the file `config/database.yml` accordingly
# sudo -u postgres psql -c "CREATE USER ofn WITH SUPERUSER CREATEDB PASSWORD 'f00d'"
sudo -u postgres createuser --superuser --pwprompt ofn # as an alternative

script/setup

# sudo postgresql-setup --upgrade # make sure that postgres schema are still compatible with the last version installed
sudo systemctl enable postgresql # always launch postgres at boot
```


## Configure git
```bash
git config --global color.ui true
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR@EMAIL.com"
```

## Install Ruby (using rbenv)

Then you can follow the instructions from [Ruby on Rails — Fedora Developer Portal](https://developer.fedoraproject.org/start/sw/web-app/rails.html)

```bash
cd
git clone git://github.com/sstephenson/rbenv.git .rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash

rbenv install 2.3.7
rbenv global 2.3.7
ruby -v
```

## Install node (using nodenv)

```sh
git clone https://github.com/nodenv/nodenv ~/.nodenv --depth 1
(cd ~/.nodenv && src/configure && make -C src)
# For Bash urers:
echo 'export PATH="$HOME/.nodenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(nodenv init -)"' >> ~/.bashrc
# for Fish users:
echo 'set PATH "$HOME/.nodenv/bin" $PATH' >> ~/.config/fish/config.fish
echo 'nodenv init - | eval' >> ~/.config/fish/config.fish

exec $SHELL -l # reload the shell
git clone https://github.com/nodenv/node-build.git "$HOME/.nodenv/plugins/node-build" --depth 1
nodenv install 5.12.0
```

## Install gems
If you don't ever use docs for gems, you can disable installation of documentation with:

```bash
echo "gem: --no-document" >> ~/.gemrc
```

Now we can install some supporting gems:

```bash
gem install bundler
gem install zeus
```

## Setup database 
```bash
# cd $SOME_PATH/openfoodnetwork
rake db:create
rake db:setup
```

## Install Chrome (for Capybara/Selenium testing) if required
**Oct 2019**: Newer installations of Ubuntu might come with Chromium installed via snap. As a result you may need to install Google Chrome and the Chrome Driver. *If your tests run correctly without these steps, you may ignore them*.

If you encounter an error such as `Failure/Error: Capybara::Selenium::Driver .new(app, browser: :chrome, options: options) .tap { |driver| driver.browser.download_path = DownloadsHelper.path.to_s } NoMethodError: undefined method `strip' for nil:NilClass`, the following installation instructions should solve your issue:

### Install Google Chrome

TODO
- determine if Chromium could work equally well
- document consequently

### Install Chrome Driver

TODO
- depending on previous section TODO, document the equivalent for Chromium, or how to deal with Chrome Driver on Fedora

```bash
wget https://chromedriver.storage.googleapis.com/2.41/chromedriver_linux64.zip
unzip chromedriver_linux64.zip
```

## Related resources

- [Ruby on Rails — Fedora Developer Portal](https://developer.fedoraproject.org/start/sw/web-app/rails.html)
- [linux - Completely reset PostgreSQL to default? - Server Fault](https://serverfault.com/questions/574474/completely-reset-postgresql-to-default)
- [postgresql - pg_upgrade: "lc_collate values for database "postgres" do not match" - Stack Overflow](https://stackoverflow.com/questions/48612313/pg-upgrade-lc-collate-values-for-database-postgres-do-not-match)
- [Managing Node.js versions with anyenv & nodenv on macOS (for fish shell) | by Koji Mochizuki | TechNest | Medium](https://medium.com/technest/managing-node-js-versions-with-anyenv-nodenv-on-macos-for-fish-shell-df28b3b15539)
- [Colombia configs by Matt-Yorkley · Pull Request #670 · openfoodfoundation/ofn-install](https://github.com/openfoodfoundation/ofn-install/pull/670#discussion_r523120101)
- [Managing Node.js versions with anyenv & nodenv on macOS (for fish shell) | by Koji Mochizuki | TechNest | Medium](https://medium.com/technest/managing-node-js-versions-with-anyenv-nodenv-on-macos-for-fish-shell-df28b3b15539)