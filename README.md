# README #

## SERVER ENVIRONMENT SETUP ON GOOGLE CLOUD COMPUTE ENGINE

+ setting environment
    + node
        + `curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -`
        + `sudo apt install nodejs`
        
    + python
        + `sudo apt install python-pip`
        + `pip install virtualenv`
        
    + python3
        + `sudo apt install python3-pip`
        + `pip3 install virtualenv` 

    + mysql
        + `sudo apt-get install mysql-server`
        + `sudo apt-get install libmysqlclient-dev`
          or `sudo apt install default-libmysqlclient-dev`
    
    + postgresql 10.0
        + install
            + `sudo apt update`
            + `sudo apt install postgresql postgresql-contrib`
        + Start PostgreSQL Server
            + `sudo systemctl start postgresql-10.service`
            + `sudo systemctl enable postgresql-10.service`
        + Access db  
            + `su - postgres -c "psql"`
            + or `sudo su -l postgres`   `psql`
            
        + Commands
            + `create role admin_user;`
            + `alter role admin_user with login;`
            + `alter role admin_user  password 'xxxxxx'`
            + `create database xxx;`
            + `grant all privileges on database dropify to admin_user;`
            + show all database `\l`
            + use dropify database `\c dropify`
            + show all tables `\dt`
            + exit `\q`
             
        + give user permission to access postgresql
            + `sudo nano /var/lib/psql/10/data/postgresql.conf` 
            + allow or add line `listen_address = '*'`
            
            ```
            trust
            Allow the connection unconditionally. 
            This method allows anyone that can connect to the PostgreSQL database server 
            to login as any PostgreSQL user they wish, without the need for a password or 
            any other authentication. See Section 19.3.1 for details.
            
            reject
            Reject the connection unconditionally. 
            This is useful for "filtering out" certain hosts from a group, for example a 
            reject line could block a specific host from connecting, while a later line 
            allows the remaining hosts in a specific network to connect.
            
            md5
            Require the client to supply a double-MD5-hashed password for authentication. 
            See Section 19.3.2 for details.
            
            password
            Require the client to supply an unencrypted password for authentication. 
            Since the password is sent in clear text over the network, this should not 
            be used on untrusted networks. See Section 19.3.2 for details.
            
            gss
            Use GSSAPI to authenticate the user. 
            This is only available for TCP/IP connections. See Section 19.3.3 for details.
            
            sspi
            Use SSPI to authenticate the user. 
            This is only available on Windows. See Section 19.3.4 for details.
            
            ident
            Obtain the operating system user name of the client by contacting the ident 
            server on the client and check if it matches the requested database user name. 
            Ident authentication can only be used on TCP/IP connections. When specified 
            for local connections, peer authentication will be used instead. 
            See Section 19.3.5 for details.
            
            peer
            Obtain the client's operating system user name from the operating system 
            and check if it matches the requested database user name. 
            This is only available for local connections. See Section 19.3.6 for details.
            
            ldap
            Authenticate using an LDAP server. See Section 19.3.7 for details.
            
            radius
            Authenticate using a RADIUS server. See Section 19.3.8 for details.
            
            cert
            Authenticate using SSL client certificates. See Section 19.3.9 for details.
            
            pam
            Authenticate using the Pluggable Authentication Modules (PAM) service provided 
            by the operating system. See Section 19.3.10 for details.
    
            ```    

+ setting service for frontend and backend
    + create the service named frontend for angular(react) //this is only for development mode
        + `sudo nano /etc/systemd/system/frontend.service`

            ```
            [Unit]
            Description=service for frontend
            After=network.target

            [Service]
            User=root
            Group=www-data
            WorkingDirectory=/home/xxxxx/frontend
            ExecStart=/usr/bin/npm start

            [Install]
            WantedBy=multi-user.target
            ```
        + start, restart, deamon-reload, stop, status
            ```
            sudo systemctl start frontend
            sudo systemctl restart frontend
            sudo systemctl stop frontend
            sudo systemctl status frontend
            sudo systemctl deamon-relaod
            ```
    + Flask service
        + `pip install gunicorn`
        + `pip install eventlet`
        + `sudo nano /etc/systemd/system/api.service`

            ```
            [Unit]
            Description=Gunicorn instance to serve white glove
            After=network.target

            [Service]
            User=root
            Group=www-data
            WorkingDirectory=/home/xxxx/apiserver
            Environment="FLASK_APP=wgl"
            Environment="PATH=/home/xxxx/apiserver/venv/bin"
            ExecStart=/home/xxxx/apiserver/venv/bin/gunicorn --worker-class eventlet -w 1 --bind 0.0.0.0:8080 -m 007 run:app
            [Install]
            WantedBy=multi-user.target
            ```
        + start, restart, deamon-reload, stop, status
            ```
            sudo systemctl start api
            sudo systemctl restart api
            sudo systemctl stop api
            sudo systemctl status api
            sudo systemctl deamon-api
            ```
            
    + Django service
        + `pip install gunicorn`
        + `sudo nano /etc/systemd/system/backend.service`

            ```
            [Unit]
            Description=Gunicorn instance to serve Django
            After=network.target
            
            [Service]
            User=root
            WorkingDirectory=/home/ubuntu/backend
            Environment="PATH=/home/ubuntu/backend/venv/bin"
            ExecStart=/home/ubuntu/backend/venv/bin/gunicorn -w 3 --bind 0.0.0.0:8080 backend.wsgi
            [Install]
            WantedBy=multi-user.target
            ```
        + start, restart, deamon-reload, stop, status
            ```
            sudo systemctl start backend
            sudo systemctl restart backend
            sudo systemctl stop backend
            sudo systemctl status backend
            sudo systemctl deamon-reload
            ```
            
