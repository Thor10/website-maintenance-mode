# Website Maintenance Mode


The Website Maintenance Mode allows you to easily and quickly put your website into maintenance mode via an Apache htaccess script and temporarily 302 redirect all incoming visitors (*accept for the computer you're working on by an IP address exception) to a custom maintenance page that states the website is down for maintenance work.

(* Note: The IP address exception allows only you to view, edit, and test any changes you make your website without being rerouted to the maintenance page. By having this exception you can ensure that everything on your live site is working correctly before returning your website to it's normal viewing mode.

### Requirements for using the Maintenance Mode

To use these scripts you will need an Apache based web server, FTP access to the server, and a basic text editor thats installed on the computer you're using.

*Note:* If you have SSH access and text editor installed on the server (Nano or Vim) you can edit the htaccess file directly on the server instead of having to edit the file locally, save it, and upload it every time you need to take the website in and out of maintenance mode.

## Quick Setup and Start

Setting up the Maintenance Mode is fairly quick, simple and straight forward. If you're familiar with working with .htaccess files, then this quick start guide should be just enough to get you acquainted with the script. 

1. Upload the files (Psst...Don't worry the .htaccess file is already commented out (turned off) so you don't throw your site into maintenance mode as soon as the file is uploaded to the server.

	1. **If you don't have an .htaccess file for your site**, upload all the files (.htaccess, maintenance.html, and maintenance.css to the root directory of your website.

	2. **If you already do have an .htacess file for your site** upload the maintenance.html and maintenance.css files to the root of your website and copy n' paste the Website Maintenance Mode script into your current .htaccess file. 

2. Setup an IP Address exception so while the site is in maintenance mode you'll still be able to see and test any changes you make to the site. Open the .htaccess file, go the line **#RewriteCond %{REMOTE_ADDR} !^255\.255\.255\.255$** and change the default IP address **255.255.255.255** to your IP address.

3. To put the website into maintenance mode and redirect all incoming visitors to the maintenance notice, just **uncomment** the lines in the .htaccess file starting with **<IfModule mod_rewrite.c>** through (and including) **</IfModule>** using your favorite text editor on the server and save the file to make those changes. 

4. When you're ready to take the website out of maintenance mode, simply go back and **re-comment** the lines **<IfModule mod_rewrite.c>** through (and including) **</IfModule>** and save the file to make those changes.

*Note: If you're connected to a router, the IP address will be the one assigned to the router, not the internal IP that your router assigns to your computer. If you don't know what your IP address is, do a quick Internet search for "What's my IP address?" with your favorite search engine and the search results should provide you with your current IP address either within the search results or give you a list of sites that will display it for you.

### Included Files

The maintenance consists of the following files:

.htaccess - The main script for redirecting the website viewers to the maintenance page. By default all lines in the script are commented out (turned off) in order to prevent the htaccess file from putting the website into maintenance mode right after upload.

maintenance.html - A simple HTML5 based document with a heading 1 tag that states the website is currently undergoing maintenance.

maintenance.css - A simple CSS file which resets all browser styles, an orange background color for the page, and font rules for the heading tag for the maintenance.html file.

### .htacess file in detail

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

### Note about routers.

If the computer you're working on is behind a router or modem-router, any computers connected to that same router will also have the same outside IP address. This may or may not be a concern for you depending on your situation, however if you're connected to a wifi hotspot at a coffee shop or any place that offers wifi, any computers that view your website while connected to that same hot spot will have the same IP address and thus will not be redirected to the web site's maintenance page.

Keep in mind that your IP address may change from time to time, so always double check your IP address before putting your website into maintenance mode. If your IP address has changed since you last edited the htaccess file and you don't change it to the new one, you'll simply be redirected to the maintenance page.
