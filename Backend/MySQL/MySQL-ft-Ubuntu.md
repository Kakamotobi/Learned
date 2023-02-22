# MySQL ft. Ubuntu 20.04

## Table of Contents
- [What is MySQL?](#what-is-mysql)
- [MySQL Installation](#mysql-installation)
- [MySQL Configuration](#mysql-configuration)
  - [MySQL Install/Config Troubleshooting](#mysql-installconfig-troubleshooting)
  - [Allowing Remote Access to MySQL](#allowing-remote-access-to-mysql)
- [Create DB](#create-db)
- [Query DB](#query-db)
- [Other MySQL Commands](#other-mysql-commands)

## What is MySQL?
- MySQL is a free and open-source RDBMS that uses SQL.

## MySQL Installation
- **Update package index on server.**
  ```zsh
  $ sudo apt update
  ```
- **Install `mysql-server` package (_a root MySQL user is created_).**
  ```zsh
  $ sudo apt install mysql-server
  ```
- **Check if server is running.**
  ```zsh
  $ sudo systemctl start mysql.service
  ```

## MySQL Configuration
- We need to setup a MySQL root password and secure the MySQL installation.
- **Change the root user's authentication method to password-based.**
  ```zsh
  $ sudo mysql
  ```
  ```sql
  mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '<mysql root account password>';
  ```
  ```sql
  mysql> exit
  ```
- **Run MySQL's security script.**
  ```zsh
  $ sudo mysql_secure_installation
  ```
- **Change root user's authentication method back to socket-based.**
  ```zsh
  $ mysql -u root -p
  ```
  ```sql
  mysql > ALTER USER 'root'@'localhost' IDENTIFIED WITH auth_socket;
  ```
  - Check to see if authentication plugin has indeed changed.
    ```sql
    SELECT user,authentication_string,plugin,host FROM mysql.user;
    ```
- **Use the root MySQL user to create more MySQL users.**
  ```zsh
  $ sudo mysql # or mysql -u root -p
  ```
  ```sql
  mysql> CREATE USER '<mysqlusername>'@'<hostname>' IDENTIFIED [WITH <authentication plugin>] BY '<mysqlpassword>';
  ```
- **Grant privileges to users.**
  ```sql
  mysql> GRANT <privileges> ON <database>.<table> TO '<mysqlusername>'@'hostname';
  ```
  ```sql
  mysql> FLUSH PRIVILEGES;
  ```
- **Login as another user.**
  ```zsh
  mysql -u <mysqlusername> -p
  ```
### MySQL Install/Config Troubleshooting
- Password Issues
  - `...(using password: NO)` means there is no password that is registered.
  - `...(using password: YES)` means the password you provided is wrong.
- Reset the Password
  - `SET PASSWORD FOR '<username>'@'<hostname>'='newpassword'`.
  - `FLUSH PRIVILEGES`.
### Allowing Remote Access to MySQL
- Users' MySQL instances, by default, are only configured to listen for local connections.
- So we need to configure MySQL to be able to listen for an external IP address that the server can be reached through.
- **In the server, find the `/etc/mysql/mysql.conf.d/mysqld.cnf` file and comment out the `bind-address` line.**
  ```zsh
  $ cd /etc/mysql
  $ grep -r 'bind'
  # bind-address=127.0.0.1 commented out
  $ cd /etc/mysql/mysql.conf.d
  $ sed -i 's/bind# bind/' mysqld.cnf
  $ cat mysqld.cnf | grep bind
  ```
- **Then, restart the MySQL service.**
- **Update the user's host to the remote server IP instead of `'localhost'`.**
  ```sql
  mysql> RENAME USER '<username>'@'localhost' TO '<username>'@'<remote server ip>';
  ```
- **Grant appropriate privileges.**
- **Open port `3306` (MySQL's default port to open up to traffic).**
  ```zsh
  $ sudo ufw allow [from <remote ip address of user machine> to any port] 3306
  ```
- _NOTE: do NOT allow external access to the root MySQL user._

## Create DB
- **Create a DB.**
  ```sql
  mysql> CREATE DATABASE `movies`;
  ```
- **Select the DB.**
  ```sql
  mysql> USE movies;
  ```
- **Create Tables in the DB.**
  ```sql
  mysql> CREATE TABLE movie (movie_id real, title varchar(30), rating real, revenue real, date_released date);
  ```
  ```sql
  mysql> INSERT INTO movie (movie_id, title, rating, revenue, date_released)
  mysql> VALUES (1, 'avatar: the way of water', 7.8, 2000000000, '2022-12-14'),
  mysql> (2, 'interstellar', 8.6, 677000000, '2014-11-06'),
  mysql> (3, 'back to the future', 8.5, 385000000, '1987-07-17');
  ```
  ```sql
  mysql> CREATE TABLE movie_cast (movie_id real, person_name varchar(30), character_name varchar(30));
  ```
  ```sql
  mysql> INSERT INTO movie_cast (movie_id, person_name, character_name)
  mysql> VALUES (1, 'sam worthington', 'jake'),
  mysql> (1, 'zoe saldana', 'neytiri'),
  mysql> (1, 'sigourney weaver', 'kiri'),
  mysql> (1, 'stephen lang', 'quaritch');
  ```
  ```sql
  mysql> INSERT INTO movie_cast (movie_id, person_name, character_name)
  mysql> VALUES (2, 'matthew mcconaughey', 'cooper'),
  mysql> (2, 'anne hathaway', 'brand'),
  mysql> (2, 'jessica chastain', 'murph'),
  mysql> (2, 'mackenzie foy', 'murph');
  ```

## Query DB
- **SELECT**
  ```sql
  mysql> SELECT title, IF(revenue>5000000, "yes", "no") AS is_recent_blockbuster FROM movie WHERE date_released>'2020-01-01';
  ```

## Other MySQL Commands
- `mysql> show databases`
- `mysql> show tables`
- `mysql> show schemas`

## Reference
[Initial Server Setup with Ubuntu 20.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04)  
[How to Install MySQL on Ubuntu 20.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04)  
[How to Change MySQL Root Password in Ubuntu 20.04](https://linuxhint.com/change-mysql-root-password-ubuntu/)  
[How To Allow Remote Access To MySQL | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-allow-remote-access-to-mysql)  
[Sample Database: Movies (ERD and SQL) - Database Star](https://www.databasestar.com/sample-database-movies/)  
