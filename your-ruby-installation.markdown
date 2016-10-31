# Your Ruby Installation

Before you can start using Ruby to develop software, you must first have Ruby installed in your development environment, i.e., on your Mac or in Cloud9. You should understand how your version of Ruby is provided to you, and which version of Ruby is supplied. For instance, on a Mac, it is possible to use the version of Ruby supplied by Apple as part of OS X or macOS. However, this will likely be an older version of Ruby lacking the most recently added features, and there may also be some usage restrictions that make it hard to use.

In this chapter, we'll talk about how to get Ruby on your system, and how to determine important information about it, especially when working with multiple Rubies.

## The System Ruby

### On a Mac

On Mac OS X and macOS, Ruby comes as part of the standard installation, and can be found at `/usr/bin/ruby`. However, as mentioned above, this is likely to be an old version of ruby. On the author's Mac, for instance:

```terminal
$ /usr/bin/ruby -v
```

reports that the version is 2.0.0p648; however, at the time this was written, the most recent production version of Ruby at [ruby-lang.org](https://www.ruby-lang.org/en/downloads/) is version 2.3.1.

Furthermore, there are certain characteristics of the Mac version of ruby that make it less than desireable from a development standpoint. For instance, it requires **root access** (a privileged level of that is only available to some users on a Mac) in order to install Gems and other tools.

So, all-in-all, the Mac version of ruby isn't optimal.

### On a Linux System

If you are using a Linux-based system, you may or may not have a system Ruby pre-installed. If you do, you will likely find it in `/usr/bin/ruby` just as on a Mac, and the same `/usr/bin/ruby -v` command can be used to show you the version number.

If you don't have ruby pre-installed, you can usually do so using your package manager, e.g., `rpm` or `yum` or something similar.

Regardless, you will into the same problem as on a Mac: the version of Ruby may be old, and you will likely need root access to install things.

### Cloud9

When you set up a Ruby workspace on Cloud9 environment, you will automatically have a recent version of Ruby installed for you. It may not be the most recent, but it will be sufficiently recent to be workable for most projects.

The Cloud9 version of Ruby will be set up so you don't require special privileges to use it. As we'll see later, it also uses the `rvm` ruby manager, which allows multiple versions of ruby to co-exist in one environment, and also permits management of ruby and its tools without root access.
