# About

Rubygems and Bundler integration, makes executable wrappers
generated by rubygems aware of bundler.

# Installation

    gem install rubygems-bundler

And follow the on screen instructions

# Description

This gem is intended to fill in the integration gap between
Bundler and Rubygems, it also backports shebang customization
from rubygems 1.9 to older versions - down to 1.3.7.

With this gem rubygems generated wrappers will allow to
use bundler when asked for.

Using this gem solves problem of calling `bundle exec ...`
with your every command.

Please note that this gem can make your gem executables
load in versions specified in Gemfile!

The problem with Gemfile is that you can have few of them,
so expect that gem version for executable will be taken from
`~/Gemfile` when your project is in `~/projects/my_project`
and does not contain `Gemfile`

Last note is that bundler handling can be used only when bundler is
installed, but you will be warned when it is 

# Controlling the wrapper

Wrappers generated by this gem will replace original rubygems wrapper,
but it will not change wrapper behavior unless explicitly asked for.

To allow using bundler when available, but fallback to rubygems when not:

    export USE_BUNDLER=try

To force usage of bundler:

    export USE_BUNDLER=force

Some gems should not be called with bundler support,
to blacklist them use the following line:

    BUNDLER_BLACKLIST="cheat heroku"

To make your choices persistent put them into `~/.bashrc` or `~/.rvmrc`.

# How it works

Installation of gem will make any new installed gem use new bundler
aware wrapper:

    gem install rubygems-bundler

Follow onscreen instructions.

Now for running haml can be controlled if it will using bundler code or not:

    mpapis@niczsoft:~/test> USE_BUNDLER=no haml -v
    Haml 3.1.1 (Separated Sally)
    
    mpapis@niczsoft:~/test> USE_BUNDLER=try haml -v
    Haml/Sass 3.0.0 (Classy Cassidy)

    mpapis@niczsoft:~/test> gem uninstall bundler
    Remove executables:
        bundle
    in addition to the gem? [Yn]  y
    Removing bundle
    Successfully uninstalled bundler-1.0.14

    mpapis@niczsoft:~/test> USE_BUNDLER=force haml -v
    /home/mpapis/.rvm/gems/ruby-1.9.2-p180@test-bundler/bin/haml:25:in `rescue in <main>':  (RuntimeError)

    Please install bundler first.

        from /home/mpapis/.rvm/gems/ruby-1.9.2-p180@test-bundler/bin/haml:18:in `<main>'

    mpapis@niczsoft:~/test> USE_BUNDLER=try haml -v
    Haml 3.1.1 (Separated Sally)

# Uninstallation

Before uninstalling change a line in `~/.gemrc` to:

    custom_shebang: $env ruby

and run `gem regenerate_binstubs`

this will set all gems to `/usr/bin/env ruby` which is one of the safest choices (especially when using rvm).

# Author

 - Michal Papis <mpapis@gmail.com>

# Thanks

 - Yehuda Katz     : the initial patch code
 - Wayne E. Seguin : support in writing good code
 - Evan Phoenix    : support on rubygems internalls
 - Andre Arko      : claryfications how rubygems/bundler works
 - Loren Segal     : shebang customization idea and explanations
