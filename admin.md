#Admin

The admin module helps you create an administration that you can easily extend and customize for your website.

- [Installation](#installation)
- [Files Structure](#files-structure)
- [Usage](#usage)
- [Entity-admin-controller](#entity-admin-controller)
- [Menus](#menus)
- [Generator](#generator)

<a name="installation"></a>
##Installation

Go to your project directory and run:

	php console install git@github.com:asgardmodules/admin.git=v0.1.0 --migrate --update-composer

This will add the files for the administration, migrate your database and update composer dependencies.

<a name="files-structure"></a>
##Files Structure

```text
app/Admin/
	Controllers/
		AdministratorAdminController.php	#To manage administrators
		DefaultAdminController.php			#index and 404 actions
		FilesController.php					#manage entity files
		LoginController.php					#login and password forgotten actions
		PreferencesAdminController.php		#manage preferences
	Entities/
		Administrator.php					#Administrator entity
	generator/*
	Hooks/
		GeneratorHooks.php					#Generator hooks
		NotfoundHooks.php					#404 error hook
	html/
		admin/
			layout.php						#Administration layout
		administratoradmin/
			form.php						#Administrators form
			index.php						#Administrators list
		defaultadmin/
			_404.php						#404 template
			index.php						#index template
		login/
			forgotten.php					#password forgotten template
			login.php						#login template
		preferencesadmin
			form.php						#preferences form template
	Libs/
		Controller/
			AdminParentController.php		#Parent for all administration controllers
			EntityAdminController.php		#Parent for all entity administration controllers
		Form/
			Fields/*						#Form fields for administration
			Widgets/*						#Form widgets for administration
			AdminEntityForm.php				#Entity form for administration
			AdminFormTrait.php				#Administration form features
			AdminSimpleForm.php				#Simple form for administration
			DynamicGroup.php				#DynamicGroup for administration
		AdminAuth.php						#Handle admin authentication
		AdminManager.php					#Associates entities with admin controllers
		AdminMenu.php						#Manage admin menus
	translations/*
	Bundle.php
migrations/Admin.php						#Migrate the administrators table
tests/AdminTest.php							#Tests for the administration
web/*
```

<a name="usage"></a>
##Usage

Go to website/admin/ (replace website with your own address).

By default, you can manage administrators in Configuration > Administrators and preferences in Configuration > Preferences.

<a name="entity-admin-controller"></a>
##Entity Admin Controller

If you already have an entity that you want to be administrable, you can create an admin controller as following:

	<?php
	namespace Article\Controllers;

	/**
	 * @Prefix("admin/articles")
	 */
	class ArticleAdminController extends \Admin\Libs\Controller\EntityAdminController {
		protected $_entity = 'Article\Entities\Article';
		protected $_singular = 'article';
		protected $_plural = 'articles';

		public function __construct() {
			$this->_messages = [
						'modified'        =>	__('Article modified with success.'),
						'created'         =>	__('Article created with success.'),
						'many_deleted'    =>	__('Articles deleted with success.'),
						'deleted'         =>	__('Article deleted with success.'),
					];
			parent::__construct();
		}
		
		public function formConfigure($entity) {
			$form = $this->$container->make('adminEntityForm', [$entity, $this]);
			
			return $form;
		}
	}

This is all you need to be able to list, create, edit and delete your article entities in your administration.

The protected attribute $_entity tells the controller which entity class to manage.

The optional protected attribute $_singular tells the controller how to name entities in singular.

The optional protected attribute $_plural tells the controller how to name entities in plural.

In the method formConfigure you can customize the entity form. For example to add relations and fields, or to remove esome.

To customize the "index" action, override:

	/**
	 * @Route("")
	*/
	public function indexAction(\Asgard\Http\Request $request) {
		//...
	}

To customize the "edit" action, override:

	/**
	 * @Route(":id/edit")
	*/
	public function editAction(\Asgard\Http\Request $request) {
		//...
	}

To customize the "new" action, override:

	/**
	 * @Route("new")
	*/
	public function newAction(\Asgard\Http\Request $request) {
		//...
	}

To customize the "delete" action, override:

	/**
	 * @Route(":id/delete")
	*/
	public function deleteAction(\Asgard\Http\Request $request) {
		//...
	}

<a name="menus"></a>
##Menus

To customize the administration menus, add the following code in a bundle's run method.

**Navigation bar:**

	$container['adminMenu']->add([
		'label' => __('News'),
		'link' => $chain->container['resolver']->url_for(['News\Controllers\NewsAdminController', 'index']),
	], '0.');

The last parameter gives the position of the item. This item will be in the first sub-menu of the navigation bar (Content). '0', would have replaced the "Content" item, while '0.' adds the item into the "Content" menu.

**Home menu:**

	$container['adminMenu']->addHome([
		'img' => $request->url->to('news/icon.svg'),
		'link' => $chain->container['resolver']->url_for(['News\Controllers\NewsAdminController', 'index']),
		'title' => __('News'),
		'description' => __('All the news.')
	]);

<a name="generator"></a>
##Generator

The admin generator hook will automatically create the entity admin controller and add items to the menus if you instruct it to do so.

In your YAML generator file, for any entity for which you want to create a controller in the administration, add the following lines:

```yaml
mybundle:
  entities:
    article:
      properties:
        title:
        content:
  admin:
    entities:
	  article:
	    form:
	      title:
	      content:
	      	render: textarea
	        params:
	          attrs:
	            class: test
	    messages:
	      modified: Article was modified.
	      created: Article was created.
	      many_deleted: Articles were deleted.
	      deleted: Article was deleted.
```

This generator file will build a bundle with an entity called "article" and everything you need to manage the entities in the administration.

You can customize the form and the messages, but this is not mandatory (default values will be used). To use the default form and messages, just add the line:

	admin:

without any parameter.

However, to customize the form, add all fields you want to appear in the form (here "title" and "content") and provide a default widget and parameters if necessary. Parameters are passed to the field. See the [form documentation](docs/form) for more information on widgets and parameters.