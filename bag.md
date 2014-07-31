#Common\Bag

Bag object offers several methods to manipulate a $data property and it implements \ArrayAccess so that you can use the object as an array.

- [Usage](#usage)
- [Accessing data](#accessing-data)
- [Getting the whole array](#whole-array)
- [Setting a key](#setting-key)
- [Checking existence](#checking-existence)
- [Removing a key](#removing-key)

<a name="usage"></a>
##Usage

	$data = [
		'key1' => [
			'key2' => [
				'key3' => 'value'
			]
		]
	];
	$bag = new \Asgard\Common\Bag($data);

<a name="accessing-data"></a>
##Accessing data

	$bag->get('key1'); #returns the whole key1 array
	#or
	$bag['key1']; #returns the whole key1 array

	$bag->get('key1.key2.key3'); #returns 'value'
	#or
	$bag['key1.key2.key3']; #returns 'value'
	#or
	$bag['key1']['key2']['key3']; #returns 'value'

Null is returned when the key does not exist.

<a name="whole-array"></a>
##Getting the whole array

	$bag->all();

<a name="setting-key"></a>
##Setting a key

	$bag->set('key1.key2.key3', 'foo');
	#or
	$bag['key1.key2.key3'] = 'foo';

<a name="checking-existence"></a>
##Checking existence

	$bag->has('key1.key2.key3');
	#or
	isset($bag['key1.key2.key3']);

<a name="removing-key"></a>
##Removing a key

	$bag->remove('key1.key2.key3');
	#or
	unset($bag['key1.key2.key3']);