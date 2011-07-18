Simple configuration loader for PHP
===================================

Simple case: All environments require the same configuration information
------------------------------------------------------------------------
Anything in the default section of the configuration file is available to any environment

	[default]
	KEY1=value1
	KEY2="value 2 has spaces in it"

Basic Case: Configuration settings for 1 server
-----------------------------------------------
Configuration can be matched to a specific HTTP_HOST

	[localhost]
	KEY1=value1
	KEY2="value 2 has spaces in it"

Normal Usage: Shared configuration settings across 2 servers
------------------------------------------------------------
Information in default will be automatically inherited by configuration for other environments.

	[default]
	KEY1=value1
	KEY2="value 2 has spaces in it"
	
	[localhost]
	KEY3="value for local environment"
	
	[dev.example.com]
	KEY3="value for dev site"
	

Advanced case: Shared configuration across multiple servers.  Some servers share the same configuration settings as others
--------------------------------------------------------------------------------------------------------------------------
If you have several environments that share configuration (not just default stuff), then you can also extend those.

	[default]
	KEY1=value1
	KEY2="value 2 has spaces in it"

	[local]
	KEY3="this value is available to anything that extends local"
	KEY4="this value is also available"

	[localhost]
	EXTENDS=local

	[127.0.0.1]
	EXTENDS=local
	
Passing values to javascript (jQuery)
-------------------------------------
You can also pass values to javascript.  Key names will be converted to camel case (first letter will be capitalized!).  On your PHP script add
the following to the head section:

	...
	<head>
		<?=$config->toJS(array(
			"KEY1",
			"KEY2",
			"KEY3"
		))?>
	</head>
	...
	
You will now have access to Key1, Key2, and Key3 from within your Javascript.
	
Using it
========
In your PHP script just do this:

	require("config.php");
	
	// assumes config.ini file in same directory
	// otherwise you can specify the file here
	$config = new configParser();
	
	// get one config value
	$key1 = $config->get('KEY1');
	
	// get config array
	$c = $config->get();
	

Misc
====
Configuration precedence is as follows (most important to least important): server specific, extended environments, default.  If a value appears in multiple parts of the configuration then the value from the highest priority area will be returned.
