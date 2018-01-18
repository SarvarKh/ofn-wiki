## Intro
This guide will help you set up a development environment for OFN on Ubuntu (tested with 14.04 Trusty Tahr).

## Step 1. Install supporting packages

```bash
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev
sudo apt-get install git postgresql-9.3 postgresql-common libpq-dev phantomjs
```
**Jan 2018**: The new [set-up script](https://github.com/openfoodfoundation/openfoodnetwork#get-it-running) requires postgres-9.5.  See [Postgres Linux download](https://www.postgresql.org/download/linux/ubuntu/) for details of how to install that version.

## Step 2. Configure git
```bash
git config --global color.ui true
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR@EMAIL.com"
```

## Step 3. Install Ruby (using rbenv)
From https://gorails.com/setup/ubuntu/14.04

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

rbenv install 2.1.5
rbenv global 2.1.5
ruby -v
```
New linux package versions for OpenSSL can cause conflicts with the build process for older ruby versions. If the build fails whilst attempting to install ruby 2.1.5, try the following:

```bash
curl -fsSL https://gist.github.com/mislav/055441129184a1512bb5.txt | rbenv install --patch 2.1.5
```

## Step 4. Install gems
If you don't ever use docs for gems, you can disable installation of documentation with:

```bash
echo "gem: --no-document" >> ~/.gemrc
```

Now we can install some supporting gems:

```bash
gem install git-up
gem install bundler
gem install zeus
```

## Step 5. Set up Postgresql
**Jan 2018**: The new [set-up script](https://github.com/openfoodfoundation/openfoodnetwork#get-it-running) now performs this step for you.

Next we create a development user (with superuser privileges) for postgres. The password should be `f00d`, as referenced in `config/database.yml`.

```bash
sudo -u postgres createuser --superuser --pwprompt ofn
```
