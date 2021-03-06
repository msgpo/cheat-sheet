# Reset Root MySQL Password

this problem occurs in **Elementary OS 5.0** and **Mysql Version 14.14**, fails to enter `mysql` with an error message: `mysqli_real_connect (): (HY000 / 1698): Access denied for user 'root' @ 'localhost'`


## Reset by using mysql_secure_installation

The simplest approach to reset MySQL database root password is to execute **mysql_secure_installation** program and when prompted entering your new root MySQL password: 

```console
$ sudo mysql_secure_installation
```


## Reset by using skip-grant-tables

- Let's start by stopping the currently running MySQL database: 

  ```console
  $ sudo service mysql stop
  ```

- Next, create a **/var/run/mysqld** directory to be used by MySQL process to store and access socket file:

  ```console
  $ sudo mkdir -p /var/run/mysqld
  $ sudo chown mysql:mysql /var/run/mysqld
  ```

- Once ready manually start MySQL with the following linux command and options:

  ```console
  $ sudo /usr/sbin/mysqld --skip-grant-tables --skip-networking &
  ```

- Confirm that the process is running as expected: 

  ```console
  $ jobs
  ````

- At this stage we are able to access MySQL database without password: 
  
  ```mysql
  $ mysql -u root
  ```

- Using the MySQL session first flush privileges: 

  ```mysql
  mysql> FLUSH PRIVILEGES;
  ```

- Next, reset root password. The following commands will reset MySQL root password to **NEWPASSWORD**

  ```mysql
  mysql> USE mysql;
  mysql> UPDATE user SET authentication_string=PASSWORD("NEWPASSWORD") WHERE User='root';
  mysql> UPDATE user SET plugin="mysql_native_password" WHERE User='root';
  ```

- Quit MySQL session: 

  ```mysql
  mysql> quit
  ```

- Gracefully terminate current mysqld process:
  
  ```console
  $ sudo pkill mysqld
  $ jobs
  ```
  
- Lastly, start MYSQL database: 

  ```console
  $ sudo service mysql start
  ```

- If all went well you should now be able to login to your MySQL database with a root password:

  ```console
  $ mysql -u root --password=NEWPASSWORD
  ```
  
  <br>
  
  [Source](https://linuxconfig.org/how-to-reset-root-mysql-password-on-ubuntu-18-04-bionic-beaver-linux)
