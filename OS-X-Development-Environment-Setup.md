## Intro
This is a rough guide to getting a development environment set up on a clean OS X Mavericks install. I make no claim whatsoever that this is the best or only way to go about this process, but I thought that I would document the strategy that worked for me in case it of use to others. While I have made an attempt to make this guide as beginner friendly as possible, users should be very comfortable with the use of command line before attempting to follow these steps, as every step requires use of the command line.

## Step 1. XCode / Command Line Tools
XCode is Apple's suite of OS X and iOS development tools, and provides a whole IDE geared towards that purpose. My understanding is that the only component of XCode that is particularly pertinent to rails development on OS X is the GCC compiler system. It used to be the case that one had to download and install the entire XCode suite in order to get the useful bits, but now Apple provides a condensed 'useful bits' package in the form of Command Line Tools (CLT).

First, check to see if gcc is available:

`gcc --version`

If not, you'll have to install CLT. You can try to install a package which depends on CLT to prompt CLT to install. A example commonly used on the interwebs is xcode-select:

`xcode-select --install`

You should be prompted to install Command Line Tools. Run through the prompts to download and install it.

When that process has finished, you should get a sensible answer out of:

`gcc --version`

My output for this is something like:

`Configured with: --prefix=/Library/Developer/CommandLineTools/usr --with-gxx-include-dir=/usr/include/c++/4.2.1`
`Apple LLVM version 5.1 (clang-503.0.40) (based on LLVM 3.4svn)`
`Target: x86_64-apple-darwin13.3.0`
`Thread model: posix`

Now we can move on to installing the interesting things!

## Step 2. Homebrew
Homebrew is the 'missing package manager' for OS X. Some people don't like it because it encourages a lack of understanding about where and how packages are installed/configured, which can lead to difficulties in debugging if something goes wrong. I tend to like it because it is quick and easy to use, and I generally have more confidence in other people's ability to put together a Homebrew package in an intelligent way, than I do in my own ability to hack together a solution.

At the time of writing, the produce to install Homebrew on your system is listed on the [Homebrew Homepage](http://brew.sh/). If what I have below doesn't work, it may be worth checking back there to see if the procedure has changed. Anyway, this worked for me (note that you will be prompted for your password):

`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

If successful you should get it should tell you to run `brew doctor`, so do that:

`brew doctor`

When that is done you should get: `Your system is ready to brew`, YAY!

## Step 3. Git
To before installing anything with Homebrew you should update your package lists with:

`brew update`

Now we can install Git with:

`brew install git`

If you get an error (as I did) that says: `Warning: /usr/bin occurs before /usr/local/bin`, it means that you probably have installed the full XCode suite and the default version of Git that ships with XCode is stilling in /usr/bin, meaning out new version of Git that we are installing with Homebrew will not be used because as the error tells us: /usr/bin occurs before /usr/local/bin in the PATH. The following command will fix that by adding a line to ~/.bash_profile.

`echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile`

Refresh your bash profile by entering:

`. ~/.bash_profile`

So now when you check which version of git you are using:

`which git`

You should see: `/usr/local/bin/git`




