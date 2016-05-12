#DAL

The DAL class lets you build queries in a object-oriented fashion.

- [Usage in the Asgard Framework](#usage-asgard)
- [Usage outside the Asgard Framework](#usage-outside)
- [Identifiers and keywords](#identifiers)
- [Build a query](#query)
- [SELECT](#select)
- [UPDATE](#update)
- [INSERT](#insert)
- [DELETE](#delete)
- [Debugging](#debugging)

<a name="usage-asgard"></a>
##Usage in the Asgard Framework

	$dal = $container->make('dal');
	
The [container](docs/container) is often accessible as a method parameter or through a [ContainerAware](docs/container#containeraware) object. You can also use the [singleton](docs/container#usage-outside) but it is not recommended.

<a name="usage-outside"></a>
##Usage outside the Asgard Framework

	$config = [/*..*/];
	$db = new \Asgard\Db\DB($config);
	$dal = new \Asgard\Db\DAL($db);
	#or
	$dal = $db->dal();

<a name="identifiers"></a>
##Identifiers and keywords

Identifiers like table and column names are automatically encapsulated in quotes. The default table name is automatically added to column identifiers that have no table name.

Identifiers, keywords and aliases are identified according to these rules:

- Identifiers: starts with a lowercase character (name, nAme, na_me, ..)
- Aliases: must start with an uppercase character (Alias, ..)
- Keywords: must be all uppercase (LIKE, IS, NOW, ..)

For example:

	SELECT name Na FROM table WHERE Na = ? ORDER BY id ASC

with the parameter: 'test'

The DAL will transform this query into:

	SELECT `name` `Na` FROM `table` WHERE `Na` = 'test' ORDER BY `table`.`id` ASC

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

	$dal->rightjoin('bar', 'foo.id=bar.foo_id');

Inner join: 

	$dal->innerjoin('bar', 'foo.id=bar.foo_id');

###Selecting tables

	$dal->from('foo');

Multiple tables, separated by a comma:

	$dal->from('foo, bar');

With aliases:

	$dal->from('foo Fo, bar Ba');

###Selecting columns

	$dal->select('title, id, content');

With aliases:

	$dal->select('title as Ti, id as Id, content as Co');

###Offset

	$dal->offset(5);

###Limit

	$dal->limit(10);

###Order by

	$dal->orderBy('title ASC');

###Group by

	$dal->groupBy('id');

Reverse order:

	$dal->reverse();

###Group by

	$dal->orderBy('foo_id');

###Reset

	$dal->reset();

This resets conditions, offset, limit and order.

###Pagination

	$dal->paginate($page, $per_page=10);

This will only return the results of the current page.

To get the paginator object ([\Asgard\Common\PaginatorInterface](docs/paginator)):

	$dal->getPaginator();

To set paginator factory:

	$dal->setPaginatorFactory(\Asgard\Common\PaginatorFactoryInterface $paginatorFactory);

###Sub-queries

You can use sub-queries in conditions and jointures:

	$dal->where('score > ?', new \Asgard\Db\Raw('SELECT sum(grade) FROM foo'));
	$dal->leftjoin('bar', ['score > ?' => new \Asgard\Db\Raw('(SELECT sum(grade) FROM foo)')]);

This will prevent the DAL from modifying your sub-query.

###Nested DALs

You can even nest DALS.

	$dal->from('news')->where('id', 
		$this->getDAL()->from('tags')->select('news_id')->where('id', 2)->limit(1)
	)

###UNIONs

You can combine DALs like SQL UNION queries.

	$dal->union([
		$dal->from('news')->where('type', 'foo'),
		$dal->from('news')->where('type', 'bar'),
	]);

<a name="select"></a>
##Select

Get the results:

	$dal->get();

Loop through:

	while($row = $dal->next())
		//...
	#or
	foreach($dal as $row)
		//...

Get first:

	$dal->first();

Get last:

	$dal->last();

Count results:

	$dal->count(); #e.g. 5

Count distinct results:

	$dal->count('id'); #e.g. 5

Group by:

	$dal->count(null, 'foo_id'); #e.g. ['1' => 2, '2' => 5, '3' => 1]

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

You can also use jointures for updates:

	$dal->from('foo')->leftjoin('bar', 'bar.id=foo.bar_id')->where('bar.id=1')->update([/*...*/]);

*Although sqlite normally does not support deletion with jointures, the DAL makes it possible.*

<a name="insert"></a>
##Insert

	$dal->into('foo')->insert(['title' => 'hello', 'content' => 'world']);

*into() is an alias of from()*

<a name="delete"></a>
##Delete

	$dal->from('foo')->delete();

You can also use jointures for deletions:

	$dal->from('foo')->leftjoin('bar', 'bar.id=foo.bar_id')->where('bar.id=1')->delete();

*Although sqlite normally does not support deletion with jointures, the DAL makes it possible.*

Deleting from specific tables:

	$dal->from('foo, bar, bob')->delete(['foo', 'bar']);

<a name="debugging"></a>
###Debugging

If you want to see the result of a query, you can use dbgSelect, dbgUpdate, dbgInsert and dbgDelete instead of their respective functions:

	$dal->dbgSelect(); #SELECT FROM ..
	$dal->dbgUpdate([/*..*/]); #UPDATE FROM ..
	$dal->dbgDelete(); #DELETE FROM ..
	$dal->dbgInsert([/*..*/]); #INSERT INTO ..