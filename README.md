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

## ✅ Communication between containers and environment variables
- Pull and run wordpress image
```
docker pull wordpress
docker run -d -p 8080:80 wordpress
```
- Communicate between different containers
```
# 1st busybox container
docker run -it busybox 
hostname -i
ping 172.17.0.3 # hostname of 2nd container

# 2nd busybox container
docker run -it busybox  
hostname -i
ping 172.17.0.2 # hostname of 1st container

# inspect container network
docker inspect CONTAINER_ID

```
- Show env of container
```
docker run -it busybox 
env

# otherways:
docker ps
docker exec CONTAINER_ID env
```

- Setup mysql container env
```
docker pull mysql
docker run -e MYSQL_ROOT_PASSWORD=my-password mysql
docker exec CONTAINER_ID env
```
- Pull and run phpmyadmin container
```
docker pull phpmyadmin
docker run -p 8080:80 phpmyadmin
```
- Connecting phpmyadmin to mysql container
```
docker ps
docker inspect MYSQL_CONTAINER_ID # then get IPaddress
docker run -p 8080:80 -e PMA_HOST=172.17.0.2 phpmyadmin

```

## ✅ Default and Custom Bridge Networks in Docker (WordPress, MySQL)
- Test communication between 2 busybox
```
# 1st busybox
docker run -it --name busybox1 -h busybox-one busybox
hostname
hostname -i

# 2nd busybox
docker run -it --name busybox2 -h busybox-two busybox
hostname
hostname -i

# inspect network bridge
docker network --help
docker network ls
docker network inspect bridge

# create a custom network
docker network create custom
## create 1st busybox using custom network
docker run -it --network custom busybox
hostname -i
hostname

## create 2nd busybox using custom network
docker run -it --network custom busybox
docker run -it --network custom --name busybox1 busybox
hostname -i
hostname

## check communication between two container
ping OTHER_CONTAINER_IP
ping OTHER_CONTAINER_HOSTNAME
ping OTHER_CONTAINER_CUSTOMNAME
```

## ✅ Additional containers - Elasticsearch, Redis, Httpd
```
docker pull appropriate/curl
docker run -it appropriate/curl sh
curl google.com

docker pull elasticsearch:7.6.2

docker run redis
docker exec -it REDIS_CONTAINER_ID redis-cli
```

## ✅ Linux
```
docker run -it ubuntu
ls
echo $SHELL
uname
uname -r
uptime
w
unminimize
apt-get install man-db
man # get tuto/doc of one cmd, e.g. man ls
q # to quit from document
# See multiple runing process
top
kill PID

# htop
man apt-get
apt-get update
apt-get install htop
which htop
htop
F2 # setup
F10 # done/quit

# Data streams and Piping in Linux
## 3 kinds of signal: stdin, stdout, stderr

## Add results of stdout into a file, e.g. file.txt
ls >stdout.txt
cat stdout.txt # see what's inside the stdout.txt
rm stdout.txt

## save stderr message into stderr.txt
mkdir 2> stderr.txt
cat stderr.txt

## see all the processes
ps -e

## numeric id for std: 0: stdint, 1: stdout, 2: stderr
ls 1> stdout.txt 2> stderr.txt

cat non_exit_file.txt 1>> stdout.txt 2>>stderr.txt
cat stderr.txt

## appending messages
mkdir 1>>stdout.txt 2>>stderr.txt
cat stderr.txt

## Piping
ls | cat
echo "Hello world" | cat >hello.txt

```

## ✅ Linux File and Directories Managements
```
pwd # print working directory
cd # comeback to root directory of user
cd .. # comeback to parent directory
cd ../ # press 'tab' to show sub folders or files inside 
cd / # go to

ls -l
ls -F
ls -F > test.txt
ls -la # see hidden files
rm *.txt

# Create and Removing directories and files
cd
pwd
mkdir test
cd test 
mkdir test 1
cd ..
mkdir -p test2/test3
rm -r test
mkdir test
cd test
touch test1.txt # create an empty file
echo "hello world" > hello.txt
touch --help
```

