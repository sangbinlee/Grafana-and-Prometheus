# Grafana-and-Prometheus
How to Install Grafana and Prometheus on Ubuntu 24.04








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


# 




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

