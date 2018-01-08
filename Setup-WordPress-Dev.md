# WordPress development setup process
## Required software
- MAMP
- GitHub Desktop

## 1. Create a local WordPress install running in MAMP
If you're not already running it, [install MAMP](https://www.mamp.info/en/downloads/) and make sure it's up to date. Set your webserver to Nginx and PHP to version 7. Get the [latest version of WordPress](https://wordpress.org/download/) and install it in your ~/Sites/{clientname}/ folder.    

*Note: This folder will _contain_ the project repos, rather than acting as the repo itself. To keep our repositories manageable we create one for the theme and one for the _client specific_ plugins.* But we'll get to that.

Complete the installation at http://localhost/

### Admin user in dev environment
When you create the admin user don't use 'admin' or 'root'. Use something like {clientname}_admin_{yourinitials} and use a strong password. This removes the possibility of launching a website with admin creds set to root/root. Which would be bad.

## 2. Creating a theme repository from Scratch

*Note: The following assumes you are creating the theme repository. If you need to work on an existing theme repo skip down to the section on [installing an existing theme](#3-working-with-an-existing-theme-repository) repo*

### Install and begin setup of JOINTSWP

We use JOINTSWP as a starting point for our themes. Download the [SASS version of JOINTSWP](http://jointswp.com/) and decompress the archive in the WordPress themes folder. Rename the folder to {clientname}_theme

Update the theme info in style.css with something similar to the following:
```
/******************************************************************
Theme Name: Client Name
Description: A custom WordPress theme for the sole use of Client Name
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

## 3. Working with an existing theme repository

### Install WordPress locally
If you are being added to a theme repository to carry out work on it you should start by installing WordPress as above. 

### Clone the theme repository from GitHub
Using GitHub Desktop you should be able to clone the theme repository directly into the /wp-content/themes/ directory of your local install where you'll be able to work on it. 

### Compiling SASS and minifying JS

While working locally run the following command in the terminal:
```
$ npm run browsersync
```
This will keep an eye on SASS, JS etc that needs compiling or minifying. It will also reload your browser automatically. MAMP must be running _before_ you run the command above.