# Docker : Installation and Basic Usage

## Installation

- Installation
    ~~~
    sudo dnf install docker
    ~~~

    ~~~
    sudo apt install docker.io
    ~~~

- to start docker server
    ~~~
    sudo systemctl start docker
    ~~~
    or
    ~~~
    sudo service docker start
    ~~~

- to configure docker to start on boot
    ~~~
    sudo systemctl enable docker
    ~~~

- to disable this behavior
    ~~~
    sudo systemctl disable docker
    ~~~

## Basic Usage

- to pull nginx image and to start nginx container in interactive mode
    ~~~
    sudo docker container run -it -p 80:80 nginx
    ~~~

- to show all docker containers
    ~~~
    sudo docker container ls -a
    ~~~

- to delete a docker container
    ~~~
    sudo docker container rm CONTAINER_ID
    ~~~
    for example
    ~~~
    sudo docker container rm 55b
    ~~~
    the CONTAINER_ID can be shortened<BR>
    or
    ~~~
    sudo docker container rm NAME
    ~~~

- to show all docker images
    ~~~
    sudo docker images
    ~~~

- to delete a docker image
    ~~~
    sudo docker image rm IMAGE_ID
    ~~~
    the IMAGE_ID can be shortened<BR>


- to download the nginx image
    ~~~
    sudo docker pull nginx
    ~~~

- to run a nginx container in detach mode
    ~~~
    sudo docker container run -d -p 8080:80 --name mynginx nginx 
    ~~~
    if there isn't a container whose name is mynginx, it will be created

- to show containers which are working
    ~~~
    sudo docker container ls
    ~~~
    or
    ~~~
    sudo docker container ps
    ~~~
    or
    ~~~
    sudo docker ps
    ~~~

- to specify nginx image's version(tag)
    ~~~
    sudo docker pull nginx:1.15
    ~~~

- to stop a docker container
    ~~~
    sudo docker stop NAME
    ~~~
    for example
    ~~~
    sudo docker stop mynginx
    ~~~

- to start a existing container
    ~~~
    sudo docker start NAME
    ~~~
    for example
    ~~~
    sudo docker start mynginx
    ~~~

- to start bash in a running container
    ~~~
    sudo docker exec -it mynginx bash
    ~~~

- to delete all containers in force
    * in fish
        ~~~
        sudo docker rm (sudo docker ps -aq) -f
        ~~~
    * in bash
        ~~~
        sudo docker rm $(sudo docker ps -aq) -f
        ~~~

- to link a host folder to a container folder
    * in fish
        ~~~
        mkdir test
        cd test
        sudo docker container run -d -p 8080:80 -v (pwd):/usr/share/nginx/html:Z --name mynginx nginx
        ~~~
    * in bash
        ~~~
        mkdir test
        cd test
        sudo docker container run -d -p 8080:80 -v $(pwd):/usr/share/nginx/html:Z --name mynginx nginx
        ~~~
    * make index.html file and you will see that http://localhost is serving the index.html file that you've just made.
    * without Z option, there may be a selinux related problem. See https://stackoverflow.com/questions/24288616/permission-denied-on-accessing-host-directory-in-docker
    * if a permission problem occur, you need to chmod your test folder and index.html

- to set hostname
    ~~~
    docker run --hostname {hostname} --name {name} {image}
    ~~~

## Example : Using Fedora

~~~
sudo docker pull fedora:29
sudo docker container run -it --name fedora --hostname fedora fedora:latest /bin/bash
sudo docker start fedora
sudo docker attach fedora
<C-p><C-q>
sudo docker fedora echo "hello"
~~~
