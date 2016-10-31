Words and phrases to try to use
  - Glue
  - Simple, not easy
  - Mental model

- Introduction

- Your Ruby Installation
  - The System Ruby (Linux, OS X)
  - Homebrew Ruby (OS X)
  - What gets installed?
  - Where are things installed?

- Executing Ruby Programs (I like the idea of covering this material; it helps to know this material when diagnosing problems with gems, rpms, and bundler)
  - Using the `ruby` command
  - Files and executables in Linux and OS X
  - \#!/something/ruby
  - \#!/something/env ruby
  - The environment (PATH, etc)
  - When things go wrong
    - command not found?
    - required file not found?
    - Wrong version of ruby/gem? (defer to rbenv/rvm/bundler)

- Gems
  - What are Gems?
  - Gems, Ruby, and your Computer
    - The Remote Repository
    - The Local Repository
    - Gems and Your File System
    - Ruby Require Paths
    - Multiple Gem Versions

- Ruby Program Managers: rbenv and rvm
  - When things go wrong
  - What do ruby program managers do?
  - How do they work
  - rbenv
    - how it works
    - setting local rubies
    - Where are my rubies, Gems and apps now?
    - When things go wrong
  - rvm
    - how it works
    - setting local rubies
    - Where are my rubies, Gems and apps now?
    - When things go wrong
  - Which ruby program manager should I use?

- Bundler, the dependency manager
  - When things go wrong
  - What does bundler do?
  - How it works
  - Setting up a Gemfile
  - Creating a Gemfile.lock
  - Where are my rubies, Gems and apps now?
  - When things go wrong

- When Things Go Wrong: Quick Overview of Other Tools
  - rubocop (using the correct version)
  - pry (using the correct version)
  - rake (bundle exec rake)

- Ruby Organization
  - The directory structure
  - Example: organizing the todo list code
    - Setting up a Rakefile
    - Setting up Gemfile
    - Creating a Gem

- Summary
