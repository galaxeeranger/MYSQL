# Start MySQL

To Start MYSQL Server

Open cmd/Command Promt

Go to path of your sql folder


$ C:\Users>cd mysql/bin

$ C:\Users\mysql\bin>mysql -u root -p        # to open mysql application

$ Enter password: ******                     # enter your password

Your MySQL server will be start
__________________________________________________________________________________________________________________
If You face any issue like this after entering password

ERROR 2003 (HY000): Can't connect to MySQL server on 'localhost:3306' (10061)

This error is flashed when we are trying to open mysql with out stating the service

-------------------------------
1. Open cmd prompt to start the service type

$ C:\Users\mysql\bin>mysqld --console     #This will start the mysql server

2. Don't close this cmd prompt and open a new cmd prompt and type

$ C:\Users\mysql\bin>mysql -u root -p

$ Enter password: ******

Great Start the MySQL server!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
