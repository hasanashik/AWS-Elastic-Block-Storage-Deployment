![](image.png)

1. We will launch AWS EC2 instance with Elastic Block Store (EBS). 

In the VM after connecting we will check disk space usage in a human-readable format:

df -h 

1. lists information about all available block devices (storage devices) on your system, such as hard drives and partitions:

lsblk

1. display information about the file system format, such as "XFS" or "ext4."

sudo file -s /dev/xvdf

1. create a new file system of type "XFS" on the /dev/xvdf device. It formats the device with the XFS file system, making it suitable for data storage.

mkfs -t xfs /dev/xvdf

Verify the file system format: 

`	`file -s /dev/xvdf

1. Finally, mount the /dev/xvdf device to the /home/ubuntu/test\_folder directory.

` 	`mount /dev/xvdf /home/ubuntu/test\_folder





Install docker and docker-compose:

Install docker: https://docs.docker.com/engine/install/ubuntu/

\# Add Docker's official GPG key:

sudo apt-get update

sudo apt-get install ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg

\# Add the repository to Apt sources:

echo \

`  `"deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \

`  `"$(. /etc/os-release && echo "$VERSION\_CODENAME")" stable" | \

`  `sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo docker run hello-world

sudo apt install docker-compose



Now we will spin up a docker container with mysql image using docker-compose:

sudo nano docker-compose.yml

version: "3"

services:

`  `mysql:

`    `image: mysql:latest

`    `container\_name: db

`    `environment:

`      `MYSQL\_ROOT\_PASSWORD: a

`      `MYSQL\_DATABASE: db

`      `MYSQL\_USER: a

`      `MYSQL\_PASSWORD: a

`    `volumes:

`      `- ./another\_folder:/var/lib/mysql

`    `ports:

`      `- "3306:3306"

`    `restart: always

sudo docker-compose up


connect to vm mysql:

sudo docker exec -it db bash

mysql -u your\_username -p

mysql -u root -p 

a 

show databases;

use db;

CREATE TABLE student (

`    `id INT AUTO\_INCREMENT PRIMARY KEY,

`    `first\_name VARCHAR(50),

`    `last\_name VARCHAR(50),

`    `age INT,

`    `grade FLOAT

);

INSERT INTO student (first\_name, last\_name, age, grade) VALUES

('John', 'Doe', 20, 85.5),

('Alice', 'Smith', 22, 91.0),

('Bob', 'Johnson', 21, 78.5),

('Emily', 'Wilson', 19, 92.5),

('Michael', 'Brown', 23, 88.0);

SELECT \* FROM student;

exit;



