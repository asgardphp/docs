#Schema

Schema lets you build and modify sql tables.

- [Usage in the Asgard Framework](#usage-asgard)
- [Usage outside the Asgard Framework](#usage-outside)
- [Creating a table](#create)
- [Modifying a table](#modify)

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
##Creating a table

//todo refaire le code

	$schema->create('news', function($table) {
		$table->add('id', 'int(11)')
		      ->add('title', 'varchar(100)');
	})

Inside the function, you create the columns like the id column in the previous example.

For each column you must give its name and its type with the length between brackets.

You can also modify a column.

Nullable:
//todo refaire le code

	$table->add('title', 'varchar(100)')->nullable();

Default value:
//todo refaire le code

	$table->add('title', 'varchar(100)')->def('abc');

Autoincrement:
//todo refaire le code

	$table->add('title', 'varchar(100)')->autoincrement();

Primary key:
//todo refaire le code

	$table->add('title', 'varchar(100)')->primary();

Unique key:
//todo refaire le code

	$table->add('title', 'varchar(100)')->unique();

Index key:
//todo refaire le code

	$table->add('title', 'varchar(100)')->index();

<a name="modify"></a>
##Modifying an existing table

Adding a column:

	$schema->table('news', function() {
		//...
	});

Inside the function you can modify, remove and add columns:
//todo refaire le code

	//...
		$table->add('id', 'int(11)');
	//...

Modifying a column:
//todo refaire le code

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
//todo refaire le code

	$table->drop('id');

Setting new primary keys:
//todo refaire le code

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

	$schema->rename('foo', 'bar');