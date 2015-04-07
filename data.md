#Data

[![Build Status](https://travis-ci.org/asgardphp/data.svg?branch=master)](https://travis-ci.org/asgardphp/data)

Data is package for key-value database storage.

- [Installation](#installation)
- [Usage in the Asgard Framework](#usage-asgard)
- [Usage outside the Asgard Framework](#usage-outside)
- [Fetch a value](#fetch)
- [Store a value](#store)
- [Delete a key](#delete)
- [Check for a key existence](#check)
- [Working with different data types](#types)

<a name="installation"></a>
##Installation
**If you are working on an Asgard project you don't need to install this library as it is already part of the standard libraries.**

	composer require asgard/data 0.*

<a name="usage-asgard"></a>
##Usage in the Asgard Framework

	$data = $container['data'];
	
The [container](docs/container) is often accessible as a method parameter or through a [ContainerAware](docs/container#containeraware) object. You can also use the [singleton](docs/container#usage-outside) but it is not recommended.

<a name="usage-outside"></a>
##Usage outside the Asgard Framework

	$config = [
		'host' => 'localhost',
		'user' => 'root',
		'password' => '',
		'database' => 'asgard',
		'prefix' => '',
		'driver' => 'mysql'
	];
	$db = new \Asgard\Db\DB($config);
	$data = new \Asgard\Data\Data($db);

<a name="fetch"></a>
##Fetch a value

	$data->get('key', 'default'); #'default' if returned in the value could not fetched
	#or
	$data['key'];

<a name="store"></a>
##Store a value

	$data->set('key', 'value');
	#or
	$data['key'] = 'value';

<a name="delete"></a>
##Delete a key

	$data->delete('key');
	#or
	unset($data['key']);

<a name="check"></a>
##Check for a key existence

	$data->has('key');
	#or
	isset($data['key']);

<a name="types"></a>
##Working with different data types

Register a type

	$data->register('obj',
		function($obj) {
			return serialize($obj);
		},
		function($str) {
			return unserialize($str);
		}
	);

Store a value with a specific type

	$obj = new StdClass;
	$obj->name = 'bob';
	$data->set('key', $obj, 'obj');