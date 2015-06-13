#Modules

Modules can be used to quickly extend your application with new features that you want to customize. For example you can install the news module which contains default entities and controllers, and directly start modifying it to your specific needs, while starting from a clean existing base.

- [Installing a module](#install)
- [Structure](#structure)

<a name="install"></a>
##Installing a module

To install a module, run:

	php console install [source=tag] --migrate --update-composer

[source=tag] must be a git repository with a tag name, e.g. https://github.com/asgardmodules/admin.git=v0.2.0

If the module has any module dependencies, it will install them beforehand. Then this command will download the module and copy its folders app/, migrations/, tests/, config/ web/ to your project.

--migrate is an option to automatically migrate the migrations after the module was added. If this option is not provided, the module migrations will still be added to your application migrations, but not automatically executed.

--update-composer is an option to automatically install the module dependencies.

<a name="structure"></a>
##Structure

The typical structure of a module is as following:

* app/: including bundles (moved to the application app/ folder)
* tests/: including test files (moved to the application tests/ folder)
* migrations/: including migration files (moved to the application migrations/ folder). [Do not forget the migrations.json file](docs/migration).
* config/: including configuration files (moved to the application config/ folder)
* web/: including web assets (moved to the application web/ folder)
* asgard.json: detailed below
* composer.json: detailed below

As indicated next to them, these files are published in your application upon installation.

###asgard.json
asgard.json contains the module name:

	{
		"name": "asgard/news"
	}

And module dependencies (which will be installed with the module):

	{
		"name": "asgard/news",
		"require": {
			"asgard/admin": "https://github.com/asgardmodules/admin.git",
	        "asgard/wysiwyg": "https://github.com/asgardmodules/wysiwyg.git"
		}
	}

###Composer
Composer dependencies should be in composer.json as for any composer project:

	{
		"name": "asgard/news",
		"license": "MIT",
		"authors": [{
			"name": "Michel Hognerud",
			"email": "michel@hognerud.net"
		}],
		"require": {
	        "asgard/entityimage": "0.*",
	        "asgard/asgard": "0.*"
		}
	}

Asgard will automatically add them to your project's composer.json file.