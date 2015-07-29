# Docker Piwik Nginx Image (francoisp/piwik)
_maintained by francoisp_

This image's scripts are derived from marvambass's Piwik image. It has enough state to be restarteable, and is based straight on the nginx reference image for clarity. It downloads the piwik install from the piwik website so if you build the image you'll get the most recent stable version of piwik.

[FAQ - marvambass's Containers](https://marvin.im/docker-faq-all-you-need-to-know-about-the-marvambass-containers/)

This Dockerfile (available as ___francoisp/piwik___) gives you a completly secure piwik.

The php nginx setup comes from the commands in this [marvambass/nginx-ssl-php](https://registry.hub.docker.com/u/marvambass/nginx-ssl-php/) Image

View in Docker Registry [francoisp/piwik-nginx](https://registry.hub.docker.com/u/francoisp/piwik-nginx/)

View in GitHub [francoisp/docker-piwik-nginx](https://github.com/francoisp/docker-piwik-nginx)

## Environment variables and defaults

### For Headless installation required

Piwik Database Settings

* __PIWIK\_MYSQL\_USER__
 * no default - if null it will start piwik in initial mode
* __PIWIK\_MYSQL\_PASSWORD__
 * no default - if null it will start piwik in initial mode
* __PIWIK\_MYSQL\_HOST__
 * default: _mysql_
* __PIWIK\_MYSQL\_PORT__
 * default: _3306_ - if you use a different mysql port change it
* __PIWIK\_MYSQL\_DBNAME__
 * default: _piwik_ - don't use the symbol __-__ in there!
* __PIWIK\_MYSQL\_PREFIX__
 * default: _piwik\__
 
Piwik Admin Settings

* __PIWIK\_ADMIN__
 * default: admin - the name of the admin user
* __PIWIK\_ADMIN\_PASSWORD__
 * default: [randomly generated 10 characters] - the password for the admin user
* __PIWIK\_ADMIN\_MAIL__
 * default: no@no.tld - only needed if you are interested in one of those newsletters
* __PIWIK\_SUBSCRIBE\_NEWSLETTER__
 * __1__ or __0__ - default: _0_
* __PIWIK\_SUBSCRIBE\_PRO\_NEWSLETTER__
 * __1__ or __0__ - default: _0_

Website to Track Settings

* __SITE\_NAME__
 * default: _My local Website_
* __SITE\_URL__
 * default: _http://localhost_
* __SITE\_TIMEZONE__
 * default: _Europe/Berlin_
* __SITE\_ECOMMERCE__
 * __1__ or __0__ - default: _0_

Piwik Track Settings

* __ANONYMISE\_IP__
 * __1__ or __0__ - this will anonymise IPs - default: _1_
* __DO\_NOT\_TRACK__
 * __1__ or __0__ - this will skip browsers with do not track enabled from tracking - default: _1_
 
### Misc Settings

* __PIWIK\_RELATIVE\_URL\_ROOT__
 * default: _/piwik/_ - you can chance that to whatever you want/need
* __PIWIK\_NOT\_BEHIND\_PROXY__
 * default: not set - if set to any value the settings to listen behind a reverse proxy server will be removed
* __PIWIK\_HSTS\_HEADERS\_ENABLE__
 * default: not set - if set to any value the HTTP Strict Transport Security will be activated on SSL Channel
* __PIWIK\_HSTS\_HEADERS\_ENABLE\_NO\_SUBDOMAINS__
 * default: not set - if set together with __PIWIK\_HSTS\_HEADERS\_ENABLE__ and set to any value the HTTP Strict Transport Security will be deactivated on subdomains

### Previously Inherited Variables

* __DH\_SIZE__
 * default: 2048 if you need more security just use a higher value
 * as was inherited from [MarvAmBass/docker-nginx-ssl-secure](https://github.com/MarvAmBass/docker-nginx-ssl-secure)

## Using the francoisp/piwik Container

First you need a MySQL Container (the following command would give you one from the plain vanilla mysql )

sudo docker run --name piwik-mysql -e MYSQL_ROOT_PASSWORD=strongMysqlpw -e MYSQL_DATABASE=piwik -e MYSQL_USER=piwik -e MYSQL_PASSWORD=strongPiwikpw  -d mysql

Then you can create a container based on this source compiled as an image from dockerhub that _--link_ the myslql one:

sudo docker run -d -p 80:80 -p 443:443 --name piwik --link piwik-mysql:mysql -e 'PIWIK_MYSQL_USER=piwik' -e 'PIWIK_MYSQL_PASSWORD=strongPiwikpw' -e 'PIWIK_ADMIN_PASSWORD=piwikPASS' francoisp/piwik-nginx

If you need to build the image from source, for example to get the latest piwik archive in the image, you can clone this repo locally, and run

sudo docker build -t youruser/piwik-nginx .


