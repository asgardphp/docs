#Database

[![Build Status](https://travis-ci.org/asgardphp/db.svg?branch=master)](https://travis-ci.org/asgardphp/db)

The DB package lets you manipulate the database, build SQL queries and manipulate the tables structure with Schema.

##DB

To connect to the database and make SQL queries. [See the documentation.](docs/db-db)

	$rows = $db->query('SELECT * FROM news ORDER BY id DESC')->all();

##DAL

To build SQL queries in a Object-Oriented manner. [See the documentation.](docs/db-dal)

	$rows = $dal->from('news')->orderBy('id DESC')->all();

##Schema

Build, modify and drop tables. [See the documentation.](docs/db-schema)

	schema->table('news', function($table) {
		$table->add('id', 'int(11)')
			->autoincrement()
			->primary();	
		$table->add('created_at', 'datetime')
			->nullable();	
		$table->add('updated_at', 'datetime')
			->nullable();	
		$table->add('title', 'varchar(255)')
			->nullable();
	});

##Commands

[List of commands that come with the DB package.](docs/db-commands)