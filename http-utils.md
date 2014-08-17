#Utils

- [HTML](#html)
- [Flash](#flash)
- [Browser](#broser)

<a name="html"></a>
##HTML
The HTML class helps to build the HTML page.

###Usage in the Asgard Framework

	$html = $container['html'];
	
The [container](docs/container) is often accessible as a parameter or through a [ContainerAware](docs/container#containeraware) object. You can also use the [singleton](docs/container#usage-outside) but it is not recommended.

###Usage outside the Asgard Framework

	#$request is a \Asgard\HTTP\Request object 
	$html = new \Asgard\Http\Utils\HTML($request);

###Title

	$html->setTitle('title');

	$html->getTitle();

	$html->printTitle();

###Description

	$html->setDescription('description');

	$html->getDescription();

	$html->printDescription();

###Keywords

	$html->setKeywords('keywords');

	$html->getKeywords();

	$html->printKeywords();

###JS

	$html->includeJS('path/to/file.js);

	$html->codeJS('var a=12; f(a);');

	$html->printJSInclude(); #prints <script type="text/javascript" src=".."></script> for all included JS files

	$html->printJSCode(); #prints the JS code between script tags

###CSS

	$html->includeCSS('path/to/file.css);

	$html->codeCSS('a.foo { border:1px solid red; }');

	$html->minifyCSS(); #prints <link rel="stylesheet" href=".."/> for all included CSS files

	$html->printCSSCode();# prints the JS code between style tags

###Others

	$html->code('<script>f(12);</script>
	<style>a.foo { border:1px solid red; }</style>');

	$html->codeStart();
	echo '<script>f(12);</script>
	<style>a.foo { border:1px solid red; }</style>';
	$html->codeEnd();

	$html->printCode(); #prints the previous code

	$html->printAll(); #calls printJSInclude, printCSSInclude, printJSSCode, printCSSCode and printCode

##Flash
The Flash class saves messages in session to display them on the next page.

###Usage in the Asgard Framework

In a \Asgard\Http\Controller, use:

	$this->getFlash();

###Usage outside the Asgard Framework

	#$request is a \Asgard\HTTP\Request object 
	$flash = new \Asgard\Http\Utils\Flash($request->session);

###Add messages

	$flash->addSuccess('message');

	$flash->addError('message');

	$flash->addInfo('message');

	$flash->addWarning('message');

	$flash->add('custom_type', 'message');

Multiple messages:

	$flash->addSuccess([
		'message1',
		'message2'
	]);

Nested arrays messages:

	$flash->addSuccess([
		'form1' => [
			'error1',
			'error2'
		],
		'form2' => [
			'error3',
			'error4'
		],'
	]);

###Show messages

	$flash->show('success');
	$flash->show('error');
	$flash->show('info');
	$flash->show('warning');
	$flash->show('custom_type');

Messages from a specific array only:

	$flash->show('success', 'form2');

All types:

	$flash->showAll();

All types from a specific array:

	$flash->showAll('form2');

##Browser
The Browser is very useful to create HTTP request and return their responses.

###Usage

	#$container is an instance of \Asgard\Container\Container
	$browser = new \Asgard\Http\Browser\Browser($container);

###Manipulate cookies and sessions

	$browser->getCookies();

returns a [\Asgard\Common\Bag](docs/bag) instance

	$browser->getSession();

returns a [\Asgard\Common\Bag](docs/bag) instance

###Send requests

All the get, post, put and delete methods return a \Asgard\Http\Response object.

**get**

	$headers = ['User-Agent' => 'IE'];
	$browser->get('news/1', 'body data', $headers);

**post**

	$headers = ['User-Agent' => 'IE'];
	$post = ['title' => 'hello!'];
	$files = ['image' => [
		'tmp_name' => 'path/to/file.jpg',
		'name'     => 'file.jpg',
		'error'    => 0,
		'size'     => 232345,
		'type'     => 'image/jpg',
	]];
	$browser->get('news/1', $post, $files, $body='', $headers);

**put**

	$headers = ['User-Agent' => 'IE'];
	$post = ['title' => 'hello!'];
	$files = ['image' => [
		'tmp_name' => 'path/to/file.jpg',
		'name'     => 'file.jpg',
		'error'    => 0,
		'size'     => 232345,
		'type'     => 'image/jpg',
	]];
	$browser->get('news/1', $post, $files, $body='', $headers);

**delete**

	$headers = ['User-Agent' => 'IE'];
	$browser->get('news/1', 'body data', $headers);

###Submit a form

	$xpath = '//form';
	$to    = 'admin/news/1/edit';
	$override = ['title' => 'foo', 'content' => 'bar'];
	$browser->submit($xpath, $to, $override);

###Get the last response

	$browser->getLast();

###Example

	$container = \Asgard\Container\Container::singleton();
	$browser = new \Asgard\Http\Browser\Browser($container);
	$browser->getSession()->set('admin_id', 123);
	
	$response = $browser->post('admin/news/new');

	if($response->getCode() != 200)
		echo 'error..';
	else
		echo $response->getContent();