#Framework

[![Build Status](https://travis-ci.org/asgardphp/validation.svg?branch=master)](https://travis-ci.org/asgardphp/asgard)

- [Installation](#installation)
- [Configuration](#configuration)
- [Project Structure](#project-structure)
- [Bundles](#bundles)
- [Services](#services)
- [Testing](#testing)

<a name="installation"></a>
##Installation

The installation is very straighforward. Open a console, go to your web directory and run:

	composer create-project asgardphp/starter [project]

Don't forgert to replace [project] with the name of your new project.

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

After installation, an Asgard project contains the fllowing directories:

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
	Hooks/			contains the [hooks containers/](docs/hook)
	translation/	contains the translation files
	Bundle.php

Any of these folder is optional and has a specific purpose.

The bundle root namespace should follow the pattern \Vendor\Bundlename. If the bundle is in app/, the namespace should only be \Bundlename.

###Bundle.php

Bundle.php contains a class called Bundle which extends \Asgard\Core\BundleLoader in the bundle root folder. The bundle class can implements two methods:

	<?php
	namespace MyBundle;

	class Bundle extends \Asgard\Core\BundleLoader {
		public function buildApp($app) {
			// ...
		}

		public function run($app) {
			parent::run($app);

			// ...
		}
	}

The method buildApp($app) is used to create the [application services container](docs/container). Here, add all your services to the container.

The method run($app) is used to bootstrap your bundle. Put here the code that needs to be executed during the loading of the application.

The parent run method automatically loads your bundle controllers, translations, commands and hooks so if you want to keep this behavior, don't remove the line "parent::run($app);".

You can also override the following methods:

	loadControllers()
	loadHooks()
	loadTranslations($translator)
	loadCommands($console)
	loadEntities()

The last two are only called when the application is in console mode.

<a name="services"></a>
##Services

A service is an object with specific responsabilities, as part of the application. Asgard provides some services by default:

	cache				Asgard\Cache\Cache
	config				Asgard\Config\Config
	console				Asgard\Db\DB
	cookiemanager		Asgard\Http\CookieManager
	db					Asgard\Db\DB
	email				Asgard\Email\SwiftEmail
	entitiesmanager		Asgard\Entity\EntitiesManager
	entityfieldssolver	Asgard\Entityform\EntityFieldsSolver
	entityform			Asgard\Entityform\EntityForm
	errorhandler		Asgard\Debug\ErrorHandler
	form				Asgard\Form\Form
	hooks				Asgard\Hook\HooksManager
	html				Asgard\Http\Utils\HTML
	httpkernel			Asgard\Http\HttpKernel
	kernel				Asgard\Core\Kernel
	logger				Logger
	migrationsmanager	Asgard\Migration\MigrationsManager
	paginator			Asgard\Common\Paginator
	resolver			Asgard\Http\Resolver
	response			Asgard\Http\Response
	rulesregistry		Asgard\Validation\RulesRegistry
	schema				Asgard\Db\Schema
	translator			Symfony\Component\Translation\Translator
	url					Asgard\Http\URL
	validator			Asgard\Validation\Validator
	widgetsmanager		Asgard\Form\WidgetsManager

Due to dependency injection, you can modify or even replace any of this service with your own.

Services are available through the services container, often called $app. In a controller, it is accessible through:

	$db = $this->app['db'];

The container can also be accessed through a singleton although this should be avoided:

	$db = \Asgard\Container\Container::singleton()['db'];

<a name="testing"></a>
##Testing

Tests are located in the tests/ folder. A PHPUnit configuration file (phpunit.xml) is also provided.

To run the tests, simple use the command:

	phpunit