# Ruby Version Managers

In the previous chapter, we discussed RubyGems and how they interact with Ruby and your local file system. In that discussion, we casually mentioned that ruby version managers play a role in how Ruby uses Gems. However, we did not go into detail. Specifically, we did not discuss what ruby version managers are nor what services they provide to a developer who is writing and using Ruby programs. In this chapter, we address that lack of detail.

## What Are Ruby Version Managers?

**Ruby version managers** are programs that let you install, manage, and use multiple versions of Ruby. If you're just getting started with Ruby, you may wonder why you could possibly need to use more than one version of Ruby. Chances are, the programs you've written so far are "version agnostic"; that is, they don't rely on features of Ruby that depend on the ruby version. If you do rely on such features, the ones you use all run well in whatever version of Ruby you use, so you're not even aware that you're using version specific features.

However, this won't always be the case: Ruby is an evolving language with features added, modified, and deleted with every new version. Eventually, you're going to write or use a ruby program that needs a different version of Ruby, and that's when you will find that you need a Ruby version manager.

To provide an idea of what might change, here's a short list of some major changes to Ruby over the past few major versions at the time of this writing:

-   Version 1.9: Changed block variables to require local variables; Introduced `{ a: 5, b: 6 }` syntax for hashes with symbol keys; Many incompatible changes.

-   Version 2.0: Added keyword arguments; Added `%i()` syntax for symbol hashes.

-   Version 2.2: Removed several obsolete libraries.

-   Version 2.3: Added squiggle heredocs (`<<~HERE`); Added safe navigation operator (`&.`); Added `#positive?`, `#negative?`, and `#zero?` methods to the `Numeric` class.

As you can see, every new version has significant changes; sometimes, programs that run on older versions of Ruby no longer work in more modern versions. Someday, you will need to install another version of Ruby without removing your existing Ruby.

## Which Ruby Version Manager Should I Use?

