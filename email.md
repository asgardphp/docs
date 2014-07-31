#Email

[![Build Status](https://travis-ci.org/asgardphp/email.svg?branch=master)](https://travis-ci.org/asgardphp/email)

- [Usage in Asgard Framework](#usage-asgard)
- [Usage outside Asgard](#usage-outside)
- [Send an email](#send)
- [Attach files](#files)
- [Attach images](#images)

<a name="usage-asgard"></a>
##Usage in Asgard

###Configuration

In a configuration file, in config/, add:

	#smtp
	'email' => [
		'transport' => [
			'transport' => 'smtp',
			'username'  => '...',
			'password'  => '...',
			'security'  => 'ssl', #or null
			'host'      => '...',
			'port'      => '...',
		]
	]
	#sendmail
	'email' => [
		'transport' => [
			'transport' => 'sendmail',
			'command'  => '...',
		]
	]
	#mail()
	'email' => []

###Service

	$email = $app['email'];

<a name="usage-outside"></a>
##Usage outside Asgard

###Configuration

	#smtp
	$config = ['transport' => [
		'transport' => 'smtp',
		'username'  => '...',
		'password'  => '...',
		'security'  => 'ssl', #or null
		'host'      => '...',
		'port'      => '...',
	]];
	#sendmail
	$config = ['transport' => [
		'transport' => 'sendmail',
		'command'  => '...',
	]];
	#mail()
	$config = [];

###Instance

	$email = new \Asgard\Email\SwiftEmail(); #the only supported driver at the moment
	$email->transport($config);

<a name="send"></a>
##Sending an email

	$email->send(function($message) {
		$message->to('bob@example.com');
		$message->from('joe@example.com');
		$message->cc('joe@example.com');
		$message->bcc('joe@example.com');
		$message->text('hello!');
		$message->html('<h1>hello!</h1>');
	});

$message inherits \Swift_Message so you can use any of its methods as well. [See its documentation](http://swiftmailer.org/docs/messages.html).

<a name="files"></a>
##Attaching files

Directly attach a file:

	$email->send(function($message) {
		//...
		$message->attachFile('/path/to/file.jpg', 'myhouse.jpg', 'image/jpeg');
	});

Or data:

	$email->send(function($message) {
		//...
		$message->attachData($data, 'myhouse.jpg', 'image/jpeg');
	});

<a name="images"></a>
##Embedding images

Directly embed an image:

	$email->send(function($message) {
		//...
		$message->html('<h1>Hello!</h2> See my house '.$message->embedFile('/path/to/file.jpg', 'myhouse.jpg', 'image/jpeg'));
	});

Or data:

	$email->send(function($message) {
		//...
		$message->html('<h1>Hello!</h2> See my house '.$message->embedData($data, 'myhouse.jpg', 'image/jpeg'));
	});