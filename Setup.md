# Installation #

The first thing you probably want to do is download the latest version of
the rFramework, go to the download page to do so!

  1. Upload the example **index.php** and **config.php**
  1. Upload the folders **Framework**, **Skin** and **Sources** from the archive

Your directory should look like this:

  * Framework/
    * Pear/
    * Actions.php
    * Framework.php
    * Layout.php
  * Skin/
    * error.tpl
    * layout.tpl
  * Sources/
    * index.php
  * config.php
  * index.php
  * style.css

## Setting up config.php ##

```
$conf['site_name'] = 'Your Website';
$conf['title_seperator'] = ' - ';
$conf['base_url'] = 'http://yoursite.com/';
```

The **site\_name** setting and **title\_seperator** affect the 

&lt;title&gt;

 tag
in your HTML headers.
If you set the current page's title to be '**hello**' in a module file,
it would look like '**hello - Your Website**' with the default settings.

```
// enable
$conf['use_db'] = 1;

// database
$conf['sql_method'] = 'mysql';
$conf['sql_host'] = 'localhost';
$conf['sql_user'] = 'root';
$conf['sql_password'] = '';
$conf['sql_database'] = 'website';
```

If you set **use\_db** to 1, the framework will enable PEAR's DB database
abstraction layer. Use the other SQL related settings to configure your database login.

PEAR's DB database abstraction layer supports over 5 database engines, you might want to read it's manual [PEAR DB Manual](http://pear.php.net/manual/en/package.database.db.php)

```
// skin
$conf['stylesheet'] = array( 'styles.css' );
$conf['script'] = array( 'scripts.css' );
$conf['skin_dir'] = 'Skin/';
$conf['actions_dir'] = 'Sources/';
```

The skin files are loaded from **Skin/** and action modules from **Sources/**.

The setting **stylesheet** holds an array of stylesheets. Add more array nodes
in order to add multiple stylesheets.

```
$conf['stylesheet'] = array( 'styles.css', 'IE6.css', 'IE7.css' );
```
_Multiple stylesheets attached to your page_

It works exactly the same for scripts.

```
// html
$conf['lineend'] = 'unix';
$conf['doctype'] = 'XHTML 1.0 Transitional';
$conf['html_lang'] = 'en';
$conf['cache'] = 'false';
```

This part of the settings sets up the HTML header output, defaulting to XHTML 1.0
Transitional, cache disabled (REQUIRED for dynamic websites)

## Setting up index.php ##
The file **index.php** is somewhat of a wrapper, that initiates the framework and
loads modules into an array of available actions.

```
//------------------------------------
// Sets the full path to the website 
//------------------------------------
define( 'PATH', dirname( __FILE__ ) ."/" );
define( 'IN_SITE', true );
```

This bit of code is **required** to make the rFramework run properly.
The constants that we make called **PATH** is the full path to the website directory.
the constant **IN\_SITE** allows you to do things like this:

```
if( !IN_SITE )
{
    echo 'You can not access this file directly!';
    exit;
}
```

In module files to check if files are accessed through the wrapper or directly.

```
//------------------------------------
// Load the Framework                
//------------------------------------
require_once( PATH . 'Framework/Layout.php' );
require_once( PATH . 'Framework/Framework.php' );

//------------------------------------
// Initialize the Framework
//------------------------------------
// The first parameter of Framework()
// should be the name of the config
// file. setActionPath sets the path
// to the action modules.
//------------------------------------
$engine = new Framework( 'config.php' );
$engine->setActionPath( $engine->conf['actions_dir'] );
```

This bit of code requires the layout wrapper and framework engine, initiates the framework
class and instance it into $engine.

We set the action modules path to the value stored in config.php, where we'll store all of our action modules.