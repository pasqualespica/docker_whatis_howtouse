# Creating and Using Containers Like a Boss


### Image vs. Container

• An Image is the application we want to run
• A Container is an instance of that image running as a process
• You can have many containers running off the same image
• In this lecture our image will be the Nginx web server
• Docker's default image "registry" is called Docker Hub (hub.docker.com)


## Check Our Docker Install and Config

```
docker version

docker info

docker

docker container run

docker run
```

## Starting a Nginx Web Server

`docker container run --publish 80:80 nginx`

1. Downloaded image 'nginx' from Docker Hub
2. Started a new container from that image
3. Opened port 80 on the host IP
4. Routes that traffic to the container IP, port 80

### What happens in 'docker container run'

1. Looks for that image locally in image cache, doesn't find anything
2. Then looks in remote image repository (defaults to Docker Hub)
3. Downloads the latest version (nginx:latest by default)
4. Creates new container based on that image and prepares to start
5. Gives it a virtual IP on a private network inside docker engine
6. Opens up port 80 on host and forwards to port 80 in container
7. Starts container by using the CMD in the image Dockerfile


`docker container run --publish 80:80 --detach nginx`

```
(base) Pasquales-MacBook-Pro:~ pasqualespica$ docker container run --publish 80:80 --detach nginx
82d8bfdaf776cd3fe524c66b7fc2b44cfc23027263ad8040a7a90c54f5b6a6cc
(base) Pasquales-MacBook-Pro:~ pasqualespica$ docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
82d8bfdaf776        nginx               "/docker-entrypoint.…"   21 seconds ago      Up 19 seconds       0.0.0.0:80->80/tcp   determined_jennings
```

return a *unique ID*

`docker container ls`

`docker container stop 82d8bfdaf776` 

default shows all containers just running 
`docker container ls`

Show all containers
`docker container ls -a`

if you don't specify a name a random name `<adjective><noun>` ex. `dreamy_einstein` is assgined otherwise
`docker container run --publish 80:80 --detach --name webhost nginx`

```
(base) Pasquales-MacBook-Pro:~ pasqualespica$ docker container run --publish 56565:80 --detach --name webhost nginx
6f004811074315d4a253829cc5081449149cccf1e1f08302f1c3e0262d41afe0
(base) Pasquales-MacBook-Pro:~ pasqualespica$ docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
6f0048110743        nginx               "/docker-entrypoint.…"   2 seconds ago       Up 1 second         0.0.0.0:56565->80/tcp   webhost
```

`docker container ls -a`

Fetch the logs of a container
`docker container logs webhost`

docker container top : Display the running processes of a container
`docker container top webhost`

```
(base) Pasquales-MacBook-Pro:~ pasqualespica$ docker container top webhost1
PID                 USER                TIME                COMMAND
7259                root                0:00                nginx: master process nginx -g daemon off;
7320                101                 0:00                nginx: worker process
```

`docker container --help`

`docker container ls -a`

`docker container rm 63f 690 ode`

`docker container ls`

Stop the container before attempting removal or force remove
`docker container rm -f 63f`

docker container ls -a

## Container VS. VM: It's Just a Process

### Cgroups, namespaces, and beyond: what are containers made from? 
https://www.youtube.com/watch?v=sK5i-N34im8&feature=youtu.be&list=PLBmVKD7o3L8v7Kl_XXh3KaJl9Qw2lyuFl

### Docker Virtualization Admin
https://github.com/mikegcoleman/docker101/blob/master/Docker_eBook_Jan_2017.pdf

### Docker for Mac Commands for Getting Into The Local Docker VM
https://www.bretfisher.com/docker-for-mac-commands-for-getting-into-local-docker-vm/

### Option 1: use Screen (not as easy as nsenter)
Note this isn't a list of commands to run in order. The first one gets you in the VM (hit return twice to see a prompt). Then other commands are for managing that connection. Not a great CLI expirence but gets the job done. Using the ctrl- options prevents garbled text on reconnect.

connect to `tty` on Docker for Mac VM

`screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty`

disconnect that session but leave it open in background

`Ctrl-a d`

list that session that's still running in background

`screen -ls`

reconnect to that session (don't open a new one, that won't work and 2nd tty will give you garbled screen)

`screen -r`

