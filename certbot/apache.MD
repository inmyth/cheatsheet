## Apache with redirect

https://www.digitalocean.com/community/tutorials/how-to-redirect-www-to-non-www-with-apache-on-ubuntu-14-04


- set DNS tables with 
    - A @ IP
	- A www IP
- install Apache2
- setup my-domain.conf with ServerName and ServerAlias, create /var/www/my_domain (https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-18-04)
- create .htaccess in the root folder, create redirect rules naked-www or www-naked(https://www.digitalocean.com/community/tutorials/how-to-redirect-www-to-non-www-with-apache-on-ubuntu-14-04)
- create certbot for two domains my-domain and www.my-domain 




