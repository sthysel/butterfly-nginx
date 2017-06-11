# butterfly-nginx

![](./docs/butterfly.png)

Install butterfly terminal emulator behind a Let's Encrypt cert secured nginx proxy.

To register with Let's Encrypt use nginx's default setup with the webroot plugin. Once that is done
use the server config detailed below, which also includes the webroot setup, to serve
butterfly.

# Prerequisites

* Ubuntu 16.04 or later server 
* You must own or control the registered domain name that you wish to use the
  certificate with. Make use of any of the many domain name registars. There must be a
  A Record that points your domain to the public IP address of your server.

Recipe follows

## Install nginx 

```bash
$ sudo apt-get install nginx
```

Edit /etc/nginx/sites-available/default to include the .well_known location for the 
webroot plugin that 'Lets Encrypt' will use to verify that you control the domain you
want the cert for.


```
server {
        listen 80 default_server;
        listen [::]:80 default_server;


        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }

       location ~ /.well-known {
                allow all;
        }
}
```

Test with ```$ sudo nginx -t``` and restart ```$ sudo systemctl restart nginx```

## Install certbot

```bash
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install certbot
```

Mint the certificate.

```bash
$ sudo certbot certonly --webroot --webroot-path=/var/www/html -d docker.sthysel.net
```

They appear here:

```bash
INSERT  thys@dockerhost   ~  sudo tree /etc/letsencrypt/live  
[sudo] password for thys: 
/etc/letsencrypt/live
└── docker.sthysel.net
    ├── cert.pem -> ../../archive/docker.sthysel.net/cert1.pem
    ├── chain.pem -> ../../archive/docker.sthysel.net/chain1.pem
    ├── fullchain.pem -> ../../archive/docker.sthysel.net/fullchain1.pem
    ├── privkey.pem -> ../../archive/docker.sthysel.net/privkey1.pem
    └── README
``` 

The butterfly nginx config will use those certs.

### Generate Strong Diffie-Hellman Group 

To further increase security, you should also generate a strong Diffie-Hellman
group. To generate a 2048-bit group, use this command:

```bash
$ sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048 
```

This may take a few minutes but when it's done you will have a strong DH group at
```/etc/ssl/certs/dhparam.pem.```

# Resources

* https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04
* https://github.com/paradoxxxzero/butterfly/wiki/Butterfly-with-nginx-reverse-proxy-and-https
* https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04

