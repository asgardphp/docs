#Config

[![Build Status](https://travis-ci.org/asgardphp/config.svg?branch=master)](https://travis-ci.org/asgardphp/config)

The Config package helps you manage the configuration of your application.

- [Usage in Asgard Framework](#usage-asgard)
- [Usage outisde Asgard](#usage-outside)
- [methods](#methods)
- [structure](#structure)
- [Commands](#commands)

<a name="usage-asgard"></a>
##Usage in Asgard Framework

	$config = $app['config'];

$app is usually available as a parameter, an object attribute or through Asgard\Container\Container::instance();

<a name="usage-outside"></a>
##Usage outside Asgard

	$config = new \Asgard\Config\Config;

<a name="methods"></a>
##Methods

Config inherits [\Asgard\Common\Bag](docs/bag) to access its data.

Besides, you can load a configuration with:

	$config->loadConfigFile('file.yml');

Or a whole directory:

	$config->loadConfigDir('config/');

<a name="structure"></a>
##Configuration file structure

A configuration file, like config.yml contains an array of parameters in YAML format:

	database:
		host: localhost
		user: root
		password:
		database: asgard

Each key can be acessed like:

	$config->get('database.user');
	#or
	$config['database.user'];
	#or
	$config['database']['user'];

<a name="commands"></a>
##Commands