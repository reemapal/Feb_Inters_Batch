# How To Run MySQL Database in Container

## Introduction:

**Podman:**  Podman is a daemonless container engine for developing, managing, and running OCI Containers on your Linux System.

**MySQL:**  MySQL is a database management system.

**Prerequisites:-**

**Platform::** Ubuntu 20.04.4

**Dependency::** you need to setup the podman repository afterward, you can install and update podman from the repository.
Pull the image:: 

|||
|-|-|
|NAME| VERSION|
|Podman| 2.0.0|
|MySQL|7.1.1|


Now, we will start a MySQL database inside a container

**Step1.Install Podman on Ubuntu:**

1.)  . /etc/os-release

2.)  echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_${VERSION_ID}/ /" | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list

3.)  curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_${VERSION_ID}/Release.key | sudo apt-key add -

4.)  sudo apt-get update 

5.)  sudo apt-get -y install podman

**Step 2: Download MySQL Docker Image** 

Before we can created MySQL instance in a container we need to pull the latest image for the MySQL version we intend on running.

> Podman

$ podman pull mysql:5.7

$ podman images 

REPOSITORY               TAG         IMAGE ID      CREATED     SIZE
docker.io/library/mysql  5.7         4181d485f650  5 days ago  454 MB


**Step3: Run MySQL Database Instance in a Container**

##### Run container with Podman

$ podman run -d --name mysql -p 3306:3306  -e MYSQL_ROOT_PASSWORD='redhat@123' -e MYSQL_USER=keenable -e MYSQL_PASSWORD='keenable' -e MYSQL_DATABASE=intern_batch docker.io/library/mysql:5.7

##### Confirm it is running:

$ podman ps

|||||||
|-|-|-|-|-|-|
|CONTAINER ID | IMAGE                       | COMMAND |     CREATED      |  STATUS          |  PORTS                 | NAMES |
|70aa56d4e558 | docker.io/library/mysql:5.7 | mysqld  |    4 seconds ago | Up 3 seconds ago | 0.0.0.0:3306->3306/tcp | mysql  |

**Step4: Running mysql commands inside the MySQL container** 

> Podman

$ podman exec -it 70aa56d4e558 /bin/bash

root@70aa56d4e558:/# 

Connect to MySQL as the database administrator user (root).

root@70aa56d4e558:/# mysql -u root -p 

Enter password: 

mysql>

Use MySQL interactive prompt to list created databases.

mysql> show databases;

| Database           |
|------------------- |
| information_schema |
| intern_batch       |
| mysql              |
| performance_schema |
| sys                |

5 rows in set (0.00 sec)


You can create new table in the database:
mysql> use intern_batch;
Database changed


#### Create a table called Logical_Task in the intern_batch database. 

mysql> CREATE TABLE Logical_Task (SNo INT NOT NULL PRIMARY KEY, Attributes VARCHAR(30), Sub_Attributes VARCHAR(30), Test_Cases VARCHAR(30), Abdul_Basit VARCHAR(30), Amar_Singh VARCHAR(30),  Desh_Deepak_Pal VARCHAR(30),  Prachi_Upadhyay VARCHAR(30), Reema VARCHAR(30), Aditya VARCHAR(30), Madhu  VARCHAR(30));


Use the show tables command to verify that the table was created. 

mysql> show tables;

| Tables_in_intern_batch |
| ---------------------- |
| Logical_Task           |

1 row in set (0.66 sec)


## How to import Excel data into a MySQL database

step1. Open your Excel file and click Save As. Choose to save it as a .CSV (Comma Separated) file. 
step2. Log into your MySQL shell and create a database. For this example the database will be named intern_batch. Note that the --local-infile option is needed by some versions of MySQL for the data loading we’ll do in the following steps.

step3. Install mysql-client

$ sudo apt-get install mysql-client

step4. sudo vi /tmp/config.cnf

$ cat /tmp/config.cnf

[client]

user = "root"

password = "redhat@123"

host = "10.0.2.15"

$ mysql --defaults-extra-file=/tmp/config.cnf --local-infile intern_batch -e "LOAD DATA LOCAL INFILE '/home/reemapal/Downloads/logical.csv'  INTO TABLE Logical_Task FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n'"

**Verify:**
$ podman exec -it 70 /bin/bash

root@70aa56d4e558:/# mysql -u root -p

Enter password: 

mysql> use intern_batch

Database changed

mysql> show tables;

| Tables_in_intern_batch |
|------------------------|
| Logical_Task           |

1 row in set (0.00 sec)

mysql> select * from Logical_Task;


| SNo | Attributes             | Sub_Attributes      | Test_Cases           | Abdul_Basit | Amar_Singh   | Desh_Deepak_Pal | Prachi_Upadhyay | Reema     | Aditya    | Madhu        |
|-----|------------------------|---------------------|----------------------|-------------|--------------|----------------|-----------------|-----------|----------|--------------|
|   0 |                        |                     |                      |             |              |                 |                    |        |           | 
|   1 | Performance Evaluation | Sheet Review        | Attributes           | Good        | Satisfactory | Fair            | Good      |     | Very Good | Very Good | Very Good
|   2 | Delivery               | Submit Task on Time | Basis of Commitment  | Good        | Excellent    | Excellent       | Excelle   |     | Excellent | Good      | Excellent
|   3 | Efforts                | Quality             | Good Quality of Work | Good        | Good         | Good            | Excelle        || Very Good | Fair      | Good
|   4 | Assesment              | Feedback            |                      | Good        | Good         | Good            | Very Go   |     | Very Good | Very Good | Very Good
|   6 | Average Rating         |                     |                      | 2.916666667 | 3.083333333  | 3               | 3.58333 |33     | 4.25      | 3         | 3.583333333

6 rows in set (0.00 sec)
