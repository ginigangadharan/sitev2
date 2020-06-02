# wp-docker

## Install Docker

https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-debian-9

```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg2 software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
sudo apt update

apt-cache policy docker-ce


sudo apt install docker-ce
sudo systemctl status docker
sudo systemctl enable docker
```

Ref:
https://medium.com/@SandeepDinesh/moving-ezschool-com-to-google-cloud-platform-bbb845ccbc97

# docker-compose.yaml

```
version: '3.3'

services:
   db:
     image: mysql:5.7
     volumes:
       - /data/database:/var/lib/mysql
       - /data/.secrets:/var/secrets
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD_FILE: /var/secrets/.secret_db

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "80:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER_FILE: /var/secrets/.secret_wp_db_user
       WORDPRESS_DB_PASSWORD_FILE: /var/secrets/.secret_wp_db_pwd
       WORDPRESS_DB_NAME_FILE: /var/secrets/.secret_wp_db_name
     volumes:
      - /data/wordpress:/var/www/html
      - /data/.secrets:/var/secrets
volumes:
    db_data: 
    wordpress:
```    