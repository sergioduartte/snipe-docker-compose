# snipe-it with docker-compose
**Atention!** This is **NOT** a oficial branch of [**Snipe-it**](https://snipeitapp.com/). This is a simple deployment with docker-compose and some configs that worked for me.

## Building the service

1. ### Prepare a new environment

     I choose the Ubuntu 16.04 LTS to deploy everything into, you can work on another that have at least 2 GB of RAM and 1 CPU.

    1.1 Update the packages. 
	```shell
	sudo apt -y update && sudo apt -y dist-upgrade
	```

2. ### Install `docker` 
	```shell
	curl -fsSL https://get.docker.com -o get-docker.sh
	sh get-docker.sh
	```

3. ### And `docker-compose`
	```shell 
	sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`- `uname -m` -o /usr/local/bin/docker-compose
	```
	```shell
	chmod +x /usr/local/bin/docker-compose
	```
4. ### Configuring the application
	4.1 Pass the files from this repository to the deployment machine. Maybe you'll need sudo to do some operations here. If you're rebuilding the application (see the section below about backup for more details), please use `rsync -ahzv /src/folder/ /dest/folder` to avoid [_fadiga_](https://www.youtube.com/watch?v=c4k-cRUFgyI).
	
	4.2 Configure the variables, walkthrough the `.my-env-file`and `docker-compose.yml` and configure your deployment as you wish.

5. ### Running the application
	```shell
	sudo docker-compose up -d
	```
6. ### Mail Service 
	To the Mail service work properly
	6.1  Get in the container
	```shell
	sudo docker exec -it snipe-it /bin/bash
	```
	6.2 Run the commands
	```shell
	openssl genrsa -out /var/www/html/storage/oauth-private.key 4096
	```
	and
	```shell
	openssl rsa -in /var/www/html/storage/oauth-private.key -pubout > /var/www/html/storage/oauth-public.key
	```
7. ### [optional]Backup!
	The Backup works basically on save the folder that contains the volume of the DB and put it back on the same folder at host before run the compose.
	
	7.1 Acess the machine as `sudo`
	7.2 Stop the containers
	7.3 Do the `rsync` of volume, put it safe on somewhere else!
	```shell
	rsync -ahzv /var/bin/docker/volumes/<snipesomething> /backup/folder
	```
	Or... just save the folder that are mounted as the `docker-compose.yml` specifies.


