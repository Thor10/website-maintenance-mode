# Website Maintenance Mode for Apache Servers

Website Maintenance Mode allows you to easily and quickly setup a 302 redirect for all visitors to a maintenance page
 on your website and it also allows you to view your site normally by excluding your IP address so you can review and
  test your site before switching it back to normal.

## Requirements

A web server running Apache ver.2.2 or greater, FTP access to the server, and a basic text editor. SSH access is highly 
recommended, but not required if you are only going to redirect the website via an .htaccess file and edit the file 
on your local machine.

## Instructions for users who have root/sudo access to their server.

**If you have root/sudo access to your server:** The .htaccess script will be copied and 
pasted to the Virtual Host file on the server.

### Editing the Virtual Host file

1. Upload the maintenance.html and maintenance.css to the server. Save these files in the website's root 
directory.

2. Navigate to the Apache configuration file for your website(s). It should be located in the following path directory 
from the root directory **/etc/apache2/sites-available**

3. Type **ls** to show the virtual host conf file for your site. *Note: It should resemble something like 
yourwebsitename.conf If you run multiple sites on your server, choose the one you want to setup maintenance mode for.*

4. Open your website's virtual host conf file using your preferred text editor. Type **sudo vim yourwebsitename.conf** 
or **sudo nano yourwebsitename.conf**

Your virtual host file will look similar to the following example below.

```
<VirtualHost *:80>
   # Admin email, Server Name (domain name), and any aliases
   ServerAdmin email@example.com
   ServerName  www.examplesite.com
   ServerAlias examplesite.com
  
   # Index file and Document Root (where the public files are located)
   DirectoryIndex index.html index.php
   DocumentRoot /var/www/examplesite.com/public_html
   # Log file locations
   LogLevel warn
   ErrorLog  /var/www/examplesite.com/log/error.log
   CustomLog /var/www/examplesite.com/log/access.log combined
</VirtualHost>

```

5. From the .htaccess file copy everything and paste it the virtual host file before the </VirtualHost> closing tag.
Your file should now look something like the example below.

```
<VirtualHost *:80>
   # Admin email, Server Name (domain name), and any aliases
   ServerAdmin email@example.com
   ServerName  www.examplesite.com
   ServerAlias examplesite.com
  
   # Index file and Document Root (where the public files are located)
   DirectoryIndex index.html index.php
   DocumentRoot /var/www/examplesite.com/public_html
   # Log file locations
   LogLevel warn
   ErrorLog  /var/www/examplesite.com/log/error.log
   CustomLog /var/www/examplesite.com/log/access.log combined
   
   # Website Maintenance Mode for Apache Servers
   # Uncomment all lines from <IfModule mod_rewrite.c> and including </IfModule> to activate.
   # Re-comment those lines with a # to turn off maintenance mode)
   
   # NOTE: You must update REMOTE_ADDR with your current IP address to view changes and preform testing.
   # Depending on whether you have IPv4 or IPv6 you will only need to un-comment and change one of the lines.
   # IPv4 will have !^ infront the IP address followed by back slashes.
   # IPv6 only requires the IP address itself.
   
   
   #<IfModule mod_rewrite.c>
    #RewriteEngine on
    #### Note: If your computer uses an IPv4 address. Only uncomment the line below. ####
    #RewriteCond %{REMOTE_ADDR} !^255\.255\.255\.255$
    #### Note: If your computer uses an IPv6 address. Only uncomment the line below. ####
    #RewriteCond %{REMOTE_ADDR} !^0000:0000:0000:0000:0000:0000:0000:0000
    #RewriteCond %{REQUEST_URI} !/maintenance.html$ [NC]
    #RewriteCond %{REQUEST_URI} !\.(jpe?g?|png|gif|svg|css|js) [NC]
    #RewriteRule .* /maintenance.html [R=302,L]
   #</IfModule>
</VirtualHost>

```

6. By default, the script is commented out so it won't unintentionally activate. The # (comments) between and 
including `<IfModule mod_rewrite.c> and </IfModule> ` and only one of the two RewriteCond %{REMOTE_ADDR} lines. Which
 one depends on if you have a IPv4 or IPv6 address.

