## Intro
This guide will help you set up a development environment for OFN on Ubuntu (tested with 14.04 Trusty Tahr).

## Step 1. Install supporting packages

```bash
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev
sudo apt-get install git postgresql-9.3 postgresql-common libpq-dev phantomjs
```

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

rbenv install 1.9.3-p392
rbenv global 1.9.3-p392
ruby -v
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