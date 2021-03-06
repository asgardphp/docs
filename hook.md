#Hook

[![Build Status](https://travis-ci.org/asgardphp/hook.svg?branch=master)](https://travis-ci.org/asgardphp/hook)

If you have ever used an event manager, you will find the Hooks component very similar. With the HookManager you can create hooks, on which you can hook callbacks to be executed when the hooks are triggered.

- [Installation](#installation)
- [Usage in the Asgard Framework](#usage-asgard)
- [Usage outside the Asgard Framework](#usage-outside)
- [Create a hook](#create)
- [Trigger a hook](#trigger)
- [Executing callbacks before and after hooks](#executing)
- [Filters](#filters)
- [The HooksChain Object](#hookschain)
- [HooksContainer](#hookscontainer)

<a name="installation"></a>
##Installation
**If you are working on an Asgard project you don't need to install this library as it is already part of the standard libraries.**

	composer require asgard/hook 0.*

<a name="usage-asgard"></a>
##Usage in the Asgard Framework

	$hm = $container['hooks'];
	
The [container](docs/container) is often accessible as a method parameter or through a [ContainerAware](docs/container#containeraware) object. You can also use the [singleton](docs/container#usage-outside) but it is not recommended.

<a name="usage-outside"></a>
##Usage outside the Asgard Framework

	$hm = new \Asgard\Hook\HookManager;

<a name="create"></a>
##Create a hook

	$hm->hook('name_of_hook', function($chain, $param1) {
		// ...
	});

The first parameter is always a \Asgard\Hook\HookChain object. The next ones are passed when the hook is triggered.

<a name="trigger"></a>
##Trigger a hook

	$hm->trigger('name_of_hook', [$param]);

If you want to execute your own function when calling trigger, use the last argument:

	$hm->trigger('name_of_hook', [$param], function($chain, $param) {
		// ...
	});

<a name="executing"></a>
##Executing callbacks before and after hooks
To execute functions before a hook:

	$hm->preHook('name_of_hook', function($chain, $param) {
		// ...
	});

And after:

	$hm->postHook('name_of_hook', function($chain, $param) {
		// ...
	});

<a name="filters"></a>
##Filters
Hooks can be used as filters when parameters are passed by reference

	$hm->hook('name_of_hook', function($chain, &$param) {
		$param = 123;
	});
	$hm->trigger('name_of_hook', [&$param]);

<a name="hookschain"></a>
##The HooksChain object
The chain contains all the callbacks to be executed in a hook. If a function returns a value, the chain stops and the value is returned by the trigger method.

The chain call also be stopped by calling:

	$chain->stop();

The function calling the stop method will be the last one to be executed.

To know how many functions have been executed in a hook:

	$hm->trigger('name_of_hook', [$param], null, $chain);
	$count = $chain->executed;

Here we provide a reference to retrieve the chain object and its executed property.

<a name="hookscontainer"></a>
##HooksContainer
A HooksContainer is a class containing hooks. It extends Asgard\Hook\HookContainer and contains methods that are matched to hooks with annotations.

Example:

	<?php
	namespace Bundle\Hook;

	class SomeHooks extends \Asgard\Hook\HookContainer {
		/**
		 * @Hook("Asgard.Http.Start")
		 */
		public static function start($chain, $request) {
			//do something when HTTP starts processing a request
		}
	}

Here the method start is executed when the hook "Asgard.Http.Start" is triggered.

If you are working with a Asgard project, all HooksContainer in the Hook/ folder of a bundle are automatically loaded.

If not, you can register the hooks with:

	$annotationsReader = new \Asgard\Hook\AnnotationReader;
	$hooks = $annotationsReader->fetchHooks('Bundle\Hook\SomeHooks');
	$hookManager->hooks($hooks);