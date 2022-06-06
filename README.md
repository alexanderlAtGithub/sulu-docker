# Sulu-Docker

**Development environment** for the [Sulu 2.4](https://sulu.io/) content management platform built with 
[Docker Compose](https://docs.docker.com/compose/).

> Docker is a great tool for trying new technologies effortlessly. Unfortunately, there are still significant
> [filesystem performance issues](https://github.com/docker/for-mac/issues/1592) when using bind mounts on some 
> systems. 
>
> If you are experiencing bad performance, we recommend to use dockerized services (MySQL, Elasticsearch) in
> combination with the [Symfony Local Web Server](https://symfony.com/doc/current/setup/symfony_server.html).
> For example, the [Sulu Demo repository](https://github.com/sulu/sulu-demo) includes such a setup.

## URLs

* Sulu-Website: `PROJECT_DOMAIN:PORT_NGINX` (default: `sulu.localhost`)
* Sulu-Admin: `PROJECT_DOMAIN:PORT_NGINX/admin` (default: `sulu.localhost/admin`)
* Elasticsearch: `PROJECT_DOMAIN:PORT_ELASTICSEARCH` (default: `sulu.localhost:9200`)

# About this Fork

its for education, its not the holy truth ...

## Install Environment

```bash
git clone https://github.com/alexanderlAtGithub/sulu-docker.git && cd sulu-docker
```

## Important I 
IF you have an Sulu 2 (Git-Repository) project, clone it into the directory "sulu" 
(or configure .env "PROJECT_PATH" & "PROJECT_PATH_PUBLIC" variable to fit your needs, add the new directory name in .gitignore too)

## Startup Containers

```bash
docker-compose up -d
```

## Stop Containers

```bash
docker-compose down --remove-orphans
```

## Important II
There are two PHP Container, first one with Port 80 (http://sulu.localhost) and with Port 8080 (http://sulu.localhost:8080) for debugging (XDebug). Configure your IDE with debugging with service "xdebug".

## Important III 
be aware of comments in docker-composer.yml, some services are not for production 

## Create your Sulu Project

IF you haven't any Sulu 2 project now, create your sulu project with "docker-composer" like
```bash
docker-compose run installsulu 
```
do uncomment the service "installsulu", reset any changes to environment variables, and then do docker-composer run installsulu
after this, you have to manually move files from "mysulu" to "PROJECT_PATH"
```bash
$ cd sulu && mv {mysulu/*,mysulu/.*} . 
```
("mysulu/" see ${INSTALL_SULU} in .env) 


(change into docker-service container php)
and here do 
```bash
# Set service urls to the `.env.local` file
echo "DATABASE_URL=mysql://$MYSQL_USER:$MYSQL_PASSWORD@mysql:3306/$MYSQL_DATABASE" >> .env.local
echo "ELASTICSEARCH_HOST=elasticsearch:9200" >> .env.local

# Initialize sulu project
bin/adminconsole sulu:build dev --destroy
```

## And
software quality as is ..
and if question, asked
