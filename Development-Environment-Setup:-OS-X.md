This is a rough guide to getting a development environment set up on OSX. This guide has been tested over the years from Mavericks v10.9 up to Catalina v10.15.

## Step 1. XCode / Command Line Tools
XCode is Apple's suite of OS X and iOS development tools, and provides a whole IDE geared for that purpose. My understanding is that the only component of XCode that is particularly pertinent to rails development on OSX is the GCC compiler system. It used to be the case that one had to download and install the entire XCode suite in order to get the useful bits, but now Apple provides a condensed 'useful bits' package in the form of Command Line Tools (CLT).

First, check to see if gcc is available on your system already:

    $ gcc --version

If it is not, you should be prompted to install Command Line Tools.
If you are not prompted to install it, you can do this:

    $ xcode-select --install

Run through the prompts to download and install it.


When that process has finished, you should get a sensible answer out of:

    $ gcc --version

The output for this is should be something like:

    Configured with: --prefix=/Library/Developer/CommandLineTools/usr --with-gxx-include-dir=/usr/include/c++/4.2.1
    Apple LLVM version 8.1.0 (clang-802.0.42)
    Target: x86_64-apple-darwin16.6.0
    Thread model: posix
    InstalledDir: /Library/Developer/CommandLineTools/usr/bin

Now we can move on to installing the interesting things!

## Step 2. Homebrew
Homebrew is the 'missing package manager' for OS X. Some people don't like it because it encourages a lack of understanding about where and how packages are installed/configured, which can lead to difficulties in debugging if something goes wrong. That is a valid concern, but I tend to like it because it is quick and easy to use, and I generally have greater confidence in other people's ability to put together a Homebrew package in an intelligent way, than I do in my own ability to hack together a solution.

At the time of writing, the process to install Homebrew is listed on the [Homebrew Homepage](http://brew.sh/). If what I have below doesn't work, it may be worth checking back there to see if the procedure has changed. Anyway, this worked for me (note that you will be prompted for your password):

    $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

When that is done, run `brew doctor` to check the status of your homebrew install:

    $ brew doctor

If successful, you should get: `Your system is ready to brew`, YAY!

## Step 3. Git
Git is the version control system used to manage all the independent code contributions made by awesome people like you! So before installing anything with Homebrew you should update your package lists with:

    $ brew update

Now we can install Git with:

    $ brew install git

And check that everything is still ok with:

    $ brew doctor

If you see a warning like "/usr/bin occurs before /usr/local/bin" check the troubleshooting section on the bottom of this page.

When you check which version of git you are using:

    $ which git

You should see: `/usr/local/bin/git`

If that has all worked, you can now set up your git credentials with:

    $ git config --global user.name "Your Name Here"
    $ git config --global user.email "email@address.here"

## Step 4 (Optional). Disabling documentation installation

If you don't ever use docs for gems (I don't), you can disable installation of documentation with:

    $ echo "gem: --no-document" >> ~/.gemrc

## Step 5. RVM
Ruby Version Manager is a very useful tool which facilitates the management of ruby configuration on a per-project basis, allowing any number of setups using different versions of ruby, rails or gems to be used on the same machine relatively seamlessly.

To install RVM on Mavericks v10.9 see the [OSX v10.9 specific setup page](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Development-Environment-Setup%3A-OS-X-(Mavericks)).

For any other OSX version, the process to download and install RVM uses [gpg](https://en.wikipedia.org/wiki/GNU_Privacy_Guard) to verify the integrity of the RVM installer. It might be worth checking the [RVM homepage](https://rvm.io/) to ensure this is still the correct process to follow. You can install gpg using Homebrew:

    $ brew install gpg
    $ brew doctor

Download the public key for the RVM installer (again, probably worth checking the [RVM homepage](https://rvm.io/) for up to date instructions):

    $ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

In OSX El Capitan use this command instead:
    $ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3

If this step fails see the gpg troubleshooting section at the bottom of this page.

To install RVM and the latest version of ruby and rails, which is probably a good idea, use the following command. NOTE: RVM can globally install the latest versions of ruby and rails for you here, see the [RVM install docs](https://rvm.io/rvm/install) for more information. NOTE: the leading backslash is used to directly call the curl binary and ignore any alias for 'curl' you may have set up.

    $ \curl -sSL https://get.rvm.io | bash -s stable --autolibs=enable

When that is done you should be able to restart your terminal OR load the rvm binary into your shell with:

    $ source ~/.rvm/scripts/rvm

Then run:

    $ type rvm | head -1

Which should give you `rvm is a function` if everything went well.

## Step 6. Installing the required version of ruby
OFN will require a specific version of Ruby. The ruby version required is stored in a file in the root folder of the project repository called [.ruby-version](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/.ruby-version). You can check the version and install it into RVM like this:

    $ rvm install 2.3.7

## Step 7. Verify environment

Run each of the following just to check that you get sensible numbers out of each of them:

    $ git --version
    $ rvm -v
    $ ruby -v


To continue you can follow one of the version specific pages:- [Mavericks](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Development-Environment-Setup%3A-OS-X-%28Mavericks%29)
- [El Capitan](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Development-Environment-Setup%3A-OS-X-%28El-Capitan%29)
- [Mojave and later](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Development-Environment-Setup%3A-macOS-%28Sierra%2C-HighSierra%2C-Mojave-and-Catalina%29)


### Troubleshooting
#### Git
If you get an error (as I did) that says: `Warning: /usr/bin occurs before /usr/local/bin`, it means that you probably have installed the full XCode suite and the default version of Git that ships with XCode is stilling in /usr/bin, meaning our new version of Git that we are installing with Homebrew will not be used because as the error tells us: /usr/bin occurs before /usr/local/bin in the PATH. The following command will fix that by adding a line to ~/.bash_profile.

    $ echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile

Refresh your .bash_profile by entering:

    $ . ~/.bash_profile

#### gpg setup
If you get `gpg: keyserver receive failed: No keyserver available`, try a different server. eg:

    $ gpg --keyserver hkp://pgp.mit.edu --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

If you still see an error like `gpg: keyserver receive failed: No data`, wait, it may be due to server unavailability, and try again a few minutes later.

If instead you see a message like `gpg: keyserver receive failed: No route to host` then chances are that gpg decides to use IPv6 while it should use IPv4. You can check out https://github.com/rvm/rvm/issues/4215#issuecomment-435221616 for a solution.
