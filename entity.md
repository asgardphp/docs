#Entities

[![Build Status](https://travis-ci.org/asgardphp/entity.svg?branch=master)](https://travis-ci.org/asgardphp/entity)

Entities are elements that represent your data. Some frameworks call them models, but they are similar, although entities don't deal themselves directly with the database. They should always be stored in the Entities folder of a bundle.

- [Instance](#instance)
- [Definition](#definition)
- [Multiple values per property](#multiple)
- [Behaviors](#behaviors)
- [Validation](#validation)
- [I18N](#i18n)
- [Utils](#utils)

<a name="instance"></a>
##Instance

An entity is instantiated like:

	$article = new Article();

You can also pass default values:

	$article = new Article(array('title'=>'hello!'));

<a name="definition"></a>
##Definition

An entity is defined by a class:

	<?php
	class Article extends \Asgard\Entity\Entity {
		public static function definition($definition) {
			$definition->properties = array(
				'title' => array(
					'required'=>true
				),
				'content' => 'longtext',
				'posted_on' => 'date'
			)
		}
	}

Here we have defined the entity Article, with 3 properties: title, content and posted_on. title has the default type "text" (text with less than 255 characters), content has the type "longtext", while posted_on is a date.
There are actually 4 different ways to define a property:

	$definition->properties = array(
		'title'
	)
	#or
	$definition->properties = array(
		'title' => 'text'
	)
	#or
	$definition->properties = array(
		'title' => array(
			'type' => 'text',
			...
		)
	)
	#or
	$definition->properties = array(
		'title' => new \Asgard\Entity\Properties\TextProperty(array(..))
	)

The type is optionnal and defaults to "text". At the moment you can use the types: "boolean", "date", "datetime", "double", "email", "file", "image", "integer", "longtext" and "text".

Each type affects the property in the way the data are stored in the entity, validated or even persisted in the database.

You can access properties values with:

	$article->title
	#or
	$article->get('title')

and set:

	$article->title = 'hello';
	$article->set('title', 'hello')

or set multiple values at once:

	$article->set(array('title'=>'hello', 'content'=>'everyone'))

<a name="multiple"></a>
##Multiple values per property
A property can even have multiple values.
For that, add the multiple paramater like so:

	$definition->properties = array(
		'title' => array(
			'multiple' => true
		)
	)

Now, $article->title will return a Asgard\Entity\Multiple object which can be used like an array:

	$article->title[] = 'new title';
	$article->title[0] //new title

<a name="validation"></a>
##Validation
Properties can have all kinds of parameters, including validation rules. For example:

	$definition->properties = array(
		'title' => array(
			'validation' => array(
				'lessThan' => 10,
				'greaterThan' => 5
			)
		)
	)

To validate your entity use:

	$article->valid() #returns true of valid, otherwise false
	$article->errors() #returns an array of errors

For more information on validation, refer to the validation section.

<a name="behaviors"></a>
##Behaviors
Entities can be enhanced by behaviors. Behaviors can add methods and properties, and modify their current behaviors with Hooks.
For example, to make entities sortable, add the following code to the definition method:

	$definition->behaviors = array(
		new \Asgard\Behaviors\SortableBehavior()
	);

This adds a property "position" for the Article entity, and two methods: moveAfter($entity) and moveBefore($entity).

You can also pass parameters to behaviors:

	$definition->behaviors = array(
		new \Asgard\Behaviors\SortableBehavior('category_id')
	);

If you want to sort articles only between the ones that have the same category_id.

Please note that this behavior will only work properly if used with the ORMBehavior.

<a name="i18n"></a>
##I18N
Entities handle internationalisation by default. Simply add the parameter i18n to a property:

	$definition->properties = array(
		'title' => array(
			'i18n' => true
		)
	)

From now on, title will have a different version for all the languages that are in the configuration file.

To get the default language:

	$article->title

To get the value in a specific language:

	$article->get('title', 'fr')

To get the values in all the available languages:

	$article->get('title', 'all')

And to set values:

	$article->title = 'hello'
	$article->set('title', 'bonjour', 'fr')
	$article->set(array('title'=>'bonjour', 'content'=>'tout le monde'), 'fr')

To know if an entity has i18n properties, use:

	Article::isI18N()

To change an entity instance's default language:

	$entity->setLocale('fr')

<a name="utils"></a>
##Utils
Convert an entity to an array with raw values (this may include objects):

	$entity->toArrayRaw()

Convert an entity to an array with only nested arrays and strings:

	$entity->toArray()

Convert an entity to JSON:

	$entity->toJSON()

Convert a group of entities to JSON:

	Article::arrayToJSON(array($article1, $article2))