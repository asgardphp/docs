#Tester

The Asgard Tester helps you built functional tests for your application.

- [Run](#run)
- [Curl Converter](#curl)
- [Coverage](#coverage)

<a name="run"></a>
##Run
Run the tester.

Usage:

	php console tester:run [--config=Namespace\ConfigClass] [--output=AppTest]

If you provide an output file it will automatically generate tests.

<a name="curl"></a>
##Curl converter
Convert an curl request to an Asgard request.

Usage:

	php console tester:curl [curl request]

Example:

	php console tester:curl "curl ""https://www.google.fr/"" -H ""Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8"" -H ""User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/42.0.2311.90 Safari/537.36"" --compressed"

Gives:

	$browser = $this->createBrowser();
	$response = $browser->req(
        '',
        'GET', [
        ], [
        ], '', [
                'Accept' => 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8',
                'User-Agent' => 'Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/42.0.2311.90 Safari/537.36',
        ]);
	$this->assertTrue($response->isOk());

<a name="coverage"></a>
##Run
Shows the coverage from a PHPUNIT file.

Usage:

	php console tester:coverage [php file]

It prompts you with 4 commands:

	coverage: show coverage percentages
	show [id]: show covered source code
	help: this
	quit: quit 