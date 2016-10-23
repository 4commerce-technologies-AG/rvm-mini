# rvm-mini environment

### Mini Ruby Version Management

Inspired by rvm (http://rvm.io) we created a very small ruby installer and
runtime environment.

Please download or check out rvm-mini from github beside your project folders.
You are able to generate as many environments for your ruby projects as you like.

Each environment possibly may have its own set of ruby release and gems. The
install script will download a rvm binary as well as the bundler gem. Afterwards you can use the environment to download and install your necessary gems and run your ruby projects.

New since release 1.0 you may also use your system wide ruby binaries but create project specific gem dependencies. This is very useful when running ruby on [Windows via Cygwin](https://www.cygwin.com/).

Since release 1.1 rvm-mini will also automatically update rubygems to latest release. This prevents a number of SSL certificate errors when using outdated rubygems.

rvm-mini is getting you small and swift ruby environments – you are able to run and organize different ruby versions and gems for each of your projects.


___


## Pre-requisites on your system

You need a running Linux/Unix or Windows/Cygwin system with bash installed. We tested rvm-mini until now just on a few popular linux distributions (Debian, Ubuntu, CentOS and Windows/Cygwin). It should at least be able to run on more with available bash. 


___


## Quick Installation 


#### Get rvm-mini

```bash
$ git clone https://github.com/4commerce-technologies-AG/rvm-mini.git path-to/my-ruby-env
```


#### Setup your environment

Get ruby binary for your os and update rubygems and install bundler gem

```bash
$ path-to/my-ruby-env/bin/rvm-mini --ruby-install
```


#### Ready

Please go on and proceed with your projects.


___


## Work on your project

After downloading and installing your ruby environment as described, you are
able to use it for running your projects. A small `hello_world` example in
your personal workspace directory will give a short usage tutorial for rvm-mini.


#### Get rvm-mini and create an example project

```bash

$ cd ~/Workspace

$ git clone https://github.com/4commerce-technologies-AG/rvm-mini.git my-ruby-env

$ my-ruby-env/bin/rvm-mini --ruby-install

$ mkdir hello_world

$ cd hello_world

```

Your projects source file `hw.rb` should look like:

```ruby

# Hello World Example
puts "Hello World!"

```


#### Run your project

Now you use the installed ruby environment to run your project:

```bash
$ ../my-ruby-env/bin/ruby hw.rb
```

Great :-)


#### Managing gems

Most ruby projects need additional gems. As an example we want to add the `mime-types` gem.

Your `Gemfile` should be like:

```ruby

# Gemfile example

source 'https://rubygems.org'

gem 'mime-types'

```

To download and install the required gems into the environment use:

```bash
$ ../my-ruby-env/bin/bundle install
```

To see the installed gems in your environment use:

```bash
$ ../my-ruby-env/bin/gem list --local
```


#### Using additional parameters to ruby and Co.

If you have divided your sources into a number of source files, you need sometimes to add parameters like `-I` to ruby. You can do so with rvm-mini. Let's say you have to include the sources from project folder `lib/addons` which are required by `require 'lib/addons/mylib'` to your source, then run:

```bash
$ ../my-ruby-env/bin/ruby -I . hw.rb
```

All additional parameter to the rvm-mini wrapper scripts will be given to their final commands.


___


## Life is easier with aliases

Instead using all the time a full-path call to rvm-mini's directories, you can generate a number of aliases to easily use the environment. To activate the aliases into your running shell you need to call the script via `source`. The alias generator should be called with the root path of your project path and is used as the aliases prefix.

#### Add aliases to your running shell

```bash
$ source ../my-ruby-env/bin/rvm-mini --aliases ~/Workspace/hello_world
```

Instead having to specify your project root as full path, rvm-mini can also handle relative pathes to your projects home and rebuild them to the correct full path. So the right call is:

```bash
$ source ../my-ruby-env/bin/rvm-mini --aliases .
```

After generating the aliases, rvm-mini will show all added aliases for your current selected environment and project. Now next time you can run just:

```bash
$ hello_world.ruby hw.rb
```

It is that easy :-)


