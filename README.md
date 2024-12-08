# Grafana-and-Prometheus
How to Install Grafana and Prometheus on Ubuntu 24.04

# https://www.howtoforge.com/tutorial/ubuntu-grafana-monitoring/

  
  In this tutorial, you’ll learn how to install Grafana, Prometheus, and node_exporter on Ubuntu 24.04 servers. 
  You’ll also install Nginx as a reverse proxy for Grafana, 
  integrate node_exporter and Prometheus, and then add Prometheus as a data source to the Grafana dashboard.

  Prerequisites
    Before you begin, make sure you have:
    
    Two or three Ubuntu 24.04 servers.
    A non-root user with administrator privileges.
  
  In this section, you’ll install dependencies including Nginx, then add the Grafana repository.
    run the command below to install dependencies for Grafana. Type Y to confirm the installation.
    sudo apt install gnupg2 apt-transport-https software-properties-common wget nginx
    
    Now add Grafana GPG key and repository with the following command.
    
    wget -q -O - https://packages.grafana.com/gpg.key > grafana.key cat grafana.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/grafana.gpg > /dev/null
    echo ‘deb [signed-by=/etc/apt/trusted.gpg.d/grafana.gpg] https://packages.grafana.com/oss/deb stable main’ | sudo tee /etc/apt/sources.list.d/grafana.list
    Then, update and refresh your package index with the command below:
    
    
    sudo apt update
    sudo apt install grafana
    sudo systemctl daemon-reload
    sudo systemctl enable --now grafana-server
    sudo systemctl status grafana-server

    With the grafana-server running, you’ll configure it to run at localhost.
    
    Open the Grafana configuration /etc/grafana/grafana.ini with nano editor.
    
    sudo nano /etc/grafana/grafana.ini
    
    Change the default configuration with the following. Make sure to change the domain option with your local domain name for Grafana. For this example, you’ll run Grafana within the domain howtoforge.local.
    
    [server]
    
    # The IP address to bind to, empty will bind to all interfaces
    http_addr = localhost
    
    # The http port  to use
    http_port = 3000
    
    # The public facing domain name used to access grafana from a browser
    domain = howtoforge.local

  
  When finished, save the file and exit the editor.
  
  Now run the command below to restart grafana-server and apply your changes. With this, Grafana should be running on localhost with the default HTTP port 3000.
  
  sudo systemctl restart grafana-server
  


    
    Setting up Nginx as a reverse proxy
    In this section, you’ll create a new Nginx server block as a reverse proxy for the grafana-server that is running on localhost with port 3000.
    
    Create a new Nginx server block configuration /etc/nginx/sites-available/grafana.conf with nano editor.
    
    sudo nano /etc/nginx/sites-available/grafana.conf
    Add the following configuration to set up Nginx as a reverse proxy for Grafana. Make sure to change the server_name option with your Grafana domain name.
    
    # this is required to proxy Grafana Live WebSocket connections.
    map $http_upgrade $connection_upgrade {
      default upgrade;
      '' close;
    }
    
    server {
        listen      80;
        server_name howtoforge.local;
    
      root /usr/share/nginx/html;
      index index.html index.htm;
    
    
      access_log /var/log/nginx/grafana-access.log;
      error_log /var/log/nginx/grafana-error.log;
    
      location / {
        proxy_set_header Host $http_host;
        proxy_pass http://localhost:3000/;
      }
    
      # Proxy Grafana Live WebSocket connections.
      location /api/live {
        rewrite  ^/(.*)  /$1 break;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_set_header Host $http_host;
        proxy_pass http://localhost:3000/;
      }
    }
    Save the file and exit the editor.
    
    Now run the following command to activate the grafana.conf server block and verify your Nginx syntax. If you’ve proper syntax, you’ll see an output test is Successful - syntax is OK.
    
    sudo ln -s /etc/nginx/sites-available/grafana.conf /etc/nginx/sites-enabled/
    sudo nginx -t
    setup nginx
    
    Next, run the systemctl command below to restart and verify the Nginx web server status.
    
    sudo systemctl restart nginx
    sudo systemctl status nginx
    You’ll see the Nginx web server is running:
    
    check nginx
    
    Next, visit your Grafana domain name such as http://howtoforge.local/. If your installation is successful, you will be prompted with the Grafana login page.
    
    Log in with the default user admin and password admin.




