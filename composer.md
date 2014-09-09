#Composer

Composer is a package that makes easier to execute scripts when a package is installed, updated and uninstalled with composer.

- [Installation](#installation)
- [Usage](#usage)

<a name="installation"></a>
##Installation

	composer require asgard/composer 0.*

<a name="usage"></a>
##Usage

Create a composer/ folder in your project root and add the PHP files corresponding to the composer method you want to hijack:

	install			composer/install.php
	update			composer/update.php
	uninstall		composer/uninstall.php
	getInstallPath	composer/getInstallPath.php
	isInstalled		composer/isInstalled.php
	supports		composer/supports.php

For more information, please check the [BundleInstaller.php source code](https://github.com/asgardphp/composer/blob/master/BundleInstaller.php) and [composer documentation](https://getcomposer.org/doc/articles/custom-installers.md).