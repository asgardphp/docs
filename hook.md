#Hook

[![Build Status](https://travis-ci.org/asgardphp/hook.svg?branch=master)](https://travis-ci.org/asgardphp/hook)

If you have ever used an event manager, you will find the Hooks component very similar. With the HooksManager you can create hooks, on which you can hook callbacks to be executed when the hooks are triggered.

- [Usage in the Asgard Framework](#usage-asgard)
- [Usage outside the Asgard Framework](#usage-outside)
- [Create a hook](#create)
- [Trigger a hook](#trigger)
- [Executing callbacks before and after hooks](#executing)
- [Filters](#filters)
- [The HooksChain Object](#hookschain)

<a name="usage-asgard"></a>
##Usage in the Asgard Framework

	$hm = $container['hooks'];
	
The [container](docs/container) is often accessible as a parameter or through a [ContainerAware](docs/container#containeraware) object. You can also use the [singleton](docs/container#usage-outside) but it is not recommended.

<a name="usage-outside"></a>
##Usage outside the Asgard Framework

	$hm = new \Asgard\Hook\HooksManager;

<a name="create"></a>
##Create a hook

	$hm->hook('name_of_hook', function($chain, $param1) {
		// ...
	});

The first parameter is always a \Asgard\Hook\HooksChain object. The next ones are passed when the hook is triggered.

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

	$hm->hookBefore('name_of_hook', function($chain, $param) {
		// ...
	});

And after:

	$hm->hookAfter('name_of_hook', function($chain, $param) {
		// ...
	});

<a name="install"></a>
##Filters
Hooks can be used as filters when parameters are passed by reference

	$hm->hook('name_of_hook', function($chain, &$param) {
		$param = 123;
	});
	$hm->trigger('name_of_hook', [&$param]);

<a name="install"></a>
##The HooksChain object
The chain contains all the callbacks to be executed in a hook. If a function returns a value, the chain stops and the value is returned by the trigger method.

The chain call also be stopped by calling:

	$chain->stop();

The function calling the stop method will be the last one to be executed.

To know how many functions have been executed in a hook:

	$hm->trigger('name_of_hook', [$param], null, $chain);
	$count = $chain->executed;

Here we provide a reference to retrieve the chain object and its executed property.