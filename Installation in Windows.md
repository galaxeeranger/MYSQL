____________________________________________________________________________________
*** WINDOWS INSTALLATION ***
--------------------
To install Mysql zip file in WINDOWS

If you have an .exe go with GUI interface/popups screen and if have zip file.

Here's How to Install and Configure MYSQL

*// Download zip file from given link : https://dev.mysql.com/downloads/

After downloading zip file extract it and copy the extracted file in C drive : C:\Users
Rename the extracted file to get easly access

I rename to "mysql"

Open Command Promt/cmd

Go to sql file path in cmd and write your first command
______________________________________________________________________________________
1.  Initialize the database. Create a root user without password.

$ C:\Users>cd mysql
$ C:\Users\mysql>cd bin
$ C:\Users\mysql\bin>mysqld --initialize --console 

Run these commands and
In last line you will get password - 'root'@'local': #fghgdok14C

Note down your password somewhere

Great Job!!!!!!!!
First step is done we created a superuser and got password
______________________________________________________________________________________
2.  start the MySQL Database Server

$ C:\Users\mysql\bin>mysqld --console

By this command we start the mysql server
______________________________________________________________________________________
Note: After 2nd step open an another Command Promt/cmd and run other commands
Go to sql file path in cmd and write your third step/command
______________________________________________________________________________________
3.  Start a client as superuser "root"

$ C:\Users\mysql\bin>mysql -u root -p   # hit enter

Note: cmd will ask for your Paasword we noted earlier

$ Password: *********

We enter our password correctly but we will see only star *** on screen

Now we run the client server as superuser
______________________________________________________________________________________
4.  change password
  Now we have to change our passowrd bcoz we cant remember the give password
  
$ ->alter user 'root'@'localhost' identified by 'your new password';

After running this query you password will get updated 

$ -> quit            #this will quit the server
$ Bye                # output
$ exit               # exit the 2nd command promt/cmd
______________________________________________________________________________________
still we are on our first cmd screen write same commands to exit

Press ctrl+C            #this will quit the server
$ C:\Users\mysql\bin>exit               # exit the 1st command promt/cmd
______________________________________________________________________________________

Note: How to start our mysql server read the README.md file

Thank You
