#Cache

[![Build Status](https://travis-ci.org/asgardphp/cache.svg?branch=master)](https://travis-ci.org/asgardphp/cache)

Asgard\Cache is a wrapper for the doctrine cache package. It provides a few additionnal features.

- [Usage in Asgard Framework](#usage-asgard)
- [Usage outside Asgard](#usage-outside)
- [Array implementation](#array)
- [Default result](#default)
- [NullCache](#nullcache)
- [Commands](#commands)

<a name="usage-asgard"></a>
##Usage in Asgard Framework
The cache is available through the service:

	$cache = $app['cache'];

<a name="usage-outside"></a>
##Usage outside Asgard

	$cache = new \Asgard\Cache\Cache(new \Doctrine\Common\Cache\Cache());

<a name="array"></a>
##Array implementation

You can access the cache like an array :

	$value = $cache['key'];
	$cache['key'] => $value;
	isset($cache['key']);
	unset($cache['key']);

<a name="default"></a>
##Default result

If the cache cannot fetch you key, it will return and store the default value. The default value can also be a callback, in which case the result will be returned and stored:

	$cache->fetch('key', 'default');
	#or
	$cache->fetch('key', function() {
		return 'default';
	})

<a name="nullcache"></a>
##NullCache

NullCache lets you use the cache without concern for its activation. The values will not be stored but the code using the cache will be same either way:

	$cache = new \Asgard\Cache\Cache(new \Asgard\Cache\NullCache());
	$res = $cache->fetch('home', function() {
		return '<h1>Home</h1>';
	});

If the cache driver was different, the result would be stored and used the next time the cache is called.

<a name="commands"></a>
##Commands

###Clear cache

Clear the cache.

Usage:

	php console cc