7. If you have a IPv4 address, uncomment and change RewriteCond %{REMOTE_ADDR} !^255\.255\.255\.255$ to your own IP 
address and skip step 9.

The script should look something like the following below.

```
<IfModule mod_rewrite.c>
    RewriteEngine on
    #### Note: If your computer uses an IPv4 address. Only uncomment the line below. ####
    RewriteCond %{REMOTE_ADDR} !^123\.456.789\.12$
    #### Note: If your computer uses an IPv6 address. Only uncomment the line below. ####
   #RewriteCond %{REMOTE_ADDR} !^0000:0000:0000:0000:0000:0000:0000:0000
    RewriteCond %{REQUEST_URI} !/maintenance.html$ [NC]
    RewriteCond %{REQUEST_URI} !\.(jpe?g?|png|gif|svg|css|js) [NC]
    RewriteRule .* /maintenance.html [R=302,L]
   </IfModule>

```

8. If you have an IPv6 address, uncomment and change RewriteCond %{REMOTE_ADDR} 0000:0000:0000:0000:0000:0000:0000:0000 
   to your own IP address.

The script should look something like the following below.

```
<IfModule mod_rewrite.c>
    RewriteEngine on
    #### Note: If your computer uses an IPv4 address. Only uncomment the line below. ####
   #RewriteCond %{REMOTE_ADDR} !^123\.456.789\.12$
    #### Note: If your computer uses an IPv6 address. Only uncomment the line below. ####
    RewriteCond %{REMOTE_ADDR} !^2301:244:4711:b:ae22:bff:fe8c:dcd8
    RewriteCond %{REQUEST_URI} !/maintenance.html$ [NC]
    RewriteCond %{REQUEST_URI} !\.(jpe?g?|png|gif|svg|css|js) [NC]
    RewriteRule .* /maintenance.html [R=302,L]
   </IfModule>

```

9. Finally restart apache by using the following command **sudo service apache2 restart** Check to ensure that no 
error messages appear from apache. If so, check the apache logs to find out where the problem may lie.

## Instructions for users who don't have root/sudo access to their server.

**If you don't have root/sudo access to your server:** The .htaccess script will edited on your
 local machine and then uploaded to the root directory of your website.

### Editing the .htaccess file 

1. Upload the maintenance.html and maintenance.css to the server. Save these files in the website's root 
   directory.

2. Open the .htaccess file with your preferred text editor. You will see the following:

```
# Website Maintenance Mode for Apache Servers
# Uncomment all lines from <IfModule mod_rewrite.c> and including </IfModule> to activate.
# Re-comment those lines with a # to turn off maintenance mode)

# NOTE: You must update REMOTE_ADDR with your current IP address to view changes and preform testing.
# Depending on whether you have IPv4 or IPv6 you will only need to un-comment and change one of the lines.
# IPv4 will have !^ infront the IP address followed by back slashes.
# IPv6 only requires the IP address itself.


#<IfModule mod_rewrite.c>
 #RewriteEngine on
 #### Note: If your computer uses an IPv4 address. Only uncomment the line below. ####
 #RewriteCond %{REMOTE_ADDR} !^255\.255\.255\.255$
 #### Note: If your computer uses an IPv6 address. Only uncomment the line below. ####
 #RewriteCond %{REMOTE_ADDR} !^0000:0000:0000:0000:0000:0000:0000:0000
 #RewriteCond %{REQUEST_URI} !/maintenance.html$ [NC]
 #RewriteCond %{REQUEST_URI} !\.(jpe?g?|png|gif|svg|css|js) [NC]
 #RewriteRule .* /maintenance.html [R=302,L]
#</IfModule>

```

3. By default, the script is commented out so it won't unintentionally activate. The # (comments) between and 
   including `<IfModule mod_rewrite.c> and </IfModule> ` and only one of the two RewriteCond %{REMOTE_ADDR} lines. Which
    one depends on if you have a IPv4 or IPv6 address.

4. If you have a IPv4 address, uncomment and change RewriteCond %{REMOTE_ADDR} !^255\.255\.255\.255$ to your own IP 
address and skip step 6.

The script should look something like the following below.

