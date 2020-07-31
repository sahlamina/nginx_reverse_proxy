# Reverse Proxy with NGINX
---
### Key definitions for context
- Web server:
    - In simple terms, a web server is a is a device that accepts requests from a client
     processes the request and then 'serves' the request back to the client.
 - Proxy server (forward proxy)
    - A proxy server is like a web server in that it fetches and serves client requests. However the two are distinct because a proxy server acts as like conduit between the client and the web server.
    - When a proxy server is used, the request leaves the client, reaches the proxy server which then retrieves the request from the web server and returns the result to the client.
    - The benefits of this are the added security benefits, privacy benefits, control of internet usage of employees or children and access to blocked resources
    
---
## This Read me covers: 
- Reverse Proxy servers
- How to create a reverse proxy server
- Automating the Process using Bash script
### What is a reverse proxy server?
- Reverse proxies provide anonymity for the back-end servers, not the clients. They mask the real location of the servers clients are accessing
- A forward proxy is the intermediary that the client puts forward between itself and any server. The reverse proxy is at the other end – something the server puts forward between itself and any client.
- In the case of this project, the aim was to provide anonymity for the back end servers by disguising the IP address. Instead of seeing ""development.local:3000/"" in the url, the user should be able to see the same web pages without seeing the port address. 

### How to create a reverse proxy:
- First we must navigate to the sites-available file. 
- In this file, we the default code that we delete ``sudo rm -r default``. We then create a new default file `` touch default`` and inject it with the preferred configuration.
- This is done through `` nano default``
- The following block of code allows for the creation of a proxy server:
``
server {
    listen 80;
    server_name _;
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
``
- We can then run the check if the code is having the desired effect by running ``node app.js``
in the VM's app directory
 
### Automating the process
- The process can be said to be automated when one command allows for all the navigating, default file deletion and reconstruction is not necessary to see the desired web pages.
- I did this by adding code which navigated to, deleted and updated default file in my provision.sh file:
 ``sudo unlink /etc/nginx/sites-enabled/default ``
- ``sudo ln -s /home/ubuntu/environment/default /etc/nginx/sites-enabled/default``
- I also created a default file in my environment directory and injected the configuration of my reverse proxy into it.
- The result of this was an automation of the reverse proxy creating process .