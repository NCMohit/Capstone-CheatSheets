### Installation
```
pacman -S docker / apt-get install docker  
systemctl enable docker  
systemctl start docker  
```
## Usage
```
docker image ls		-	lists images  
docker ps -a		-	lists all containers ( remove -a to show running )  
docker run <img_name>	-	downloads an image and runs a container  
				--name <name_of_container>  
				-i	interactive shell  
				-t	open tty terminal  
				-d	detach and run in background
				-e  manipulate the environment variables  
				-p <ext_port>:<intern_port>	publish port  
docker start <container_name>	-	Start existing container  
docker attach <container_name>	-	Open terminal of running container  
docker exec <container_name> <cmd>	-	Just run command in container  
docker rm <container_name>	-	Remove container  
docker rmi <image_name>		-	Remove image (remove dep containers first)  
  
# Building image  
  
docker build <path>	-	Build image from existing image ( Path must have a docker file )  
				-t	Give a nametag to image  
```
# Docker file format
```
FROM <img>:<version>  
COPY <path> <path_in_container>  
WORKDIR <path_in_container>  
RUN <cmd>			# This will run only on creation of image  
EXPOSE <port>  
CMD ["space","seperated","command"]	# This will run everytime container is run  
  
ex: CMD ["python","manage.py","runserver","0.0.0.0:8000"]  
```
## Volumes ( Spaces that store data even if container is removed )
``` 
docker volume ls	-	List of all volumes  
docker volume create <name>	-	Creates new volume space  
docker volume inspect <vol_name>	-	Attributes of volume  
```
# Usage
```
docker run .... -v <vol_name>:<path_in_container> <img_name> ...	-	Will sync files in the path to volume  
OR  
docker run .... -v <path_in_host>:<path_in_container> <img_name> ...	-	Will sync files in vol_path to host_path  
  
If used volume_name instead of path, same voume can be accessed in host in /var/lib/docker/volumes/<vol_name>/_data  
```
## Docker Swarm		( Creating clones and maintaining for Scaling, etc )  
```
pacman -S docker-machine  
pacman -S virtualbox  
  
docker-machine create --driver virtualbox manager1  
docker-machine create --driver virtualbox worker1  
docker-machine create --driver virtualbox worker2	-	Create multiple machines  
  
docker-machine ls	-	See all machines, their IPs  
  
docker-machine ssh <machine_name>	-	SSH into created machines  
```
# Inside manager  
```
docker swarm init --advertise-addr <ip_of_manager>	-	Create a swarm  
docker node ls						-	See all machines in swarm ( Only works in manager )  
docker node inspect <machine_name>			-	See details of worker  
docker swarm join-token worker				-	Get command to run in worker machine to join swarm  
docker service create <img>	-	Create service that runs multiple img on all machines  
					--replicas <num>	-	Num of instances of containers  
					-p <ext_port>:<int_port>	-	Port mapping  
					--name <serviceName>		-	Name of service  
docker service ps <service_name>	-	See list of all running service instances  
docker service scale <service_name>=<num_of _inst>	-	Scale amount of service instances  
docker node update --availability drain <machine_name>	-	Shutdown node  
docker service update --image <img_name>:<version> <service_name>	-	Updates service to run new image  
docker service rm <service_name>	-	Remove service from swarm  
docker swarm leave			-	Make machine leave swarm  
  
docker-machine stop <machine_name>	-	Stops machine  
docker-machine rm <machine_name>	-	Removes machine  
```
# Docker network
```
docker network create <network-name>	-	used in connecting multiple container within docker  
docker network ls			-	lists all networks  
```
## Docker Compose

pacman -S docker-compose / apt-get install docker-compose  
  
# your-docker-compose-file.yaml		-	installation  

```
version: <version>  
services:  
	<service-1>:  
		image: <image-name>  
		ports:  
			- <ext_port>:<internal_port>  
		environment:  
			- <env_var1>=<val1>  
			- <env_var2>=<val2>  
	<service-2>:  
		...  
  
docker-compose -f <compose_file> up	-	builds and runs containers based on yaml file  
docker-compose -f <compose_file> down	-	stops containers  
```

 










