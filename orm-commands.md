#ORM Commands

- [Generate](#generate)
- [Automigrate](#automigrate)

<a name="generate"></a>
##Generate

Automatically generate a new database migration from your entities.

Usage:

	php console orm:generate [migration_name]

The migration file (in migrations/) will be like:

	<?php
	class Data extends \Asgard\Migration\DBMigration {
		public function up() {
			$table = $this->container['config']['database.prefix'].'data';
			$this->container['schema']->create($table, function($table) {	
				$table->add('id', 'int(11)')
					->autoincrement()
					->primary();	
				$table->add('created_at', 'datetime')
					->nullable();
				$table->add('updated_at', 'datetime')
					->nullable();
				$table->add('key', 'varchar(255)')
					->nullable();
				$table->add('value', 'text')
					->nullable();
			});
		}
		
		public function down() {
			$this->container['schema']->drop($this->container['config']['database.prefix'].'data');
		}
	}

To migrate it:

	php console migrations:migrate [migration_name]

<a name="automigrate"></a>
##Automigrate

Automatically generate and migrate a new database migration from your entities.

Usage:

	php console orm:automigrate [migration_name]