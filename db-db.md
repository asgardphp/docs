#DB

The DB class lets you connect and query the database.

- [Usage in Asgard Framework](#usage-asgard)
- [Usage outside Asgard](#usage-outside)
- [Query object](#query)
- [Last inserted id](#id)
- [Transactions](#transactions)

<a name="usage-asgard"></a>
##Usage in Asgard Framework

###Configuration
In config/database.yml:

	database:
	  host: localhost
	  user: root
	  password:
	  database: asgard
	  prefix:
	  driver: mysql #this line is optional and defaults to "mysql"

###Instance

	$db = $app['db'];

$app is usually available as a parameter, an object attribute or through Asgard\Container\Container::instance();

<a name="usage-asgard"></a>
##Usage outside Asgard Framework

###Configuration

	$config = [
		'host' => 'localhost',
		'user' => 'root',
		'password' => '',
		'database' => 'asgard',
		'prefix' => '',
		'driver' => 'mysql'
	];

###Instance

	$db = new \Asgard\Db\DB($config);

<a name="query"></a>
##Query Object

Execute a query:

	$query = $db->query('SELECT * FROM foo');

It returns a Query object which gives you access to some methods:

Loop through a query:

	while($row = $db->next()) { /*...*/ }

Get all rows at once:

	$rows = $db->all();

Count the number of rows:

	$db->count();

Get the first row only:

	$row = $db->first();

Get the number of affeted rows:

	$count = $db->query('DELETE FROM foo')->affected();

<a name="id"></a>
##Last inserted id

	$id = $db->id();

<a name="transactions"></a>
##Transactions

Begin a new transaction:

	$db->beginTransaction();

Commit:

	$db->commit();

Rollback

	$db->rollback();