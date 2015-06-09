#ORM

The ORM class lets you fetch, update and delete entities en masse.

- [Instantiate an entity ORM](#instantiate)
- [Building a query](#query)
- [Fetch results](#fetch)
- [DAL](#dal)
- [Update](#update)
- [Delete](#delete)

<a name="instantiate"></a>
##Instantiate an entity ORM

	$orm = $dataMapper->orm('Vendor\EntityClass');
	#or
	\Vendor\EntityClass::orm(); #only when the entity has the orm behavior

[See the datamapper documentation to create a DataMapper object](docs/orm-datamapper).

<a name="query"></a>
##Building a query

*Note: all SQL keywords must be uppercase and identifiers (columns & tables) must start with a lowercase character.* 

###Conditions (WHERE)

	$orm->where('foo', 2);
	#or
	$orm->where(['foo' => 2]); #same as above
	#or
	$orm->where(['foo=?' => 2]); #same as above
	#or
	$orm->where(['foo!=?' => 2]);
	#or
	$orm->where('foo IS NULL');

OR:

	$orm->where([
		'or' => [
			'foo' => 2,
			'foo' => 3,
		]
	]);

AND:

	$orm->where([
		'and' => [
			'foo' => 2,
			'bar' => 3,
		]
	]);

The "and" is optional here and can be reduced to:

	$orm->where([
		'foo' => 2,
		'bar' => 3,
	]);

Nested:

	$orm->where([
		'or' => [
			'foo > ?' => 2,
			'and' => [
				'bar' => 3,
				'zoo' => 4,
			]
		]
	]);

**Sub-queries**

	$orm->where('score > ?', new \Asgard\Db\Raw('(SELECT sum(grade) FROM foo)'));

This will prevent the ORM from modifying your sub-query.

###Order, limit and offset
***Order***

	$orm->orderBy('date DESC, updated DESC');

Reverse order:

	$orm->reverse();

***Offset***

	$orm->offset(10);

***Limit***

	$orm->limit(10);

###Jointures

**Simple jointure**

	$orm->join('parent');

**Cascade jointures**

	$orm->join(['article' => ['category' => 'parentcategory']]);

This will create a LEFT jointure with the related entities "article", with the "category" relation from "article", and then from "category" to "parentcategory". The final SQL query would thus be similar to:

	SELECT * FROM author
	LEFT JOIN article  ON article.author_id=author.id
	LEFT JOIN category ON article.category_id=category.id
	LEFT JOIN category_parentcategory cp ON cp.category_id=category.id
	LEFT JOIN parentcategory p ON p.id=cp.parentcategory_id

**Aliases**

You can use aliases like this:

	$orm->join(['article' => ['category cat' => 'parentcategory']]);

leading to the following SQL query:

	SELECT * FROM author
	LEFT JOIN article  ON article.author_id=author.id
	LEFT JOIN category cat ON article.category_id=cat.id
	LEFT JOIN category_parentcategory cp ON cp.category_id=cat.id
	LEFT JOIN parentcategory p ON p.id=cp.parentcategory_id

If two tables use the same name or alias, the second one will be incremented starting from 1. For example category1, category2, etc.

You can use this name/alias in the WHERE conditions and other fields. For example:

	$orm->join('article a')->where('a.posted > ?', '2015-01-01')

*Note that this won't eagerload related entities. For this, use "with". Jointures are only used to create conditions, modify the order, etc.*

###Selects

Useful for HAVING conditions or to fill entities with additional data.

	$orm->addSelect('COUNT(f.id) as total');

###Having

	$orm->having('total > 10');

###Access through relations

You can access entities directly through their relations:

	$categoriesOrm->articles()->comments()

This will fetch the comments of the articles which belong to the selected categories. You can even combine this with conditions:

	$categoriesOrm->where('status', 1)->articles()->where('posted > ?', '2015-01-01')->comments()->orderBy('id DESC')

The corresponding SQL query will be similar to:

	SELECT * FROM comment c
	INNER JOIN article a ON c.article_id=a.id
	INNER JOIN category c ON a.category_id=c.id
	WHERE
		a.posted > '2015-01-01'
		AND c.status = 1
	ORDER BY c.id DESC

###Join to entity

While building a query for articles, you can tell the ORM to fetch only the articles belonging to a specific category entity:

	$articlesOrm->joinToEntity('parent', $category);

The 1st argument is the name of the relation.

This is the same as:

	$category->articles();

You can even use for multiple relations/entities:

	$articlesOrm
		->joinToEntity('parent', $category)
		->joinToEntity('tag', $tag);

Hence, selecting articles that is related to $category AND $tag.

###Scopes

**Adding a scope**

Scopes are used to modify the query before it is being processed.

For example:

	$orm->addScope(new \Asgard\Orm\Scope(function($orm) {
		$orm->where('published', 1);
	}));

Scopes can be added to the entity definition:

	$definition->set('orm.scopes.published', new \Asgard\Orm\Scope(function($orm) {
		$orm->where('published', 1);
	}));

All ORMs derived from this entity, will now only fetch published entities.

**Removing a scope**

	$orm->removeScope('published');

**Reset all scopes**

	$orm->resetScopes();

###With

To eager load a relationship:

	$orm->with('childs');

This will select all entities from the "childs" relation along with the original entities. To add constraints, use a callback:

	$orm->with('childs', function($childsOrm) {
		$childsOrm->where('category_id', 5);
	});

Nested:

	$orm->with('childs', function($childsOrm) {
		$childsOrm->with('articles', function($articlesOrm) {
			$articlesOrm->where('category_id', 5);
		});
	});

###DAL Callbacks

Before running a query, the ORM will construct a DAL object based on the inputs from the previous methods. If you want to customize this DAL object before running the query, you can add callbacks:

	$orm->dalCallback(function($dal) {
		$dal->addSelect('f.name');
		$dal->leftjoin('foo f', 'f.id=p.f_id');
	});

<a name="fetch"></a>
##Fetch results

**Get**

	$orm->get();

Returns an array with all selected entities. Don't use it if you expect many entities in return as it could quickly run the application out of memory.

**Pagination**

	$orm->paginate($page=1, $per_page=10);

The ORM will only return the entities of the page.

**First**

	$orm->first();

**Last**

	$orm->last();

**Foreach loop**

	foreach($orm as $entity)
		//...

**While loop**

	while($entity = $orm->next())
		//...

**Fetch a single property for all entities**

	$orm->values('title'); #e.g. ['Foo', 'Bar', 'Boo']

**Fetch all ids**

	$orm->ids(); #e.g. [4, 5, 6]

###Agregation

**Count**

	$orm->count(); #e.g. 10

Group by:

	$orm->count('category_id'); #e.g. ['1'=>5, '2'=>7]

**Sum**

	$orm->sum('score'); #e.g. 10

Group by:

	$orm->sum('score', 'category_id'); #e.g. ['1'=>5, '2'=>7]

**Min**

	$orm->min('score'); #e.g. 10

Group by:

	$orm->min('score', 'category_id'); #e.g. ['1'=>5, '2'=>7]

**Max**

	$orm->max('score'); #e.g. 10

Group by:

	$orm->max('score', 'category_id'); #e.g. ['1'=>5, '2'=>7]

**Avg**

	$orm->avg('score'); #e.g. 10

Group by:

	$orm->avg('score', 'category_id'); #e.g. ['1'=>5, '2'=>7]

<a name="dal"></a>
##DAL

	$dal = $orm->getDAL();

This will construct a DAL object from the ORM, which can then use as a direct access to the database. [See documentation](docs/db-dal).

For example if you need to debug the query:

	echo $orm->getDAL()->dbgSelect();

<a name="update"></a>
##Update

	$orm->update(['score'=>5]);

You can of course combine it with conditions:

	$orm->where('score > ?', 5)->update(['score'=>5]);

<a name="delete"></a>
##Delete

	$orm->delete();

You can of course combine it with conditions:

	$orm->where('score > ?', 5)->delete();

