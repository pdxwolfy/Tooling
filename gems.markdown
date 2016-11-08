# Gems

As we learned earlier, a standard Ruby installation includes a large library of code that is always available to your programs, as well as another large library of code that you can require into your programs. But wait: there's more!

## What are Gems?

**RubyGems**, frequently just called **Gems**, are packages of code that can be downloaded, installed, and then used in your ruby programs or from the command line. The `gem` command helps you manage your Gem library; all versions of Ruby since version 1.9 install the command automatically.

There are thousands of Gems; you may already be familiar with some of them:

-   `rubocop`, a program that checks for Ruby style guide violations and other potential issues in your code.
-   `pry`, a program that provides debugging facilities for Ruby programs
-   `sequel`, a set of classes and methods that simplify access to databases
-   `rails`, a web application framework that makes developing web applications easier.

The basics of Gems are pretty simple: you search the [RubyGems](https://rubygems.org/) website to find a Gem you want to install, and then run the `gem install` command to install the Gem on your system. Once installed, you can start using most Gems immediately, though you will usually have to read the documentation for that Gem first.

A complete discussion of Gems is beyond the scope of this lesson; we instead focus on understanding how Gems interact with Ruby and your system. For more detailed information on how to install, create, and manage Gems, please see [the RubyGems documentation](http://guides.rubygems.org/). Much of this documentation deals with creating and updating existing Gems, which is probably more than you need initially. However, the [RubyGems Basics page](http://guides.rubygems.org/rubygems-basics/) provides a useful synopsis of the most useful commands. There's also a useful [command reference](http://guides.rubygems.org/command-reference/) for the `gem` command.

## Gems, Ruby, and your Computer

When you run the `gem` command to install a Gem, what exactly happens? Where are these Gems found? Where are they stored on your system? How does Ruby find them? Can you install multiple versions of a Gem? If so, how do you use a specific version? Let's examine these questions.

### The Remote Gem Library

Since your Ruby installation already includes the `gem` program (provided you're using version 1.9 or higher of Ruby), you don't have to do anything special to use it. All you need to do is find some Gems you want to install, and then install them. Where are these Gems, though? How do you find them?

When you begin using Ruby and Gems, someone will probably tell you what Gems you need to install. For instance, Launch School recommends that you install the `rubocop` and `pry` gems; both of these tools are useful to new (and experienced) Ruby developers. Later, you may learn about other Gems from other people, or just learn about one from a a Google search. Eventually, though, you will need to search through a remote library of Gems -- the one you'll use most often is the main [RubyGems library](https://rubygems.org/gems). You may also need to search a specialized remote library run by, say, your employer or school.

{{#info}}

Gem descriptions in the RubyGems library are often extremely minimal. You will find some additional links on the right hand side of each Gem's main page; specifically, the "Homepage" and "Documentation" pages often contain more detailed information. Sometimes, though, you may actually have to read the source code to learn how to use a Gem.

{{/info}}

Once you find a Gem that you want to install, you run `gem install [gem_name]` to install it. This command connects to the remote library, downloads the appropriate code, and installs it. If you specify additional remote libraries, `gem` also searches those libraries to find the Gems you want.

### The Local Gem Library

When the `gem` command installs a Gem, it places the files provided by the Gem where Ruby can find them and your system can find any commands provided by the Gem. This location is the local library; it is stored on your local file system.

Several factors influence where the local library resides on your system, including whether you are using a system-provided version of Ruby that needs root access, a user maintainable version of Ruby, the version of Ruby, and whether you are using a ruby version manager. Fortunately, the `gem` command usually knows where to put things, and things just work.

{{#info}}

Ruby version managers, of which rbenv and RVM are the most commonly used, manage multiple versions of Ruby on a system. We discuss these in detail later on; for now, just be aware that the location of your local Gem library depends on the version manager that you use.

{{/info}}

### Gems and Your File System

It's sometimes important to know where the `gem` command puts things on your system. You may need to look at some source code to better understand a particular Gem, or perhaps something just isn't working correctly and you need to diagnose the problem. So, where do you look? The easiest way to find out is to run:

```terminal numbers=off
$ gem env
```

This prints a longish list of information about your RubyGems installation. Here is some of the more pertinent information produced by this command on the author's system:

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

This output is on a Mac OS X system running ruby 2.3.1 from Homebrew using the rbenv version manager. Your configuration may show different results.

Let's examine some of these settings:

-   **RUBY VERSION** This is the version number of the Ruby associated with `gem`. Each version of Ruby has its own copy of the `gem` command; if you have multiple versions of Ruby installed, then each `gem` command manages the Gems for its specific version of Ruby. Each `gem` command installs its Gems in a separate library. It's important to always use the correct `gem` command for the version of Ruby you use. The RUBY VERSION can be a helpful diagnostic: if you're getting unexpected results, you may not be using the right copy of `gem` or `ruby` for your situation.

-   **RUBY EXECUTABLE** This is the location of the ruby command that you should use with the Gems managed by this `gem` command. This information is useful when RUBY VERSION shows a `gem`/`ruby` mismatch.

-   **INSTALLATION DIRECTORY** This directory is the location where `gem` installs new Gems by default. Note that the location may vary based on the ruby version number (`2.3.1` in this case). The `2.3.0` reference refers to a major version; you can usually ignore this, but remember that it is part of the directory name.

    Inside the INSTALLATION DIRECTORY, there is a subdirectory named `gems`; this is where `gem` installs most of the files associated with each Gem. The files are in subdirectories named with the name and version number of the installed Gems. For instance, pry version `0.10.4` is in `{INSTALLATION DIRECTORY}/gems/pry-0.10.4`.

      Inside each Gem-specific directory, you will find additional subdirectories and files. Here's a typical directory for a Gem named `colorize`:

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

    The `lib` subdirectory is the most important: this is where your ruby finds the `require` files for that package. For instance, when you write `require 'colorize'` in a program, Ruby loads `colorize.rb` from the `lib` subdirectory.

    Don't be afraid to look inside these files: as long as you don't change them, you're free to read the programs to see how they work, learn more about how to use them, and to diagnose problems that you are having with the Gem.

{{#info}}

The remainder of this section isn't critical to your learning path. However, if you ever need to diagnose a Gem-related issue, you may return to this section for additional guidance.

{{/info}}

-   **USER INSTALLATION DIRECTORY** Depending on your installation of Ruby and the options you pass to `gem`, `gem` may install your Gem files somewhere in your home directory instead of one of the system level directories. When this happens, `gem` installs the Gems in this directory.

    The structure of the USER INSTALLATION DIRECTORY is the same as that used by the INSTALLATION DIRECTORY.

-   **EXECUTABLE INSTALLATION DIRECTORY** Some Gems provide commands that you can use directly from a terminal prompt; `gem` places those commands in this directory. Note that you need to include this directory in your shell `PATH` variable so that the shell can find these commands. Usually, you don't need to worry about setting this yourself, but it's useful to know where to look if you have problems. (See also SHELL PATH below.)

-   **REMOTE SOURCES** This is the remote library used by this `gem` installation.

-   **SHELL PATH** A list of the directories in your shell `PATH` variable. If you are having problems with `command not found` messages or running the wrong versions of programs, this variable may provide valuable clues about where the system is looking for your programs.

    If you don't remember what the `PATH` is or how to set it, please take some time to review the chapter on the environment in our [Introduction to the Command Line](https://launchschool.com/books/command_line/read/environment) book.

## Requiring Gems

You use Gems in your Ruby programs in the same way that you use the standard libraries: you `require` them in your program:

```ruby numbers=off
require 'colorize'
```

Here, we load the Gem named `colorize`. This makes Ruby search through a list of directories for the file named `colorize.rb`. The default list of directories includes every subdirectory of both `{USER INSTALLATION DIRECTORY}/gems` and `{INSTALLATION DIRECTORY}/gems`. Ruby loads the first occurrence of `colorize.rb` that it finds and makes it available for use.

You don't often need to worry about this search order; the `gem install` command ensures that your Gems are in the right locations. However, you may sometimes run into strange problems; sometimes, loading the wrong Gem is the only reasonable explanation. For example, suppose your program needs the `disable_colorization` feature of the `colorize` Gem (`colorize` version 0.7.5 introduces this feature). However, when you try to run the program, you get an error that says that feature doesn't exist. How can you determine which version of the Gem Ruby loaded?

To determine the Gem version, you must ascertain the path name of the Gem loaded by Ruby. The easiest way to do this is to add some debugging code (or a call to `binding.pry`) to your program shortly after the point where you require the Gem:

```ruby numbers=off
require 'colorize'
 ...
puts $LOADED_FEATURES.grep(/\bcolorize\.rb/)
```

This displays something like this when you run the program (the value displayed may be different):

```terminal numbers=off
/usr/local/rbenv/versions/2.2.4/lib/ruby/gems/2.2.0/gems/colorize-0.7.4/lib/colorize.rb
```

This command searches the `$LOADED_FEATURES` Array for any entries that include `colorize.rb` in the name, and displays all entries that match. With luck, you have only one entry, though sometimes you may have several to choose from. It's usually pretty easy to choose the right entry though. This name is the name of the file that Ruby loaded into your program; by looking at that name, you can see that Ruby loaded the file from `colorize-0.7.4`. This means that Ruby loaded version 0.7.4 of the Gem, not version 0.7.5 (or higher) as needed.

Note that `$LOADED_FEATURES` is a global Array that is available to every Ruby program. You don't have to do anything special to use it.

If you confirm that you're using the wrong version of the Gem, you probably need to:

-   install the desired version.

-   use a more recent version of Ruby.

-   run your program with `bundle exec` (we'll discuss this command later).

## Multiple Gem Versions

In the previous section, we described an issue where our program used the wrong version of a Gem. One of the possible solutions, once we confirmed that we were using the wrong version, was to install the correct version. However, this introduces another opportunity for version-related problems with Gems.

New versions of Gems aren't always backward compatible with older versions; that is, something that works in an old version of the Gem no longer works in a newer version. This has the potential to break code that was working fine..

 For instance, suppose you have `colorize` version 1.2.3, and it isn't backwards compatible with version 0.7.5. Suppose further that you have a ruby program that requires a feature of version 0.7.5 that is no longer present in version 1.2.3. What happens when this program tries to `require 'colorize'`?

Here, Ruby loads the most recent version of the Gem. This means trouble for your hypothetical program; it needs the older version. You can handle this in several ways:

-   Provide an absolute path name to `require`.

-   Add an appropriate `-I` option to the ruby invocation.

-   Modify `$LOAD_PATH` prior to requiring `colorize`.

-   <odify the `RUBYLIB` environment variable.

These fixes are all hacks though; they will quickly become unmanageable and an enormous mess and source of bugs. You definitely do not want to go down this road except in the extremely short-term. The right choice is to use Bundler, as we discuss later.

## Summary

RubyGems (Gems) are packages of code designed for use with Ruby. Most Gems provide useful modules and classes designed to simplify working with certain types of data; others provide programs intended for use with Ruby code, but that run independent of your code. Some even provide both types of functionality.

The `gem install` command installs Gems on your system. It fetches the Gem code from a remote library, and stores it in a local library on your local file system. The exact location of that local library varies based on several factors. You can use the `gem env` command to determine the location of your Gem files: it reports the names of the USER INSTALLATION DIRECTORY and the INSTALLATION DIRECTORY. Ruby loads files from the `gems` subdirectory of these two directories.

Ruby programs often need a specific version of Ruby, and specific versions of the Gems it uses. This often leads to situations in which your system has multiple versions of Ruby installed, and each version of Ruby has its own set of Gems. There are Ruby Version Managers that manage multiple versions of Ruby and its Gems, and a program named Bundler that lets you configure the versions of Ruby and the Gems that it should use when running a specific program. In the next lesson, we discuss the Ruby Version Managers; namely rbenv and RVM. We'll discuss Bundler in the lesson after that.
