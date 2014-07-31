#Data Mapper

- [Instance](#instance)
- [Methods](#methods)

<a name="instance"></a>
##Instance

	$db = new \Asgard\Db\DB(['host'=>'localhost', 'user'=>'root', 'password'=>'', 'database'=>'asgard']);
	$locale = 'en';
	$prefix = '';
	$app = new \Asgard\Container\Container;
	$dm = new \Asgard\Orm\DataMapper($db, $locale, $prefix, $app);

Only the first argument is required for DataMapper __construct.

<a name="methods"></a>
##Methods

Check if the entity has already been persisted

	$dm->isNew($post)
	$dm->isOld($post)

Load an entity with a given id

	$post = $dm->load('Blog\Entities\Post', 3);

Load all entities

	$posts = $dm->all('Blog\Entities\Post');

Destroy all entities

	$dm->destroyAll('Blog\Entities\Post');

Destroy an entity

	$dm->destroy($post);

Create and persist a new entity

	$post = $dm->create('Blog\Entities\Post', ['title'=>'hello!'], $force=false); #if force=true, it skips the validation

Validate entity with relations

	$dm->valid($post);

Get the errors

	$dm->errors($post);

Persist an entity

	$dm->save($post['title'=>'new title!'], $force=false); #if force=true, it skips the validation

Get the entity ORM for specific queries

	$dm->orm('Blog\Entities\Post');

Get an entity's relation ORM

	$dm->relation($post, 'tags');

To get all tags associated with the entity

	$tags = $dm->relation($post, 'tags')->get();