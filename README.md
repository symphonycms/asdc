# Advanced Symphony Database Connector (ASDC)

- Version: 1.5
- Author: Alistair Kearney (hi@alistairkearney.com)
- Build Date: 19th September 2012
- Requirements: Symphony 2.3 or above

A more efficient database library that can be used by developers as an alternative to the built in MySQL database connector. Note, this does not replace the existing driver used throughout Symphony, but is used in tandem to it.

`ASDCLoader::instance()` will auto-discover the existing Database object and use its connection resource. It is possible to also use this library outside of Symphony, or establish a new database connection (See example 3)

## INSTALLATION

** Note: The latest version can always be grabbed with `git clone git://github.com/symphonycms/asdc.git`**

1. Upload the `asdc` folder in to your Symphony `/extensions` folder.

2. Enable it by selecting the "Advanced Symphony Database Connector (ASDC)", choose Enable from the with-selected menu, then click Apply.

## USAGE

#### Example 1 - no profiling:

	require_once(EXTENSIONS . '/asdc/lib/class.asdc.php');

	try{
		$db = ASDCLoader::instance();
		$result = $db->query("SELECT * FROM `tbl_pages` LIMIT 1");

		print_r($result->current());
	}
	catch(Exception $e){
		vprintf('%d: %s on query "%s"', $db->lastError());
	}


#### Example 2 - profiling:

	require_once(EXTENSIONS . '/asdc/lib/class.asdc.php');

	try{
		$db = ASDCLoader::instance(true);  ## TURN ON PROFILING HERE!
		$result = $db->query("SELECT * FROM `tbl_pages` LIMIT 1");

		print_r($result->current());
		print_r($db->log());

	}
	catch(Exception $e){
		vprintf('%d: %s on query "%s"', $db->lastError());
	}


#### Example 3 - Connection without the loader:

	require_once(EXTENSIONS . '/asdc/lib/class.asdc.php');

	$db = new ASDCMySQL;

	### Optional ###
	$db->character_encoding = 'utf8';
	$db->character_set = 'utf8';
	$db->prefix = 'eh_';
	$db->force_query_caching = true;
	###

	try{
		$db->connect('mysql://username:password@host:3306/my_table/);
		print 'SUCCESS!';
	}
	catch(Exception $e){
		print $e->getMessage();
	}
