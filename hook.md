#Hook

[![Build Status](https://travis-ci.org/asgardphp/hook.svg?branch=master)](https://travis-ci.org/asgardphp/hook)

If you have ever used an event manager, you will find the Hooks component very similar. With the HooksManager you can create hooks, on which you can hook callbacks to be executed when the hooks are triggered.

- [Using HooksManager](#using)
- [Executing callbacks before and after hooks](#executing)
- [Filters](#filters)
- [The HooksChain Object](#hookschain)

<a name="install"></a>
##Using HooksManager
To get a new instance of HooksManager use:

	$hm = new HooksManager();

In an Asgard project, you can use the service: $app['hooks']

Then to hook a function, use:

	$hm->hook('name_of_hook', function($chain, $param1) {
		// ...
	});

The first parameter is always a HooksChain object. The next ones are passed when the hook is triggered.

Now, here is how you trigger a hook:

	$hm->trigger('name_of_hook', array($param));

If you want to execute your own function when calling trigger, use the last argument:

	$hm->trigger('name_of_hook', array($param), function($chain, $param) {
		// ...
	});

<a name="install"></a>
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
	$hm->trigger('name_of_hook', array(&$param));

<a name="install"></a>
##The HooksChain object
The chain contains all the callbacks to be executed in a hook. If a function returns a value, the chain stops and the value is returned by the trigger method.

The chain call also be stopped by calling:

	$chain->stop();

The function calling the stop method will be the last one to be executed.

To know how many functions have been executed in a hook:

	$hm->trigger('name_of_hook', array($param), null, $chain);
	$count = $chain->executed;

Here we provide a reference to retrieve the chain object and its executed property.