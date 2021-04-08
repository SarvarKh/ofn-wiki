## Intro
This guide will help you set up a development environment for OFN on Ubuntu (tested with Ubuntu 18, 19 and 20).

## Step 1. Install supporting packages

```bash
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev
sudo apt-get install git postgresql-9.5 postgresql-common libpq-dev
```

In Ubuntu 18, you will need to replace `python-software-properties` with `software-properties-common`.
Also in Ubunut 18 and above, you can use psql 10 with `sudo apt-get install git postgresql-10`

In Ubuntu 20.04, to install postgreSQL-10 you need to add the repository/key before installing postgresql-10:
```bash
sudo add-apt-repository 'deb http://apt.postgresql.org/pub/repos/apt/ focal-pgdg main'
sudo apt-get update
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get install postgresql-10
```

## Step 2. Configure git
```bash
git config --global color.ui true
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR@EMAIL.com"
```

## Step 3. Install Ruby (using rbenv)

**If you are on Ubuntu 18**, you'll need to install the `libssl1.0-dev` package first with:
```
sudo apt install libssl1.0-dev
```
**If you are on Ubuntu 19 or 20**, you'll need to update your apt sources in order to install `libssl1.0-dev` and add the **bionic-security** source below. You can remove it from your sources again after installing `libssl1.0-dev`.
1. Open `/etc/apt/sources.list` in a text editor of your choice.
2. Append `deb http://security.ubuntu.com/ubuntu bionic-security main` to the end of the file
3. Run `sudo apt update && apt-cache policy libssl1.0-dev`
4. Run `sudo apt install libssl1.0-dev`


Then you can follow the instructions from https://gorails.com/setup/ubuntu/14.04

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

rbenv install 2.5.8
rbenv global 2.5.8
ruby -v
```

## Step 4. Install node (using nodenv)

```sh
git clone https://github.com/nodenv/nodenv ~/.nodenv --depth 1
(cd ~/.nodenv && src/configure && make -C src)
echo 'export PATH="$HOME/.nodenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(nodenv init -)"' >> ~/.bashrc
export PATH="$HOME/.nodenv/bin:$PATH"
eval "$(nodenv init -)"
git clone https://github.com/nodenv/node-build.git "$(nodenv root)/plugins/node-build" --depth 1
nodenv install 5.12.0
```

## Step 5. Install gems
If you don't ever use docs for gems, you can disable installation of documentation with:

```bash
echo "gem: --no-document" >> ~/.gemrc
```

Now we can install some supporting gems:

```bash
gem install bundler
gem install zeus
```
## Step 6. Install Chrome (for Capybara/Selenium testing) if required
**Oct 2019**: Newer installations of Ubuntu might come with Chromium installed via snap. As a result you may need to install Google Chrome and the Chrome Driver. *If your tests run correctly without these steps, you may ignore them*.

If you encounter an error such as `Failure/Error: Capybara::Selenium::Driver .new(app, browser: :chrome, options: options) .tap { |driver| driver.browser.download_path = DownloadsHelper.path.to_s } NoMethodError: undefined method `strip' for nil:NilClass`, the following installation instructions should solve your issue:

### Install Google Chrome

```bash
curl -sS -o - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add
sudo echo "deb [arch=amd64]  http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list
sudo apt-get -y update
sudo apt-get -y install google-chrome-stable
```

In Ubuntu 20.04, you may need to run the 2nd command above like this instead:
```bash
sudo echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee -a /etc/apt/sources.list.d/google-chrome.list
```

### Install Chrome Driver

```bash
wget https://chromedriver.storage.googleapis.com/2.41/chromedriver_linux64.zip
unzip chromedriver_linux64.zip
```