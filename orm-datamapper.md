#Data Mapper

- [Usage in the Asgard Framework](#usage-asgard)
- [Usage outside the Asgard Framework](#usage-outside)
- [Methods](#methods)

<a name="usage"></a>
##Usage in the Asgard Framework

	$dm = $container['datamapper'];
	
The [container](docs/container) is often accessible as a method parameter or through a [ContainerAware](docs/container#containeraware) object. You can also use the [singleton](docs/container#usage-outside) but it is not recommended.

<a name="usage-outside"></a>
##Usage outside the Asgard Framework

	$db = new \Asgard\Db\DB(['host'=>'localhost', 'user'=>'root', 'password'=>'', 'database'=>'asgard']);
	$entityManager = new \Asgard\Entity\EntityManager;
	$dm = new \Asgard\Orm\DataMapper($db, $entityManager, $locale, $prefix, $ormFactory, $collectionOrmFactory);

Only the first argument is required for DataMapper __construct.

<a name="methods"></a>
##Methods

Check if the entity has already been persisted

	$dm->isNew($post)
	$dm->isOld($post)

Load an entity with a given id

	$post = $dm->load('Blog\Entity\Post', 3);

Load all entities

	$posts = $dm->all('Blog\Entity\Post');

Destroy all entities

	$dm->destroyAll('Blog\Entity\Post');

Destroy an entity

	$dm->destroy($post);

Create and persist a new entity

	$post = $dm->create('Blog\Entity\Post', ['title'=>'hello!'], $validationGroups=[]);

Validation groups ($validationGroups) define the set of rules to apply to the validation. [See the validation documenatation.](docs/validation)

Validate entity with relations

	$dm->valid($post, $validationGroups=[]);

Get the errors

	$dm->errors($post, $validationGroups=[]);

Persist an entity

	$dm->save($post['title'=>'new title!'], $validationGroups=[]);

Get the entity ORM for specific queries

	$dm->orm('Blog\Entity\Post');

Get an entity's relation ORM

	$dm->relation($post, 'tags');

To get all tags associated with the entity

	$tags = $dm->relation($post, 'tags')->get();