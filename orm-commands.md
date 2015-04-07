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
			$this->schema->create($table, function($table) {	
				$table->addColumn('id', 'integer', [
					'length' => 11,
					'autoincrement' => true,
				]);
				$table->addColumn('created_at', 'datetime', [
				]);
				$table->addColumn('updated_at', 'datetime', [
				]);
				$table->addColumn('key', 'string', [
					'length' => '255',
				]);
				$table->addColumn('value', 'text', [
					'length' => '65535',
				]);

				$table->setPrimaryKey(['id']);
			});
		}
		
		public function down() {
			$this->schema->drop($this->container['config']['database.prefix'].'data');
		}
	}

To migrate it:

	php console migrations:migrate [migration_name]

<a name="automigrate"></a>
##Automigrate

Automatically generate and migrate a new database migration from your entities.

Usage:

	php console orm:automigrate [migration_name]