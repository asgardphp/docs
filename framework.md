#Framework

[![Build Status](https://travis-ci.org/asgardphp/asgard.svg?branch=master)](https://travis-ci.org/asgardphp/asgard)

- [New project](#new-project)
- [Configuration](#configuration)
- [Project Structure](#project-structure)
- [Bundles](#bundles)
- [Services](#services)
- [Testing](#testing)

<a name="new-project"></a>
##New project

The installation is very straightforward. Open a console, go to your web directory and run:

	composer create-project asgardphp/starter [folder]

Don't forget to replace [folder] with the location of your new project.

The command creates a project, installs Asgard, its dependencies and initialize the configuration file. From there you can already start working on your application.

If you need to use a database, run:

	php console db:init

<a name="configuration"></a>
##Configuration

The configuration files are all located in the config/ folder, in the YAML format. Example:

	database:
	  host: localhost
	  user: root
	  password:
	  database: asgard
	  prefix:

Configuration files concerning a specific environment must end with _ and the name of environment. Example:

	config.yml			for all environments
	config_dev.yml		for the dev environment
	config_prod.yml		for the prod environment
	config_test.yml		for the test environment

<a name="project-structure"></a>
##Project Structure

After installation, an Asgard project contains the following directories:

	app/			The application source-code
		General/	Default bundle
		bootstrap_[env].php		called at the application bootstrap
		helpers.php				global namespace functions
		Kernel.php				application Kernel
		Logger.php				error logger
		start.php				called for all new incoming request
	config/			Configuration
	composer/		Functions executed on a composer package installation
	translations/	Translations
	migrations/		Migrations
	tests/			Tests
	web/			Web assets
	vendors/		Dependencies
	autoload.php	PHP autoloader
	console			Console
	LICENSE			Project license
	phpunit.xml		PHPUnit configuration
	README.md		Project README

app/ is the place where you should write PHP code for your application, except the migrations and tests.

It is divided into bundles, including the bundle "General" which contains the default homepage, layout and 404 error handler.

<a name="bundles"></a>
##Bundles

###Structure
A bundle typically consists of:

	Controllers/	contains [HTTP controllers](docs/http-controllers)
	Entities/		contains [entities](docs/entity)
	html/			contains the [controllers views](docs/http-controllers#views)
	Hooks/			contains the [hooks containers/](docs/hook#hookscontainer)
	translation/	contains the translation files
	Bundle.php

Any of these folder is optional and has a specific purpose.

The bundle root namespace should follow the pattern \Vendor\Bundlename. If the bundle is in app/, the namespace should only be \Bundlename.

###Bundle.php

Bundle.php contains a class called Bundle which extends \Asgard\Core\BundleLoader in the bundle root folder. The bundle class can implements two methods:

	<?php
	namespace MyBundle;

	class Bundle extends \Asgard\Core\BundleLoader {
		public function buildContainer(\Asgard\Container\ContainerInterface container) {
			// ...
		}

		public function run(\Asgard\Container\ContainerInterface container) {
			parent::run(container);

			// ...
		}
	}

The method buildContainer($container) is used to create the [application services container](docs/container). Here, add all your services to the container.

The method run($container) is used to bootstrap your bundle. Put here the code that needs to be executed during the loading of the application.

The parent run method automatically loads your bundle controllers, translations, commands and hooks so if you want to keep this behavior, don't remove the line "parent::run($container);".

You can also override the following methods:

	loadControllers()
	loadHooks()
	loadTranslations($translator)
	loadCommands($console)
	loadEntities()

Use the [Core/BundleLoader.php](https://github.com/asgardphp/asgard/blob/master/Core/BundleLoader.php) file as reference.

The last two are only called when the application is in console mode.

<a name="services"></a>
##Services container

A service is an object with specific responsibilities, as part of the application. Asgard provides some services by default:

	cache				Asgard\Cache\CacheInterface
	config				Asgard\Config\ConfigInterface
	console				Asgard\Console\Application
	cookiemanager		Asgard\Common\BagInterface
	db					Asgard\Db\DBInterface
	email				Asgard\Email\SwiftEmail
	entitymanager		Asgard\Entity\EntityManagerInterface
	entityfieldsolver	Asgard\Entityform\EntityFieldSolverInterface
	entityform			Asgard\Entityform\EntityFormInterface
	errorhandler		Asgard\Debug\ErrorHandler
	flash				Asgard\Http\Utils\Flash
	form				Asgard\Form\FormInterface
	hooks				Asgard\Hook\HookManagerInterface
	html				Asgard\Http\Utils\HTMLInterface
	httpkernel			Asgard\Http\HttpKernel
	intl				Asgard\Common\Intl
	kernel				Asgard\Core\Kernel
	logger				Logger
	migrationmanager	Asgard\Migration\MigrationManagerInterface
	paginator			Asgard\Common\PaginatorInterface
	resolver			Asgard\Http\ResolverInterface
	rulesregistry		Asgard\Validation\RulesRegistryInterface
	schema				Asgard\Db\SchemaInterface
	session				Asgard\Common\BagInterface
	translator			Symfony\Component\Translation\TranslatorInterface
	url					Asgard\Http\URLInterface
	validator			Asgard\Validation\ValidatorInterface
	widgetmanager		Asgard\Form\WidgetManagerInterface

See what services your project contains with:

	php console services

Due to dependency injection, you can modify or even replace any of this service with your own.

Services are available through the services container. In a controller, it is accessible through:

	$db = $this->container['db'];

The container can also be accessed through a singleton although this should be avoided:

	$db = \Asgard\Container\Container::singleton()['db'];

<a name="testing"></a>
##Testing

Tests are located in the tests/ folder. A PHPUnit configuration file (phpunit.xml) is also provided.

To run the tests, simple use the command:

	phpunit