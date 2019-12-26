# docker-compose-lamp
A local development lamp stack using docker + docker-compose

#Services

- Apache 
	° httpd:2.4.35-alpine
	° Includes a httpd-vhosts file
	° DocumentRoot: public_html
- MySQL
	° mysql:8.0.13
- Php
	° php:7.3-rc-fpm-alpine
