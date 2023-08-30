## CLIENT-SERVER ARCHITECTURE WITH MYSQL
Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another.
In their communication, each machine has its own role: the machine sending requests is usually referred as “Client” and the machine responding (serving) is called “Server”.
Assuming that you go on your browser, and typed in there www.google.com. It means that your browser is considered the “Client“. Essentially, it is sending request to the remote server, and in turn, would be expecting some kind of response from the remote server.
curl -Iv www.google.com---------Note: If your Ubuntu does not have ‘curl’, you can install it by running -- sudo apt install curl (https://ezekiel69.github.io/Portfolio-site/---my portfolio site)
In this example, your terminal will be the client, while www.google.com will be the server.
See the response from the remote server in below output. You can also see that the requests from the URL are being served by a computer with an IP address 160.153.133.153 on port 80.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/a1d939eb-820d-4ea5-ada2-51862df8812b)
Read about ping and traceroute network diagnostic utilities. Be able to make sense out of the results of using these tools.
## IMPLEMENT A CLIENT SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM (DBMS).
TASK – Implement a Client Server Architecture using MySQL Database Management System (DBMS).
To demonstrate a basic client-server using MySQL Relational Database Management System (RDBMS), follow the below instructions
1. Create and configure two Linux-based virtual servers (EC2 instances in AWS).
a.Server A name - `mysql server`   b.Server B name - `mysql client`
On mysql server Linux Server install MySQL Server software.
Interesting fact: MySQL is an open-source relational database management system. Its name is a combination of “My”, the name of co-founder Michael Widenius’s daughter, and “SQL”, the abbreviation for Structured Query Language.
On mysql client Linux Server install MySQL Client software.
By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of your ‘mysql client’.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/08830b7f-2361-41f8-a765-ac2c47dce569)-----Ensure you do 2 instances while creating the EC2 instance
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/2952807e-3fe0-442d-82bf-f16d11e34c5b)-----Mysql server connected
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/01a73641-323b-41d7-a58a-3ef83aee834e)-----Update done on mysql server
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/f47964de-7939-4c10-83da-974edd10136f)-----Client server connected and update done \
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/63f19389-9081-42ed-8955-f5d85a6be0f3)-----Mysql server installed
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/d8e32b9f-0d8c-4724-a465-d7514a10fd4f)-----My client server installed
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/dabf95d3-5a16-46c4-a3c5-a2fdac0d6cb7)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/c602e545-8f38-4bea-85a1-fc69be3da969)---25&26 commands to start, check status of service and enable service
To know what was installed/what to start, do which mysql
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/8eeeafb8-a503-4666-86bc-4aca3784ae58)-----you will notice because its not a service you don't need to install it 
By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of your ‘mysql client’.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/9edbe256-00d5-485b-9a5f-7b4ed9a3d599)---use only the private iP in your documentation (setting the inbound rule)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/25c19681-a538-456e-9917-2979431ef22e)---to check your IP do ip a
You might need to configure MySQL server to allow connections from remote hosts.--------sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
Replace ‘127.0.0.1’ to ‘0.0.0.0’
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/518ba429-31d6-493c-b040-d28ca5cf7de8)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/1d97650a-3928-44ec-bd5c-91b64dad26e1)---service started>>Anytime you make a change on a configuration file,Please restart the service>>if you have an error that means there something wrong with your confirguration file
From mysql client Linux Server connect remotely to mysql server Database Engine without using SSH. You must use the mysql utility to perform this action.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/0524feb7-ff70-4f09-b54f-9d8d7c8696ed)--Means anybody can acess it. But if you do the other secure installation its different (Do the below on mysql server database engine)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/d9de641d-10c7-48cc-86c3-679ece585990)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/4f9eb771-f46f-4854-a2c7-5002e0c1d14d)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/fa55d9f1-520e-4e7c-a4de-75f242f6b307)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/9d6bd046-3aa0-4343-bee3-8402665b453f)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/a27dde02-586f-467a-868f-bc7ea5d25ab4)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/7c78a982-16e1-4430-9327-f0d6f9e15d18)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/90e6342b-ffa7-45ab-ad4d-9a014e110cda)------exit
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/0190028f-1229-4acf-b9d9-2ab392ecef94)
Check that you have successfully connected to a remote MySQL server and can perform SQL queries
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/a5cc610b-9baa-4025-a1cf-004d625fa29a)---Run this on my client server, if you can connect to the database then you have successfully completed project 5 like me.
If you see an output similar to the below image, then you have successfully completed this project – you have deloyed a fully functional MySQL Client-Server set up.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/5701dea9-34f7-491b-a7b6-70aecdcc79f2)
connected to the database
password is password---Password because we did not so secure installation so by default it is password.
sudo mysql -u ezekiel -h 16.171.36.61 -p---this is the command you run on your client server to cnnect to your db server.






