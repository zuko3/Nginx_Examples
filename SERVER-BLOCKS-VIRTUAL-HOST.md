#  Set Up Nginx Server Blocks (Virtual Hosts).
When using the Nginx web server, server blocks we can host more than one domain off of a single server.

# Step One: Set Up New Document Root Directories
By default, Nginx on Ubuntu has one server block enabled by default. It is configured to serve documents out of a directory at /var/www/html.<br/>
This works well for a single site, we need additional directories if we’re going to serve multiple sites. <br/>
We will create a directory structure within /var/www for each of our sites.<br/>
We need to create these directories for each of our sites. The -p flag tells mkdir to create any necessary parent directories along the way:
```
    sudo mkdir -p /var/www/example.com/html
    sudo mkdir -p /var/www/test.com/html
```

The Assign proper permissions of our web roots
```
sudo chmod -R 755 /var/www
```

# Step Two: Create Sample Pages for Each Site
create a default page for each of our sites so that we will have something to display.<br/>
Create an index.html file in your first domain:
```
nano /var/www/example.com/html/index.html
```
Paste below code in index.html file
```
/var/www/example.com/html/index.html
<html>
    <head>
        <title>Welcome to Example.com!</title>
    </head>
    <body>
        <h1>Success!  The example.com server block is working!</h1>
    </body>
</html>
```

our second site is basically going to be the same, we can copy it over to our second document root like this:
```
cp /var/www/example.com/html/index.html /var/www/test.com/html/
```
open the new file in our editor:
```
nano /var/www/test.com/html/index.html
```
Modify it with below code
```
/var/www/test.com/html/index.html

<html>
    <head>
        <title>Welcome to Test.com!</title>
    </head>
    <body>
        <h1>Success!  The test.com server block is working!</h1>
    </body>
</html>
```
We now have some pages to display to visitors of our two domains.