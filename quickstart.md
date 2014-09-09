#Quickstart

- [Introduction](#Introduction)
- [Requirements](#Requirements)
- [Installation](#installation)
- [Installing modules](#modules)
- [Building a bundle](#bundle)
- [Search form](#search)
- [Reviews](#reviews)
- [Testing](#testing)
- [Conclusion](#conclusion)

<a name="introduction"></a>
##Introduction
Here is a 1 hour tutorial that will give you a taste of Asgard and discover how its functionalities can help you improve your workflow. In that short amount of time we will go through all the necessary steps to build a small website with manageable news and products.

However this tutorial is only meant to introduce you to Asgard, and as such it does not intend to provide detailed documentation. For more information please refer to the documentation.

If you encounter any trouble, [please let us know](https://github.com/asgardphp/docs/issues).

<a name="requirements"></a>
##Requirements
* Web server, as Apache for example.
* MySQL
* PHP 5.4+
* [Composer](https://getcomposer.org/download/)
* [Git](http://git-scm.com/downloads)
* PHPUnit (last section only)

<a name="installation"></a>
##Installation
Open the console, go to your web directory, and run:

	composer create-project asgard/starter asgard

This will create a new project in the directory "asgard", retrieve all its dependencies and initialize the configuration file all by itself.

Now we need to initialize the database. This is done with the following command:

	php console db:init

This will prompt you for the configuration of the database, and if possible, create it if it does not exist yet.

And that's it! You have installed and configured your new project.
If you open [http://localhost/asgard/web/](http://localhost/asgard/web/) in your browser, you should see the Asgard default homepage.

<a name="modules"></a>
##Installing modules
One of the nice things that comes with Asgard, is the ability to build your application with existing modules with a single command.

Here we are not using composer because we will not use these modules as dependencies but as actual parts of our application that we want to customize to our taste.

To install the news module, run:

```text
php console install https://github.com/asgardmodules/news.git=v0.1.0 --update-composer --migrate
```

Along with the news, this will install some dependencies such as the admin module and a ckeditor wysiwyg.

Because of the --update-composer and --migrate options, the command will automatically update composer dependencies and run the migrations. Hence, the tables have been automatically created for your new modules. If you didn't want to update composer and execute the migrations automatically, you could simply remove these options from the command line.

Besides composer and migrations, the installation also published the web assets, configuration files and even tests in the appropriate folders.

Now open [http://localhost/asgard/web/admin/](http://localhost/asgard/web/admin) and login using admin as both username and password.

As you can see, the administration is already functional. Click on "News" in the menu and let's add a few elements (click on "Add").

Now that you are done with the administration, open [http://localhost/asgard/web/news/](http://localhost/asgard/web/news/). This is the default index for our news module and it displays the elements you have just created.

###Files structure

Open your project folder. The structure should be like:

	app/
		app/Admin/
		app/General/
		app/News/
		app/Wysiwyg/
		..
	config/
	translations/
	migrations/
	tests/
	vendor/
	web/
	...

For now let's just look at app/, which contains your application. Your application is made of bundles, that you can either install automatically (as we just did) or create yourself.

If you dive in app/Admin/ you will see that a typical bundle is made of:

* Controllers/ (the logic, handles the requests)
* Entities/ (data entities)
* translations/ (translations)
* html/ (contains your html templates)
* Bundle.php (initialization functions)

If you now open Controllers/NewsController.php, you can see how the requests are handled and notice how the annotated routes work. But for now let's only focus on the views and open html/news/index.php

The default view is pretty simple and a bit boring.. but we can improve it:

	<?php foreach($list as $news): ?>
	<h2><?=$news?></h2>
	<p><?=substr($news->content, 100).'...'?></p>
	<a href="<?=$news->url()?>">Read more</a>
	<hr>
	<?php endforeach; ?>

Refresh, and voilà! By the way, have you noticed how we can easily print the news page url? This function is defined in Entities/News.php.

<a name="bundle"></a>
##Building a bundle
Good job! But installing an existing module was a bit too easy and we won't always have an existing module for everything. So we are now going to build our own bundle from scratch. This bundle will contains the code for our products catalog. Products will belongs to categories and have their own preview image as well as some information such as the price and name.

To make things faster, Asgard provides a tool that will generate most of the code for us, based on our instructions which come in the form of a YAML file. For example, for our new products catalog, we will use:

	Catalog:
	  Entities:
	    Product:
	      properties:
	        name:
	        price: integer
	        image:
	          type: image
	          web: true
	      relations:
	        category:
	          entity: Catalog\Entities\Category
	          has: one
	      behaviors:
	        \Asgard\Behaviors\SortableBehavior:
	      admin: true
	      front: true
	    Category:
	      meta:
	        plural: categories
	      properties:
	        name:
	      relations:
	        products:
	          entity: Catalog\Entities\Product
	          has: many
	      admin: true
	  tests: true

So copy it, save it as a YAML file in your project directory, and run:

	php console generate instructions.yml

That's it, we have a products catalog with a back-office on our website. I guess you have never built a products catalog that fast before! There is a lot of things happening under the hood though.

Here we asked the generator to create a new bundle "Catalog" with two entities: Product and Category. First we define the Product entity. Its properties, its relation with Category, and we make it sortable by adding a behavior.

Then, by saying "admin: true", we tell the generator that we want an admin interface for our new entity. "front: true" on the other hand, tells the generator to build the front controller.

Same thing with Category excepts we don't generate the front controller.

The last line, "tests: true", tells the generator to generate the functional tests but we will get back to it in the last section.

**But what about the database?** Well, Asgard is here to help again.

We have the entities Product and Category. By running the following command, Asgard will analyze these entities, generate a migration file for them, and execute it.

	php console orm:automigrate

But had you preferred not to execute it automatically, the following command would only generate the migration, leaving up to you the possibility to modify it (in migrations/):

	php console orm:generate

Anyway, let's play with our new catalog and add some products in [http://localhost/asgard/web/admin/products](http://localhost/asgard/web/admin/products). You can also manage categories by clicking on "Categories" in the "Content" sub-menu.

Now that you have filled up your catalog, let's have a look at the front-office: [http://localhost/asgard/web/products](http://localhost/asgard/web/products)
See? All your newly created products with their own individual page.

Everything can be modified directly in app/Catalog/Controllers/ProductController.php and app/Catalog/html/.

###Debugging under Asgard
As you already have made modifications to the code, you might have already come across the debugging screen. If not, add some invalid code to app/Catalog/html/index.php and see what happens when you refresh your products page.

Besides the error message, you get the full backtrace with the code excerpts and arguments. To access specific files and lines of code in a single click, open config/config.yml and replace the "debug_url" value. You will also have to register the protocol handler ([how to on windows](http://msdn.microsoft.com/en-us/library/ie/aa767914(v=vs.85).aspx)).

<a name="search"></a>
##Search form
We now want to be able to let our visitors search through our catalog. For that, we will have to modify our controller, so open Catalog/Controllers/ProductController.php

So far the controller only has 2 actions. An action handles specific requests. indexAction handles requests going to "products/index" while showAction handles requests to "products/:id", :id being the identifier of the product.

For our search form we will add the following action:

	/**
	 * @Route("search")
	 */
	public function searchAction($request) {
		$this->form = $this->$container->make('form', ['search']);
		$this->form['term'] = new \Asgard\Form\Fields\TextField();
		$this->form['min'] = new \Asgard\Form\Fields\TextField();
		$this->form['max'] = new \Asgard\Form\Fields\TextField();
		$choices = [];
		foreach(\Catalog\Entities\Category::all() as $category)
			$choices[$category->id] = (string)$category;
		$this->form['category'] = new \Asgard\Form\Fields\SelectField(['choices' => $choices]);

		$orm = \Catalog\Entities\Product::orm();
		if($this->form->sent()) {
			$term = $this->form['term']->value();
			$min = $this->form['min']->value();
			$max = $this->form['max']->value();
			$category = $this->form['category']->value();
			if($term !== null)
				$orm->where(['name LIKE ?'=>"%$term%"]);
			if($min !== null)
				$orm->where(['price >= ?'=>$min]);
			if($max !== null)
				$orm->where(['price <= ?'=>$max]);
			if($category !== null)
				$orm->where('category_id', $category);
			$this->products = $orm->get();
		}
		else
			$this->products = $orm->get();
	}

As you may have guessed already, the url is products/search. However a bit of explanation is necessary to understand how the action works.

First, we create the form with 4 fields. One for the search term, one for the minimum price and another for the maximum price, and one for the category. The last one is filled with all existing categories.

Then if the form was posted, we perform the search, otherwise we select all products. To retrieve the product that meet the criteria, we use the ORM. With the ORM we search for products whose name contains the term, whose price is greater than min and less than max, and whose category corresponds to the one selected.

That's it for the action so let's now move to the view in Catalog/html/product/search.php:

	<?=$form->open()?>
	<?=$form['term']->label()?>: <?=$form['term']->def()?><br>
	<?=$form['min']->label()?>: <?=$form['min']->def()?><br>
	<?=$form['max']->label()?>: <?=$form['max']->def()?><br>
	<?=$form['category']->label() ?>: <?=$form['category']->def()?><br>
	<input type="submit" value="Search">
	<?=$form->close()?>

	<hr>

	<?php foreach($products as $product): ?>
	<p>
		<b><?=$product?></b><br>
		Price: <?=$product->price?><br>
		Category: <?=$product->category?>
	</p>
	<?php endforeach ?>

First we display the form we have constructed in the action, then we display with a foreach loop all the selected products. Open [http://localhost/asgard/web/procucts/search](http://localhost/asgard/web/procucts/search) and give it a try.

<a name="reviews"></a>
##Products reviews
The final addition to our website will be a review system for our products. For that we will modify the action showAction to make a form to add reviews, and display them.

But before, let's create the Review entity in Catalog/Entites/Review.php

	<?php
	namespace Catalog\Entities;

	class Review extends \Asgard\Entity\Entity {
		public static function definition(\Asgard\Entity\EntityDefinition $definition) {
			$definition->properties = [
				'name',
				'comment',
				'rating' => 'integer',
			];

			$definition->relations = [
				'product' => [
					'has' => 'one',
					'entity' => 'Catalog\Entities\Product'
				]
			];
		}
	}

and mofidy Catalog/Entites/Product.php's relations:

	$definition->relations = [
		'reviews' => [
			'has' => 'many',
			'entity' => 'Catalog\Entities\Review'
		]
	];

And finally, migrate the database:

	php console orm:automigrate

Then let's go back to our ProductController, to modify the showAction:

	/**
	 * @Route(":id")
	 */
	public function showAction(\Asgard\Http\Request $request) {
		if(!($this->product = \Catalog\Entities\Product::load($request['id'])))
			$this->notfound();

		$review = new \Catalog\Entities\Review(['product_id'=>$this->product->id]);
		$this->form = $this->$container->make('entityForm', [$review]);
		if($this->form->sent()) {
			try {
				$this->form->save();
				$this->form->reset();
				$this->getFlash()->addSuccess('Your review was posted with success. Thank you.');
			} catch(\Asgard\Form\FormException $e) {
				$this->getFlash()->addError($e->errors);
			}
		}
	}

Here we create an EntityForm for the entity Review. The EntityForm will create the appropriate fields for the entity properties and save the entity if the input is valid.

The last thing to edit before we can start adding reviews, is the view at Catalog/html/product/show.php:

	<?php $this->getFlash()->showAll() ?>

	<h1><?=$product?></h1>

	<h2>Reviews</h2>
	<ul id="reviews">
	<?php foreach($product->reviews as $review): ?>
	<li>
	<b><?=$review->name?></b><br>
	Rating: <?=$review->rating?><br>
	<?=$review->comment?>
	</li>
	<?php endforeach ?>
	</ul>

	<h2>Add a review</h2>
	<?=$form->open()?>
	<?=$form['name']->label()?>: <?=$form['name']->def()?><br>
	<?=$form['comment']->label()?>: <?=$form['comment']->def()?><br>
	<?=$form['rating']->label()?>: <?=$form['rating']->def()?><br>
	<input type="submit" value="Submit">
	<?=$form->close()?>

Congrats! If you followed all the steps accordingly you should now be able to add reviews to your products.

Our website now has manageable news, products and categories, and a review system.

<a name="testing"></a>
##Testing
We are now done with the application code. This last section covers the testing of our application.

To execute the tests, run the following command at your project root:

	phpunit

This executes the tests that are already in tests/. You might wonder which tests as we haven't made any yet.

Actually, when we installed the news and the admin modules, their tests were automatically added to the tests folder.

And when we generated the catalog bundle, the tests were partially automatically generated in tests/Catalog.php

If you open this file you will notice that some tests are commented out. This is because their urls contains dynamic parameters that cannot be guessed so you will have to complete them manually.

For the other pages, such as the search page, Asgard can help you generate tests. Run:

	php console generate-tests AppTest.php

This created the file tests/AppTest.php with the newly generated tests. Within this file you should find a test for our search page:

	$browser = $this->getBrowser();
	$this->assertTrue($browser->get('products/search')->isOK(), 'GET products/search');

But to make it more useful, let's replace it with:

	#Fixtures
	$cat1 = \Catalog\Entities\Category::create(['id'=>4, 'name'=>'BarCat']);
	$cat2 = \Catalog\Entities\Category::create(['id'=>5, 'name'=>'BarCat']);
	\Catalog\Entities\Product::create(['name'=>'foo', 'price'=>15, 'category'=>4]); #in
	\Catalog\Entities\Product::create(['name'=>'foooooo', 'price'=>5, 'category'=>4]); #not in
	\Catalog\Entities\Product::create(['name'=>'foobar', 'price'=>17, 'category'=>4]); #in
	\Catalog\Entities\Product::create(['name'=>'bar', 'price'=>13, 'category'=>4]); #not in
	\Catalog\Entities\Product::create(['name'=>'foofoo', 'price'=>13, 'category'=>5]); #not in

	#Browser
	$browser = $this->getBrowser();
	$browser->get('products/search');
	$formParser = new \Asgard\Http\Browser\FormParser();
	$formParser->parse($browser->getLast()->getContent(), '//form');
	$formParser->get('search[term]')->setValue('foo');
	$formParser->get('search[min]')->setValue(10);
	$formParser->get('search[max]')->setValue(20);
	$formParser->get('search[category]')->setValue(4);
	$this->assertTrue($browser->post('products/search', $formParser->values())->isOK(), 'POST products/search');
	$this->assertEquals(2, substr_count($browser->getLast()->getContent(), '<p>'));

Here we initialize some fixtures, open the search page, fill in the form, trigger the search, and verify that our products are showed.

Now every time you will execute the tests, you will know whether your search form is still functional.

<a name="conclusion"></a>
##Conclusion
Alright, we are done! I really hope you enjoyed this little journey with Asgard.

Again, if you came across any difficulty or you just want to share a word, [contact us](about), join the [community](community) or [report an issue](https://github.com/asgardphp/docs/issues).