```
<IfModule mod_rewrite.c>
    RewriteEngine on
    #### Note: If your computer uses an IPv4 address. Only uncomment the line below. ####
    RewriteCond %{REMOTE_ADDR} !^123\.456.789\.12$
    #### Note: If your computer uses an IPv6 address. Only uncomment the line below. ####
   #RewriteCond %{REMOTE_ADDR} !^0000:0000:0000:0000:0000:0000:0000:0000
    RewriteCond %{REQUEST_URI} !/maintenance.html$ [NC]
    RewriteCond %{REQUEST_URI} !\.(jpe?g?|png|gif|svg|css|js) [NC]
    RewriteRule .* /maintenance.html [R=302,L]
   </IfModule>

```

5. If you have an IPv6 address, uncomment and change RewriteCond %{REMOTE_ADDR} 
!^0000:0000:0000:0000:0000:0000:0000:0000 
   to your own IP address.

The script should look something like the following below.

```
<IfModule mod_rewrite.c>
    RewriteEngine on
    #### Note: If your computer uses an IPv4 address. Only uncomment the line below. ####
   #RewriteCond %{REMOTE_ADDR} !^123\.456.789\.12$
    #### Note: If your computer uses an IPv6 address. Only uncomment the line below. ####
    RewriteCond %{REMOTE_ADDR} !^2301:244:4711:b:ae22:bff:fe8c:dcd8
    RewriteCond %{REQUEST_URI} !/maintenance.html$ [NC]
    RewriteCond %{REQUEST_URI} !\.(jpe?g?|png|gif|svg|css|js) [NC]
    RewriteRule .* /maintenance.html [R=302,L]
   </IfModule>

```
6. Save the file and upload it to the root directory of your website.

### Included Files

The maintenance consists of the following files:

**.htaccess** - The main script for redirecting the website viewers to the maintenance page.

**maintenance.html** - An extremely simple mobile friendly HTML5 based document that states "Our website is currently undergoing maintenance."

**maintenance.css** - A simple CSS file which controls the look of the maintenance.html page.
Styles include a browser styles reset, an orange background color for the page, centered message, 
sans-serif font family, and media break points to provide responsive styles for the h1 tag.

### .htaccess file in detail

When you first view the .htaccess file, every line in the file will be commented out (maintenance mode will be turned off) as you can see in the following below.

```
# Website Maintenance Mode for Apache Servers
# Uncomment all lines from <IfModule mod_rewrite.c> and including </IfModule> to activate.
# Re-comment those lines with a # to turn off maintenance mode)

# NOTE: You must update REMOTE_ADDR with your current IP address to view changes and preform testing.
# Depending on whether you have IPv4 or IPv6 you will only need to un-comment and change one of the lines.
# IPv4 will have !^ infront the IP address followed by back slashes.
# IPv6 only requires the IP address itself.


#<IfModule mod_rewrite.c>
 #RewriteEngine on
 #### Note: If your computer uses an IPv4 address. Only uncomment the line below. ####
 #RewriteCond %{REMOTE_ADDR} !^255\.255\.255\.255$
 #### Note: If your computer uses an IPv6 address. Only uncomment the line below. ####
 #RewriteCond %{REMOTE_ADDR} !^0000:0000:0000:0000:0000:0000:0000:0000
 #RewriteCond %{REQUEST_URI} !/maintenance.html$ [NC]
 #RewriteCond %{REQUEST_URI} !\.(jpe?g?|png|gif|svg|css|js) [NC]
 #RewriteRule .* /maintenance.html [R=302,L]
#</IfModule>

```
#### Breakdown of some of .htaccess script lines

The following lines might be of interest to you, if you wish to customize the .htaccess file. *Note: Making changes to the .htaccess file can cause your site to become un-viewable if you make a syntax error in the file.*

`RewriteCond %{REMOTE_ADDR} !^255\.255\.255\.255$` This line sets the IPv4 address exception for your computer.

`RewriteCond %{REMOTE_ADDR} !^0000:0000:0000:0000:0000:0000:0000:0000` This line sets the IPv6 address exception for 
your computer.

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

## License

This software is licensed under the public domain, see the license.md file for more information.
