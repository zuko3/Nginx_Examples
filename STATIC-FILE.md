# Serving Static Content
An important web server task is serving out files (such as images or static HTML pages).
Implementing an example where, depending on the request, files will be served from different local directories: /data/www (which may contain HTML files) and /data/images (containing images). <br/>

This will require editing of the configuration file and setting up of a server block inside the http block with two location blocks. 