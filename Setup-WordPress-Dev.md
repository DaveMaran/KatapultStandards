# WordPress development setup process

## Important notes

- All development work must be effectively verision controlled and pushed to GitHub every 2-3 days. 
- GitHub repos are linked to a live dev server via [deploybot](https://github.com/katapultstudios/KatapultStandards#deploymentintegration-deploybot)
- Repos are for Themes and Plugins, _not_ WordPress core files 
- WordPress themes are for styles
- [WordPress plugins](#4-bespoke-plugins) are for functionality
- For consistency we use Nginx for all WordPress projects

## Required software
We use the following software internally. External developers may use alternative tools if they prefer.
- [MAMP Pro](https://www.mamp.info/en/mamp-pro/)
- [GitHub Desktop](https://desktop.github.com/)

## 1. Create a local WordPress install running in MAMP
If you're not already running it, [install MAMP Pro](https://www.mamp.info/en/downloads/) and make sure it's up to date. Set your webserver to Nginx and PHP to version 7. Get the [latest version of WordPress](https://wordpress.org/download/) and install it in your ~/Sites/{clientname}/ folder.    

*Note: This folder will _contain_ the project repos, rather than acting as the repo itself. To keep our repositories manageable we create one for the theme and one for the _client specific_ plugins.* But we'll get to that.

Setup a host in MAMP Pro as follows: 
- Create a new host named: local.real-client-domain.com (e.g. local.katapult.co.uk)
- Create a database if you need to
- Set the host to use Nginx
- Add `$uri $uri/ /index.php?$args;` to the _try_files:_ field in the Nginx tab (This allows permalinks to work.)

You should now be able to start the server and access your install at http://local.real-client-domain.com:7888

### Finish the WordPress installation

Finish setting up WordPress in the usual way through the browser. When you create the admin user don't use 'admin' or 'root'. Use something like {clientname}_admin_{yourinitials} and use a strong password. This removes the possibility of launching a website with admin creds set to root/root. Which would be bad.

### Update the wp-config file

Add the following to the wp-config.php file under the database connection settings: 

```
define('FORCE_SSL_ADMIN', false);
$_SERVER["HTTPS"] = 'off';
$scheme = 'http://';
$website_url = 'local.real-client-domain.com:7888';

$_SERVER["HTTP_HOST"] = $website_url;
define("WP_HOME", $scheme.$website_url);
define("WP_SITEURL", $scheme.$website_url);
```

## 2. If creating a theme repository from Scratch

*Note: The following assumes you are creating the theme repository. If you need to work on an existing theme repo skip down to the section on [installing an existing theme](#3-working-with-an-existing-theme-repository) repo*

### Install and begin setup of JOINTSWP

We use JOINTSWP as a starting point for our themes. This includes all package management tools and dependencies. Download the [SASS version of JOINTSWP](http://jointswp.com/) and decompress the archive in the WordPress themes folder. Rename the folder to {clientname}_theme

Update the theme info in style.css with something similar to the following:
```
/******************************************************************
Theme Name: {Client Name}
Description: A custom WordPress theme for the sole use of {Client Name}
Author: Pete Clark
Author URI: https://www.katapult.co.uk
Version: 0.1 (Development)
Tags: Sass
******************************************************************/
```
- Activate the theme in WordPress
- Navigate to the theme folder in the terminal and run the following command:
```
$ npm install
```
npm install can take a while. Be patient. 

### Create the project repo locally in GitHub desktop
- Click Add
- Enter local path (e.g. /Users/username/Sites/clientname/wp-content/themes/clientname_theme)
- Click 'Create & Add Repository'

### Ignore build files
- JOINTSWP comes with a .gitignore file which is already set to ignore /node_modules/ and the like so you shouldn't need to worry about that.

### Publish repo on GitHub (from GitHub Desktop)
- Click the 'Publish' button in GitHub Desktop
- The name should already be set to {clientname}_theme. 
- Set the description to _A custom WordPress theme for the use of {Client Name}_
- Click 'Publish Repository'

## 3. If working with an existing theme repository

### Install WordPress locally
If you are being added to a theme repository to carry out work on it you should start by [installing WordPress](#1-create-a-local-wordpress-install-running-in-mamp) as above. 

### Clone the theme repository from GitHub
Using GitHub Desktop you should be able to clone the theme repository directly into the /wp-content/themes/ directory of your local install where you'll be able to work on it. 

### Compiling SASS and minifying JS

While working locally run the following command in the terminal:
```
$ npm run browsersync
```
This will keep an eye on SASS, JS etc that needs compiling or minifying. It will also reload your browser automatically. MAMP must be running _before_ you run the command above.

## 4. Bespoke plugins

In addition to the theme repository we should have a repo for custom WordPress plugins. You can either create this and publish it to the Katapult GitHub account (if you have access) or clone it into /wp-content/plugins/ if you're being given access to a repo that already exists. If you are creating the plugin/repo name it {clientname}_plugins

As far as possible functional elements of a WordPress website should be developed as custom plugins, rather than being included in the theme file. Technically this is not necessary but it makes for cleaner code that's easier to manage. So that's what we do. Examples of things that should be plugins, not part of the theme:

- Custom post types and taxonomies
- WordPress Shortcodes
- WordPress Widgets
- API integrations

### Nested plugins

You could go super-granular with plugins and create a plugin for _each_ custom post type. There's probably some good reasons for doing that but to keep things a little simpler our preferred development approach is to create a single plugin for clients which includes each of the required bits of functionality. You can still keep things really modular by splitting the plugin into separate directories and files.

Here's an example. The following is the main plugin file. It includes all sorts of other things.

```
<?php
/*
Plugin Name: Client Name — Custom Post Types and Taxonomies
Plugin URI: https://www.katapult.co.uk
Description: Adds custom post types which are fundamental to the site. DON'T REMOVE THIS!
Version: 1.0
Author: Pete Clark
Author URI: https://www.katapult.co.uk
*/

// Import our custom taxonomies
include dirname(__FILE__) . '/clientname_taxonomies/clientname_taxonomies.php';

// Custom Post Type: Events
include dirname(__FILE__) . '/clientname_cpt/clientname_cpt-events.php';
include dirname(__FILE__) . '/clientname_cpt_meta/clientname_cpt_meta-events.php';

// Custom Post Type: Staff profiles
include dirname(__FILE__) . '/clientname_cpt/clientname_cpt-profiles.php';
include dirname(__FILE__) . '/clientname_cpt_meta/clientname_cpt_meta-profiles.php';

// Custom Post Type: Jobs
include dirname(__FILE__) . '/clientname_cpt/clientname_cpt-jobs.php';
include dirname(__FILE__) . '/clientname_cpt_meta/clientname_cpt_meta-jobs.php';

```

## 5. Third-party plugins

Acceptable third-party Wordpress plugins to use are:
* [Advanced Custom Fields](https://www.advancedcustomfields.com/)
* [Formidable Forms](https://formidableforms.com/)
* [Hubspot Tracking Code](https://en-gb.wordpress.org/plugins/hubspot-tracking-code/)
* [Redirection](https://redirection.me/)
* [Smush Image Compression](https://wordpress.org/plugins/wp-smushit/)
* [Wordfence](https://www.wordfence.com/)

Any third-party plugin not on this list must be verified and added before use in a client website.

---

### Produced by Pete Clark - Katapult (Last updated 13/01/2018)

