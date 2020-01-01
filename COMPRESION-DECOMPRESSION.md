# Compression and Decompression

Compressing responses often significantly reduces the size of transmitted data. However, since compression happens at runtime it can also add considerable processing overhead which can negatively affect performance. NGINX performs compression before sending responses to clients, but does not “double compress” responses that are already compressed.

# Enabling Compression
To enable compression, include the gzip directive with the on parameter.
```
gzip on;
```

By default, NGINX compresses responses only with MIME type text/html. To compress responses with other MIME types, include the gzip_types directive and list the additional types.
```
gzip_types text/plain application/xml;
```


As with most other directives, the directives that configure compression can be included in the http context or in a server or location configuration block.
```
server {
    gzip on;
    gzip_types      text/plain application/xml;
    ...
}
```

# Enabling Decompression
Some clients do not support responses with the gzip encoding method.<br/>
NGINX can decompress data on the fly when sending it to that type of client.<br/>
To enable runtime decompression, use the gunzip directive.

```
location /storage/ {
    gunzip on;
    ...
}
```

# Sending Compressed Files
To send a compressed version of a file to the client instead of the regular one, set the gzip_static directive to on within the appropriate context.

```
location / {
    gzip_static on;
}
```

In this case, request for /path/to/file, NGINX tries to find and send the file /path/to/file.gz. If the file doesn’t exist, or the client does not support gzip, NGINX sends the uncompressed version of the file.<br/>
Note that the gzip_static directive does not enable on-the-fly compression. It merely uses a file compressed beforehand by any compression tool.

Reference: https://docs.nginx.com/nginx/admin-guide/web-server/compression/


