#Behaviors

[![Build Status](https://travis-ci.org/asgardphp/behaviors.svg?branch=master)](https://travis-ci.org/asgardphp/behaviors)

The Behaviors package provides some behaviors ready to use out-of-the-box for your entities.

- [Installation](#installation)
- [MetasBehavior](#metas)
- [PublishBehavior](#publish)
- [SlugifyBehavior](#slugify)
- [SortableBehavior](#sortable)
- [TimestampsBehavior](#timestamps)

<a name="installation"></a>
##Installation
**If you are using the Asgard Framework you don't need to install this library as it part of the default libraries that Asgard uses.**

In your composer file:

    "require": {
        "asgard/behaviors": "0.*"
	}

<a name="metas"></a>
##MetasBehavior

###Usage

In your entity definition:

	$definition->behaviors = [
		new \Asgard\Behaviors\MetasBehavior
	];

###Properties

	meta_title:
		type: text
		required: false
	meta_description:
		type: text
		required: false
	meta_keywords:
		type: text
		required: false

###Methods

	$entity->showMetas();

outputs the entity metas in the html

<a name="publish"></a>
##PublishBehavior

###Usage

In your entity definition:

	$definition->behaviors = [
		new \Asgard\Behaviors\PublishBehavior
	];

###Properties

	published:
		type: boolean
		default: true

###Methods

	Entity::published();

returns an orm with a published entities scope.

	Entity::loadPublished($id);

returns an entity from a given id only if the entity is published.

<a name="slugify"></a>
##SlugifyBehavior

###Usage

In your entity definition:

	$definition->behaviors = [
		new \Asgard\Behaviors\SlugifyBehavior
	];

This will use the first entity property as a slug.

To use a speficic property as slug:

	$definition->behaviors = [
		new \Asgard\Behaviors\SlugifyBehavior('title')
	];

###Properties

	slug:
		type: text
		required: false

###Methods

	$entity->slug();

returns the entity slug

<a name="sortable"></a>
##SortableBehavior

###Usage

In your entity definition:

	$definition->behaviors = [
		new \Asgard\Behaviors\SortableBehavior
	];

###Properties

	position:
		type: integer
		required: false
		default: 0
		editable: false

###Hooks

If the position is not set, when saving the entity, the position will be incremented from the last entity.

###Methods

	$entity->moveAfter($anotherEntityId);

moves the entity after another entity with a given id

	$entity->previous();

returns the entity before the current one.

	$entity->next();

returns the entity after the current one.

<a name="timestamps"></a>
##TimestampsBehavior

###Usage

In your entity definition:

	$definition->behaviors = [
		new \Asgard\Behaviors\TimestampsBehavior
	];

###Properties

	created_at:
		type: datetime
		required: false
		default: function() { return \Carbon\Carbon::now(); }
		editable: false
	updated_at:
		type: datetime
		required: false
		default: function() { return \Carbon\Carbon::now(); }
		editable: false

###Hooks

Upon saving an entity, the updated_at property will be updated with the current date and time.