There are two major ruby version managers in common use: [RVM](https://rvm.io/) and [rbenv](https://github.com/rbenv/rbenv). (There are others as well, like `chruby`, but their use is much less widespread.) They take different approaches to the problem of using multiple versions of Ruby, but the result is the same: you can easily use multiple versions of Ruby on the same system.

Which version manager should you use, though? This partly depends on your own preferences and needs, but most people can just pick one and start using it. Functionally, the two systems perform all the tasks most developers need. It most cases, there is little in the way of features to recommend one over the other.

At Launch School, we recommend `RVM`, primarily because the Cloud 9 development system provides RVM by default. If you don't use Cloud 9, but have a Mac and use Homebrew, rbenv is slightly easier to install and set up.

For information on how to install and use RVM or rbenv, see the documentation at the respective websites. In this booklet, we focus attention on how they interact with your system.

## RVM

### How RVM Works

At RVM's core is a set of directories in which RVM stores all your Ruby versions, its associated tools (such as `gem` and `irb`), and its Gems. Each directory is specific to a given Ruby version. If you need Ruby 2.3.1, RVM uses the files in the `ruby-2.3.1` directory. If you need Ruby 2.2.4, though, RVM gets the files from the `ruby-2.2.4` directory.

To accomplish this feat, RVM defines a shell function (see your shell's documentation for information on functions) named `rvm`. Your shell uses this function in preference to executing the disk-based `rvm` command. There are complex reasons behind this, but the main reason is that a function can modify your environment, but a disk-based command cannot.

When you run `rvm use VERSION` to change the Ruby version you want to use, you actually invoke the `rvm` function. The function modifies your environment so that the various ruby commands invoke the correct version. For instance, `rvm use 2.2.4` modifies your `PATH` variable so that the `ruby` command in the `ruby-2.2.4` directory. It makes other changes as well, but the `PATH` change is the most noticeable.

Things get more interesting when you set a project-specific version of Ruby, as we'll see in Setting Local Rubies.

If this sounds pretty complex, it is; but, in day-to-day use, it's mostly invisible. If you want to run, say, `rubocop` from the Ruby 2.2.3 directory, you tell RVM to use Ruby 2.2.3, then run the `rubocop` command. The system searches for a `rubocop` command in your `PATH`, and runs the first one it finds. Since the RVM directories usually occur early in your `PATH`, the system finds the desired `rubocop` command.

### Setting Local Rubies

How do you tell RVM to use a specific version of Ruby? There are several ways, each described in RVM's documentation. The easiest way is to first run:

```terminal
rvm use 2.2.4
```

As we saw above, this command modifies your environment in a way that ensures that the first ruby your system finds is the 2.2.4 version. This same change also works for related commands and gems like `irb` and `rubocop`.

Usually, though, you won't use this command. Instead, you will define specific versions for specific projects. You should define a default version of Ruby--the Ruby version that RVM uses automatically in a new terminal session or shell. You can do this with the command:

```terminal
rvm use 2.3.1 --default
```

If you set a different version as the current Ruby at some point, you can restore the default with:

```terminal
rvm use default
```

So far, so good. This is straightforward, and easily fits in with the `PATH` changes described in the previous section. Suppose, though, that you want to use a different version of Ruby with one of your projects? It's not too hard to switch versions with the `rvm use VERSION` command, but it is error prone: you will sometimes forget to change versions, and may also forget to switch back to the the default ruby when you finish. Instead, automating the version changes is desirable; this is where things get complex, though.

[There are several ways to do this](https://rvm.io/workflow/projects), but the easiest is to create a `.ruby-version` file in your project directory; the content of this file is just the version number of ruby that you want to use when using programs from that directory. For example, suppose we want to use Ruby 2.1.5 for the project in our `~/src/magic` directory. To do this, run:

```terminal
cd ~/src/magic
rvm --ruby-version use 2.1.5
```

This creates a `.ruby-version` file in the directory. Once set, you don't need to worry about setting the version for this project. For good measure, you should set your default version in your home directory:

```terminal
cd ~
rvm --ruby-version default
```

This ensures that RVM uses the default Ruby when you aren't in the project directory.
To make use of the `.ruby-version` file, RVM replaces the `cd` command from your shell with a shell function. This function invokes the real `cd` command, then checks for the `.ruby-version` file (it also checks for some other files we won't discuss). If it finds one of these files, it retrieves the version number from the file, and then modifies your environment in the same way the `rvm` function does. Thus, every time you change directories with the `cd` command, RVM modifies your environment to run the proper Ruby version.

RVM also uses the `Gemfile` for ruby projects that use Bundler (described in the next chapter). If the `Gemfile` contains a `ruby` directive, RVM uses that version of ruby to run the program. Note, however, that the `.ruby-version` file takes precedence.

See the [RVM](https://rvm.io/) documentation for more information on this process.

### Where Are My Rubies, Gems and Apps Now?

RVM creates a directory at installation known as the RVM path; it also installs all RVM-related files, including all the Rubies and Gems that it manages, in this directory. To determine the location of the RVM path, run:

```terminal
rvm info rvm
```

On the author's Cloud 9 account, this displays:

```
ruby-2.2.4:

  rvm:
    version:      "rvm 1.27.0 (latest) by Wayne E. Seguin <wayneeseguin@gmail.com>, Michal Papis <mpapis@gmail.com> [https://rvm.io/]"
    updated:      "2 months 6 days 20 hours 34 minutes 44 seconds ago"
    path:         "/usr/local/rvm"
```

The `path` entry in that listing is the RVM path.

There are two important subdirectories in the RVM path: the `rubies` directory, and the `gems` directory. The `rubies` directory contains all the rubies managed by RVM, as well as its associated utilities, libraries, and Gems. The `gems` directory has a similar structure, but each of the subdirectories contains a `bin` subdirectory: this directory contains the executable programs associated with your Gems, like `rubocop` and `rails`.
You may recall that `gem env` displays some interesting details about your Ruby and Gem configuration, most notably the directories it uses. After installing RVM, `gem env` still works; it shows you the details for the Ruby and Gem currently active in RVM; many of these files are under the RVM path.

### When Things Go Wrong

RVM is a complex system. While problems are rare once you have it running, this complexity can sometimes get in the way of stability, especially since it modifies system commands like `cd` by invoking them through shell functions. There are many moving parts that have the potential to go wrong. Probably they won't go wrong, but the potential remains.

Given that, the most likely issue with RVM is that you get confused about which version of Ruby you are running, or you install or update Gems with the wrong `gem` command.

Here are some useful troubleshooting hints and commands:

-   Make sure your directory name does not include any spaces, of none of ancestors (parents, grandparents, etc) do either. RVM does not currently support directory names with spaces.


-   `type cd | head -1 ; type rvm | head -1`

    Use this command to verify that you are using the `cd` and `rvm` functions, not the built-in `cd` command nor the executable file `rvm`. These commands should print:

    ```terminal
    cd is a function
    rvm is a function
    ```

    If it does, everything is okay. Otherwise, the `cd` and `rvm` functions don't get defined properly and RVM fails to work.
    To load `cd` and `rvm` as functions, confirm that your shell startup file (`.profile`, `.bash_profile`, etc.--see your specific shell's documentation) includes a command that looks like something this:

    ```terminal
    source "{RVM PATH}"/scripts/rvm"
    ```

    (`{RVM PATH}` is the name of your RVM path.) This command should be near the end of the file, after any commands that set or modify your `PATH` variable or replace built-in commands with functions. If you must change the file, start a new terminal session after making the changes, then close any previously open terminal sessions.

-   `echo $PATH`

Confirm that the `{RVM PATH}/rubies-VERSION/bin` and `{RVM PATH}/gem/rubies-VERSION/bin` directories are present in your `PATH`, and listed before any other directories that may contain programs with the same name. If, for example, `/usr/bin` occurs before `{RVM_PATH}/rubies-VERSION/bin` in your PATH, your system may load the system version of Ruby instead of one managed by RVM.

-   `rvm info`

    Displays a longish list of information about the current RVM enviroment. Similar to `gem env` in the information provided, but formatted differently, and including RVM-specific information.

-   `rvm current`

    Displays the active version of Ruby.If you see the wrong version displayed and don't understand why, see [the documentation](https://rvm.io/). The section on "Choosing the Ruby Version" is particularly important. If you use gemsets (see below), this command includes the gemset name in its output.

-   `rvm fix-permissions`

    Repairs the permissions on RVM files. This may be useful if you accidentally use `sudo` to install, modify or update RVM, or any of its rubies or Gems. If you see "permission denied" messages on any of these files, try running this command.

-   `rvm repair all`

Repairs files that help RVM manage the different rubies.This may be useful if RVM seems completely broken in areas.

-   `rvm get latest`

    Download and install the latest version of RVM. This is most useful when you are using a new or unfamiliar feature that may not be available or working correctly in your current version of RVM

-   `gem env`

    Display configuration information about your RubyGems system.

-   https://rvm.io/support/troubleshooting

    Read the official troubleshooting information for RVM.

### RVM Gemsets

RVM supports something called gemsets. This feature provides capabilities similar to that provided by Bundler (discussed in the next chapter): it lets you tie specify Gems to each of your projects. Bundler is, by far, more widely used, so we recommend using it instead of gemsets. However, you may one day need to learn about gemsets, so you might want to take a look at [the basic gemsets documentation](https://rvm.io/gemsets/basics).

## `rbenv`

### How rbenv Works

At rbenv's heart is a set of directories very similar to the directories at RVM's core. It stores and uses the rubies, associated tools, and Gems from these directories. There are subdirectories for each version of Ruby. If you need Ruby 2.3.1, rbenv uses the files in the 2.3.1 directory. If you need Ruby 2.2.4, though, rbenv gets the files from the 2.2.4 directory.

How rbenv does this is a significant departure from how RVM does it. It uses a set of small scripts called **shims**; the scripts have the same names as the various ruby and Gem programs. They live in the `shims` sub-directory of the main rbenv installation directory. Valid rbenv installations include the `shims` directory in the `PATH`; rbenv places it before any other directories that contain ruby or any related programs so that the system searches the `shims` first. This way, when you run one of the ruby commands or Gems, the system executes the proper shim script. The shim script, in turn, executes `rbenv exec PROGRAM`; this command determines what version of Ruby it should use, and executes the appropriate program from the Ruby version-specific directories.

That all sounds pretty complex, but it isn't. If you want to run, say, `rubocop` from the Ruby 2.2.3 directory, you tell rbenv use Ruby 2.2.3, then run the `rubocop` command. Magically, the system finds the `rubocop` shim, the shim runs `rbenv exec rubocop`, and that runs the Ruby 2.2.3 version of `rubocop`.

### Setting Local Rubies

How do you tell rbenv to use a specific version of Ruby? There are several ways, each described in rbenv's Github README file. The easiest way is to first run:

```terminal
rbenv global 2.3.1
```

This sets the default version of Ruby to use as 2.3.1. From there, you only need to worry about programs that need a different version. For example, let's say the program in your `~/src/magic` directory needs Ruby 2.0.0. To arrange that, run:

```terminal
cd ~/src/magic
rbenv local 2.0.0
```

This creates a `.ruby-version` file in the `~/src/magic` directory; when you run any Ruby-based programs in this directory, `rbenv` checks the `.ruby-version` file, and uses that version of Ruby for the program. If there is no `.ruby-version` file, `rbenv` launches a search for an alternative `.ruby-version` file, and, if it still can't find one, it uses the global version of Ruby.

Note that this `.ruby-version` file is identical to the `.ruby-version` file used by RVM: same name, same content, same function, and the same search sequence.

See the [rbenv](https://github.com/rbenv/rbenv) documentation for more information on this process.

### Where Are My Rubies, Gems and Apps Now?

Rbenv creates a directory at installation known as the rbenv root directory; it also installs all rbenv-related files, including all the Rubies and Gems that it manages, in this directory. To determine the location of the rbenv root directory, run:

```terminal
rbenv root
```

On the author's system, this displays:

```
/usr/local/rbenv
```

There are two important subdirectories in the root directory: the `shims` directory, and the `versions` directory. The `shims` directory contains all the shims used by rbenv, while `versions` contains all the different Rubies. Inside the `versions` directory, you will find one directory for each Ruby version you have installed; inside each of these directories, you will find the specific version of Ruby, all its companion programs, and your Gems for that version.

You may recall that can use `gem env` to learn some interesting details about your Ruby and Gem configuration, most notably the directories it uses. After installing rbenv, `gem env` still works, and shows you the details for the Ruby and Gem currently active in rbenv; you will find many of these files stored under the rbenv root directory.

### When Things Go Wrong

Compared to RVM, rbenv uses a conservative approach to perform its tasks: it doesn't rely on making dynamic changes to your environment or system commands. Instead, it makes a small, one-time change to your `PATH`, and leaves your system to run exactly as designed. This is the real strength of rbenv, and the main reason many developers prefer it over RVM; it's less likely to go wrong. There's just not that many moving parts to worry about.

Hence, problems with rbenv are few once you have it running. The worst that is likely to happen is that you might accidentally delete or move some files it needs, which you can usually fix from backups (you have backups, right?). The most likely issue with rbenv is that you get confused about which version of Ruby you are running, or you use the wrong `gem` command to install or update some Gems.

Here are some useful troubleshooting commands:

-   `rbenv version`

    Displays the currently active version of Ruby, along with a short explanation of how `rbenv` determined the version. If you see the wrong version displayed and don't understand why, see [the documentation](https://github.com/rbenv/rbenv). The section on "Choosing the Ruby Version" is particularly important.

-   `echo $PATH`

Confirm that your `shims` directory is in your PATH. Specifically, verify that it occurs early in your path. If, for example, `/usr/bin` occurs before `/usr/local/rbenv/shims` in your PATH, your system may load the system version of Ruby instead of one managed by rbenv.

-   `rbenv which COMMAND`

    Displays the disk location of the command named by COMMAND (e.g., `ruby`, `irb`, `rubocop`)

-   `rbenv rehash`

    Rebuilds the `shims` directory. If you can't execute some commands or `rbenv` doesn't appear to be running the correct commands, try this command.

-   `rbenv root`

    Display the path of the rbenv root directory.

-   `rbenv shims`

    Display a list of all current shims.

-   `gem env`

    Display configuration information about your RubyGems system.

### Plugins

There are a dozen or so [plugins](https://github.com/rbenv/rbenv/wiki/Plugins) that extend the capabilities provided by rbenv. Don't forget to check them out as your use of rbenv expands. Of particular interest to almost everybody is the [ruby-build](https://github.com/rbenv/ruby-build#readme); this adds the `install` command to the `rbenv` command so you can install rubies directly with rbenv; this is easier than manually configuring and installing each version.

## Summary

Ruby Version Managers let you manage multiple versions of Ruby, the utilities (such as `irb`) associated with each version, and the RubyGems installed for each Ruby. With version managers, you can install and uninstall ruby versions and gems, and run specific versions of ruby with specific programs and environments.

The two main version managers, RVM and rbenv, are similar in function, with little to distinguish between the two for most developers. By default, RVM has more features, but rbenv plugins provide much of the functionality not provided by the base install of rbenv. RVM works by dynamically managing your enviroment, mostly by modifying your `PATH` variable and replacing the built-in `cd` command with an RVM-aware shell function.

As we've seen, Ruby programs often need a specific version of Ruby, and specific versions of the Gems it uses. Ruby Version Managers take care of most of the issues arising from these differing requirements, but sometimes you'll find that they aren't enough. For example, you may need to use Ruby 2.2.4 for two different projects instead of your default 2.3.1, but you may also need separate versions of the Rails Gem, say 4.2.7 for on project, and version 5.0.0 for the other. While both RVM and rbenv (with the aid of a plugin) can handle these requirements, the easier and more common path is to use a RubyGem called Bundler. We discuss Bundler in the next chapter.
