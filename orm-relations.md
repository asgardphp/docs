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
	namespace Blog\Entity\Post;

	class Post extends \Asgard\Entity\Entity {
		public static function definition(\Asgard\Entity\Definition $definition) {
			$definition->properties = [
				'title',
				'content',
				'tags' => [
					'type' => 'entity',
					'entity' => 'Blog\Entity\Tag',
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

	#Blog\Entity\Member
	$definition->properties = [
		'position' => [
			'type' => 'entity',
			'entity' => 'Blog\Entity\Position',
		]
	];

	#Blog\Entity\Position
	$definition->properties = [
		'member' => [
			'type' => 'entity',
			'entity' => 'Blog\Entity\Member',
		]
	];

<a name="hasmany"></a>
##hasMany: many <-> one

	#Blog\Entity\Post
	$definition->properties = [
		'comments' => [
			'type' => 'entity',
			'entity' => 'Blog\Entity\Comment',
		]
	];

	#Blog\Entity\Comment
	$definition->properties = [
		'post' => [
			'type' => 'entity',
			'entity' => 'Blog\Entity\Post',
		]
	];

<a name="belongsto"></a>
##belongsTo: one <-> many

	#Blog\Entity\Post
	$definition->properties = [
		'category' => [
			'type' => 'entity',
			'entity' => 'Blog\Entity\Category',
		]
	];

	#Blog\Entity\Category
	$definition->properties = [
		'posts' => [
			'type' => 'entity',
			'entity' => 'Blog\Entity\Post',
			'many'    => true
		]
	];

<a name="hmabt"></a>
##HMABT (HasManyAndBelongsTo): one <-> one

	#Blog\Entity\Post
	$definition->properties = [
		'tags' => [
			'type' => 'entity',
			'entity' => 'Blog\Entity\Tag',
			'many'    => true
		]
	];

	#Blog\Entity\Tag
	$definition->properties = [
		'posts' => [
			'type' => 'entity',
			'entity' => 'Blog\Entity\Post',
			'many'    => true
		]
	];
