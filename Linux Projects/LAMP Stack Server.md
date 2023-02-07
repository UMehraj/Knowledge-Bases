<H1 style="text-align: center;"> LAMP Stack Server </H1> 

### Outline: 
1. What is LAMP Stack Server? 
2. How to setup a LAMP Stack server on CentOS 7? 
3. Additional Information and Optional Configurations
---
### 1. What is LAMP Stack? 
---

<p> A LAMP Stack server is composed of 4 different technologies and can be used to host static/dynamic websites: </p>


* **Linux** - is an open-source OS that you can install and modify according to your application requirements. Linux is responsible for hosting the LAMP stack and is the first foundational block of the stack. 
* **Apache** - is the second layer of the LAMP stack server and is a free and open-source software that allows users to deploy their websites on the internet. It is one of the oldest and most reliable web server software maintained by the Apache Software Foundation, and it uses the HTTP protocol to establish connection between the server and client.
* **MySQL** (or MariaDB) - is the third layer of the LAMP stack server and is an open-source relational database management system. The database is used to store information and is capable of retrieving information when a user executes a query on their end.
* **PHP (or Python)** - is the fourth layer of the LAMP Stack server and is an open-source scripting language that works with Apache to help create dynamic web pages. HTML alone can't be used to perform dynamic processes, such as retrieving information from a databases. Ths is where PHP is useful, it can be used to establish functionality between Front-End and Back-End technologies.
---
### 2. How to setup a LAMP Stack server on CentOS 7?
--- 
```
To setup a LAMP stack server, execute the following commands on a CentOS 7 server with a user who has sudo privileges or as the root user:  
```
<details><summary>Steps to Install the <em>Apache</em> Web Server</summary> <br/>

Step 1 - Install the <em>Apache</em> web server package (httpd): 
```
    yum install -y httpd
```
Step 2 - Start and Enable the <em>httpd</em> service: 
```
    systemctl start httpd ; systemctl enable httpd 
```
Step 3 - By default the <em>Apache</em> web server uses the HTTP/S protocols, so we will have to open up ports 80/443:
```
    firewall-cmd --permanent --add-port={80,443}/tcp
    firewall-cmd --reload
```
Step 4 - With the installation successful and the ports opened, navigate to your server's IP address and you will see <em>Apache HTTP Server</em> Testing page. To get the IP address of your server, run the following command:
```
    ifconfig
```
<img src="https://pbs.twimg.com/media/EfsSthhXoAAgwIn.jpg" alt="Apache Testing Page">

</details>
<br/>

<details><summary>Steps to Install <em>MySQL (MariaDB)</em> Database </summary> <br/>

Step 1 - To install the <em>MariaDB</em>, execute the following command: 
```
    yum install mariadb-server
```
Step 2 - To start and enable <em>MariaDB</em>, run the following command:
```
    systemctl start mariadb ; systemctl enable mariadb 
```
Step 3 - To secure your database, <em>MariaDB</em>comes with pre-installed security script that you can invoke by running the following command: 
```
    sudo mysql_secure_installation
```
During the script installation, there will be a series of prompts that you will have to answer. The first prompt will be <strong>"Enter current password for root (enter for none):" </strong>, for this prompt press "Enter" for none because we have not setup a database root password. For the second prompt, you will be asked <strong> "Set root password? [Y/n]" </strong> type "N" and press "Enter". Since we are creating this LAMP Stack server in a lab environment, we can bypass setting a root password for now but in a production environment it would be best practice to set a root password for better security.   

The third prompt will ask <strong>"Remove anonymous users? [Y/n] </strong>, type "Y" and press "Enter". This will remove anonymous users. 

Next prompt will ask <strong>"Disallow root login remotely? [Y/n] </strong>, type "Y" and press "Enter". This prevent will prevent remote root login onto the database.

The last prompt will ask to reload the settings entered in the pervious prompts, <strong> "Reload privilege tables now? [Y/n] </strong>. For this prompt, type "Y" and press enter so the settings can be implemented. 

Once done with prompts, you should see the following message: 
```
All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```
Step 4 - Log onto the MariaDB console, by running: 
```
    sudo mysql
```
Once connected you should see the following message or something similar: 
```
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 8
Server version: 5.5.68-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>

```

Seeing the above message confirms that <em>MariaDB</em> was successfully installed!  

</details> <br/>

<details><summary>Steps to Install <em>PHP</em> Scripting Language </summary> <br/>