#### Automatic include your project root

When using the aliased commands, your project root path is automaticly added as call param to ruby, rake and irb. So you can require files from project subfolders without extra specifying to include your root path.


___


## Available wrappers

To get a full running running ruby environment, rvm-mini has 6 wrapper scripts on board. The wrappers handle to call the environments local installed ruby and gems. Following wrappers exist in each environments `bin` folder:

> `my-ruby-env/bin/ruby`

> `my-ruby-env/bin/rake`

> `my-ruby-env/bin/irb`

> `my-ruby-env/bin/erb`

> `my-ruby-env/bin/gem`

> `my-ruby-env/bin/bundle`

They do what you expect them to do. If you are using aliases, then you will have a project related alias for each, like:

> `project.ruby`

> `project.rake`

> `project.irb`

> `project.erb`

> `project.gem`

> `project.bundle`


---


## Addtional gem run option

Sometimes you will get some additional exec commands when installing gems like rails. To run such kind of additional commands you need to wrap your environment as well. This is done by the new `run` parameter:

```bash
$ ../my-ruby-env/bin/gem run rails --version
```

or as aliased command:

```bash
$ hello_world.gem run rails --version
```

You may get a list of available gem tools when using `-l` or `--list` option:

```bash
$ ../my-ruby-env/bin/gem run --list
```


___


## rvm-mini script

Main functions from rvm-mini are handled via the `bin/rvm-mini` bash script. To get all options and a short help call:

```bash
$ ../my-ruby-env/bin/rvm-mini --help
```


___


## Typical installation structure

This is how projects and environments sit beside each other and are typically structured:

```

+ my-workspace
  
  + my-project
      main.rb
    + lib
        local.rb
    ...

  + my-ruby-env
      ruby-release (default ruby version to install)
    + bin
        ruby
        irb
        rake
        gem
        bundle
        rvm-mini
        rvm-mini.env

    + tools
        ruby-install

    + vendor    -> is created during install
        ruby    -> rvm binary will be install here
        gems    -> your gems will be install here
        bundler -> bundler gem will be install here

```


#### Cleanup ruby and gems

Easy, just use `rvm-mini --clean` or drop the folder `vendor`


#### Remove a rvm-mini environment

Easy as well, just drop the `environment` folder.


___


## Ruby releases and this repo management

#### Branch master

The master branch will try to always direct to the latest available realease. If there are important functional extensions to rvm-mini the version branches get updates as well.

#### Branch ruby-x.x.x

The branch is numbered as the corresponding ruby version, e.g. `ruby-2.2.1` has defined ruby version 2.2.1 as default.

#### Default ruby version

The default ruby version to install is stored in file `ruby-release`. With any branch of rvm-mini you can install any available binary from rvm binaries. You just have to specify your selected version when calling `rvm-mini --ruby-install` or use `latest` to automatically fetch latest available from rvm.

More infos about specific options and versions are available on the binaries download area from rvm or by help from rvm-mini.

> `$ rvm-mini --ruby-install --help`.

> https://rvm.io/binaries/


___


## Issues and suggestions

Please use the activated issues tracking here on GitHub:

> https://github.com/4commerce-technologies-AG/rvm-mini/issues

#### Known restrictions

All scripts will not operate correct if pathnames for environment or project folder containing spaces. Please have a look to your names before you run into problems.


## References

The original Ruby Version Manager hompage:

> http://rvm.io

> https://rvm.io/binaries/

The RubyGems Manager hompage

> https://rubygems.org/pages/download


### Author & Credits

Author: [Tom Freudenberg](http://about.me/tom.freudenberg)

Copyright (c) 2014-2016 [Tom Freudenberg](http://www.4commerce.de/), [4commerce technologies AG](http://www.4commerce.de/), released under the MIT license

