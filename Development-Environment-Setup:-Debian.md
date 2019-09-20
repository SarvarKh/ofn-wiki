## Debian 10 Buster

At the time of writing, the OFN runs on Ruby v2.1.9 which is very old depends on an old version of openssl. That old version of openssl is not available on newer Debian and Ubuntu versions. Here is how you get around it:

```sh
# Install an old version of libssl. You may need to remove your current libssl first.
sudo apt-add-repository "deb http://security.ubuntu.com/ubuntu bionic-security main"
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3B4FE6ACC0B21F32
sudo apt update
sudo apt install libssl1.0-dev

# Install Ruby.
git clone https://github.com/rbenv/rbenv.git ~/.rbenv --depth=1
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build --depth 1
(cd ~/.rbenv && src/configure && make -C src)
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
. ~/.bashrc
rbenv install 2.1.9

# Install gems.
cd openfoodnetwork
./script/install-bundler
bundle
```