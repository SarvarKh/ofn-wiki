## Intro
This guide will help you set up a development environment for OFN on Ubuntu (tested with Ubuntu 18, 19 and 20).

## Step 1. Install supporting packages

```bash
# In all supported versions
sudo apt-get install 
  build-essential \
  curl \
  git \
  git-core \
  libcurl4-openssl-dev \
  libffi-dev \
  libpq-dev \
  libreadline-dev \
  libsqlite3-dev \
  libssl-dev \
  libxml2-dev \
  libxslt1-dev \
  libyaml-dev \
  nodejs \
  postgresql-common \
  sqlite3 \
  yarn \
  zlib1g-dev

ubuntu_version=$(lsb_release -a 2> /dev/null | grep Release | cut -f2 | cut -d. -f1)

if [ $ubuntu_version -ge 20 ] ; then
  sudo sh -c "
  add-apt-repository 'deb http://apt.postgresql.org/pub/repos/apt/ focal-pgdg main'
  apt-get update
  wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
  "
fi

if [ $ubuntu_version -ge 18 ] ; then
  sudo apt install postgresql-10 postgresql-client-10 software-properties-common
else 
  sudo apt install postgresql-9.5 postgresql-client-9.5 python-software-properties 
fi

sudo systemctl start postgresql.service # start right now
sudo systemctl enable postgresql.service # start at every boot
```

## Step 2. Configure git
```bash
git config --global color.ui true
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR@EMAIL.com"
# That might be set that later at a repository level, without the --global flag, after entering its cloned directory
# or take that as option for all projects by running the following command as is.
# git config --global pull.rebase false
```

## Step 3. Install Ruby (using rbenv)

### Install libssl1.0-dev

> This step might no longer be required, as last version of the OFN platform use Ruby 2.5.8, and this specific dependency was pertaining to   Ruby 2.4. Related discussions:
> - https://openfoodnetwork.slack.com/archives/C2GQ45KNU/p1620201776106200
> - https://github.com/rvm/rvm/issues/3862
>
> Still if during the install process some compilation step complains about SSL library, that might be the relevant step to follow.

#### Ubuntu 18
**If you are on Ubuntu 18**, you'll need to install the `libssl1.0-dev` package first with:
```
sudo apt install libssl1.0-dev
```
#### Ubuntu 19 or 20

You'll need to update your apt sources in order to install `libssl1.0-dev` and add the **bionic-security** source below. You can remove it from your sources again after installing `libssl1.0-dev`.

```bash
sudo sh -c "
  add-apt-repository 'http://security.ubuntu.com/ubuntu bionic-security main'
  apt update
  # some additional action might be required here regarding missing repository certificate key…
  apt-cache policy libssl1.0-dev
  apt install libssl1.0-dev
"
```

### Installing Ruby itself
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

## Step 4. Installing node 

### Ubuntu Package

Previous steps already took care of that if needed, but this can be specifically treated though 
```sh
# make sure that there is some node available or install it through apt
which node >/dev/null || sudo apt install nodejs
```

### Using Nodenv

[Nodenv](https://github.com/nodenv/nodenv) allows to install miscellaneous versions of Node on the same system and easily switch between version to match dependencies of different projects on the same system. Note that it is optional to use it, as long as the system provides a compatible Node version.

> Beware that Ubuntu also allows you to `apt install nodeenv` (*nod**ee**nv* with two *e*), which is yet an other environment versioning tool.
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
gem install bundler:1.17.3
```

## Step 6. Install Chrome (for Capybara/Selenium testing) if required
**Oct 2019**: Newer installations of Ubuntu might come with Chromium installed via snap. As a result you may need to install Google Chrome and the Chrome Driver. *If your tests run correctly without these steps, you may ignore them*.

If you encounter an error such as `Failure/Error: Capybara::Selenium::Driver .new(app, browser: :chrome, options: options) .tap { |driver| driver.browser.download_path = DownloadsHelper.path.to_s } NoMethodError: undefined method `strip' for nil:NilClass`, follow the next section.

Note that once bundler installed, it should now be possible to run `bundle install && yarn install` to install all gems and Javascipt dependencies in the directory of cloned repository.

## Install a compatible browser to run tests

### Install Chromium

Chromium is the FLOSS alternative of the non-FLOSS Chrome browser.
```sh
sudo apt install -y chromium-chromedriver 
```

### Install Google Chrome

```bash
sudo sh -c"
  curl -sS -o - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add
  add-apt-repository "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main"
  apt update -y
  apt install -y google-chrome-stable
"
```

### Install Chrome Driver

```bash
# Consider running these command in $XDG_DATA_HOME/bin which by default is ~/.local/bin
wget https://chromedriver.storage.googleapis.com/90.0.4430.24/chromedriver_linux64.zip
 && unzip chromedriver_linux64.zip
 && rm chromedriver_linux64.zip
```

# Related resources

- https://github.com/openfoodfoundation/openfoodnetwork/issues/7521