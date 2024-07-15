## Overview

SSL certificates increase security for the Droplet and users by enabling encrypted connections to to server. Use purchase certificates through a commercial SSL certificate authority (CA) or use a free, open source CA like [Let’s Encrypt](https://letsencrypt.org/).

## Prerequisites

Before begin this guide, should have a regular, non-root user with `sudo` privileges configured on the server. Learn how to configure a regular user account by following our [Initial server setup guide for Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu).

Optionally to have registered a domain name before completing the last steps of this tutorial. To learn more about setting up a domain name with [DigitalOcean](https://www.digitalocean.com/), please refer the [Introduction to DigitalOcean DNS](https://docs.digitalocean.com/products/networking/dns/).
## Install Nginx on Ubuntu 22.04

Update the local package index so that have access to the most recent package listings

```bash
sudo apt update
```

Install [[Nginx]]

```bash
sudo apt install nginx
```

Start and enable [[Nginx]]

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

## Secure Nginx with Let's Encrypt on Ubuntu

Install `Snapd`

```bash
sudo apt install snapd
```

Update `Snapd`

```bash
sudo snap install core; sudo snap refresh core
```

Remove any existing [Certbot](https://certbot.eff.org/) packages to avoid conflicts:

```bash
sudo apt-get remove certbot
```

Install [Certbot](https://certbot.eff.org/) using `Snapd`

```bash
sudo snap install --classic certbot
```

Create a symbolic link to ensure [Certbot](https://certbot.eff.org/) command is accessible:

```bash
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

Run [Certbot](https://certbot.eff.org/) to get and install the certificate:

```ssh
sudo certbot --nginx
```

[Certbot](https://certbot.eff.org/) will prompt some information:

- Email address for urgent renewal and security notices.
- Agree to the Terms of Service.
- Choose whether to share the email address with the Electronic Frontier Foundation.
- Enter the domain(s) want to use with the [[Nginx]] server.

[Certbot](https://certbot.eff.org/) will automatically obtain a certificate and modify the [[Nginx]] configuration to use the certificate.

```ssh
sudo certbot --nginx

Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): nurakmaljalil91@gmail.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.4-April-3-2024.pdf. You must agree in
order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y
Account registered.
Please enter the domain name(s) you would like on your certificate (comma and/or
space separated) (Enter 'c' to cancel): akmal-cerxos.xyz
Requesting a certificate for akmal-cerxos.xyz

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/akmal-cerxos.xyz/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/akmal-cerxos.xyz/privkey.pem
This certificate expires on 2024-10-13.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for akmal-cerxos.xyz to /etc/nginx/sites-enabled/default
Congratulations! You have successfully enabled HTTPS on https://akmal-cerxos.xyz

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```
### Verify Renewal

[Certbot](https://certbot.eff.org/) includes a cron job to automatically renew the certificates. Verify this by checking the cron jobs:

```sh
sudo systemctl list-timers | grep certbot
```

### Adjust Firewall (if needed)

Ensure that the firewall is configured to allow HTTPS traffic:
**Allow 'Nginx Full' profile which opens both port 80 and 443:**

```sh
sudo ufw allow 'Nginx Full'
```
### Example Nginx Configuration

If need to manually adjust the [[Nginx]] configuration, find the default configuration file at `/etc/nginx/sites-available/default`. Here is an example of what it might look like after [Certbot](https://certbot.eff.org/) modifies it:

```nginx
##
# You should look at the following URL's in order to grasp a solid understanding
# of Nginx configuration files in order to fully unleash the power of Nginx.
# https://www.nginx.com/resources/wiki/start/
# https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/
# https://wiki.debian.org/Nginx/DirectoryStructure
#
# In most cases, administrators will remove this file from sites-enabled/ and
# leave it as reference inside of sites-available where it will continue to be
# updated by the nginx packaging team.
#
# This file will automatically load configuration files provided by other
# applications, such as Drupal or Wordpress. These applications will be made
# available underneath a path with that package name, such as /drupal8.
#
# Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
##

# Default server configuration
#
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        # SSL configuration
        #
        # listen 443 ssl default_server;
        # listen [::]:443 ssl default_server;
        #
        # Note: You should disable gzip for SSL traffic.
        # See: https://bugs.debian.org/773332
        #
        # Read up on ssl_ciphers to ensure a secure configuration.
        # See: https://bugs.debian.org/765782
        #
        # Self signed certs generated by the ssl-cert package
        # Don't use them in a production server!
        #
        # include snippets/snakeoil.conf;

        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }

        # pass PHP scripts to FastCGI server
        #
        #location ~ \.php$ {
        #       include snippets/fastcgi-php.conf;
        #
        #       # With php-fpm (or other unix sockets):
        #       fastcgi_pass unix:/run/php/php7.4-fpm.sock;
        #       # With php-cgi (or other tcp sockets):
        #       fastcgi_pass 127.0.0.1:9000;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #       deny all;
        #}
}


# Virtual Host configuration for example.com
#
# You can move that to a different file under sites-available/ and symlink that
# to sites-enabled/ to enable it.
#
#server {
#       listen 80;
#       listen [::]:80;
#
#       server_name example.com;
#
#       root /var/www/example.com;
#       index index.html;
#
#       location / {
#               try_files $uri $uri/ =404;
#       }
#}

server {

        # SSL configuration
        #
        # listen 443 ssl default_server;
        # listen [::]:443 ssl default_server;
        #
        # Note: You should disable gzip for SSL traffic.
        # See: https://bugs.debian.org/773332
        #
        # Read up on ssl_ciphers to ensure a secure configuration.
        # See: https://bugs.debian.org/765782
        #
        # Self signed certs generated by the ssl-cert package
        # Don't use them in a production server!
        #
        # include snippets/snakeoil.conf;

        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;
    server_name akmal-cerxos.xyz; # managed by Certbot


        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }

        # pass PHP scripts to FastCGI server
        #
        #location ~ \.php$ {
        #       include snippets/fastcgi-php.conf;
        #
        #       # With php-fpm (or other unix sockets):
        #       fastcgi_pass unix:/run/php/php7.4-fpm.sock;
        #       # With php-cgi (or other tcp sockets):
        #       fastcgi_pass 127.0.0.1:9000;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #       deny all;
        #}


    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/akmal-cerxos.xyz/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/akmal-cerxos.xyz/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}
server {
    if ($host = akmal-cerxos.xyz) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


        listen 80 ;
        listen [::]:80 ;
    server_name akmal-cerxos.xyz;
    return 404; # managed by Certbot
}
```

To link your [[Nginx]] configuration to your containerized `app1` running on port 8080, modify the [[Nginx]] configuration to use a reverse proxy to forward requests to the container.

Here’s how to update the [[Nginx]] configuration: 

**Edit the existing server block to proxy requests to the Docker container.**

**Ensure that your `app1` container is listening on the correct internal network and port.**


Here’s how to modify your existing [[Nginx]] configuration:
### Updated Nginx Configuration

```nginx
# Default server configuration
#
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name akmal-cerxos.xyz www.akmal-cerxos.xyz;

    location / {
        proxy_pass http://127.0.0.1:8080; # Forward to the web app container
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /api/ {
        proxy_pass http://127.0.0.1:5000; # Forward to the web API container
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location ~ /.well-known/acme-challenge {
        allow all;
    }

    listen 443 ssl; # managed by Certbot
    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/akmal-cerxos.xyz/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/akmal-cerxos.xyz/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

# Redirect HTTP traffic to HTTPS
server {
    if ($host = akmal-cerxos.xyz) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

    listen 80;
    listen [::]:80;

    server_name akmal-cerxos.xyz www.akmal-cerxos.xyz;
    return 404; # managed by Certbot
}
```

Test [[Nginx]] Configuration:

```sh
sudo nginx -t
```

Reload [[Nginx]]:

```sh
sudo systemctl reload nginx
```