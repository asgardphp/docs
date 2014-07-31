#HTTP

[![Build Status](https://travis-ci.org/asgardphp/http.svg?branch=master)](https://travis-ci.org/asgardphp/http)

Handles the HTTP requests, routing, controllers and responses..

##Request & Response

Handling HTTP requests and building responses. [See the documentation.](docs/http-requestresponse)

	$request = \Asgard\Http\Request::createFromGlobals();
	//...
	return (new \Asgard\Http\Response)->setCode(404)->setContent('not found :(');

##Controllers

Structure your code around controllers. [See the documentation.](docs/http-controllers)

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

##Commands

[List of commands that come with the HTTP package.](docs/http-commands)