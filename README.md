# (Culture Amp Web Team’s) Scripts To Rule Them All

_bash nazg gimbatul_

This is our set of boilerplate scripts fulfilling the [normalized script
pattern][pattern], based on [GitHub’s own scripts][upstream].

[pattern]: http://githubengineering.com/scripts-to-rule-them-all/
[upstream]: https://github.com/github/scripts-to-rule-them-all

While this pattern can work for projects based on any framework or language,
the example scripts in this repo are intended for Ruby applications.

## The idea

If our scripts are normalized by name across all of our projects, contributors
only need to know this naming pattern in order to get started working on a
project.

The project's particulars can be managed by its primary maintainers and encoded
in its scripts, which ensures everyone gets what they expect when they run any
given script.

## Requirements

These scripts are intended to work with [Homebrew][brew], [Homebrew
Bundle][brew-bundle], and our [cultureamp/web-team-devtools][devtools] Homebrew
tap.

When installing the scripts, also copy this [`Brewfile`][brewfile] into the root
of the project

[brew]: https://brew.sh
[brew-bundle]: https://github.com/Homebrew/homebrew-bundle
[devtools]: https://github.com/cultureamp/homebrew-web-team-devtools
[brewfile]: support/Brewfile

## The scripts

Each of these scripts is responsible for a unit of work. This way they can be
called from other scripts.

This not only cleans up a lot of duplicated effort, it means contributors can do
the things they need to do, without having an extensive fundamental knowledge of
how the project works. Lowering friction like this is key to faster and happier
contributions.

The following is a list of scripts and their primary responsibilities.

### script/bootstrap

[`script/bootstrap`][bootstrap] is used solely for fulfilling dependencies of
the project.

This can mean RubyGems, npm packages, Homebrew packages, Ruby versions, Git
submodules, etc.

The goal is to make sure all required dependencies are installed.

### script/support

[`script/support`][support] is used to start the supporting services required by
the application, such as the database, etc.

Run this script before running all other scripts below.

### script/setup

[`script/setup`][setup] is used to set up a project in an initial state.
This is typically run after an initial clone, or, to reset the project back to
its initial state.

This is also useful for ensuring that the bootstrapping actually works well.

### script/update

[`script/update`][update] is used to update the project after a fresh pull.

If you have not worked on the project for a while, running
[`script/update`][update] after a pull will ensure that everything inside the
project is up to date and ready to work.

Typically, [`script/bootstrap`][bootstrap] is run inside this script. This is
also a good opportunity to run database migrations or any other things required
to get the state of the app into shape for the current version that is checked
out.

### script/server

[`script/server`][server] is used to start the application.

For a web application, this might start up any extra processes that the
application requires to run in addition to itself.

[`script/update`][update] should be called ahead of any application booting to
ensure that the application is up to date and can run appropriately.

### script/test

[`script/test`][test] is used to run the test suite of the application.

A good pattern to support is having an optional argument that is a file path.
This allows you to support running single tests.

Linting (i.e. rubocop, jshint, pmd, etc.) can also be considered a form of
testing. These tend to run faster than tests, so put them towards the beginning
of a [`script/test`][test] so it fails faster if there's a linting problem.

[`script/test`][test] should be called from [`script/cibuild`][cibuild], so it
should handle setting up the application appropriately based on the environment.
For example, if called in a development environment, it should probably call
[`script/update`][update] to always ensure that the application is up to date.
If called from [`script/cibuild`][cibuild], it should probably reset the
application to a clean state.

### script/console

[`script/console`][console] is used to open a console for the application.

A good pattern to support is having an optional argument that is an environment
name, so you can connect to that environment's console.

You should configure and run anything that needs to happen to open a console for
the requested environment.

[bootstrap]: script/bootstrap
[support]: script/support
[setup]: script/setup
[update]: script/update
[server]: script/server
[test]: script/test
[console]: script/console
