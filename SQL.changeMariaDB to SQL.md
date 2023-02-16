XAMPP - Replace MariaDB with MySQL
Since XAMPP 5.5.30 and 5.6.14, XAMPP ships MariaDB instead of MySQL. MariaDB is not 100% compatible with MySQL and can be replaced with the "orginal" MySQL server.

Backup
Backup the old database into a sql dump file
Stop the MariaDB service
Rename the folder: c:\xampp\mysql to c:\xampp\mariadb
Installation
Download MySQL Community Server: https://dev.mysql.com/downloads/mysql/
Goto: Other Downloads > ZIP Archive 5.7.19 306.2M (mysql-5.7.19-win32.zip) > No thanks, just start my download.
Create a new and empty folder: c:\xampp\mysql
Extract mysql-5.7.19-win32.zip to: c:\xampp\mysql
Create a new and empty folder: c:\xampp\mysql\data
Create a new file: c:\xampp\mysql\bin\my.ini and copy this content:
[mysqld]
# set basedir to your installation path
basedir=c:/xampp/mysql
# set datadir to the location of your data directory
datadir=c:/xampp/mysql/data
Initializing the data directory
Initialize a MySQL installation by creating the data directory and populating the tables in the mysql system database.

Open the console (cmd) and enter:

cd c:\xampp\mysql\bin
mysqld --initialize
mysqld --initialize-insecure
The server creates a 'root'@'localhost' and a random password.

Check to make sure that the c:\xampp\mysql\data directory was created. If successful you will find a a [computer name].err file in the data folder with the temporary one time use password for you to login as root for the first time.

Open with Notepad++: c:\xampp\data\[computer name].err

[Note] A temporary password is generated for root@localhost: **************

Copy the password

Start the server
You can use the XAMPP Control Panel (MySQL > Start) to start the MySQL service.

Connect to the server
Open a new console (cmd) window and enter:

cd c:\xampp\mysql\bin
mysql -u root -p
Enter the password you have found in the .err file and press enter.

Now reset the root password to '' (empty).

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '';
Exit the mysql command:

mysql> exit