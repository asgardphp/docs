#Common\Bag

Paginator is a tool to construct pagination.

- [Installation](#installation)
- [Usage in the Asgard Framework](#usage-asgard)
- [Usage outside the Asgard Framework](#usage-outside)
- [Methods](#methods)

<a name="installation"></a>
##Installation
**If you are working on an Asgard project you don't need to install this library as it is already part of the standard libraries.**

	composer require asgard/common 0.*

<a name="usage-asgard"></a>
##Usage in the Asgard Framework

	$paginator = $container->make('paginator');
	
The [container](docs/container) is often accessible as a method parameter or through a [ContainerAware](docs/container#containeraware) object. You can also use the [singleton](docs/container#usage-outside) but it is not recommended.

<a name="usage-outside"></a>
##Usage outside the Asgard Framework

	$paginator = new \Asgard\Common\Paginator($total, $page=1, $per_page=10, \Asgard\Http\Request $request=null);

<a name="methods"></a>
##Methods

**Get the first position of the current page:**

	$paginator->getFirstNbr();

Position starts with 1.

**Get the last position of the current page:**

	$paginator->getLastNbr();

**Get the number of elements per page:**

	$paginator->getPerPage();

**Get the number of pages:**

	$paginator->getPages();

**Check if there is a previous page:**

	$paginator->hasPrev();

**Check if there is a next page:**

	$paginator->hasNext();

**Get the previous page:**

	$paginator->getPrev();

**Get the next page:**

	$paginator->getNext();

**Render a default pagination:**

	$paginator->render();