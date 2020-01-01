# Nginx reverse proxy
A reverse proxy is an intermediary proxy service which takes a client request, passes it on to one or more servers, and subsequently delivers the server's response to the client.<br/><br/>
To pass a request to an HTTP proxied server, the proxy_pass directive is specified inside a location. For example:

```
location /some/path/ {
    proxy_pass http://www.example.com/link/;
}

location /someother/path/ {
    proxy_pass http://localhost:8000;
}
```

Reference: https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/