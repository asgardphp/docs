#Commands

- [Core commands](#core)
  - [Compile](#compile)
  - [Console](#console)
  - [Deploy](#deploy)
  - [Down](#down)
  - [Execute](#execute)
  - [Generate](#generate)
  - [Help](#help)
  - [Install](#install)
  - [List](#list)
  - [Publish](#publish)
  - [Services](#services)
  - [Publish](#publish)
  - [Server](#server)
  - [Show environment](#showenv)
  - [Switch environment](#switchenv)
  - [Up](#up)
  - [Version](#version)
- [Packages commands](#packages)

To execute an asgard command, open a console, go your project folder, and run your command preceded by "php console".

Below are the core asgard commands. However the packages come with their own commands too. See the list at the end.

To get a list of the available commands in your project, run:
	
	php console list

<a name="core"></a>
##Core commands

<a name="compile"></a>
###Compile

Compile classes into one file for better performance.

Usage:

	php console compile

<a name="console"></a>
###Console

Interact with your application.

Usage:

	php console console

<a name="deploy"></a>
###Deploy

Will install composer packages, clear cache, compile classes and execute migrations.

Usage:

	php console deploy

<a name="down"></a>
###Down

Put the application into maintenance mode.

Usage:

	php console down

<a name="execute"></a>
###Execute

Execute PHP code in the application context.

Usage:

	php console execute [php code]

<a name="generate"></a>
###Generate

Generate bundles from a single YAML file.

Usage:

	php console generate file.yml

<a name="help"></a>
###Help

Displays help for a command.

Usage:

	php console help [command]

<a name="install"></a>
###Install

Install a module into your application.

Usage:

	php console install [git repository] --migrate --update-composer

<a name="list"></a>
###List

Lists commands.

Usage:

	php console list

<a name="publish"></a>
###Publish

Publish a bundle files.

Usage:

	php console publish [path to bundle]

<a name="server"></a>
###Server

Run the PHP built-in server.

Usage:

	php console server [--host=localhost] [--port=8000]

<a name="showenv"></a>
###Show environment

Show the current default environment.

Usage:

	php console env:show

<a name="switchenv"></a>
###Switch environment

Switch the default environment.

Usage:

	php console env:switch [env]

<a name="up"></a>
###Up

Bring the application out of maintenance mode.

Usage:

	php console up

<a name="version"></a>
###Version

Display this application version.

Usage:

	php console version

<a name="packages"></a>
##Packages commands

 * [Cache](docs/cache#commands)
 * [Container](docs/container#commands)
 * [Config](docs/config-commands)
 * [DB](docs/db-commands)
 * [HTTP](docs/http-commands)
 * [Migration](docs/migration-commands)
 * [ORM](docs/orm-commands)
 * [Translation](docs/translation-commands)