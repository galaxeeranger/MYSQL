____________________________________________________________________________________
*** LINUX INSTALLATION ***
--------------------
Here's How to Install and Configure MYSQL in Linux based OS like Ubuntu,MacOS

Please read the notes carefully. OR Ref: https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04

Open Terminal/Command Promt/Shell
______________________________________________________________________________________
Step 1.  Installing MySQL

To install it, update the package index on your server if you’ve not done so recently:
$ sudo apt update           -> Type in Terminal and hit Enter

Then install the mysql-server package:
$ sudo apt install mysql-server        -> Type in Terminal and hit Enter

Ensure that the server is running using the systemctl start command:
$ sudo systemctl start mysql.service        -> Type in Terminal and hit Enter

These commands will install and start MySQL, but will not prompt you to set a password or make any other configuration changes.
Because this leaves your installation of MySQL insecure, we will address this next.
______________________________________________________________________________________
Step 2.  Configuring MySQL

For fresh installations of MySQL, you’ll want to run the DBMS’s included security script.
This script changes some of the less secure default options for things like remote root logins and sample users.

Run the security script with sudo:
$ sudo mysql_secure_installation                           -> Type in Terminal and hit Enter

Note: This will take you through a series of prompts where you can make some changes to your MySQL 
	installation’s security options. The first prompt will ask whether you’d like to set up the Validate 
	Password Plugin, which can be used to test the password strength of new MySQL users before deeming them valid.

	If you elect to set up the Validate Password Plugin, any MySQL user you create that 
	authenticates with a password will be required to have a password that satisfies the policy you select. 
	The strongest policy level — which you can select by entering 2 — will require passwords to be at least 
	eight characters long and include a mix of uppercase, lowercase, numeric, and special characters:

Output: 
	Securing the MySQL server deployment.

	Connecting to MySQL using a blank password.

	VALIDATE PASSWORD COMPONENT can be used to test passwords
	and improve security. It checks the strength of password
	and allows the users to set only those passwords which are
	secure enough. Would you like to setup VALIDATE PASSWORD component?

	Press y|Y for Yes, any other key for No: Y

	There are three levels of password validation policy:

	LOW    Length >= 8
	MEDIUM Length >= 8, numeric, mixed case, and special characters
	STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

	Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 2

Note: Regardless of whether you choose to set up the Validate Password Plugin, 
	the next prompt will be to set a password for the MySQL root user. Enter and then confirm a secure password of your choice:

Output:
	Please set the password for root here.


	New password: #Set any Password you like

	Re-enter new password:  #Re-enter your Password

Note that even though you’ve set a password for the root MySQL user, this user is not currently 
configured to authenticate with a password when connecting to the MySQL shell.

Note: If you used the Validate Password Plugin, you’ll receive feedback on the strength of your new password. 
	Then the script will ask if you want to continue with the password you just entered or if you want to enter a new one. 
	Assuming you’re satisfied with the strength of the password you just entered, enter Y to continue the script:

Output:
	Estimated strength of the password: 100
	Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : Y

Note: From there, you can press Y and then ENTER to accept the defaults for all the subsequent questions. 
	This will remove some anonymous users and the test database, disable remote root logins, 
	and load these new rules so that MySQL immediately respects the changes you have made.

	Once the script completes, your MySQL installation will be secured. You can now move on to 
	creating a dedicated database user with the MySQL client.
______________________________________________________________________________________
Step 3.  Creating a Dedicated MySQL User and Granting Privileges

Note: Upon installation, MySQL creates a root user account which you can use to manage your database. 
	This user has full privileges over the MySQL server, meaning it has complete control over every database, 
	table, user, and so on. Because of this, it’s best to avoid using this account outside of administrative 
	functions. This step outlines how to use the root MySQL user to create a new user account and grant it privileges.

	In Ubuntu systems running MySQL 5.7 (and later versions), the root MySQL user is set to authenticate using 
	the auth_socket plugin by default rather than with a password. This plugin requires that the name of the 
	operating system user that invokes the MySQL client matches the name of the MySQL user specified in the command, 
	so you must invoke mysql with sudo privileges to gain access to the root MySQL user:

$ sudo mysql                   -> Type in Terminal and hit Enter

Note: If you installed MySQL with another tutorial and enabled password authentication for root, 
	you will need to use a different command to access the MySQL shell. The following will run your 
	MySQL client with regular user privileges, and you will only gain administrator 
	privileges within the database by authenticating:

$ mysql -u root -p                    -> Type in Terminal and hit Enter

Once you have access to the MySQL prompt, you can create a new user with a CREATE USER statement. These follow this general syntax:
$ mysql> CREATE USER 'username'@'host' IDENTIFIED WITH authentication_plugin BY 'password';                -> Type in Terminal/Mysql and hit Enter

Note: After CREATE USER, you specify a username. This is immediately followed by an @ sign and then the 
	hostname from which this user will connect. If you only plan to access this user locally from your 
	Ubuntu server, you can specify localhost. Wrapping both the username and host in single quotes isn’t 
	always necessary, but doing so can help to prevent errors.

