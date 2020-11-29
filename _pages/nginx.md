---
layout: post
title: nginx configuration samples
author: gini
categories: [ web ]
image: "assets/images/2020/pexels-luis-gomes-codes.jpg"
tags: [ web, nginx, proxy ]
show-avatar: true
permalink: /nginx
featured: false
hidden: false
titleshort: nginx
---

- [Basic website configuration](#basic-website-configuration)
- [nginx `proxy_pass` - `site2.com` -> `site1.com/site2-page`](#nginx-proxy_pass---site2com---site1comsite2-page)
- [Basic Reverse Proxy Configuration](#basic-reverse-proxy-configuration)


## Basic website configuration

```
server {
    listen 8080;
    listen [::]:8080;

    server_name example.com;
    root /var/www/example;
    index index.php index.html index.htm;
    access_log /var/log/nginx/access_example.log;
    error_log /var/log/nginx/error_example.log;
}
```

## nginx `proxy_pass` - `site2.com` -> `site1.com/site2-page`

**Scenarios**
- nginx and wordpress - `site2.com` need to load content of of `site1.com/site2` without url change
- show a landing page for **subdomain** (`sub.site1.com`) by using `site1.com/sub-page`
- How to Redirect a Domain without Changing the URL on browser.

Sample nginx configuration file for `site2.com`

```
server {    
    index index.php index.html index.htm;

    server_name site2.com www.site2.com;

    access_log /var/log/nginx/access_site2.log;
    error_log /var/log/nginx/error_site2.log;

    location / {
        proxy_pass https://www.site1.com/site2/;
        proxy_ssl_server_name on;
    }

    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/site2.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/site2.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}
server {
    if ($host = www.site2.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

    if ($host = site2.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

    listen 80;
    listen [::]:80;
    server_name site2.com www.site2.com;
    return 404; # managed by Certbot
}
```

## Basic Reverse Proxy Configuration

```
server {
  listen 80;
  listen [::]:80;

  access_log /var/log/nginx/reverse-access.log;
  error_log /var/log/nginx/reverse-error.log;

  location / {
    proxy_pass http://127.0.0.1:8080;
  }
}
```

(*Photo by [luis gomes](https://www.pexels.com/@luis-gomes-166706?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from Pexels*)

# References

- [How to Redirect a Domain without Changing the URL(.htaccess and apache)](https://www.inmotionhosting.com/support/domain-names/redirect-without-changing-url/)
- [My post on serverfault.com](https://serverfault.com/questions/1044292/nginx-and-wordpress-site2-com-need-to-load-content-of-of-site1-com-site2-witho)