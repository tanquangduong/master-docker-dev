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
- Nginx
```
docker pull nginx
```
  - open/run nginx container: define the local port is 8080, mapping to internal port 80 of nginx
  ```
  docker run -p 8080:80 nginx
  ```
  - connecting local html file in local pc to nginx container. 
  ```
  docker run -p 8081:80 -v "$(pwd):/usr/share/nginx/html" nginx
  ```
  - check ip in Windown:
  ```
  ipconfig
  ```
  - check ip in Mac:
  ```
  ifconfig
  ```
  - Create example favicon in  favicon.io. 
    - Copy all created favion to the server folder of nginx
    - Create 'index.html': Tap '!', then press 'tab' to create a Html template, paste the html code generated in the 'favicon.io', then pass 'h1' to the body'
    - rerun the nginx server

## ✅ Docker Container Management (Ubuntu, Nginx)
- Run in the background
```
docker run -p 8080:80 -d nginx
```
- Refresh the page: 'http://localhost:8080/' and see the logs
```
docker logs CONTAINER_ID
```
- Stop container
```
docker stop CONTAINER_ID
```
- Run inside container
```
docker run -it alpine
docker run -id ubuntu
ls
exit
```
- Create multiple containers on the same image
```
# First container
docker run -id ubuntu
ls
mkdir test
cd test
touch text.txt
cd ..
ls
hostname
hostname -i # ip address

# Second container using ubuntu image
docker run -id ubuntu
ls
mkdir test2
cd test2
touch text2.txt
cd ..
ls
hostname
hostname -i # ip address
```
- Create multiple Nginx server
```
# First container
cd nginx1
docker run -p 5555:80 -v "$(pwd):/usr/share/nginx/html" --name nginx1 nginx
# Second container using ubuntu image
cd nginx2
docker run -p 7777:80 -v "$(pwd):/usr/share/nginx/html" --name nginx2 nginx

```
- Start/Remove a container
```
# To re-start an stopped existing container
docker start CONTAINER_NAME

# To remove  an stopped existing container
docker rm CONTAINER_NAME

# To remove all stopped containers
docker container prune

# To remove  multiple stopped containers
docker container rm CONTAINER_ID_1 CONTAINER_ID_2 

# Verify stopped containers
docker ps -a
```

## ✅ Running Python Container in Docker
```
docker pull python
docker run -it python
help()
exit()
cd python
docker run -it -v "$(pwd):/app" python python3 /app/hello-world.py
# or specify -w /app for Working directory
docker run -it -v "$(pwd):/app" -w /app  python python3 hello-world.py

# Run Calendar application with python container
docker run -it -v "$(pwd):/app" -w /app  python python3 calendar-app.py
```

## ✅ Running Node Container in Docker
- Pull and run node container
```
docker pull node
docker run -it node
.help
console.log('Hello from Node.js!')
const a = 5
2+3
.exit
```
- Running basic examples with Node.js container: npm init, nmp install, express package etc
```
mkdir node
cd node
docker run -it -v "$(pwd):/app" -w /app  node node hello.js
cd express
docker run -it -v "$(pwd):/app" -w /app  node npm init
docker run -it -v "$(pwd):/app" -w /app  node npm install express
docker run -it -v "$(pwd):/app" -w /app -p 3000:3000 node node index.js
# To interrupt:
Ctr+C
# To stop:
docker stop CONTAINER_ID
```
- Create Node application to create txt file
```
cd files
docker run -it -v "$(pwd):/app" -w /app node node index.js
```


## ✅ Running MongoDB Container in Docker
- Pull and run mongo container
```
docker pull mongo
docker run mongo
# Run multiple process in the same container

## Execute in bash
docker exec -it CONTAINER_ID bash
ls /usr/local/bin
cat /usr/local/bin/docker-entrypoint.sh

## Execute in sh
docker exec -it CONTAINER_ID sh

## See which running execution
ps -e

# Inspect container
docker inspect CONTAINER_ID
```
- Create a database table in mongo container
```
docker exec -it mongosh
db.version()
use test
db
db.animals.insertOne({"animal": "cat"})
db.animals.insertOne({"animal": "dog"})
db.animals.insertOne({"animal": "monkey"})
db.animals.find()
```
- Mapping db in local computer to different mongo container
```
cd mongo
docker run -d -v "$(pwd)/db:/data/db" mongo
docker ps
docker exec -it CONTAINER_ID bash
mongosh
use mydb # create a database named 'mydb'
db.fruits.insertOne({"fruit": "kiwi"})
db.fruits.insertOne({"fruit": "orange"})
db.fruits.insertOne({"fruit": "banana"})
db.fruits.find()
exit
docker stop CONTAINER_ID
# Create a new mongo container, sharing the same mydb in db
docker run -d -v "$(pwd)/db:/data/db" mongo
use my_db
db.fruits.find()
```