Run the following command to create a user that authenticates with caching_sha2_password. 
Be sure to change shiv to your preferred username and password to a strong password of your choosing:
$ mysql> CREATE USER 'shiv'@'localhost' IDENTIFIED BY 'password';                                    -> Type in Terminal/Mysql and hit Enter

Note: There is a known issue with some versions of PHP that causes problems with caching_sha2_password. 
	If you plan to use this database with a PHP application — phpMyAdmin, for example — you may want to 
	create a user that will authenticate with the older, though still secure, mysql_native_password plugin instead:
$ mysql> CREATE USER 'shiv'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';            -> Type in Terminal/Mysql and hit Enter

If you aren’t sure, you can always create a user that authenticates with caching_sha2_plugin and then ALTER it later on with this command:
$ mysql> ALTER USER 'shiv'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';        -> Type in Terminal/Mysql and hit Enter

After creating your new user, you can grant them the appropriate privileges. The general syntax for granting user privileges is as follows:
$ mysql> GRANT PRIVILEGE ON database.table TO 'username'@'host';                  -> Type in Terminal/Mysql and hit Enter

Run this GRANT statement, replacing shiv with your own MySQL user’s name, to grant these privileges to your user:
$ mysql> GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'shiv'@'localhost' WITH GRANT OPTION;      -> Type in Terminal/Mysql and hit Enter

Note: This statement also includes WITH GRANT OPTION. This will allow your MySQL user to grant any permissions that it has to other users on the system.

Warning: Some users may want to grant their MySQL user the ALL PRIVILEGES privilege, which will provide 
	them with broad superuser privileges akin to the root user’s privileges, like so:

$ mysql> GRANT ALL PRIVILEGES ON *.* TO 'shiv'@'localhost' WITH GRANT OPTION;          -> Type in Terminal/Mysql and hit Enter

	Such broad privileges should not be granted lightly, as anyone with access to this MySQL user 
	will have complete control over every database on the server.

Following this, it’s good practice to run the FLUSH PRIVILEGES command. This will free up any memory that 
the server cached as a result of the preceding CREATE USER and GRANT statements:
$ mysql> FLUSH PRIVILEGES;                             -> Type in Terminal/Mysql and hit Enter

Then you can exit the MySQL client:
$ mysql> exit                                          -> Type in Terminal/Mysql and hit Enter

In the future, to log in as your new MySQL user, you’d use a command like the following:
$ mysql -u shiv -p                                     -> Type in Terminal and hit Enter

Note: The -p flag will cause the MySQL client to prompt you for your MySQL user’s password in order to authenticate.


Great Job!!!!!!!!!!!!!!!!!! Everything goes well.

Finally, let’s test the MySQL installation
______________________________________________________________________________________
Step 4: Testing MySQL

Regardless of how you installed it, MySQL should have started running automatically. To test this, check its status.
$ systemctl status mysql.service                -> Type in Terminal and hit Enter

You’ll see output similar to the following:

Output: 
	● mysql.service - MySQL Community Server
     	  Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
     	  Active: active (running) since Tue 2020-04-21 12:56:48 UTC; 6min ago
   	Main PID: 10382 (mysqld)
     	  Status: "Server is operational"
      	   Tasks: 39 (limit: 1137)
     	  Memory: 370.0M
     	  CGroup: /system.slice/mysql.service
             └─10382 /usr/sbin/mysqld

If MySQL isn’t running, you can start it with sudo systemctl start mysql.

Note: For an additional check, you can try connecting to the database using the mysqladmin tool, which is a client that lets you run 
	administrative commands. For example, this command says to connect as a MySQL user named shiv (-u shiv), prompt for a password (-p), 
	and return the version. Be sure to change shiv to the name of your dedicated MySQL user, and enter that user’s password when prompted:
$ sudo mysqladmin -p -u shiv version                             -> Type in Terminal and hit Enter

You should see output similar to this:
Output:
	mysqladmin  Ver 8.0.19-0ubuntu5 for Linux on x86_64 ((Ubuntu))
	Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

	Oracle is a registered trademark of Oracle Corporation and/or its
	affiliates. Other names may be trademarks of their respective
	owners.

	Server version        8.0.19-0ubuntu5
	Protocol version    10
	Connection        Localhost via UNIX socket
	UNIX socket        /var/run/mysqld/mysqld.sock
	Uptime:            10 min 44 sec

	Threads: 2  Questions: 25  Slow queries: 0  Opens: 149  Flush tables: 3  Open tables: 69  Queries per second avg: 0.038

This means MySQL is up and running.
______________________________________________________________________________________
$ mysql> quit               # this will quit the server
$ mysql> Bye                # output
$ mysql> exit               # exit the 2nd command promt/cmd
______________________________________________________________________________________

Note: How to start our mysql server read the README.md file

Thank You
