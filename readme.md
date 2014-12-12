# Website Maintenance Mode


The Website Maintenance Mode allows you to easily and quickly put your website into maintenance mode via an Apache htaccess script and temporarily 302 redirect all incoming visitors **accept for the computer you're working on by an IP address exception* to a custom maintenance page that states the website is down for maintenance work.

*Note:* The IP address exception allows only you to view, edit, and test any changes you make your website without being rerouted to the maintenance page. By having this exception you can ensure that everything on your live site is working correctly before returning your website to it's normal viewing mode.

### Requirements for using the Maintenance Mode

To use these scripts you will need an Apache based web server, FTP access to the server, and a basic text editor thats installed on the computer you're using. SSH access is helpful, but not required.

## Quick Setup and Start

Setting up the Maintenance Mode is fairly quick, simple and straight forward. If you're not too familiar with working with .htaccess files, this quick start guide should be enough to get you started fairly quickly. 

1. Upload the files (Psst...Don't worry the .htaccess file is already commented out (turned off) so you don't throw your site into maintenance mode as soon as the file is uploaded to the server.

	1. **If you don't have an .htaccess file for your site**, upload all the files (.htaccess, maintenance.html, and maintenance.css to the root directory of your website.

	2. **If you already do have an .htacess file for your site** upload the maintenance.html and maintenance.css files to the root of your website and copy n' paste the Website Maintenance Mode script into your current .htaccess file. 

2. Setup an IP Address exception so while the site is in maintenance mode you'll still be able to see and test any changes you make to the site. Open the .htaccess file, go the line **#RewriteCond %{REMOTE_ADDR} !^255\.255\.255\.255$** and change the default IP address **255.255.255.255** to your IP address.

	1. *Note:* If you're connected to a router, the IP address will be the one assigned to the router, not the internal IP that your router assigns to your computer. If you don't know what your IP address is, do a quick Internet search for "What's my IP address?" with your favorite search engine and the search results should provide you with your current IP address either within the search results or give you a list of sites that will display it for you.

	2. Also, keep in mind that any computers connected to that very same router will also have the same outside IP address and therefore they will not be re-routed to the maintenance page. This may or may not be a concern for you depending on your situation. To better clarify, if you're connected to a wifi hotspot at a coffee shop or any place that offers wifi, any computers that are connected to that same hot spot will have the same IP address and thus will not be redirected to the web site's maintenance page.

	3. Lastly keep in mind that your IP address may change from time to time even if you work from the same location with the same internet connection. Double check your IP address before putting your website into maintenance mode so you don't get redirected to the maintenance page.

3. To put the website into maintenance mode and redirect all incoming visitors to the maintenance notice, just **uncomment** the lines in the .htaccess file starting with **<IfModule mod_rewrite.c>** through (and including) **</IfModule>** using your favorite text editor on the server and save the file to make those changes.

*Note:* If you have SSH access and text editor installed on the server (Nano, Vim, etc.) you can edit the .htaccess file directly on the server instead of having to edit the file locally, save it, and upload it every time you need to take the website in and out of maintenance mode. 

4. When you're ready to take the website out of maintenance mode, simply go back and **re-comment** the lines **<IfModule mod_rewrite.c>** through (and including) **</IfModule>** and save the file to make those changes.

### Included Files

The maintenance consists of the following files:

.htaccess - The main script for redirecting the website viewers to the maintenance page. By default all lines in the script are commented out (turned off) in order to prevent the htaccess file from putting the website into maintenance mode right after upload.

maintenance.html - An extremely simple mobile friendly HTML5 based document that states "Our website is currently undergoing maintenance."

maintenance.css - A simple CSS file which controls the look of the maintenance.html page. resets all browser styles, provides an orange background color for the page, centers the message, sets the font family to sans-serif,.

### .htaccess file in detail

When you first view the .htaccess file, every line in the file will be commented out (maintenance mode will be turned off) as you can see in the following below.

```
## Website Maintenance Mode (Uncomment all lines from <IfModule mod_rewrite.c> through (and including) </IfModule> to activate. Re-comment those lines with a # to turn off maintenance mode)
## NOTE: You must update REMOTE_ADDR with your current IP address to view changes and preform testing.
############################################################################################################################################

#<IfModule mod_rewrite.c>
 #RewriteEngine on 
 #RewriteCond %{REMOTE_ADDR} !^255\.255\.255\.255$
 #RewriteCond %{REQUEST_URI} !/maintenance.html$ [NC]
 #RewriteCond %{REQUEST_URI} !\.(jpe?g?|png|gif|svg|css|js) [NC]
 #RewriteRule .* /maintenance.html [R=302,L]
#</IfModule>
```
#### Breakdown of some of .htaccess script lines

The following lines might be of interest to you, if you wish to customize the .htaccess file. *Note: Making changes to the .htaccess file can cause your site to become un-viewable if you make a syntax error in the file.*

`RewriteCond %{REMOTE_ADDR} !^255\.255\.255\.255$` This line sets the IP address exception for your computer.

`RewriteCond %{REQUEST_URI} !/maintenance.html$ [NC]` This line sets the condition for maintenance.html

*Note: If you would like to modify the redirect page to a different page, make sure to make the change in both the RewriteCond %{REQUEST_URI} !/maintenance.html$ [NC] and RewriteRule .* */maintenance.html [R=302,L] lines*

`RewriteCond %{REQUEST_URI} !\.(jpe?g?|png|gif|svg|css|js) [NC]` This line allows what types of files can be displayed in the redirect page. (Currently it is set to allow the following types of file extensions: .jpeg, .png, .gif, .svg, .css, and .js)

`RewriteRule .* /maintenance.html [R=302,L]` This line redirects all visitors to maintenance.html and is set as a 302 temporary redirect.

### maintenance.html file in detail

The maintenance.html file is very minimal so you can customize it fairly quickly without having to deal with excessive code.

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta http-equiv="content-type" content="text/html; charset=utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" type="text/css" href="maintenance.css" media="screen" />
<title>Our website is currently undergoing maintenance</title>
</head>
	<body>
		<h1>Our website is currently undergoing maintenance.</h1>
	</body>
</html>
```
The following lines might be of interest to you, if you want to customize the maintenance.html file.

`<meta name="viewport" content="width=device-width, initial-scale=1.0">` This is a mobile friendly tag line that optimizes the page based on the width of the device.

`<link rel="stylesheet" type="text/css" href="maintenance.css" media="screen" />` This is the link for the maintenance.css file. For simplicity and compatibility reasons the maintenance.css is set to the root directory (same directory) as the maintenance.html file. Also, the css file is set to screen viewing only.

`<title>Our website is currently undergoing maintenance</title>` The default maintenance page title.

`<h1>Our website is currently undergoing maintenance.</h1>` Between the body tags, the message that all viewers will see is set with h1 tags.

### maintenance.css file in detail

The maintenance.css file provides styles for the maintenance.html file. The css file is split into two sections:

1. Browser reset style rules (written by Eric Meyer) to eliminate any conflicting browser styles. (I suggest keeping this code intact)

2. Maintenance mode styles section (written by myself) provides styling for the background and the h1 tag.

*Note: Eric Meyer's Reset Styles section has been omitted in the code example below*

```
body {
	background-color: #ff5800;
}

h1 {
	text-align: center;
	margin: 0;
	padding: 1em 0 0 0;
	color: #e0e6e6;
	font-family: arial, sans-serif;
	font-size: 3em;
}
```
The following lines might be of interest to you, if you want to customize the maintenance.css file.
*Note:* Em values were used were necessary instead of pixel values in order to provide a more mobile friendly page across different screen sizes.

`body { background-color: #ff5800; }` This style rule defines the background color for the maintenance.html page. By default it is set to a bright shade of orange using a hexadecimal color.

*Note:* h1 tag rules will be explained separately for better clarity.

`text-align: center;` This style centers the text on the screen regardless of screen size.

`margin: 0;` The margin rule is set to a zero margin on all sides.

`padding: 1em 0 0 0;` The padding rule adds 1em of padding to the top of the screen in order to push down the heading somewhat. (Right, bottom, and left are set to zero)

`color: #e0e6e6;` The font heading color is set to a light Grey.

`font-family: arial, sans-serif;` The heading typeface is set to display in arial first and then sans-serif if the viewer doesn't have arial installed on their computer.

`font-size: 3em;` The size of the typeface is set to 3em.