kill this session (window) and exit

`Ctrl-a k`

### Container VS Virtual Machine
- Containers aren’t Mini-VM’s
- They are just processes
- Limited to what resources they can access (file paths, network devices, running processes)
- Exit when process stops

`docker run --name mongo -d mongo`

list running processes in specific container 
`docker top mongo`

ps aux

docker stop mongo

ps aux

docker run --name mongo -d mongo

`docker ps` like `docker container ls`

`docker top mongo`

`docker stop mongo`

`docker ps`

`docker top mongo`

`docker start mongo`

`docker ps`

`docker top mongo`

## Assignment Answers: Manage Multiple Containers

- Clean it all up with `docker container stop` and `docker container rm` (both can accept multiple names or ID's)

`docker container top` - process list in one container 
`docker container inspect` - details of one container config
`docker container stats` - performance stats for all containers

1. mysql 

`docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql`

then type
`docker container logs db` to find pwd like thi

```
GENERATED ROOT PASSWORD: gaa1EeFo4bah3ih0eHaj8iolu3hesh9j
```


2. httpd
`docker container run -d --name webserver -p 8080:80 httpd`

`docker ps`

3. nginx
`docker container run -d --name proxy -p 80:80 nginx`

`docker ps`

`docker container ls`


Try to see apache typing ... 
```
(base) Pasquales-MacBook-Pro:~ pasqualespica$ curl localhost:8080
<html><body><h1>It works!</h1></body></html>
```

`docker container stop TAB COMPLETION`
or to stop all active container
`docker container stop $(docker container ls -q)`

```
docker ps -a
docker container ls -a
docker container rm
docker ps -a
docker image ls
```

## What's Going On In Containers: CLI Process Monitoring

Run
```
docker container run -d --name nginx nginx
docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=true mysql
```
Show it
```
docker container ls
docker container top mysql
docker container top nginx
```

Display detailed information on one or more containers
`docker container inspect mysql`  
show a **json** info file

Display a live stream of container(s) resource usage statistics
(defaults show all)
`docker container stats <OPTIONAL CONTAINERS>`

## Getting a Shell Inside Containers: No Need for SSH

- `docker container run -it`  start new container interactively 
- `docker container exec -it` run additional command in existing container

`docker container run -it --name proxy nginx bash`
    -i, --interactive                    Keep STDIN open even if not attached
    -t, --tty                            Allocate a pseudo-TTY
`docker container run --help`

```
docker container ls
docker container ls -a
docker container run -it --name ubuntu ubuntu
docker container ls
docker container ls -a
```

```
docker container start --help
docker container start -ai ubuntu
```

Run a command in a running container
`docker container exec --help`

`docker container exec -it mysql bash`

```
docker container ls
docker pull alpine
docker image ls
```

`docker container run -it alpine bash`

`docker container run -it alpine sh`


## Docker Networks: Concepts for Private and Public Comms in Containers
https://docs.docker.com/config/formatting/

docker container run -p 80:80 --name webhost -d nginx

docker container port webhost

docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost

## Docker Networks: CLI Management of Virtual Networks

docker network ls

docker network inspect bridge

docker network ls

docker network create my_app_net

docker network ls

docker network create --help

docker container run -d --name new_nginx --network my_app_net nginx

docker network inspect my_app_net

docker network --help

docker network connect

docker container inspect TAB COMPLETION

docker container disconnect TAB COMPLETION

docker container inspect

## Docker Networks: DNS and How Containers Find Each Other

docker container ls

docker network inspect TAB COMPLETION

docker container run -d --name my_nginx --network my_app_net nginx

docker container inspect TAB COMPLETION

docker container exec -it my_nginx ping new_nginx

docker container exec -it new_nginx ping my_nginx

docker network ls

docker container create --help

## Assignment Answers: Using Containers for CLI Testing

docker container run --rm -it centos:7 bash

docker ps -a

docker container run --rm -it ubuntu:14.04 bash

docker ps -a

## Assignment Answers: DNS Round Robin Testing

docker network create dude

docker container run -d --net dude --net-alias search elasticsearch:2

docker container ls

docker container run --rm -- net dude alpine nslookup search

docker container run --rm --net dude centos curl -s search:9200

docker container ls

docker container rm -f TAB COMPLETION
