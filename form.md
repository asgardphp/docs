#Form

[![Build Status](https://travis-ci.org/asgardphp/form.svg?branch=master)](https://travis-ci.org/asgardphp/form)

Easy to build and show, remember inputs, automatic building for entities, automatic inputs processing, validation, integrated csrf protection, ..

-creation
-..
-examples

##Creation in Asgard Framework
The advantage of using the form service is that it will provides the Form with all the necessary dependencies:

	$app->make('form', [
		'name', #name (optional)
		[ #params (optional)
			'action'  => 'form.php',
			'enctype' => 'multipart/form-data',
			'attrs'   => [
				'class' => 'formClass'
			]
		],
		\Asgard\Http\Request::CreateFromGlobals(), #request (optional, Asgard will feed the form with the current request)
		[ #fields (optional)
			'name'    => new TextField(),
			'content' => new TextField(),
		]
	]);

In a controller (or any class using the view ContainerAware), $container is available through $this->getContainer(). You can also access it by \Asgard\Container\Container::singleton().

##Creation outside Asgard
Here you will have to provide the dependencies yourself (see the next section):

	$form = new \Asgard\Form\Form(
		'name', #name (optional)
		[ #params (optional)
			'action'  => 'form.php',
			'enctype' => 'multipart/form-data',
			'attrs'   => [
				'class' => 'formClass'
			]
		],
		\Asgard\Http\Request::CreateFromGlobals(), #request (optional)
		[ #fields (optional)
			'name'    => new TextField(),
			'content' => new TextField(),
		]
	);

##Parameters

To access or modify a form parameters:

	$param = $form->getParam('action');
	$form->setParam('action', $action);

##Dependencies

###Request

	$form->setRequest(\Asgard\Http\Request::createFromGlobals());

###Translator

	$form->setTranslator(new \Symfony\Component\Translation\Translator('en'));

###Services container or App

	$form->setApp(new \Asgard\Container\Container);

##Usage

###Form HTTP Method

By default the form gets its inputs from POST requests, but if you want to make a form that works with GET inputs:

	$form->setMethod('get');

##Groups
A Group is a group of fields. For example you may have a group for the shipping address fields and another group with the billing address fields.

To know how many fields or sub-groups a Group or a Form has:

	$count = $group->size();

##Fields

Forms are built like arrays. To add a field:

	$form['title'] = new Fields\TextField;

To get its value after the form was submitted:

	$value = $form['title']->getValue();

###List of Fields

###Field validation

print a field and label

###Groups of fields:
A Group is a group of fields. For example you may have a group for the shipping address fields and another group with the billing address fields.

You can add a whole group to a form:

	$form['address'] = [
		'number' => new Fields\TextField,
		'street' => new Fields\TextField,
		'city' => new Fields\TextField,
		'zipcode' => new Fields\TextField
	];

In the form, the array will be converted to a Group object. You can access a group's fields like a form:

	$value = $form['address']['street']->getValue();

A Form is a Group, so you can also embed forms in another:

	$personForm = new \Asgard\Form\Form;
	$personForm['firstname'] = new Fields\TextField;
	$personForm['lastname'] = new Fields\TextField;

	$form['person'] = $personForm;

And access fields like:

	$form['person']['firstname']->getValue();

###Save and Validation

To check that a form was sent:

	$form->sent(); #returns true|false

To check that the inputs are valid:

	$form->isValid(); #returns true|false

To get the errors:

	$errors = $form->errors();

To get a field or a group errors:

	$errors = $form['title']->errors();

To get only general errors or errors that do not belong to a specific field:

	$errors = $form->getGeneralErrors();

To save a form:

	$form->save();

In case of an error, an exception will be raised:

	try {
		$form->save();
	} catch(\Asgard\Form\FormException $e) {
		//...
	}

If all is valid, it will execute the form's doSave() method, which by default does not do anything. It will also try to save each nested-form.

However you can create your own classes that extend the Form class and override the "doSave" method or hook "save" (see [Hook documentation](docs/hook)):

	$form->hook('save', function($form) {
		// do something with form
	});
	$form->save()

###CSRF Protection

To enable CSRF protection for a form:

	$form->csrf();

To disable it:

	$form->csrf(false);

The error will be available through $form->getGeneralErrors() or $form['_csrf_token'].

###Form Data

Get data from a form after it has been sent:

	$data = $form->data();

Reset data:

	$form->reset();

Set data:

	$form->setData(['title' => 'abc']); 

##Rendering a form

	echo $form->open($params=[]); #prints the opening tag. Params will override the parameters passed to the form instance
	//...
	echo form->submit('Send'); #prints a simple submit button with text "Send"
	echo $form->close(); #prints the closing tag and an hidden input for csrf if enabled



##Widgets
widgetsmanager

fields
groups
group/form size

----------------------------------

Group

constructor
dependencies
	getTranslator
	getRequest
	getApp
General
	name
	setName
	size
	hasFile
Widgets
	getWidget
	getWidgetsManager
	setWidgetsManager
Rendering
	render
Save et validation
	save
	isValid
	sent
	errors
Fields
	remove
	get
	add
	has
	resetFields
	fields
	addFields
Data
	reset
	setData
	data
array/iterator

------------

Report
	->all()


##Examples

###User registration###

Building the form:

	$form = $app->make('form', ['user']);
	$form['username'] = new \Asgard\Form\Fields\TextField(['validation'=>'required']);
	$form['password'] = new \Asgard\Form\Fields\TextField(['validation'=>'required', 'widget' => 'password']);

	$form->setSaveCallback(function($form) {
		$username = $form['username']->value();
		$password = $form['password']->value();
		// save in database..
	});

	if($form->sent()) {
		try {
			$form->save();
			echo 'Good!';
		} catch(\Asgard\Form\FormException $e) {
			echo 'Bad..';
			foreach($e->errors() as $error)
				echo "\n".$error;
		}
	}

Showing the form:

	echo $form->open();
	echo $form['username']->def();
	echo $form['password']->def();
	echo $form->close();
