# Homebrew-PHP

A centralized repository for PHP-related brews.

## Common Issues

Bugs inevitably happen - none of us is running EVERY conceivable setup - but hopefully the install process can be made smoother through the following tips:

- If you have recently upgraded your Mac OS version or Xcode, read [this section](#common-upgrade-issues).
- Upgrade your OS X to the latest patch version. So if you are on `10.7.0`, upgrade to `10.7.4` etc.
- Ensure Xcode is installed and up to date.
- Run `brew update`. If you tapped an old version of `homebrew-php` or have an old brew installation, this may cause some installation issues.
- Run `brew upgrade`. This will upgrade all installed formulae. Sometimes an old version of a formula is installed and this breaks our dependency management. Unfortunately, there is currently no way to force homebrew to upgrade only those we depend upon. This is a possible fix for those with `libxml` related compilation issues.
- If `brew doctor` complains about `Error: Failed to import: homebrew-php-requirement` or similar, you can find broken php requirement files using `find $(brew --prefix)/Library/Formula -type l -name "*requirement.rb"`. Run this with the `-delete` flag if you are sure the results of the find contain only the files producing import failures. You can also remove them manually.
- If you upgraded to Mavericks 10.9.x, please also upgrade to the latest Xcode, 5.0.1 and make sure you re-install Command Line Tools: `xcode-select --install`
- On Mavericks 10.9.x, if  `xcode-select --install` displays the error message `Can't install the software because it is not currently available from the Software Update server.`, download Command Line Tools from [Apple Developer]( https://developer.apple.com/downloads/index.action?name=Command%20Line%20Tools).
![Xcode 5 command line tool installation](http://i.imgur.com/BKde0XJ.jpg)
- If you are using Xcode 4, install the `Command Line Tools`. If you think you have them installed, please ensure that an update of Xcode or OS X did not remove them. You can verify this by launching Xcode, opening preferences, going to the Downloads tab, and clicking the `Install` button:
![command line tool installation](http://f.cl.ly/items/411X3k0m2O1p1U2Y0I30/Image%202012.11.15%2011:32:41%20AM.png)
- Delete your `~/.pearrc` file before attempting to install a `PHP` version, as the pear step will fail if an existing, incompatible version exists. We try to detect and remove them ourselves, but sometimes this fails.
- Run `brew doctor` and fix any issues you can.
- If you are using Mountain Lion `10.8.x`, please install [XQuartz](http://xquartz.macosforge.org/landing/) so that the `png.h` header exists for compilation of certain brews. Mountain Lion removes X11, which contained numerous headers. A permanent fix is forthcoming.
- If you upgraded to Mountain Lion `10.8.x`, please also upgrade to the latest Xcode, 4.4.
- File an awesome bug report, using the information in the next section.
- If you have a failing install due to `GD build test failed`, try running the following before attempting to reinstall:

```
brew rm freetype jpeg libpng gd zlib
brew install freetype jpeg libpng gd zlib
```

Doing all of these might be a hassle, but will more than likely ensure you either have a working install or get help as soon as possible.

## Common upgrade issues

If you have recently upgraded your Mac OS X version or Xcode, you may have some compilation or missing libraries issues. The following information may help you solve most of the problems:


- Ensure you have properly upgraded CLT depending on your Xcode version.
- Proceed step by step to isolate the responsible formula. If you need to install `php55` and `php55-imagick`, don't do `brew install php55 php55-imagick`. Just do `brew install php55`, ensure everything is working as expected, check the output of `phpinfo()`, restart your Apache server with `sudo apachectl restart`. Then you can install the next formula `brew install php55-imagick`.
- If `php53`, `php54` or `php55` build fails, remove all their dependencies and reinstall the formula. For instance: If `brew install php55` fails, do the following: `brew rm php55 && brew deps php55 | xargs brew rm`. If `brew install php55 --with-gmp` fails, do the following: `brew rm php55 && brew deps php55 --with-gmp | xargs brew rm`. Then reinstall a clean version of the formula: `brew update && brew upgrade && brew install php55`.
- If an extension build fails, try also to remove all its dependencies and reinstall it.
- Sometimes it appears that a formula is not available anymore, do the following: `brew tap --repair`.

----

**SUPERHACK DANGEROUS DON'T DO IT**

If none of the above works, and we are unable to fix your issue after you've filed a bug report, try installing the [`OS X GCC Installer`](https://github.com/kennethreitz/osx-gcc-installer/). A small number of users have reported success after doing so.

----

### Filing Bug Reports

Please include the following information in your bug report:

- OS X Version: ex. 10.7.3, 10.6.3
- Homebrew Version: `brew -v`
- PHP Version in use: stock-apple, homebrew-php stable, homebrew-php devel, homebrew-php head, custom
- Xcode Version: 4.4, 4.3, 4.0, 3 etc.
  - If you are on Mountain Lion `10.8.x`, please also upgrade to the latest Xcode, 4.4.
  - If using 4.3, verify whether you have the `Command Line Tools` installed as well
  - If on Snow Leopard, you may want to install the [`OS X GCC Installer`](https://github.com/kennethreitz/osx-gcc-installer/)
- Output of `gcc -v`
- Output of `php -v`
- Output of `brew install -V path/to/homebrew-php/the-formula-you-want-to-test.rb --with-your --opts-here` within a [gist](http://gist.github.com). Please append any options you added to the `brew install` command.
- Output of `brew doctor` within a [gist](http://gist.github.com)

This will help us diagnose your issues much quicker, as well as find commonalities between different reported issues.

## Background

This repository contains **PHP-related** formulae for [Homebrew](https://github.com/mxcl/homebrew).

(This replaces the php formulae that used to live under [adamv's homebrew-alt repository](https://github.com/adamv/homebrew-alt).)

The purpose of this repository is to allow PHP developers to quickly retrieve
working, up-to-date formulae. The mainline homebrew repositories are maintained
by non-php developers, so testing/maintaining PHP-related brews has fallen by
the wayside. If you are a PHP developer using homebrew, please contribute to
this repository.

## Requirements

* [Homebrew](https://github.com/mxcl/homebrew)
* Snow Leopard, Lion, Mountain Lion. Untested everywhere else.
* The homebrew `dupes` tap - `brew tap homebrew/dupes`
* The homebrew `versions` tap - `brew tap homebrew/versions`

## Installation

_[Brew Tap]_

Setup the `homebrew/dupes` tap which has dependencies we need:

    brew tap homebrew/dupes

Setup the `homebrew/versions` tap which has dependencies we need:

    brew tap homebrew/versions

Then, run the following in your commandline:

    brew tap josegonzalez/homebrew-php

## Usage

Tap the `homebrew/dupes` repository into your brew installation:

    brew tap homebrew/dupes

Tap the `homebrew/versions` repository into your brew installation:

    brew tap homebrew/versions

Tap the repository into your brew installation:

    brew tap josegonzalez/homebrew-php

**Note:** For a list of available configuration options run:

    brew options php55

Then install php53, php54, php55, or any formulae you might need:

    brew install php55

That's it!

Please also follow the instructions from brew info at the end of the install to ensure you properly installed your PHP version.

### Installing Multiple Versions

Using multiple PHP versions from `homebrew-php` is pretty straightforward.

If using Apache, you will need to update the `LoadModule` call. For convenience, simply comment out the old PHP version:

```sh
# /etc/apache2/httpd.conf
# Swapping from PHP53 to PHP54
# $HOMEBREW_PREFIX is normally `/usr/local`
# LoadModule php5_module    $HOMEBREW_PREFIX/Cellar/php53/5.3.25/libexec/apache2/libphp5.so
LoadModule php5_module    $HOMEBREW_PREFIX/Cellar/php54/5.4.15/libexec/apache2/libphp5.so
```

If using FPM, you will need to unload the `plist` controlling php, or manually stop the daemon, via your command line:

```sh
# Swapping from PHP53 to PHP54
# $HOMEBREW_PREFIX is normally `/usr/local`
cp $HOMEBREW_PREFIX/Cellar/php54/5.4.15/homebrew-php.josegonzalez.php54.plist ~/Library/LaunchAgents/
launchctl unload -w ~/Library/LaunchAgents/homebrew-php.josegonzalez.php53.plist
launchctl load -w ~/Library/LaunchAgents/homebrew-php.josegonzalez.php54.plist
```

If you would like to swap the PHP you use on the command line, you should update the `$PATH` variable in either your `.bashrc` or `.bash_profile`:

```sh
# Swapping from PHP53 to PHP54
# export PATH="$(brew --prefix josegonzalez/php/php53)/bin:$PATH"
export PATH="$(brew --prefix josegonzalez/php/php54)/bin:$PATH"
```

Please be aware that you must make this type of change EACH time you swap between PHP `minor` versions. You will typically only need to update the Apache/FPM when upgrading your php `patch` version.

### PEAR Extensions

If installing `php53` or `php54`, please note that all extensions installed with the included `pear` will be installed to the respective php's bin path. For example, supposing you installed `PHP_CodeSniffer` as follows:

    pear install PHP_CodeSniffer

It would be nice to be able to use the `phpcs` command via commandline, or other utilities. You will need to add the installed php's `bin` directory to your path. The following would be added to your `.bashrc` or `.bash_profile` when running the `php54` brew:

```sh
export PATH="$(brew --prefix php54)/bin:$PATH"
```

Some caveats:

- Remember to use the proper php version in that export. So if you installed the `php53` formula, use `php53` instead of `php54` in the export.
- Updating your installed php will result in the binaries no longer existing within your path. In such cases, you will need to reinstall the pear extensions. Alternatives include installing `pear` outside of `homebrew-php` or using the `homebrew-php` version of your extension.
- Uninstalling your `homebrew-php` php formula will also remove the extensions.

## Contributing

The following kinds of brews are allowed:

- PHP Extensions: They may be built with PECL, but installation via homebrew is sometimes much easier.
- PHP Utilities: php-version, php-build fall under this category.
- Common PHP Web Applications: phpmyadmin goes here. Note that WordPress would not qualify because it requires other migration steps, such as database migrations etc.
- PHP Frameworks: These are to be reviewed on a case-by-case basis. Generally, only a recent, stable version of a popular framework will be allowed.

If you have any concerns as to whether your formula belongs in PHP, just open a pull request with the formula and we'll take it from there.

### PHP Extension definitions

PHP Extensions MUST be prefixed with `phpVERSION`. For example, instead of the `Solr` formula for PHP54 in `solr.rb`, we would have `Php54Solr` inside of `php54-solr.rb`. This is to remove any possible conflicts with mainline homebrew formulae.

The template for the `php54-example` pecl extension would be as follows. Please use it as an example for any new extension formulae:

require File.join(File.dirname(__FILE__), 'abstract-php-extension')

```ruby
class Php54Example < AbstractPhp54Extension
  init
  homepage 'http://pecl.php.net/package/example'
  url 'http://pecl.php.net/get/example-1.0.tgz'
  sha1 'SOMEHASHHERE'
  version '1.0'
  head 'https://svn.php.net/repository/pecl/example/trunk', :using => :svn

  def install
    Dir.chdir "example-#{version}" unless build.head?

    ENV.universal_binary if build.universal?

    safe_phpize
    system "./configure", "--prefix=#{prefix}", phpconfig
    system "make"
    prefix.install "modules/example.so"
    write_config_file unless build.include? "without-config-file"
  end
end
```

Before testing the extension, you will need run the command `brew tap --repair` to create a symlink in `$HOMEBREW_PREFIX/Library/Formula`.

Before finalizing the extension, run the command `brew audit` to check that your formula respects Homebrew best practice and syntax.

Defining extensions inheriting AbstractPhp5(345). Extension will provide a `write_config_file` which add `ext-{extension}.ini` to `conf.d`, don’t forget to remove it manually upon extension removal. Please see [abstract-php-extension.rb](Formula/abstract-php-extension.rb) for more details.

Please note that your formula installation may deviate significantly from the above; caveats should more or less stay the same, as they give explicit instructions to users as to how to ensure the extension is properly installed.

The ordering of formula attributes, such as the `homepage`, `url`, `sha1`, etc. should follow the above order for consistency. The `version` is only included when the URL does not include a version in the filename. `head` installations are not required.

All official PHP extensions should be built for all stable versions of PHP included in `homebrew-php`. These versions are `5.3.28`, `5.4.26` and `5.5.10`.

Please also consider adding PHP extensions for the PHP 5. alpha version : `5.6.0-alpha3`.

## Todo

* ~~Proper PHP Versioning? See issue [#1](https://github.com/josegonzalez/homebrew-php/issues/8)~~
* ~~Pull out all PHP-related brews from Homebrew~~

## License

Copyright (c) 2012 Jose Diaz-Gonzalez and other contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
