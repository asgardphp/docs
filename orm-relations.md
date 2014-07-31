#Entities Relations

- [ORM](#definition)
- [hasOne](#hasone)
- [hasMany](#hasmany)
- [belongsTo](#belongsto)
- [HMABT](#hmabt)

<a name="definition"></a>
##Definition

Relations must be defined in the entity definition method. For example:

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

There are four types of relations between entities as described below.

<a name="hasone"></a>
##hasOne: one <-> one

	#Blog\Entities\Member
	$definition->relations = [
		'position' => [
			'entity' => 'Blog\Entities\Position',
			'has'    => 'one'
		]
	];

	#Blog\Entities\Position
	$definition->relations = [
		'member' => [
			'entity' => 'Blog\Entities\Member',
			'has'    => 'one'
		]
	];

<a name="hasmany"></a>
##hasMany: many <-> one

	#Blog\Entities\Post
	$definition->relations = [
		'comments' => [
			'entity' => 'Blog\Entities\Comment',
			'has'    => 'many'
		]
	];

	#Blog\Entities\Comment
	$definition->relations = [
		'post' => [
			'entity' => 'Blog\Entities\Post',
			'has'    => 'one'
		]
	];

<a name="belongsto"></a>
##belongsTo: one <-> many

	#Blog\Entities\Post
	$definition->relations = [
		'category' => [
			'entity' => 'Blog\Entities\Category',
			'has'    => 'one'
		]
	];

	#Blog\Entities\Category
	$definition->relations = [
		'posts' => [
			'entity' => 'Blog\Entities\Post',
			'has'    => 'many'
		]
	];

<a name="hmabt"></a>
##HMABT (HasManyAndBelongsTo): one <-> one

	#Blog\Entities\Post
	$definition->relations = [
		'tags' => [
			'entity' => 'Blog\Entities\Tag',
			'has'    => 'many'
		]
	];

	#Blog\Entities\Tag
	$definition->relations = [
		'posts' => [
			'entity' => 'Blog\Entities\Post',
			'has'    => 'many'
		]
	];
