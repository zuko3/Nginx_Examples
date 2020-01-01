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

# Step Three: Create Server Block Files for Each Domain

# Create the First Server Block File
Only one of our server blocks on the server can have the default_server option enabled. <br/>
This specifies which block should serve a request if the server_name requested does not match any of the available server blocks.<br/>

You can choose to designate one of your sites as the “default” by including the default_server option in the listen directive, or you can leave the default server block enabled, which will serve the content of the /var/www/html directory if the requested host cannot be found.

<br/>You can check that the default_server option is only enabled in a single active file by typing:
```
grep -R default_server /etc/nginx/sites-enabled/
```
If matches are found uncommented in more than on file (shown in the leftmost column), Nginx will complain about an invalid configuration.

<br/>Now Create the config for First domain example.com

```
sudo nano /etc/nginx/sites-available/example.com
```
Add Below code to the file

```
server {
        listen 80;
        listen [::]:80;

        root /var/www/example.com/html;
        index index.html index.htm index.nginx-debian.html;

        server_name example.com www.example.com;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

# Create the Second Server Block File
Create the config file for second domain.
```
sudo nano /etc/nginx/sites-available/test.com
```
Add Below code to the file
```
server {
        listen 80;
        listen [::]:80;

        root /var/www/test.com/html;
        index index.html index.htm index.nginx-debian.html;

        server_name test.com www.test.com;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

# Step Four: Enable your Server Blocks and Restart Nginx
Now we need to enable server blocks, We can do this by creating symbolic links from these files to the sites-enabled directory, which Nginx reads from during startup.

```
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/test.com /etc/nginx/sites-enabled/
```

example.com: Will respond to requests for example.com and www.example.com<br/>
test.com: Will respond to requests for test.com and www.test.com<br/>
default: Will respond to any requests on port 80 that do not match the other two blocks.<br/>

Test to make sure that there are no syntax errors in any of your Nginx files
```
sudo nginx -t
```

restart Nginx to enable your changes, Nginx should now be serving both of your domain names

```
sudo systemctl restart nginx
```