Step 1 - To install <em>PHP</em>, you will need to install the following packages: 
```
yum install -y php php-mysql
```

Step 2 - Since <em>PHP</em> is a module and not a service, we will have restart <em>httpd</em> to enable <em>PHP</em> by invoking the following command: 
```
systemctl restart httpd
```

Step 3 - To test if <em>PHP</em> is working correctly, create a file called ```info.php``` underneath ``` /var/www/html/ ``` and inside the file add the following line: 
```
<?php phpinfo(); ?>
```

Afterwards navigate to your server's IP address: ``` <IP Address>/info.php ``` and it should display something similar to this: <br/>  

<img src="https://www.webhostinghub.com/help/images/stories/website/phpinfo.jpg" alt="PHP Information Page">

</details> <br/>

```
With Apache, MySQl, and PHP installed, you have successfully configured a LAMP Stack Server!
```
---
### 3. Additional Information and Optional Configurations - 
---
```
In this section we will cover where the Apache Web Server Logs, and Root Web Directory are located. Additionally we will cover steps you take on your MariaDB database to make it more secure.
```
<ul> <details><summary> <em>Apache </em> Web Server Logs </summary> <br/>

The <em>Apache</em> web server logs are located underneath the following directory: ``` /var/log/httpd/ ```. 
Underneath this directory, you will find the following files: 
```
[root@webserver1 httpd]# pwd
/var/log/httpd
[root@webserver1 httpd]# ll
total 12
-rw-r--r--. 1 root root 6503 Feb  6 17:35 access_log
-rw-r--r--. 1 root root 3065 Feb  6 17:35 error_log
```
* Error Logs - This log file will contain all or any errors associated to the <em>Apache</em>, and some errors will distinguishable by HTTP Response Codes. 
* Access Logs - This log file will contain event logs for whenever someone accessed your website. 

</details></ul>

<ul> <details><summary> <em>Apache </em> Web Server Root Web Directory </summary> <br/>

The root web directory for <em>Apache</em> Web Server is the following directory: ```/var/www/html/```. Underneath this directory, you will store your webpages. In the following example, I created a webpage that contains "Hello, I am testing my webserver" and used the ```curl``` command to display my webpage: 
```
[root@webserver1 html]# pwd
/var/www/html
[root@webserver1 html]# echo "Hello, I am testing my webserver" >> index.html
[root@webserver1 html]#
[root@webserver1 html]# ll
total 4
-rw-r--r--. 1 root root 33 Feb  6 19:57 index.html
[root@webserver1 html]#
[root@webserver1 html]# cat index.html
Hello, I am testing my webserver
[root@webserver1 html]#
[root@webserver1 html]# curl 192.168.1.51
Hello, I am testing my webserver
```
</details> </ul>

<ul> <details><summary> Optional Configurations: Making our <em>MariaDB </em> database more secure</summary> <br/>

To add an extra layer of security to our database, we can configure user accounts to have less privileges for every database and this is helpful when you have multiple databases being hosted on a single server. 

To illustrate this I will create a new database called ```Practice``` and give full user ```john``` full privileges and at the same time restricting ```john``` from creating and modifying other databases on your server. 

To follow along execute the following commands: 
```
[root@webserver1 ~]# mysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 14
Server version: 5.5.68-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> CREATE DATABASE Practice;
Query OK, 1 row affected (0.00 sec)
MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| Practice           |
| mysql              |
| performance_schema |
| testing            |
+--------------------+
5 rows in set (0.00 sec)

```
Next execute the following command: 
```
GRANT ALL ON Practice.* TO 'john'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
```
* This command will grant all privileges to the user, john, for the Practice database and prevent John from creating/modifying any other databases on the server. The command will assign "password" as the database password for user john

Next, run the following command to reload and save the privileges granted to user John: 
```
FLUSH PRIVILEGES;
```
```
MariaDB [(none)]> GRANT ALL ON Practice.* TO 'john'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
Query OK, 0 rows affected (0.00 sec)
MariaDB [(none)]> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)
```

Now exit out of the database and log onto the database as user john: 
```
[root@webserver1 ~]# mysql -u john -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 17
Server version: 5.5.68-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

```
To ensure the changes to privileges went through, attempt to create a database and you will see an access denied error: 

```
MariaDB [(none)]> CREATE DATABASE Dev;
ERROR 1044 (42000): Access denied for user 'john'@'localhost' to database 'Dev'
```
</details> </ul>
