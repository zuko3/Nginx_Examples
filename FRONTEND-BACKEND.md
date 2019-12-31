# FRONTEND BUILD PREPRATION.

Create a React component that fetches data from server.
```
import React, { useEffect } from 'react';
function App(){
    useEffect(() => {
        fetch("http://localhost:3000").then(res=>res.json()).then(data=>console.log(data))	
    },[]);
    return("Front End");
}

export default App
```

Next create the production version build of your frontend application using the command
```
npm run build
```
Create a folder named react-app inside /var/www/<br/>
Copy the contents from build folder to react-app folder(/var/www/react-app)


# BACKEND PREPERATION
Make a simple api that runs on port provided as arguments and output some json

```
// app.js
const http = require('http');
const hostname = '127.0.0.1';
const port =  process.argv[2] || 3000;
const server = http.createServer((req, res) => {
	res.statusCode = 200;
    res.setHeader('Content-Type', 'application/json');
    res.setHeader("Access-Control-Allow-Origin", "*");
    res.end(JSON.stringify({ port: port,host:'localhost'}));
});
server.listen(port, hostname, () => {
	console.log(`Server running at http://${hostname}:${port}/`);
});
```

# SETTING UP CONFIGURATION FILE.
Our node apis are running at two ports 3001 and 3002.<br/>
Our proxy server(Nginx) is listening at port 3000 for api Calls, when ever we make an api call from front end at port 3000 (localhost:3000/api/v1)out proxy server will direct the call to either port 3001 or 3002 based on load (Load Balancing). we dont have to hit 3001 or 3002 we are just exposed port 3000 for apis Nginx takes care of rest.<br/>

Similarly our front end is running at port 80. when we hit localhost:80 it will serve index.html file
from mentioned root directory(/var/www/react-app)<br/>


Copy below configuration to default.config file inside  /etc/ngnix/sites-available

```
upstream app_servers {
    server 127.0.0.1:3001;
    server 127.0.0.1:3002;
}

server {
  listen 3000;
  location / {
        proxy_pass  http://app_servers;
    }
}

server {
    listen 80;
    root /var/www/react-app;
    location /{
    	try_files $uri /index.html;
    }
}
```


Start the node servers
```
node app.js 3001
node app.js 3002
```

Reload the Nginx server
```
systemctl reload nginx
```
Visit localhost:80 to see your application running integrated with backend.<br/>

Some Ngnix commands
```
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
systemctl reload nginx
```