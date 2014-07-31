#ORM

[![Build Status](https://travis-ci.org/asgardphp/orm.svg?branch=master)](https://travis-ci.org/asgardphp/orm)

The ORM package gives you the possibility to store, fetch, search entities and define their relations to each other.

##Entity Relations

To define relations between entities, please read [ORM Relations](docs/orm-relations).

	<?php
	namespace Blog\Entities\Post;

	class Post extends \Asgard\Entity\Entity {
		public static function definition(\Asgard\Entity\EntityDefinition $definition) {
			$definition->properties = [
				'title', 'content'
			];

			$definition->relations = [
				'tags' => [
					'entity' => 'Blog\Entities\Tag',
					'has'    => 'many'
				],
			];

			$definition->behaviors = [
				new \Asgard\Orm\ORMBehavior
			];
		}
	}

##Persistence

To persist and fetch entities, there are two options:

[Data Mapper](docs/datamapper)

	$dataMapper->save($entity);

[ORMBehavior (ActiveRecord pattern)](docs/ormbehavior)

	$entity->save();

##ORM

The ORM class helps you construct queries to manipulate your stored entities. [See the documentation.](docs/orm-orm)

	$entities = $orm->where('position > ?', 5)->orderBy('title ASC')->get();

##Commands

[List of commands that come with the ORM package.](docs/orm-commands)