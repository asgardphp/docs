#Schema

Schema lets you build and modify sql tables.

- [Usage in Asgard Framework](#usage-asgard)
- [Usage outside Asgard](#usage-outside)
- [Creating a table](#create)
- [Modifying a table](#modify)

<a name="usage-asgard"></a>
##Usage in Asgard Framework

###Instance

	$schema = $app['schema'];

$app is usually available as a parameter, an object attribute or through Asgard\Container\Container::instance();

<a name="usage-outside"></a>
##Usage outside Asgard Framework

###Instance

	$config = [/*..*/];
	$db = new \Asgard\Db\DB($config);
	$dal = new \Asgard\Db\Schema($db);

<a name="create"></a>
##Creating a table

	$schema->create('news', function($table) {
		$table->add('id', 'int(11)')
			->add('title', 'varchar(100)');
	})

Inside the function, you create the columns like the id column in the previous example.

For each column you must give its name and its type with the length between brackets.

You can also modify a column.

Nullable:

	$table->add('title', 'varchar(100)')->nullable();

Default value:
	$table->add('title', 'varchar(100)')->def('abc');

Autoincrement:
	$table->add('title', 'varchar(100)')->autoincrement();

Primary key:
	$table->add('title', 'varchar(100)')->primary();

Unique key:
	$table->add('title', 'varchar(100)')->unique();

Index key:
	$table->add('title', 'varchar(100)')->index();

<a name="modify"></a>
##Modifying an existing table

Adding a column:

	$schema->table('news', function() {
		//...
	});

Inside the function you can modify, remove and add columns:

	//...
		$table->add('id', 'int(11)');
	//...

Modifying a column:

	$table->col('id')->nullable();
	$table->col('id')->notNullable();
	$table->col('id')->rename('identifier');
	$table->col('id')->def('abc');
	$table->col('id')->first();
	$table->col('id')->after('title');
	$table->col('id')->autoincrement();
	$table->col('id')->notAutoincrement();
	$table->col('id')->dropIndex();
	$table->col('id')->primary();
	$table->col('id')->unique();
	$table->col('id')->index();
	$table->col('id')->type('varchar(25)');

Removing a column:

	$table->drop('id');

Setting new primary keys:

	#one
	$table->primary('id');
	#multiple
	$table->primary(['id', 'title']);

Emptying a table

	$schema->emptyTable('news');

Removing a table

	$schema->drop('news');

Removing all tables

	$schema->dropAll();

Renaming all tables

	$schema->renaming('foo', 'bar');