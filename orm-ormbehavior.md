#ORMBehavior

The ORMBehavior allows you to use your entities as in the ActiveRecord pattern.

- [Adding ORMBehavior to an entity](#adding)
- [Behavior](#behavior)
- [Static Methods](#static)
- [Methods](#methods)
- [Relations](#relations)
- [ORM](#orm)

<a name="adding"></a>
##Adding ORMBehavior to an entity

In the entity definition method

	public static function definition(\Asgard\Entity\Definition $definition) {
		$definition->behaviors = [
			new \Asgard\Orm\ORMBehavior
		];
	}

<a name="behavior"></a>
##Behavior

* The entity gets another property: id, which is unique to all entities of the same type.

* When an entity has the ORMBehavior, the relations are validated along with the entity properties.

* And it adds additional methods as shown below.

<a name="static"></a>
##Static Methods

Load by a specific property

	$post = Blog\Entity\Post::loadBy('title', 'hello!');

Load an entity with a given id

	$post = Blog\Entity\Post::load(4);

Get the ORM for specific queries

	$orm = Blog\Entity\Post::orm();

Destroy all entities

	Blog\Entity\Post::destroyAll();

Destroy an entity

	Blog\Entity\Post::destroy($post);

Create an entity

	$post = Blog\Entity\Post::create(['title'=>'hello!', $validationGroups=[]]);

Validation groups ($validationGroups) define the set of rules to apply to the validation. [See the validation documenatation.](docs/validation)

<a name="methods"></a>
##Methods

Save an entity

	$post->save(['title'=>'new title!'], $validationGroups=[]);

Destroy an entity

	$post->destroy();

Check if an entity has never been persisted

	$post->isNew();

Or opposite

	$post->isOld();

<a name="relations"></a>
##Relations

To get an entity's relation ORM

	$tagsOrm = $post->tags();

To get an entity associated entities

	$tags = $post->tags;

<a name="orm"></a>
##ORM

Instead of passing by the orm like

	$post = Blog\Entity\Post::orm()->where('id', 1)->first();

You can skip orm() and directly make static calls with the entity class

	$post = Blog\Entity\Post::where('id', 1)->first();