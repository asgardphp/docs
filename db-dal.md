#DAL

The DAL class lets you build queries in a object-oriented fashion.

- [Usage in the Asgard Framework](#usage-asgard)
- [Usage outside the Asgard Framework](#usage-outside)
- [Build a query](#query)
- [SELECT](#select)
- [UPDATE](#update)
- [INSERT](#insert)
- [DELETE](#delete)

<a name="usage-asgard"></a>
##Usage in the Asgard Framework

	$dal = $container->make('dal');
	
The [container](docs/container) is often accessible as a parameter or through a [ContainerAware](docs/container#containeraware) object. You can also use the [singleton](docs/container#usage-outside) but it is not recommended.

<a name="usage-outside"></a>
##Usage outside the Asgard Framework

	$config = [/*..*/];
	$db = new \Asgard\Db\DB($config);
	$dal = new \Asgard\Db\DAL($db);

<a name="query"></a>
##Building a query

###Conditions (WHERE)

	$dal->where('foo', 2);
	#or
	$dal->where(['foo' => 2]);
	#or
	$dal->where(['foo=?' => 2]);
	#or
	$dal->where(['foo!=?' => 2]);
	#or
	$dal->where('foo IS NULL');

OR:

	$dal->where([
		'or' => [
			'foo' => 2,
			'foo' => 3,
		]
	]);

AND:

	$dal->where([
		'and' => [
			'foo' => 2,
			'bar' => 3,
		]
	]);

###Jointures

Left join:

	$dal->leftjoin('bar', 'foo.id=bar.foo_id');

Right join:

	$dal->leftjoin('bar', 'foo.id=bar.foo_id');

Inner join: 

	$dal->leftjoin('bar', 'foo.id=bar.foo_id');

###Selecting tables

	$dal->from('foo');

Multiple tables, separated by a comma:

	$dal->from('foo, bar');

With aliases:

	$dal->from('foo f, bar b');

###Selecting columns

	$dal->select('title, id, content');

With aliases:

	$dal->select('title, id, content');

###Offset

	$dal->offset(5);

###Limit

	$dal->limit(10);

###Order by

	$dal->orderBy('title ASC');

###Group by

	$dal->orderBy('foo_id');

###Reset

	$dal->reset();

<a name="select"></a>
##Select

Get the results:

	$dal->get();

Count results:

	$dal->count(); #e.g. 5

Group by:

	$dal->count('foo_id'); #e.g. ['1' => 2, '2' => 5, '3' => 1]

Get the minimum value of a column:

	$dal->min('score');

Group by:

	$dal->min('score', 'food_id');

Get the maximum value of a column:

	$dal->max('score');

Group by:

	$dal->max('score', 'food_id');

Get the average value of a column:

	$dal->avg('score');

Group by:

	$dal->avg('score', 'food_id');

Get the sum of a column:

	$dal->sum('score');

Group by:

	$dal->sum('score', 'food_id');

<a name="update"></a>
##Update

	$dal->from('foo')->update(['title' => 'hello', 'content' => 'world']);

<a name="insert"></a>
##Insert

	$dal->into('foo')->insert(['title' => 'hello', 'content' => 'world']);

<a name="delete"></a>
##Delete

	$dal->from('foo')->delete();

Deleting from specific tables:

	$dal->from('foo, bar, bob')->delete(['foo', 'bar']);