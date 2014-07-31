#Autoloader

The Asgard Autoloader supports PSR-4 and provides some additional features that can be used besides the composer autoloader.

- [Usage](#usage)
- [Nested Namespaces](#nested-namespaces)
- [Global Classes](#global-classes)
- [Namespace Mapping](#namespace-mapping)
- [Class Mapping](#class-mapping)
- [Preloading Files](#preloading-files)

<a name="usage"></a>
##Usage

	$autoloader = new \Asgard\Autoloader\Autoloader();
	$autoloader->register();

This register the autoloader in the PHP process.

<a name="nested-namespaces"></a>
##Nested Namespaces
When looking for a class like Namespace\Class in Namespace\Subnamespace\Class, unlike some other languages, PHP does not look up in the parents namespaces.

Asgard autoloader however does it if you enable it:

	$autoloader->nestedNamespace(true);

<a name="global-classes"></a>
##Global classes

When using a class without namespace, the autoloader will search for the class in the existing or registered ones whose class name is the same.

For example if the class \Namespace\Class exists or is registered via $autoloader->map($class, $file), and you call \Class, the autoloader will be smart enough to alias \Class to \Namespace\Class, but only if there is only class with the same name.

To enable it:

	$autoloader->global(true);

By combining global and nestedNamespace, the autoloader will be able to match classes like \Namespace\Class to \Anothernamespace\Class if it has to. However it is recommended to use class mapping for better performances.

<a name="namespace-mapping"></a>
##Namespace mapping
Like with composer, you can map a namespace to a specific directory:

	$autoloader->mapNamespace('Namespace', 'myclasses/');

<a name="class-mapping"></a>
##Class mapping
Like the composer autoloader, you can map a class to a specific file:

	$autoloader->map('MyClass', 'myclass.php');

<a name="preloading-files"></a>
##Preloading files
You can ask the autoloader to analyse a folder's files whose names start with a capital to help it find global classes:

	$autoloader->preload('classes/');

Files like classes\Subfolder\MyClass.php will be mapped to class \MyClass. This preloading can be cached if you provide the autoloader with a cache driver:

	$autoloader->setCache(new \Asgard\Cache\ApcCache());

#todo ^^^^