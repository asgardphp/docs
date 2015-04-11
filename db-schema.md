#Schema

Schema lets you build and modify sql tables. It uses the Doctrine DBAL and the API is very similar. [Please see their documentation.](http://doctrine-dbal.readthedocs.org/en/latest/index.html)

- [Usage in the Asgard Framework](#usage-asgard)
- [Usage outside the Asgard Framework](#usage-outside)
- [Creating a table](#create)
- [Modifying a table](#modify)
- [Indexes](#indexes)

<a name="usage-asgard"></a>
##Usage in the Asgard Framework

	$schema = $container['schema'];
	
The [container](docs/container) is often accessible as a method parameter or through a [ContainerAware](docs/container#containeraware) object. You can also use the [singleton](docs/container#usage-outside) but it is not recommended.

<a name="usage-outside"></a>
##Usage outside the Asgard Framework

	$config = [/*..*/];
	$db = new \Asgard\Db\DB($config);
	$schema = new \Asgard\Db\Schema($db);
	#or
	$schema = $db->getSchema();

<a name="create"></a>
##Creating a table with columns

	$schema->create('news', function($table) {
		$table->addColumn('id', 'integer', [
			'length' => 11
		]);
		$table->addColumn('score', 'decimal', [
			'length'   => 50,
			'precision'=> 5,
			'scale'    => 5,
			'notnull'  => true, #false by default
			'default'  => 'foo',
			'unsigned' => true,
			'fixed'    => true,
		]);
	});

<a name="modify"></a>
##Modifying an existing table

	$schema->table('news', function() {
		//...
	});

Inside the callback you can modify, remove and add new columns:

	//...
		$table->addColumn('id', 'integer', [/*...*/]);
	//...

Modifying a column:

	$table->changeColumn('title', [
		'type'     => 'string',
		'length'   => 50,
		'precision'=> 5,
		'scale'    => 5,
		'notnull'  => true, #false by default
		'default'  => 'foo',
		'unsigned' => true,
		'fixed'    => true,
	]);

Removing a column:

	$table->dropColumn('id');

Emptying a table

	$schema->emptyTable('news');

Removing a table

	$schema->drop('news');

Removing all tables

	$schema->dropAll();

Renaming all tables

	$schema->rename('foo', 'bar');

##Indexes

Set the primary key:

	#single column
	$table->setPrimaryKey(['id', 'title']);
	#multiple columns
	$table->setPrimaryKey(['id', 'title']);

Create a unique index:

	$schema->table('test', function($table) {
		$table->addUniqueIndex(['title'], 'title_index'); #index name is optional
	});

Create an simple index:

	$schema->table('test', function($table) {
		$table->addIndex(['title'], 'title_index'); #index name is optional
	});

Rename an index:

	$schema->table('test', function() {
		$table->renameIndex('title_index', 'new_index');
	});

Drop an index:

	$schema->table('test', function() {
		$table->dropIndex('title_index', 'new_index');
	});

Drop the primary key:

	$schema->table('test', function() {
		$table->dropPrimaryKey();
	});