# Serving Static Content
An important web server task is serving out files (such as images or static HTML pages).
Implementing an example where, depending on the request, files will be served from different local directories: /data/www (which may contain HTML files) and /data/images (containing images). <br/>

This will require editing of the configuration file and setting up of a server block inside the http block with two location blocks. <br/>

First, create the /data/www directory and put an index.html file with any text content into it and create the /data/images directory and place some images in it. <br/><br/>

Open the configuration file, comment out all server blocks and start a new server block: 
```
server 
    location / {
        root /data/www;
    }

    location /images/ {
        root /data;
    }
}
```

This is a working configuration of a server that listens on the standard port 80 and is accessible on the local machine at http://localhost/. In response to requests with URIs starting with /images/, the server will send files from the /data/images directory.<br/><br/>
For example, in response to the http://localhost/images/example.png request nginx will send the /data/images/example.png file. If such file does not exist, nginx will send a response indicating the 404 error.<br/><br/>
Requests with URIs not starting with /images/ will be mapped onto the /data/www directory.<br/>
For example, in response to the http://localhost/some/example.html request nginx will send the /data/www/some/example.html file.


# Trying Several Options
The try_files directive can be used to check whether the specified file or directory exists; NGINX makes an internal redirect if it does, or returns a specified status code if it doesn’t.

```
server {
    root /www/data;
    location / {
        try_files $uri $uri/ /test/index.html;
    }
}
```
Here try_files means when you receive a URI that's matched by this block try $uri first, for example http://example.com/images/image.jpg nginx will try to check if there's a file inside /images called image.jpg if found it will serve it first.<br/><br/>

Second condition is $uri/ which means if you didn't find the first condition $uri try the URI as a directory, for example http://example.com/images/, ngixn will first check if a file called images exists then it wont find it, then goes to second check $uri/ and see if there's a directory called images exists then it will try serving it.
```
if you don't have autoindex on you'll probably get a 403 forbidden error, because directory listing is forbidden by default.
```

Third condition /test/index.html is considered a fall back option, (you need to use at least 2 options, one and a fall back),nginx will look for the file index.html inside the folder test and serve it if it exists.<br/><br/>
If the third condition fails too, then nginx will serve the 404 error page.

# Tip
If you only have 1 condition you want to serve, like for example inside folder images you only want to either serve the image or go to 404 error, you can write a line like this:
```
location /images {
    try_files $uri =404;
}
```
which means either serve the file or serve a 404 error, you can't use only $uri by it self without =404 because you need to have a fallback option.<br/>


# Trying Several Options
The try_files directive can be used to check whether the specified file or directory exists; NGINX makes an internal redirect if it does, or returns a specified status code if it doesn’t. 
```
server {
    root /www/data;

    location /images/ {
        try_files $uri /images/default.gif;
    }
}
```
The file is specified in the form of the URI, which is processed using the root.
For example, in response to the http://localhost/images/example.png request nginx will check if inside images folder example.png exists if yes then it send the /www/data/images/example.png file.<br/><br/>

If the file corresponding to the original URI doesn’t exist, NGINX makes an internal redirect to the URI  specified as fallback, returning /www/data/images/default.gif.