# Grafana 
https://grafana.com/docs/grafana/latest/setup-grafana/installation/docker/


# first go into the directory where you have created this docker-compose.yaml file
cd /path/to/docker-compose-directory

# now create the docker-compose.yaml file
  touch docker-compose.yaml
  
    services:
      grafana:
        image: grafana/grafana-enterprise
        container_name: grafana
        restart: unless-stopped
        ports:
          - '3000:3000'
        volumes:
          - grafana-storage:/var/lib/grafana
    volumes:
      grafana-storage: {}

    
# docker compose up -d


# Create a new Nginx server block configuration /etc/nginx/sites-available/grafana.conf with nano editor.

  sudo nano /etc/nginx/sites-available/grafana.conf

  Add the following configuration to set up Nginx as a reverse proxy for Grafana. Make sure to change the server_name option with your Grafana domain name.
    
    # this is required to proxy Grafana Live WebSocket connections.
    map $http_upgrade $connection_upgrade {
      default upgrade;
      '' close;
    }
    
    server {
        listen      80;
        server_name howtoforge.local;
    
      root /usr/share/nginx/html;
      index index.html index.htm;
    
    
      access_log /var/log/nginx/grafana-access.log;
      error_log /var/log/nginx/grafana-error.log;
    
      location / {
        proxy_set_header Host $http_host;
        proxy_pass http://localhost:3000/;
      }
    
      # Proxy Grafana Live WebSocket connections.
      location /api/live {
        rewrite  ^/(.*)  /$1 break;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_set_header Host $http_host;
        proxy_pass http://localhost:3000/;
      }
    }


# nginx conf


  
  
    834  history | grep nginx
  root@dev9-ubuntu24-1:/etc/nginx/sites-available# nginx -t
  nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
  nginx: configuration file /etc/nginx/nginx.conf test is successful
  root@dev9-ubuntu24-1:/etc/nginx/sites-available# service nginx restart
  root@dev9-ubuntu24-1:/etc/nginx/sites-available# certbot --nginx -d grafana.dev9.store
  Saving debug log to /var/log/letsencrypt/letsencrypt.log
  Requesting a certificate for grafana.dev9.store
  
  Successfully received certificate.
  Certificate is saved at: /etc/letsencrypt/live/grafana.dev9.store/fullchain.pem
  Key is saved at:         /etc/letsencrypt/live/grafana.dev9.store/privkey.pem
  This certificate expires on 2025-03-08.
  These files will be updated when the certificate renews.
  Certbot has set up a scheduled task to automatically renew this certificate in the background.
  
  Deploying certificate
  Successfully deployed certificate for grafana.dev9.store to /etc/nginx/sites-enabled/grafana.dev9.store
  Congratulations! You have successfully enabled HTTPS on https://grafana.dev9.store
  
  - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
  If you like Certbot, please consider supporting our work by:
   * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   * Donating to EFF:                    https://eff.org/donate-le
  - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
  root@dev9-ubuntu24-1:/etc/nginx/sites-available#
  










  server {
          server_name grafana.dev9.store;
  
          location / {
                  proxy_set_header Host $host;
                  proxy_set_header X-Real-IP $remote_addr;
                  #proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                  #proxy_set_header X-Forwarded-Proto $scheme;
                  proxy_pass http://localhost:3000;
          }
  }
 



  server {
          server_name grafana.dev9.store;
  
          location / {
                  proxy_set_header Host $host;
                  proxy_set_header X-Real-IP $remote_addr;
                  #proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                  #proxy_set_header X-Forwarded-Proto $scheme;
                  proxy_pass http://localhost:3000;
          }
  
      listen 443 ssl; # managed by Certbot
      ssl_certificate /etc/letsencrypt/live/grafana.dev9.store/fullchain.pem; # managed by Certbot
      ssl_certificate_key /etc/letsencrypt/live/grafana.dev9.store/privkey.pem; # managed by Certbot
      include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
      ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
  
  }
  
  server {
      if ($host = grafana.dev9.store) {
          return 301 https://$host$request_uri;
      } # managed by Certbot
  
  
  
          server_name grafana.dev9.store;
      listen 80;
      return 404; # managed by Certbot
  }
  ~








# proxy_pass http://localhost:3000;  -> proxy_pass http://192.168.0.26:3000;


![image](https://github.com/user-attachments/assets/00c59c2d-3ab8-4f02-b549-912cd739b2f8)









# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 


# 

