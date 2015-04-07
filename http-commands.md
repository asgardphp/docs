#HTTP Commands

- [Browser](#browser)
- [Generate tests](#generate)
- [Routes](#routes)

##Browser

Simulate an HTTP request.

Usage:

	php console browser [method] [url] [--h=headers in json] [--p=post in json] [--f=files in json] [--s=session in json] [--sr=server in json] [--c=cookies in json] [--b=body]

##Generate tests

Generate tests stubs.

Usage:

	php console generate-tests [output file. Default: tests/AutoTest.php]

##Routes

Show all the registered routes.

Usage:

	php console routes