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
		public static function definition(\Asgard\Entity\Definition $definition) {
			$definition->properties = [
				'title',
				'content',
				'tags' => [
					'type' => 'entity',
					'entity' => 'Blog\Entities\Tag',
					'many'    => true
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
	$definition->properties = [
		'position' => [
			'type' => 'entity',
			'entity' => 'Blog\Entities\Position',
		]
	];

	#Blog\Entities\Position
	$definition->properties = [
		'member' => [
			'type' => 'entity',
			'entity' => 'Blog\Entities\Member',
		]
	];

<a name="hasmany"></a>
##hasMany: many <-> one

	#Blog\Entities\Post
	$definition->properties = [
		'comments' => [
			'type' => 'entity',
			'entity' => 'Blog\Entities\Comment',
		]
	];

	#Blog\Entities\Comment
	$definition->properties = [
		'post' => [
			'type' => 'entity',
			'entity' => 'Blog\Entities\Post',
		]
	];

<a name="belongsto"></a>
##belongsTo: one <-> many

	#Blog\Entities\Post
	$definition->properties = [
		'category' => [
			'type' => 'entity',
			'entity' => 'Blog\Entities\Category',
		]
	];

	#Blog\Entities\Category
	$definition->properties = [
		'posts' => [
			'type' => 'entity',
			'entity' => 'Blog\Entities\Post',
			'many'    => true
		]
	];

<a name="hmabt"></a>
##HMABT (HasManyAndBelongsTo): one <-> one

	#Blog\Entities\Post
	$definition->properties = [
		'tags' => [
			'type' => 'entity',
			'entity' => 'Blog\Entities\Tag',
			'many'    => true
		]
	];

	#Blog\Entities\Tag
	$definition->properties = [
		'posts' => [
			'type' => 'entity',
			'entity' => 'Blog\Entities\Post',
			'many'    => true
		]
	];
