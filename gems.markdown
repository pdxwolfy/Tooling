## Gems

As we learned earlier, a standard Ruby installation includes a large library of code that is always available to your programs, as well as another large library of code that can be required into your programs. But wait: there's more!

## What are Gems?

**RubyGems**, frequently just called **Gems**, are packages of code that can be downloaded, installed, and then used in your ruby programs or from the command line. The `gem` command is used to manage your Gem library, and is provided with all versions of Ruby since version 1.9.

There are thousands of Gems; you may already be familiar with some of them:

-   `rubocop`, a program that checks for Ruby style guide violations and other potential issues in your code.
-   `pry`, a program that assists in debugging Ruby programs
-   `sequel`, a set of classes and methods that simplifies access to databases
-   `rails`, a web application framework that is designed to make developing web applications easier.

The basics of Gems are pretty simple: you search the [RubyGems](https://rubygems.org/) website to find a Gem you want to install, and then run the `gem install` command to install the Gem on your system. Once installed, you can start using most Gems immediately, though you will usually have to read the documentation for that Gem first.

A complete discussion of Gems is beyond the scope of this lesson; we will instead focus on understanding how Gems interact with Ruby and your system. For more detailed information on how to install, create, and manage Gems, please see [the RubyGems documentation](http://guides.rubygems.org/). Much of this documentation deals with creating and updating existing Gems, which is probably more than you need initially. However, the [RubyGems Basics page](http://guides.rubygems.org/rubygems-basics/) provides a nice synopsis of the commands you will need most often. And, there's also a nice [command reference](http://guides.rubygems.org/command-reference/) for the `gem` command.

## Gems, Ruby, and your Computer

When you run the `gem` command to install a Gem, what exactly happens? Where are these Gems found? Where are they stored on your system? How does Ruby find them? Can you install multiple versions of a Gem? If so, how do you use a specific version? Let's take a look at these questions.

### The Remote Gem Library

Since your Ruby installation already includes the `gem` program (provided you're using version 1.9 or higher of Ruby), the first steps in working with Gems are to find some Gems you want to install, and then install them. Where are these Gems, though? How do you find them?

When you first start out with Ruby and Gems, you will usually be told to install certain Gems. For instance, Launch School will tell you early on to install the `rubocop` and `pry` gems as both of these tools are useful to new (and experienced) Ruby developers. Later on, you may learn about other Gems from other people, particularly after searching for tips on how to deal with a particular Ruby problem. Eventually, though, you will need to search through a remote library of Gems -- the one you'll use most often is the main [RubyGems library](https://rubygems.org/gems). You may also need to search a specialized remote library run by, say, your employer or school.

{{#info}}

Gem descriptions at the RubyGems library are often extremely minimal. You will find some additional links on the right hand side of each Gem's main page; in particular, the "Homepage" and "Documentation" pages will usually contain more detailed information. In some cases, you may actually have to look at the source code to figure out how to use a Gem. (We'll discuss how Gem directories are laid out later on.)

{{/info}}

Once you've found a Gem that you want to install, you run `gem install [gem_name]` to install the Gem. The `gem` command will then connect to the remote library, download the appropriate Gems, and install them. If you specify additional remote libraries, `gem` will also connect to those libraries in an effort to find the Gems you want.

### The Local Gem Library

When the `gem` command installs a Gem, it places all of the files related to the Gem in a location where Ruby can find the appropriate `require` files and where your system can find any commands meant to be run from a terminal or graphical interface. This location is the local library, which is stored on your local filesystem.

Where, precisely, the local library is stored depends on a number of factors, including whether you are using a system-provided version of Ruby that requires root access, a user maintainable version of Ruby, the version of Ruby, and whether you are using a ruby version manager. Fortunately, the `gem` command usually knows where to put things, and things just work.

{{#info}}

Ruby version managers, of which `rbenv` and `rvm` are the most commonly used, are used to manage multiple versions of Ruby on a system. We will discuss these in detail later on; for now, just be aware that the location of your local Gem library depends on which version manager you use, assuming that you use one.

{{/info}}

### Gems and Your File System

From time to time, it's important to know where the `gem` command puts things on your system. Sometimes, you need to look at some source code to better understand a particular Gem; other times, something may not be working properly, and you will need to diagnose the problem.

So, where do you look? The easiest way to find out is to run:

```terminal numbers=off
$ gem env
```

This will print a longish list of information about your RubyGems installation. Here is some of the more pertinent information produced by this command on the author's system:

```terminal numbers=off
RubyGems Environment:
  - RUBYGEMS VERSION: 2.6.7
  - RUBY VERSION: 2.3.1 (2016-04-26 patchlevel 112) [x86_64-darwin15]
  - INSTALLATION DIRECTORY: /usr/local/rbenv/versions/2.3.1/lib/ruby/gems/2.3.0
  - USER INSTALLATION DIRECTORY: /Users/wolfy/.gem/ruby/2.3.0
  - RUBY EXECUTABLE: /usr/local/rbenv/versions/2.3.1/bin/ruby
  - EXECUTABLE DIRECTORY: /usr/local/rbenv/versions/2.3.1/bin
    ...
  - REMOTE SOURCES:
     - https://rubygems.org/
  - SHELL PATH:
     - /usr/local/rbenv/versions/2.3.1/bin
     - /usr/local/Cellar/rbenv/1.0.0/libexec
     - /usr/local/rbenv/shims
     ...
```

This particular output is on a Mac OS X system running ruby 2.3.1 from Homebrew using the rbenv version manager. Your configuration may show different results.

Let's look at some of these settings:

-   **RUBY VERSION** This is the version number of the Ruby associated with `gem`. The `gem` command is always tied to a specific version of Ruby; if you have multiple versions of Ruby installed, then each version of Ruby will have its own independent version of the `gem` command and its own independent local library. The RUBY VERSION can be a helpful diagnostic: if you're getting unexpected results, you may not be using the right copy of `gem` or `ruby` for your situation.

-   **RUBY EXECUTABLE** This is the location of the ruby command that you should use with the Gems managed by this `gem` command. This information is useful when RUBY VERSION reveals a `gem`/`ruby` mismatch.

-   **INSTALLATION DIRECTORY** This directory is the location where new Gems are installed by default. Note that the location may vary based on the ruby version number (`2.3.1` in this case). The `2.3.0` reference refers to a major version; it can mostly be ignored.

    Inside the INSTALLATION DIRECTORY, there is a subdirectory named `gems`; this is where all of the files associated with each Gem are located. The files are in subdirectories named with the name and version number of the installed Gems. For instance, pry version `0.10.4` is in `{INSTALLATION DIRECTORY}/gems/pry-0.10.4`.

      Inside each Gem-specific directory you will find one or more additional subdirectories and some files. Here's a typical directory for a Gem named `colorize`:

    ```terminal numbers=off
    $ cd /usr/local/rbenv/versions/2.3.1/lib/ruby/gems/2.3.0/gems
    $ tree colorize-0.8.1/
    colorize-0.8.1/
    ├── CHANGELOG
    ├── LICENSE
    ├── README.md
    ├── Rakefile
    ├── colorize.gemspec
    ├── lib
    │   ├── colorize
    │   │   ├── class_methods.rb
    │   │   └── instance_methods.rb
    │   ├── colorize.rb
    │   └── colorized_string.rb
    └── test
        └── test_colorize.rb
    ```

    The `lib` subdirectory is the most important: this is where your Ruby will find the `require` files for that package. For instance, when you write `require 'shotgun'` in a program, Ruby will load `colorize.rb` from the `lib` subdirectory.

    Don't be afraid to look inside these files: as long as you don't modify them, you're free to read the programs to see how they work, learn more about how to use them, and to diagnose problems that you are having with the Gem.

{{#info}}

The remainder of this section isn't critical to your learning path. However, if you ever do need to diagnose a Gem-related issue, you may need to return to this section for additional guidance.

{{/info}}

-   **USER INSTALLATION DIRECTORY** Depending on your installation of Ruby and the options you pass to `gem`, your Gem files may be installed somewhere in your home directory instead of one of the system level directories. In such cases, the Gems will be installed in this directory.

    The structure of the USER INSTALLATION DIRECTORY is identical to that used by the INSTALLATION DIRECTORY.

-   **EXECUTABLE INSTALLATION DIRECTORY** Some Gems provide commands that can be used directly from a terminal prompt; copies of those commands are placed in this directory. Note that you need to include this directory in your shell `PATH` variable so that the shell can find these commands. Usually, you don't need to worry about setting this yourself, but it's useful to know where to look if you have problems. (See also SHELL PATH below.)

-   **REMOTE SOURCES** This is the remote library used by this `gem` installation.

-   **SHELL PATH** A list of the directories in your shell `PATH` variable. If you are having problems with `command not found` messages or running the wrong versions of programs, this variable may provide valuable clues as to where the system is looking for your programs.

    If you don't remember what the `PATH` is or how to set it, please take some time to review the chapter on the environment in our [Introduction to the Command Line](https://launchschool.com/books/command_line/read/environment) book.

## Requiring Gems

Gems are used by Ruby programs in the same way that any of the standard libraries are used: you `require` them in your program:

```ruby numbers=off
require 'colorize'
```

In this case, we are requiring the Gem named `colorize`. This causes Ruby to search through a list of directories looking for the file named `colorize.rb`. By default, the list of directories includes every subdirectory of both `{USER INSTALLATION DIRECTORY}/gems` and `{INSTALLATION DIRECTORY}/gems`; the first occurrence of `colorize.rb` located by this search will be loaded into the current program, and be made available for use.

You don't often need to worry about this search order; the `gem install` command will make sure your Gems are in the right locations. However, you may sometimes run into odd problems that can only be explained by the wrong Gem being required into a program. For example, suppose your program needs the `disable_colorization` feature of the `colorize` Gem (this feature was introduced in `colorize` version 0.7.5), but when you try to run the program, you get an error that says that feature doesn't exist. How can you find out which version of the Gem you're really loading?

To determine the Gem version, you need to find the full path name of the file that was required into your program. The easiest way to do that is to insert some debugging code (or a call to `binding.pry`) into your program shortly after the point where you require the Gem:

```ruby numbers=off
require 'colorize'
 ...
puts $LOADED_FEATURES.grep(/\bcolorize\.rb/)
```

This will display something like this (the value displayed may be different):

```terminal numbers=off
/usr/local/rbenv/versions/2.2.4/lib/ruby/gems/2.2.0/gems/colorize-0.7.4/lib/colorize.rb
```

This command searches for any entries in the `$LOADED_FEATURES` Array that include `colorize.rb` in the name, and displays all entries that match. With luck, you only have one entry, though sometimes you may have several to choose from. It's usually pretty easy to choose the right entry though. The resulting name is the name of the file that was required into the program; by looking at that name, you can see that the file was loaded from `colorize-0.7.4`, which tells you that version 0.7.4 of the Gem was loaded, not version 0.7.5 (or higher) as needed.

If you do determine that you're using the wrong version of the Gem, you will probably need to:

-   install the desired version

-   use a more recent version of Ruby

-   you have an issue with the Bundler program (which we'll discuss later)

## Multiple Gem Versions

In the previous section, we described an issue where we were using the wrong version of a Gem. One of the possible solutions, once we determined that we were using the wrong version, was to install the correct version. However, this introduces another opportunity for version-related problems with Gems.

New versions of Gems aren't always backward compatible with older versions; that is, features are sometimes removed or modified in such a way that something that works in an old version of the Gem doesn't work in the new version. For instance, suppose you have `colorize` version 1.2.3, and it isn't backwards compatible with version 0.7.5. Suppose further that you have a ruby program that requires a feature of version 0.7.5 that has been removed from version 1.2.3. What happens when this program tries to `require 'colorize'`?

In this case, Ruby will require the most recent of the Gems by the required name. This means trouble for your hypothetical program; it needs the older version. This can be addressed in several ways:

-   by specifying an absolute path name to `require`

-   adding an appropriate `-I` option to the ruby invocation

-   by modifying `$LOAD_PATH` prior to requiring `colorize`

-   by modifiying the `RUBYLIB` environment variable

These fixes are all hacks though; they will quickly become unmanageable and an enormous mess and source of bugs. You definitely do not want to go down this road except in the extremely short term. The right choice is to use Bundler, as described later.

## Summary

RubyGems (Gems) are packages of code desired for use with Ruby. Most Gems provide useful modules and classes designed to simplify working with certain types of data; others provide programs intended for use with Ruby code, but that run independently of your code. Some even provide both types of functionality.

The `gem install` command installs Gems on your system. It fetches the Gem code from a remote library, and stores it in a local library on your local file system. The exact location of that local library varies based on a number of factors. You can use the `gem env` command to help determine where the Gems for the current Ruby are located: this information is shown as either the USER INSTALLATION DIRECTORY or the INSTALLATION DIRECTORY; the actual files used by Ruby will be mostly installed in the `gems` subdirectory of one of these two configration directories.

Ruby programs often need a particular version of Ruby, and a particular version of the Gems it uses. This often leads to situations in which your system has multiple versions of Ruby installed, and each version of Ruby has its own set of Gems. There are Ruby Version Managers that aid in maintaining multiple versions of Ruby and its Gems, and a program named Bundler that allows you to configure exactly which versions of Ruby and the Gems should be used. In the next lesson, we will discuss the Ruby Version Managers, namely `rbenv` and `rvm`. We'll discuss Bundler in the lesson after that.
