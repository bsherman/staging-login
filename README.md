staging-login
==================
HOW TO USE

The idea is that you have a directory or website for  which you want some limited
access. Perhaps just enough to keep out search spiders and users who might stumble
onto it.

This could be done with basic auth and .htaccess solutions, but if you have apps
running on that site which don't like basic auth, or already use it for something
else, you need a different solution.

This solution uses Apache mod\_rewrite to check for the presence of a cookie which
verifies that the user should have access.

This static HTML file includes all you need to create that cookie using javascript
and no external dependencies.

At any location where mod\_rewrite directives may be used and which you wish to
protect, include the following in your httpd.conf:
--------SNIP---------
    RewriteEngine On
    RewriteCond %{HTTP_COOKIE} !staging-login
    RewriteCond %{REQUEST_FILENAME} !/staging-login.html
    RewriteRule ^(.*)$ /staging-login.html?ref=$1 [R,L]
--------SNIP---------

Note that this is not intended to be a high security mechanism, only a
simple deterrent messure. As such, I'm making a known faux-paux by storing the
password hash in this page. If you do want to keep out any users, ensure you
change the username and password from the defaults below, else they'll be able
to view the source and automatically know the username and password.

Even more, this is not secure because at best, it's security through obscurity.
Anyone reading this file will know your "secure" cookie name and valid required
for site access. So again, this is inteded only as a simple deterrent and is not
a true secure system.

Note: For information on mod\_rewrite and where it directives may be used
see: http://httpd.apache.org/docs/2.2/mod/mod\_rewrite.html

