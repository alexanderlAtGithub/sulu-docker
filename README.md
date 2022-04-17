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
* MySQL: `PROJECT_DOMAIN:PORT_MYSQL` (default: `sulu.localhost:3306`)
* Elasticsearch: `PROJECT_DOMAIN:PORT_ELASTICSEARCH` (default: `sulu.localhost:9200`)

## Install Environment

```bash
git clone https://github.com/alexanderlAtGithub/sulu-docker.git && cd sulu-docker
```

Important, 
IF you have an Sulu (Git-Repository) project, clone it into the directory "sulu"

IF not, create your sulu project with "docker" composer like
```bash
docker run --rm --interactive --tty --volume $PWD:/app composer create-project sulu/skeleton sulu --ignore-platform-reqs
```
something on Sulu side check s the gd lib, in the end you got an error that Gd driver is not installed (which was right, the offical (docker) composer doesnt have the gd)

But now, do 
```bash
docker-compose build --build-arg HAS_XDEBUG=true
```
if you want Xdebug for work !

If not or after build do 

```bash
docker-compose up -d
```
then you have to wait for composer to do his job twice (now without the error for Gd driver is not installed)
If composer has done his job, call in the browser http://sulu.localhost/?XDEBUG_SESSION (if php container has an xdebug, you debug)


The `.env` file contains several environment variables that are used to throughout the environment. 
This allows to configure the project path, database settings, public ports of the services and the domain name.

## Startup Containers

```bash
docker-compose up -d
```

You can also startup the containers in the background by executing:

```bash
docker-compose start
```

## Create Sulu Project

(in docker-service container php)
```bash
# Set service urls to the `.env.local` file
echo "DATABASE_URL=mysql://$MYSQL_USER:$MYSQL_PASSWORD@mysql:3306/$MYSQL_DATABASE" >> .env.local
echo "ELASTICSEARCH_HOST=elasticsearch:9200" >> .env.local

# Initialize sulu project
bin/adminconsole sulu:build dev --destroy
```

After completing these steps the services are accessible via the URLs listed above. If not, do a restart of docker-services. 

## Update Container Configuration

When changing configuration inside of the `docker` folder, the environment must be rebuilt and restarted:

```bash
docker-compose down
docker-compose build
docker-compose up
```
