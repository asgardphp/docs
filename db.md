#Database

[![Build Status](https://travis-ci.org/asgardphp/db.svg?branch=master)](https://travis-ci.org/asgardphp/db)

The DB package lets you manipulate the database, build SQL queries and manipulate the tables structure with Schema.

##Installation
**If you are working on an Asgard project you don't need to install this library as it is already part of the standard libraries.**

	composer require asgard/db 0.*

##DB

To connect to the database and make SQL queries. [See the documentation.](docs/db-db)

	$rows = $db->query('SELECT * FROM news ORDER BY id DESC')->all();

##DAL

To build SQL queries in a Object-Oriented manner. [See the documentation.](docs/db-dal)

	$rows = $db-dal->from('news')->orderBy('id DESC')->all();

##Schema

Build, modify and drop tables. [See the documentation.](docs/db-schema)

	schema->table('news', function($table) {
		$table->addColumn('id', 'integer', [
			'length' => 11,
			'autoincrement' => true,
		]);
		$table->addColumn('created_at', 'datetime', [
		]);
		$table->addColumn('updated_at', 'datetime', [
		]);
		$table->addColumn('title', 'string', [
			'length' => '255',
		]);

		$table->setPrimaryKey(['id']);
	});

##Commands

[List of commands that come with the DB package.](docs/db-commands)