+ install nginx and configuration
    + install nginx
        +   ```
            sudo apt-get update
            sudo apt-get install nginx
            sudo ufw app list
            sudo ufw allow 'Nginx HTTP'
            sudo ufw status
            systemctl status nginx
            ```
    + configuration for development mode with socket
        + `sudo nano /etc/nginx/sites-available/xxx`
            ```
            server {
                listen 80;
                server_name xxxx.com;
            
                location / {
                    include proxy_params;
                    proxy_pass http://localhost:4200;
                }
            
                location /api/ {
                    include proxy_params;
                   proxy_pass http://localhost:8080/api/;
                }
            
                location /socket {
                    include proxy_params;
                    proxy_http_version 1.1;
                    proxy_buffering off;
                    proxy_set_header Upgrade $http_upgrade;
                    proxy_set_header Connection "Upgrade";
                    proxy_pass http://localhost:8080/socket;
                }
            }
            
            ```
    + configuration for production mode
        + build frontend(angular, react.js) `npm run-script build`
        + `sudo nano /etc/nginx/sites-available/xxx`
            ```
            server {
                listen 80;
                server_name xxxx.com;
            
                location / {
                    include /home/ubuntu/www/frontend/build;
                    index index.html index.htm;
                }
            
                location /api/ {
                    include proxy_params;
                    proxy_pass http://localhost:8080/api/;
                }
            }
            
            ```
    + configuration for fontend & admin page with sub-domain
        + in domain server you should create the sub-domain named admin.xxx.com
        + `sudo nano /etc/nginx/sites-available/xxx`
            ```
            server {
                listen 80;
                server_name xxxx.com;
            
                location / {
                    include /home/ubuntu/www/frontend/build;
                    index index.html index.htm;
                }
            
                location /api/ {
                    include proxy_params;
                    proxy_pass http://localhost:8080/api/;
                }
            }
            
            server {
                listen 80;
                server_name admin.xxxx.com;
            
                location / {
                    include /home/ubuntu/www/admin/build;
                    index index.html index.htm;
                }
            
                location /api/ {
                    include proxy_params;
                    proxy_pass http://localhost:8080/api/;
                }
            }
            
            ```
    
    + `sudo ln -s /etc/nginx/sites-available/white-glove /etc/nginx/sites-enabled`
    + `sudo systemctl restart nginx`
    + `sudo ufw allow 'Nginx Full'`
    + `sudo systemctl restart nginx`


## How To Secure Nginx with Let's Encrypt on Ubuntu 16.04
+ Installing Certbot
    + `sudo add-apt-repository ppa:certbot/certbot`
    + `sudo apt-get update`
    + `sudo apt-get install python-certbot-nginx`
+ Setting up Nginx
    + `sudo nano /etc/nginx/sites-available/xxx`
    + Find the existing server_name line and replace the underscore, _, with your domain name:
        ```
        . . .
        server_name xxx.com;
        . . .
        ```
    + Then, verify the syntax of your configuration edits.
        `sudo nginx -t`
    + `sudo systemctl reload nginx`
+ Allowing HTTPS Through the Firewall
    + `sudo ufw allow 'Nginx Full'`
    + `sudo ufw delete allow 'Nginx HTTP'`
+ Obtaining an SSL Certificate
    + `sudo certbot --nginx -d xxx.com`
+
    ```
    IMPORTANT NOTES:
     - Congratulations! Your certificate and chain have been saved at:
       /etc/letsencrypt/live/xxx.com/fullchain.pem
       Your key file has been saved at:
       /etc/letsencrypt/live/xxx.com/privkey.pem
       Your cert will expire on 2018-12-10. To obtain a new or tweaked
       version of this certificate in the future, simply run certbot again
       with the "certonly" option. To non-interactively renew *all* of
       your certificates, run "certbot renew"
     - If you like Certbot, please consider supporting our work by:

       Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
       Donating to EFF:                    https://eff.org/donate-le

        ```
+ Verifying Certbot Auto-Renewal
    + To test the renewal process, you can do a dry run with certbot:
        `sudo certbot renew --dry-run`


