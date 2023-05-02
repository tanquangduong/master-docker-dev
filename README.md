# master-docker-dev
Master Docker for building applications

## ✅ Basic Docker Container
- Basic prompt:\
  - Run/download the images from DockerHub:\
`docker run hello-world`\
`docker run ubuntu`\
`docker run busybox`
  - See a list a running container:\
`docker ps` or including all history: `docker ps -a`, similarly:\
`docker container ls` or including all history: `docker container ls -a`
  - See list of images:\
`docker images`
  - Run inside a container:\

  - Use-cases:
    - hello-world
    - ubuntu
  ```
  docker run -it ubuntu
  hostname
  hostname -i
  ls
  echo "Hello from Ubuntu"
  cat /etc/os-release
  exit
  ```
    - busybox: is not a full-featured Linux OS. It is just set of Linux utilities
  ```
  docker run -it busybox
  ls
  echo "Hello from Ubuntu"
  mkdir my_folder
  cd my_folder
  touch file.txt
  exit
  ```
  - alpine: built on top of busybox
  ```
  docker pull alpine
  docker run -it alpine
  cat \etc\os-release
  ls
  ls -la bin
  busybox
  ifconfig --help
  ls sbin
  ls -la sbin
  apk
  busybox ifconfig
  busybox ping google.com
  ping google.com
  exit
  ```
  - stop running container
  ```
  docker stop CONTAINER_ID/NAMES
  ```
## ✅ Port and Volume mapping
-Nginx
```
docker pull nginx
```
  - open/run nginx container: define the local port is 8080, mapping to internal port 80 of nginx
  ```
  docker run -p 8080:80 nginx
  ```