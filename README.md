![](image.png)

# AWS EC2 Instance Setup with EBS and Docker

This repository contains instructions and scripts for setting up an AWS EC2 instance with Elastic Block Store (EBS) and running a MySQL Docker container. Follow the steps below to get started.

## Launching AWS EC2 Instance with EBS

1. **Create an AWS EC2 Instance:**

   Launch an EC2 instance on AWS with your preferred configuration.

   **Connect to the VM:**

   Use SSH to connect to your EC2 instance after it's up and running.

   ```bash
   ssh -i your-key.pem ubuntu@your-instance-ip
   ```
   **Check Disk Space Usage:**

   To check disk space usage in a human-readable format, use the following command:
   ```bash
   df -h
   ```
   **To list information about all available block devices (storage devices) on your system, such as hard drives and partitions, you can use:**
   ```bash
   lsblk
   ```
2. **File System Information:**

   Display information about the file system format, such as "XFS" or "ext4."
   ```bash
   sudo file -s /dev/xvdf
   ```
3. **Create a New XFS File System:**
   Create a new file system of type "XFS" on the /dev/xvdf device. It formats the device with the XFS file system, making it suitable for data    storage.
   ```bash
   mkfs -t xfs /dev/xvdf
   ```
4. **Verify the File System Format:**
   Verify that the file system format is set to XFS.
    ```bash
    file -s /dev/xvdf
    ```
5. **Mount the Device:**

   Finally, mount the /dev/xvdf device to the /home/ubuntu/test_folder directory.
   ```bash
   sudo mount /dev/xvdf /home/ubuntu/test_folder
   ```
6. Installing Docker and Docker-Compose:

   Install Docker by following the official Docker installation guide for Ubuntu here: https://docs.docker.com/engine/install/ubuntu/
   ```bash
   sudo apt-get update
   sudo apt-get install ca-certificates curl gnupg
   sudo install -m 0755 -d /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   sudo chmod a+r /etc/apt/keyrings/docker.gpg
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo $VERSION_CODENAME) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   sudo docker run hello-world
   ```
   Install Docker Compose using the following command:
   ```bash
   sudo apt install docker-compose
   ```
7. **Running MySQL Docker Container:**

   Create a Docker Compose File:
   Create a docker-compose.yml file to define the MySQL Docker container.
   sudo nano docker-compose.yml
   Use the following content for your docker-compose.yml file:
   ```bash
   version: "3"
      services:
        mysql:
          image: mysql:latest
          container_name: db
          environment:
            MYSQL_ROOT_PASSWORD: a
            MYSQL_DATABASE: db
            MYSQL_USER: a
            MYSQL_PASSWORD: a
          volumes:
            - ./another_folder:/var/lib/mysql
          ports:
            - "3306:3306"
          restart: always
    ```
Save and exit the file.

8. **Start the MySQL Docker Container:**
Start the MySQL Docker container using Docker Compose.
```bash
sudo docker-compose up
```
9. **Connect to the MySQL Container:**
To connect to the MySQL container running inside your VM, use the following commands:
```bash
sudo docker exec -it db bash
mysql -u your_username -p
```
You will be prompted to enter the MySQL password (a in this case). You can now interact with your MySQL database.
```bash
mysql -u root -p
a
show databases;
use db;
```
You can also create tables and insert data into the database as needed.
```bash
CREATE TABLE student (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    age INT,
    grade FLOAT
);

INSERT INTO student (first_name, last_name, age, grade) VALUES
('John', 'Doe', 20, 85.5),
('Alice', 'Smith', 22, 91.0),
('Bob', 'Johnson', 21, 78.5),
('Emily', 'Wilson', 19, 92.5),
('Michael', 'Brown', 23, 88.0);

SELECT * FROM student;
exit;
```
Now your AWS EC2 instance is set up with EBS, Docker, and a running MySQL container. You can use this setup for your development and testing purposes.






