## Intro
This guide will help you set up a development environment for OFN on Ubuntu (tested with Ubuntu 18, 19 and 20).

## Step 1. Install supporting packages

```bash
sudo dnf install -y git-core git curl zlib-devel openssl-libs openssl-devel readline-devel libyaml-devel sqlite-devel sqlite libxml2-devel  postgresql libpq-devel epel-release nodejs
```


## Step 2. Configure git
```bash
git config --global color.ui true
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR@EMAIL.com"
```

## Step 3. Install Ruby (using rbenv)

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