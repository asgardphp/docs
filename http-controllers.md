#Controllers

- [Construction](#construction)
- [Routing](#routing)
- [Errors and exceptions](#errors)
- [Filters](#filters)
- [Views](#views)

<a name="validator"></a>
##Construction
A controller class name must follow the pattern NameController. With Asgard Framework, stores them in the bundle Controller/ folder for automatic loading.

The controller class is composed of actions. Each action handles a specific request. For example:

	class ProductController extends \Asgard\Http\Controller {
		public function indexAction(\Asgard\Http\Request $request) {
			//...
		}
	}

Here we have only one action called indexAction, which receives the request as a method parameter.

<a name="validator"></a>
##Routing

###Creating routes
You can use the resolver to route requests to specific actions. In Asgard, the resolver object is available through the services container:

	$resolver = $container['resolver'];

To create a new route, open app app/start.php and add for example:

	$resolver->route('products', 'Products\Controller\ProductController::index');
	#or
	$resolver->route('products', function(\Asgard\Http\Request $request) {
		//...
	});

To route only specific HTTP methods, use the 3rd argument:

	$resolver->route('products', 'Products\Controller\ProductController::index', ['post', 'put']);

###Annotated Routes
To ease the development you can use annotations in your controllers as following:

	/**
	 * @Prefix("products")
	 */
	class ProductController extends \Asgard\Http\Controller {
		/**
		 * @Prefix("index")
		 */
		public function indexAction(\Asgard\Http\Request $request) {
			//...
		}

		/**
		 * @Prefix("search")
		 */
		public function searchAction(\Asgard\Http\Request $request) {
			//...
		}
	}

Here we prefix all the actions routes with "products/". "products/index" points to indexAction while "news/search" points to searchAction.

###Route parameters

You can include parameters in your routes like this:
	
	/**
	 * @Prefix(":id")
	 */
	public function showAction(\Asgard\Http\Request $request) {
		//...
	}

URLs following the pattern "products/:id" (e.g. products/24) will be caught by this route.

To access the parameter from the action:

	$id = $request['id'];

<a name="errors"></a>
##Errors and exceptions

###404
To return a 404 error use the following line inside an action:

	$this->notFound();

This will throw an \Asgard\Http\ControllerException with a 404 code.

###ControllerException

ControllerExceptions are caught by HttpKernel, which "converts" them into response objects. You can throw a ControllerException with any HTTP code:

	throw new \Asgard\Controller\ControllerException(404);

HttpKernel will return a 404 HTTP response.

If you want to modify exceptions responses, use the hook Asgard.Http.Exception.[ExceptionClass]:

	$container['hooks']->hook('Asgard.Http.Exception.Asgard\Controller\ControllerException', function($chain, $e, &$response) {
		if($e->getCode() == 404)
			$response->setContent('Sorry, page not found.');
	})

<a name="filters"></a>
##Filters

Filters let you execute code before and after any or all controller actions.

To create a new filter:

	$filter = new \Asgard\Http\Filters\PageLayout('layout.php', 'html.php');

To add a filter to all controllers:

	$container['httpKernel']->filterAll($filter);

To add a filter to all actions in a controller:

	$container['httpKernel']->filterController('Controller', $filter);

To add a filter to a specific action:

	$container['httpKernel']->filterController('Controller:index', $filter);

To add a filter based on the route:

	$container['httpKernel']->filterRoute('products/index', $filter);

All "before" methods of filters will be executed before the action. They can return a response object, in which case the action will not be executed. For example:

	public function before(\Asgard\Http\Controller $controller) {
		return (new \Asgard\Http\Response())->setCode(400);
	}

The "after" methods of filters are executed after the action (or the "before" methods if the action was skipped). They can modify the response before it is returned by the controller: 

	public function after(\Asgard\Http\Controller $controller, $response) {
		$response->setCode(404);
	}

<a name="views"></a>
##Views

###Default views
In Asgard, when a controller action is executed and does not provide its own view, the controller will try to automatically generate the view. 

When a controller is located in the Controllers folder of a bundle, it will search for views in the bundle views/ folder. For example if the ProductController class is in bundle/Controller/Product.php, and its action indexAction is executed, it will look for the view in bundle/views/product/index.php. If it exists it will use it to construct the response, otherwise the response will be empty.

Before generating the view, the controller will extract all its properties into variables and pass them to the view. Hence, $this->header in the controller becomes $header in the php view file:

	#bundle/Controller/Product.php
	public function indexAction(\Asgard\Http\Request $request) {
		$this->header = '<h1>Procucts</h1>';
	}

	#bundle/views/product/index.php
	<?php echo $header ?>

###Custom views
You can also provide your own views. To do that you need to create a new class which extends \Asgard\Http\View and override the render method:

	class MyView extends \Asgard\Http\View {
		public function render($template, array $params=[]) {
			$html = file_get_contents($template);
			foreach($params as $k=>$v)
				$html = str_replace(':'.$k, $v, $html);
			return $html;
		}
	}

and return it in the action:

	#bundle/Controller/Product.php
	public function indexAction(\Asgard\Http\Request $request) {
		return new MyView('index.template', ['header'=>'Products']);
	}

The controller will return a response with the result of the view as its body.