## ✅ Edditing files with Vim and Nano Editors
```
apt-get update
apt-get install vim nano

# check vim
vim
:q
vim file1.txt
Shift+I # start editting
ESC # Exit from editing mode
:wq # Save changes and quit vim
cat file1.txt

# check nano
nano
Ctr+X
nano file2.txt
Ctr+X # Save and quit editing mode
Enter # Quit nano

```
## ✅ Linux: files manipulation
```
# copy file
cp file3.txt file4.txt
cp /etc/libaudit.conf libaudit-backup.conf
mkdir etc-backup
cp -r /etc/ etc-backup
rm -r etc-backup
cp -r /etc/* etc-backup

# rename file
rm file4.txt file5.txt
file1.txt  file2.txt  file3.txt  file5.txt  gai.conf  libaudit-backup.conf

# create large file
# https://www.lipsum.com/
cat > large-file.txt
## the above cmd will listen the input stream, then copy all the long content
ls
cat large-file.txt

# see first ten lines of the large file
head large-file.txt
head -n 3 large-file.txt 

# see last ten lines of the large file
tail large-file.txt
tail -n 3 large-file.txt 
tail -f large-file.txt
## open 2nd terminal, add/append lines into large file
echo "Additional line 1" >> large-file.txt

# see large file with cmd 'more'
more large-file.txt

# See last cmd
history
```

## ✅ Linux: search and filter file using 'grep'
```
cd /
ls
cd bin
ls -l
ls -l | grep hostname

ls -l | grep ^l # looking line start with 'l'
ls -l | grep ch$ # looking for line ends with 'ch'
ls -l | grep "\->" 
ls -l | grep " 2 root"
ls -li | grep " 2 root" # 'i' represent inode

# search keyword in text
cat file5.txt | grep key_word

```

## ✅ Linux: create soft and hard link
```
# create soft link
ln -s file5.txt file5-softlink.txt
ls
ls -l
cp file5.txt file6.txt
ls -l
nano file5.txt
cat file5.txt
head -n 1 file5.txt
head -n 1 file5-softlink.txt
history

# create hard link
ln file6.txt file6-hardlink.txt
```

## ✅ Linux: Search operations
```
touch file-one.pdf
touch file-two.pdf
find -name file-one.pdf
find .. -name file-one.pdf
find -name "*.pdf"
find . -type f # find all file
find . -type d # find all directory
find . -type l -ls
find . -type d -ls
find . -type f -ls
find . -type f -empty
find / -perm 777 -ls # perm stands for permission
find . -perm 777 -ls
find . -name "*.txt" -exec  ls -la {} \;
find . -exec  ls -la --color {} \;
find . -name "*.txt" -printf "%p %k KB\n"
find / -name kernel -type d -exec ls -l --color {} \;
find / -name kernel -type d | xargs ls -l --color
```

## ✅ Linux: Compressing and Sorting
```
docker ps -a
docker start CONTAINER_ID
docker exec -it CONTAINER_ID bash 
tar --help
gzip --help

# create a tar file
tar -cf archive.tar ~
ls -lh # h stands for human readble

# read tar file
tar -tf archive.tar

# compress tar file
gzip archive.tar
ls -la
ls -lah

# remove archive file
rm archive.tar.gz

# recreate archive commpresion with tar
tar -czf ../archive.tar.gz .

# extract tar file
cd 
mkdir tmp
cd tmp
tar -xvzf /archive.tar.gz
ls 

# get info from tar file
apt-get install file
file \archive.tar.gz

# Sorting file
ls -lah
ls -lS
ls -lh --sort=
ls -lh --sort=size
ls -lh --sort=extension
ls -lh --sort=version
ls -lh --sort=time

# Sort content in file

nano animals.txt
sort --help
nano animals.txt
cat animals.txt
sort --help
sort animals.txt
sort -r animals.txt
sort -o sorted.txt animals.txt
cat sorted.txt
cat animals.txt
sort -ro sorted-r.txt animals.txt
cat sorted-r.txt
nano numbers.txt
cat numbers.txt
sort -n numbers.txt
sort -nr numbers.txt
sort -n -o number-sorted.txt  numbers.txt
sort -nr -o number-sorted-r.txt  numbers.txt
ls
cat number-sorted.txt
cat number-sorted-r.txt
sort -nr numbers.txt > sorted-numbers.txt
cat sorted-numbers.txt
sort --help | grep unique
nano numbers.txt
sort -n numbers.txt
sort -nu numbers.txt
```