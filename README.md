# butterfly-nginx

Install butterfly terminal emulator behind a Let's Encrypt cert secured nginx proxy.

To register with Let's Encrypt use nginx's default setup with the webroot plugin. Once that is done
use the server config detailed below, which also includes the webroot setup, to serve
butterfly.

# Prerequisites

* Ubuntu 16.04 or later server 
* You must own or control the registered domain name that you wish to use the
  certificate with. Make use of any of the many domain name registars. There must be a
  A Record that points your domain to the public IP address of your server.

# Installs

## nginx

```bash
$ sudo apt-get install nginx
```


# Resources

* https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04
* https://github.com/paradoxxxzero/butterfly/wiki/Butterfly-with-nginx-reverse-proxy-and-https
* https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04

