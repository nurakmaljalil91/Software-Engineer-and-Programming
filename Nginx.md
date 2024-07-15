## Overview

[Nginx](https://www.nginx.com/) is one of the most popular web servers in the world and is responsible for hosting some of the largest and highest-traffic sites on the internet. It is a lightweight choice that can be used as either a web server or reverse proxy.
## Prerequisites

To begin this guide, have a regular, non-root user with `sudo` privileges configured on the server. Learn how to configure a regular user account by following this [Initial server setup guide for Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu).

Optionally want to have registered a domain name before completing the last steps of this tutorial. To learn more about setting up a domain name with [DigitalOcean](https://www.digitalocean.com/), please refer to the [Introduction to DigitalOcean DNS](https://docs.digitalocean.com/products/networking/dns/).

When have an account available, log in as non-root user to begin.

## Installing Nginx

Because [Nginx](https://www.nginx.com/) is available in Ubuntu’s default repositories, it is possible to install it from these repositories using the `apt` packaging system.

Since this is the first interaction with the `apt` packaging system in this session, update the local package index so that have access to the most recent package listings. Afterwards, install `nginx`:

```bash
sudo apt update
sudo apt install nginx
```


Press `Y` when prompted to confirm installation. If you are prompted to restart any services, press `ENTER` to accept the defaults and continue. `apt` will install [Nginx](https://www.nginx.com/) and any required dependencies to your server.

## Adjusting the Firewall

Before testing [Nginx](https://www.nginx.com/), the firewall software needs to be configured to allow access to the service. [Nginx](https://www.nginx.com/) registers itself as a service with `ufw` upon installation, making it straightforward to allow [Nginx](https://www.nginx.com/) access.

List the application configurations that `ufw` knows how to work with by typing:

```bash
sudo ufw app list
```

Should get a listing of the application profiles:

```bash
OutputAvailable applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```

As demonstrated by the output, there are three profiles available for Nginx:

- **Nginx Full**: This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)
- **Nginx HTTP**: This profile opens only port 80 (normal, unencrypted web traffic)
- **Nginx HTTPS**: This profile opens only port 443 (TLS/SSL encrypted traffic)

It is recommended that to enable the most restrictive profile that will still allow the traffic to be configured. Right now, only need to allow traffic on port 80.

Enable this by typing:

```bash
sudo ufw allow 'Nginx HTTP'
```

Verify the change by typing:

```bash
sudo ufw status
```

The output will indicated which HTTP traffic is allowed:

```bash
OutputStatus: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
Nginx HTTP                 ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```

## Checking your Web Server

At the end of the installation process, Ubuntu 22.04 starts [Nginx](https://www.nginx.com/). The web server should already be up and running.

Check with the `systemd` init system to make sure the service is running by typing:

```bash
systemctl status nginx
```

```bash
Output● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2022-03-01 16:08:19 UTC; 3 days ago
     Docs: man:nginx(8)
 Main PID: 2369 (nginx)
    Tasks: 2 (limit: 1153)
   Memory: 3.5M
   CGroup: /system.slice/nginx.service
           ├─2369 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
           └─2380 nginx: worker process
```

As confirmed by this out, the service has started successfully. However, the best way to test this is to actually request a page from [Nginx](https://www.nginx.com/).

Access the default Nginx landing page to confirm that the software is running properly by navigating to your server’s IP address. If do not know your server’s IP address, find it by using the [icanhazip.com](http://icanhazip.com/) tool, which will give the public IP address as received from another location on the internet:

```bash
curl -4 icanhazip.com
```

When you have the server’s IP address, enter it into your browser’s address bar:

```
http://your_server_ip
```

You should receive the default Nginx landing page:

![Nginx default page](https://assets.digitalocean.com/articles/nginx_1604/default_page.png)

If on this page, the server is running correctly and is ready to be managed.
## Managing the Nginx Process

Now that  web server up and running, let’s review some basic management commands.

To stop the web server, type:

```bash
sudo systemctl stop nginx
```

To start the web server when it is stopped, type:

```bash
sudo systemctl start nginx
```

To stop and then start the service again, type:

```bash
sudo systemctl restart nginx
```

If  only making configuration changes, [Nginx](https://www.nginx.com/) can often reload without dropping connections. To do this, type:

```bash
sudo systemctl reload nginx
```

By default, [Nginx](https://www.nginx.com/) is configured to start automatically when the server boots. If this is not wanted, disable this behavior by typing:

```bash
sudo systemctl disable nginx
```

To re-enable the service to start up at boot,  type:

```bash
sudo systemctl enable nginx
```

Now learned basic management commands and should be ready to configure the site to host more than one domain.

## Setting Up Server Blocks (Recommended)

When using the [Nginx](https://www.nginx.com/) web server, _server blocks_ (similar to virtual hosts in Apache) can be used to encapsulate configuration details and host more than one domain from a single server. Set up a domain called **your_domain**, but should **replace this with the domain name**.

[Nginx](https://www.nginx.com/) on Ubuntu 22.04 has one server block enabled by default that is configured to serve documents out of a directory at `/var/www/html`. While this works well for a single site, it can become unwieldy if hosting multiple sites. Instead of modifying `/var/www/html`, let’s create a directory structure within `/var/www` for the **your_domain** site, leaving `/var/www/html` in place as the default directory to be served if a client request doesn’t match any other sites.

Create the directory for **your_domain** as follows, using the `-p` flag to create any necessary parent directories:

```bash
sudo mkdir -p /var/www/your_domain/html
```

Next, assign ownership of the directory with the `$USER` environment variable:

```bash
sudo chown -R $USER:$USER /var/www/your_domain/html
```

The permissions of the  web roots should be correct if  haven’t modified the `umask` value, which sets default file permissions. To ensure that the permissions are correct and allow the owner to read, write, and execute the files while granting only read and execute permissions to groups and others, input the following command:

```bash
sudo chmod -R 755 /var/www/your_domain
```

Next, create a sample `index.html` page using `nano` or any editor:

```bash
nano /var/www/your_domain/html/index.html
```

Inside, add the following sample HTML:

`/var/www/your_domain/html/index.html`

```html
<html>
    <head>
        <title>Welcome to your_domain!</title>
    </head>
    <body>
        <h1>Success!  The your_domain server block is working!</h1>
    </body>
</html>
```

Save and close the file by pressing `Ctrl+X` to exit, then when prompted to save, `Y` and then `Enter`.

In order for [Nginx](https://www.nginx.com/) to serve this content, it’s necessary to create a server block with the correct directives. Instead of modifying the default configuration file directly, let’s make a new one at `/etc/nginx/sites-available/==your_domain==`:

```bash
sudo nano /etc/nginx/sites-available/your_domain
```

Paste in the following configuration block, which is similar to the default, but updated for our new directory and domain name:

`/etc/nginx/sites-available/your_domain`

```nginx
server {
        listen 80;
        listen [::]:80;

        root /var/www/your_domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain www.your_domain;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

Notice that  updated the `root` configuration to the new directory, and the `server_name` to the domain name.

Next, let’s enable the file by creating a link from it to the `sites-enabled` directory, which [Nginx](https://www.nginx.com/)reads from during startup:

```bash
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```

**Note:** [Nginx](https://www.nginx.com/) uses a common practice called symbolic links, or symlinks, to track which of your server blocks are enabled. Creating a symlink is like creating a shortcut on disk, so that you could later delete the shortcut from the `sites-enabled` directory while keeping the server block in `sites-available` if you wanted to enable it.

Two server blocks are now enabled and configured to respond to requests based on their `listen` and `server_name` directives (you can read more about how Nginx processes these directives [here](https://www.digitalocean.com/community/tutorials/understanding-nginx-server-and-location-block-selection-algorithms)):

- `your_domain`: Will respond to requests for `your_domain` and `www.your_domain`.
- `default`: Will respond to any requests on port 80 that do not match the other two blocks.

To avoid a possible hash bucket memory problem that can arise from adding additional server names, it is necessary to adjust a single value in the `/etc/nginx/nginx.conf` file. Open the file:

```bash
sudo nano /etc/nginx/nginx.conf
```

Find the `server_names_hash_bucket_size` directive and remove the `#` symbol to uncomment the line. If you are using nano, you can quickly search for words in the file by pressing `CTRL` and `w`.

**Note:** Commenting out lines of code – usually by putting `#` at the start of a line – is another way of disabling them without needing to actually delete them. Many configuration files ship with multiple options commented out so that they can be enabled or disabled, by toggling them between active code and documentation.

`/etc/nginx/nginx.conf`

```nginx
...
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}
...
```

Save and close the file when you are finished.

Next, test to make sure that there are no syntax errors in any of your Nginx files:

```bash
sudo nginx -t
```

If there aren’t any problems, restart [Nginx](https://www.nginx.com/) to enable your changes:

```bash
sudo systemctl restart nginx
```

[Nginx](https://www.nginx.com/) should now be serving your domain name. You can test this by navigating to `http://==your_domain==`, where you should see something like this:

![Nginx first server block](https://assets.digitalocean.com/articles/how-to-install-nginx-u18.04/your-domain-server-block-nginx.PNG)

##  Getting Familiar with Important Nginx Files and Directories

Now that you know how to manage the [Nginx](https://www.nginx.com/) service itself, you should take a few minutes to familiarize yourself with a few important directories and files.

### Content

- `/var/www/html`: The actual web content, which by default only consists of the default [Nginx](https://www.nginx.com/)page you saw earlier, is served out of the `/var/www/html` directory. This can be changed by altering [Nginx](https://www.nginx.com/) configuration files.

### Server Configuration

- `/etc/nginx`: The [Nginx](https://www.nginx.com/) configuration directory. All of the [Nginx](https://www.nginx.com/) configuration files reside here.
- `/etc/nginx/nginx.conf`: The main [Nginx](https://www.nginx.com/) configuration file. This can be modified to make changes to the [Nginx](https://www.nginx.com/) global configuration.
- `/etc/nginx/sites-available/`: The directory where per-site server blocks can be stored. [Nginx](https://www.nginx.com/) will not use the configuration files found in this directory unless they are linked to the `sites-enabled` directory. Typically, all server block configuration is done in this directory, and then enabled by linking to the other directory.
- `/etc/nginx/sites-enabled/`: The directory where enabled per-site server blocks are stored. Typically, these are created by linking to configuration files found in the `sites-available` directory.
- `/etc/nginx/snippets`: This directory contains configuration fragments that can be included elsewhere in the [Nginx](https://www.nginx.com/) configuration. Potentially repeatable configuration segments are good candidates for refactoring into snippets.

### Server Logs

- `/var/log/nginx/access.log`: Every request to your web server is recorded in this log file unless Nginx is configured to do otherwise.
- `/var/log/nginx/error.log`: Any [Nginx](https://www.nginx.com/) errors will be recorded in this log.

## Conclusion

Now that you have your web server installed, you have many options for the type of content to serve and the technologies you want to use to create a richer experience.

If you’d like to build out a more complete application stack, check out the article [How To Install Linux, Nginx, MySQL, PHP (LEMP stack) on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu).

In order to set up HTTPS for your domain name with a free SSL certificate using _Let’s Encrypt_, you should move on to [How To Secure Nginx with Let’s Encrypt on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-22-04).


## Read More

- [https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04#step-5-%E2%80%93-setting-up-server-blocks-(recommended)](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04#step-5-%E2%80%93-setting-up-server-blocks-(recommended))