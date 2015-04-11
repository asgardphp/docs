#DB

The DB class lets you connect and query the database. It supports postgresql, mysql, sqlite and microsoft sql server.

- [Usage in the Asgard Framework](#usage-asgard)
- [Usage outside the Asgard Framework](#usage-outside)
- [PDO and Connection](#pdoconnection)
- [Query object](#query)
- [Last inserted id](#id)
- [Transactions](#transactions)
- [Schema and dal](#schemadal)

<a name="usage-asgard"></a>
##Usage in the Asgard Framework

###Configuration
In config/database.yml:

	database:
	  host: localhost
	  user: root
	  password:
	  database: asgard
	  prefix:
	  driver: mysql #this line is optional and defaults to "mysql"

Or for sqlite:

	database:
	  database: :memory: #or db.sqlite
	  driver: sqlite

Driver can be: pgsql, mysql, sqlite or msssql.

###Service

	$db = $container['db'];

The [container](docs/container) is often accessible as a method parameter or through a [ContainerAware](docs/container#containeraware) object. You can also use the [singleton](docs/container#usage-outside) but it is not recommended.

<a name="usage-asgard"></a>
##Usage outside the Asgard Framework

###Configuration

	$config = [
		'host' => 'localhost',
		'user' => 'root',
		'password' => '',
		'database' => 'asgard',
		'prefix' => '',
		'driver' => 'mysql'
	];

Or for sqlite:

	$config = [
		'database' => ':memory:', #or db.sqlite
		'driver' => 'sqlite',
	]

Driver can be: pgsql, mysql, sqlite or msssql.

###Instance

	$db = new \Asgard\Db\DB($config);

<a name="pdoconnection"></a>
##PDO and Connection

Get the PDO object:

	$db->getPDO();

Re-build the db PDO object:

	$db->buildPDO($config);

Get a doctrine connection object (Doctrine\DBAL\Connection):

	$db->getConn();

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

Get the number of affected rows:

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

<a name="schemadal"></a>
##Schema and DAL

Get a schema object:

	$schema = $db->getSchema();
	#or
	$schema = new \Asgard\Db\Schema($db);

Get a DAL object:

	$dal = $db->dal();
	#or
	$dal = new \Asgard\Db\DAL($db);