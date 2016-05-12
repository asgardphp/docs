#Generator

The generator helps to build applications very very quickly with the Asgard Framework.

 - [Usage](#usage)
 - [Instructions](#instructions)
 - [Full Example](#example)

<a name="usage"></a>
##Usage

Put the instructions as explained below in a YAML file, and run the generator command:

	php console generate instructions.xml

Options:

	--override-bundles: override existing budles
	--override-files: override existing files (instead of the entire the whole bundles)
	--skip: skip existing bundles

<a name="instructions"></a>
##Instructions

###List of bundles

	Bundle1:
	  #...
	Bundl2:
	  #...

###Bundle entities

	Bundle1:
	  entities:
	    Product:
	      #...
	    Category:
	      #...

###Entity definition

Please refer to the [entity documentation](docs/entity).

	Bundle1:
	  entities:
	    Product:
	      properties:
	        name:
	        price: integer
	        image:
	          type: image
	          web: true
	        category:
	          type: entity
	          entity: Bundle1\Entity\Category
	      behaviors:
	        \Asgard\Behaviors\SortableBehavior:
	    Category:
	      meta:
	        plural: categorys
	      properties:
	        name:
	        products:
	          type: entity
	          entity: Bundle1\Entity\Product
	          many: true

This will generate the following entities, in app/Bundle1/Entity/Product.php:

	<?php
	namespace Bundle1\Entities;

	class Product extends \Asgard\Entity\Entity {
		public static function definition(\Asgard\Entity\Definition $definition) {
			$definition->properties = [
				'name'	=>	[
					'type'	=>	NULL,
				],
				'description'	=>	[
					'type'	=>	'text',
				],
				'prix'	=>	[
					'type'	=>	'integer',
				],
				'image'	=>	[
					'type'	=>	'file',
					'many' => true,
					'web'	=>	true
				],
				'category' => [
					'type' => 'entity',
					'entity' => 'Bundle1\Entity\Category',
				]
			];
		}
		
		public function __toString() {
			return (string)$this->name;
		}

		public function url() {
			return $this->getDefinition()->getContainer()['resolver']->url(['Bundle1\Controller\ProductController', 'show'], array('id'=>$this->id));
		}
	}

And app/Bundle1/Entity/Category.php

	<?php
	namespace Bundle1\Entities;

	class Category extends \Asgard\Entity\Entity {
		public static function definition(\Asgard\Entity\Definition $definition) {
			$definition->properties = [
				'name'	=>	[
					'type'	=>	NULL,
				],
				'products' => [
					'type' => 'entity',
					'entity' => 'Bundle1\Entity\Product',
					'many' => true,
				]
			];
		}
		
		public function __toString() {
			return (string)$this->name;
		}

		public function url() {
			return $this->getDefinition()->getContainer()['resolver']->url(['Bundle1\Controller\CategoryController', 'show'], array('id'=>$this->id));
		}
	}

###Entity front controller

	Bundle1:
	  entities:
	    Product:
	      front: true

is the same as:

	Bundle1:
	  entities:
	    Product:
	      front: [index, show]

You can choose to generate only one action in the front controller:

	Bundle1:
	  entities:
	    Product:
	      front: [index] #or: front: [show]

This will generate the following controller in app/Bundle1/Controller/Product.php:

	<?php
	namespace Bundle1\Controller;

	/**
	 * @Prefix("products")
	 */
	class ProductController extends \Asgard\Http\Controller {
		/**
		 * @Route("")
		 */
		public function indexAction(\Asgard\Http\Request $request) {
			$page = $request->get['page'] ? $request->get['page']:1;
			$orm = \Bundle1\Entity\Product::paginate($page, 10);
			$this->products = $orm->get();
			$this->paginator = $orm->getPaginator();
		}

		/**
		 * @Route(":id")
		 */
		public function showAction(\Asgard\Http\Request $request) {
			if(!($this->product = \Bundle1\Entity\Product::load($request['id'])))
				$this->notfound();
		}
	}

###Controllers

Please refer to the [controllers documentation](docs/http-controllers).

	Bundle1:
	  controllers:
	    Controller1:
	    Controller2:

This will generate the controllers:

- app/Bundle1/Controller/Controller1.php
- app/Bundle1/Controller/Controller2.php

####Controller definitions

	Bundle1:
	  controllers:
	    Member:
	      prefix: members
	      actions:
	        login:
	          route: login
	        register:
	          route: register
	        profile:
	          route: profile/:id
	          template: memberprofile.php
	          viewFile: /path/to/memberprofile.html

The prefix is used as a prefix for all routes.

For each action you can provide the route, the template file and the source file from which to copy the content for the template.

The previous instructions will generate the controller as following:

	<?php
	namespace \Bundle1\Controller;

	/**
	 * @Prefix("members")
	 */
	class MemberController extends \Asgard\Http\Controller {
		/**
		 * @Route("login")
		 */
		public function loginAction(\Asgard\Http\Request $request) {
		}

		/**
		 * @Route("register")
		 */
		public function registerAction(\Asgard\Http\Request $request) {
		}

		/**
		 * @Route("profile/:id")
		 */
		public function profileAction(\Asgard\Http\Request $request) {
			$this->view = 'memberprofile.php';
		}
	}

###Tests
To automatically generate tests in the tests/ folder.

	Bundle1:
	  tests: true
 
<a name="example"></a>
##Full Example

The instructions must be written in a YAML file. For example:

	Bundle1:
	  entities:
	    Product:
	      properties:
	        name:
	        price: integer
	        image:
	          type: image
	          web: true
	        category:
	          type: entity
	          entity: Bundle1\Entity\Category
	      behaviors:
	        \Asgard\Behaviors\SortableBehavior:
	      front: true
	    Category:
	      meta:
	        plural: categorys
	      properties:
	        name:
	        products:
	          type: entity
	          entity: Bundle1\Entity\Product
	          many: true
		controllers:
		  Member:
		    prefix: members
		    actions:
		      login:
		        route: login
		      register:
		        route: register
		      profile:
		        route: profile/:id
		        template: memberprofile.php
		        viewFile: /path/to/memberprofile.html
	  tests: true

Here we are building a single bundle called "Bundle1". In that bundle we are creating two entities "Product" and "Category". The "Product" entity has the following properties: name, price and image, the relation "category" and the bahavior "\Asgard\Behaviors\SortableBehavior". We also tell the generator to build a front controller.

It's pretty similar for the Category entity, except that we need to tell the generator that the plural form of "category" is "categorys" (otherwise it would use "categorys" by default), and we do not generate a front controller for the categorys.

Then we are building a controller called "Member". This will create a controller with three actions: "login", "register" and "profile", with respectively the routes "members/login", "members/register" and "members/profile/:id". By default all actions use the template at html/[controller_name]/[action_name].php, but for the profile action we will be using the template html/member/memberprofile.php and we will it with the content from the file "/path/to/memberprofile.html".

The line "tests: true" tells the generator that we want to generate tests for our bundle in the tests/ folder.