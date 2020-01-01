# Proxying HTTP Traffic to a Group of Servers

NGINX is used to load balance HTTP traffic to a group of servers.
<br/>First you need to define the group with the upstream directive. The directive is placed in the http context.
<br/>To pass requests to a server group, the name of the group is specified in the proxy_pass directive.

<br/>The following example combines the two snippets above and shows how to proxy HTTP requests to the backend server group.

```
http {
    upstream backend {
        server backend1.example.com;
        server backend2.example.com;
        server 192.0.0.1 backup;
    }
    
    server {
        location / {
            proxy_pass http://backend;
        }
    }
}
```

Reference: https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/