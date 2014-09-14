## Intro
This is a rough guide to getting a development environment set up in OS X Mavericks. I make no claim whatsoever that this is the best or only way to go about this process, but I thought that I would document the strategy that worked for me in case it of use to others. While I have made an attempt to make this guide as beginner friendly as possible, users should be very comfortable with the use of command line before attempting to follow these steps.

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

`Configured with: --prefix=/Library/Developer/CommandLineTools/usr --with-gxx-include-dir=/usr/include/c++/4.2.1
Apple LLVM version 5.1 (clang-503.0.40) (based on LLVM 3.4svn)
Target: x86_64-apple-darwin13.3.0
Thread model: posix
`


