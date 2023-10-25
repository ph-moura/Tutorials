# Using ArchLinux in a Docker container

## Install docker

#### Install:
On ArchLinux, one can install the docker package as:
```
sudo pacman -S docker
```
For other distributions, see https://docs.docker.com/install/

#### Start docker daemon:
```
systemctl enable docker.service  # enable start on boot
systemctl start docker.service
```

#### Allow your user to run the docker:
```
sudo groupadd docker
sudo usermod -aG docker $USER
```

#### ArchLinux image
```
docker run -it -v ~/archlinux-docker:/home/archlinux-user --name myArchLinux -p 8888:8888 --user $(id -u):$(id -g) archlinux
```

This is what the `docker run` command does:
- `docker run` creates and starts a new docker container from a pre-defined image archlinux, which will be downloaded if necessary.
- `-it` runs the container with an interactive shell.
- `-v` mounts a shared folder between your machine (at ~/archlinux-docker) and the container (at /home/archlinux-user), through which you can transfer files to and from the container. You can edit the location of the folder on your machine as you like.
- `--name` (optional) sets a name for your container, for convenience. Edit it as you like.
- `--user $(id -u):$(id -g)`  runs the docker container with the same user permissions as the current user on your machine (since docker uses the same kernel as your host machine, the UIDs are shared). Note that the prompt will display "I have no name!", which is normal.
- `-p 8888:8888` forwards port 8888 from inside the container to outside.

Some useful commands:
- To see the containers you have running, and get their ID: `docker container ls` (`-a` to see also stopped containers)
- To stop the container: `docker stop <container>` or `exit`
- To re-start the container: `docker start -ai <container>`
- To delete a container: `docker container rm <container>`
