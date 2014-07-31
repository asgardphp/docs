#Container

[![Build Status](https://travis-ci.org/asgardphp/container.svg?branch=master)](https://travis-ci.org/asgardphp/container)

The container provides services to the application. In the Asgard framework, the container is often stored in the $app variable.

- [Usage](#usage)
- [Registering a service](#registering)
- [Accessing a service](#accessing)
- [Creating a new service instance](#creating)
- [Checking if a service exists](#checking)
- [Removing a service](#removing)
- [Commands](#commands)

<a name="usage"></a>
##Getting a container instance

	$container = new \Asgard\Container\Container;
	#or
	$container = new \Asgard\Container\Container::singleton();

<a name="usage"></a>
##Registering a service

	$container->register('cache', function($container, $param) {
		return new \Cache($param);
	});
	#or
	$container['cache'] = new \Cache($param);
	#or
	$container->set('cache', new \Cache($param));

<a name="usage"></a>
##Accessing a service

	$cache = $container->get('cache', [$param]);
	#or
	$cache = $container['cache'];

If you call it multiple times, the container will make sure the same instance is returned every time.

<a name="usage"></a>
##Creating a new service instance

	$cache = $container->make('cache', [$param]);

<a name="usage"></a>
##Checking if a service exists

	$container->has('cache');
	#or
	isset($container['cache']);

<a name="usage"></a>
##Removing a service

	$container->remove('cache');
	#or
	unset($container['cache']);

<a name="usage"></a>
##Commands

###ListCommand

Show all the services loaded in the application.

Usage:

	php console services [--defined] [--registered]

--defined: to show where a service was defined

--registered: to shown where a service was registered