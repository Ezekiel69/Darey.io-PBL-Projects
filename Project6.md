## WEB SOLUTION WITH WORDPRESS (THE REDHAT WAY)
In this project you will be tasked to prepare storage infrastructure on two Linux servers and implement a basic web solution using WordPress. WordPress is a free and open-source content management system written in PHP and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS).
## Project 6 consists of two parts:
1. Configure storage subsystem for Web and Database servers based on Linux OS. The focus of this part is to give you practical experience of working with disks, partitions and volumes in Linux.
2. Install WordPress and connect it to a remote MySQL database server. This part of the project will solidify your skills of deploying Web and DB tiers of Web solution.
As a DevOps engineer, your deep understanding of core components of web solutions and ability to troubleshoot them will play essential role in your further progress and development.
## Three-tier Architecture
Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.
Three-tier Architecture is a client-server software architecture pattern that comprise of 3 separate layers.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/e3bd2f12-d0a5-4b34-9c85-4bf56682381a)
1. Presentation Layer (PL): This is the user interface such as the client server or browser on your laptop.
2. Business Layer (BL): This is the backend program that implements business logic. Application or Webserver
3. Data Access or Management Layer (DAL): This is the layer for computer data storage and data access. Database Server or File System Server such as FTP server, or NFS Server
In this project, you will have the hands-on experience that showcases Three-tier Architecture while also ensuring that the disks used to store files on the Linux servers are adequately partitioned and managed through programs such as gdisk and LVM respectively.
You will be working working with several storage and disk management concepts, to have a better understanding, watch following video:
Disk management in Linux
Note: We are gradually introducing new AWS elements into our solutions, but do not be worried if you do not fully understand AWS Cloud Services yet, there are Cloud focused projects ahead where we will get into deep details of various Cloud concepts and technologies – not only AWS, but other Cloud Service Providers as well.
>>>>>>>>> ## Your 3-Tier Setup
1. A Laptop or PC to serve as a client
2. An EC2 Linux Server as a web server (This is where you will install WordPress)
3. An EC2 Linux server as a database (DB) server
## Use RedHat OS for this project: Redhad in your insatnce creation rather ubuntu
In project 6 when creating your instance instead of using the ubuntu use redhat Please!
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/aeb633be-8ba2-4026-8861-4c14063f11e5)---Instance has been launched successfully. Remember you need two server.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/6f34b048-ea96-45c5-ad20-9480bea50e3c)---Instance running. You edit the name after specifying two instances when creating the instance 
## LAUNCH AN EC2 INSTANCE THAT WILL SERVE AS “WEB SERVER”.
Step 1 — Prepare a Web Server
Launch an EC2 instance that will serve as “Web Server”. Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/da280f4c-cc8c-406b-a1e5-9123536c3ae3)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/1e5b8861-61c7-4636-998a-2dbcda9d7f4b)--Volumes attached
Open up the Linux terminal to begin configuration
Use lsblk command to inspect what block devices are attached to the server. Notice names of your newly created devices. All devices in Linux reside in /dev/ directory. Inspect it with ls /dev/ and make sure you see all 3 newly created block devices there – their names will likely be nvme1n1,nvme2n1 and nvme2n1
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/c429ac9a-0b5e-46a8-83e7-9ab9af61c767)
Use df -h command to see all mounts and free space on your server
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/c84cc136-7562-4642-9f74-fb226f5a642c)
Use gdisk utility to create a single partition on each of the 3 disks
sudo gdisk /dev/nvme1n1---my first disk
sudo gdisk /dev/nvme2n1---my second disk
sudo gdisk /dev/nvme3n1---my third disk
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/f8e6b427-885a-4153-8ae2-893170a104e7)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/8c2548e9-dcc0-4c4d-9478-d4261638134b)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/6496c748-ae9c-49a9-ad7c-16e7612c9109)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/beed578e-2014-46b0-863b-cf95e9946f2f)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/7206764a-45c2-463e-9d73-801871fadedb)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/167eefd1-a802-4b28-9242-068074b36c93)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/8f291a23-ee82-4c09-a3a2-eee578799329)
Do this gdisk for each of the devices
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/e2da0cfd-c42a-4047-9d41-4e3cdb18b3bc)
Use lsblk utility to view the newly configured partition on each of the 3 disks.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/1b779197-8663-4255-bf55-a6623fecc878)
Install lvm2 package using ----sudo yum install lvm2. ----Run sudo lvmdiskscan command to check for available partitions.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/4533656e-bc9f-419b-8dcf-564942113dd7)
Note: Previously, in Ubuntu we used apt command to install packages, in RedHat/CentOS a different package manager is used, so we shall use yum command instead.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/6dda08fb-4b6d-4d69-9772-9da9ade2bc07)
Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM
If i want to create a physical volume directly on my device nvme1n1 i have to do it on the partitioning 
Run sudo pvcreate /dev/nvme1n1p1 /dev/nvme2n1p1 /dev/nvme3n1p1--this will create it on all the partioning 
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/69786666-61b0-450c-95e7-c8ecf221a463)
Verify that your Physical volume has been created successfully by running sudo pvs
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/cfd1ad34-d577-45c4-8270-fe287496ff54)
Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg, Vgcreate(volume group)----to add together>>>>the LVM has to be in a group>>>----sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/498a0769-8cce-4e16-8ee2-14a59fbec65c)
Verify that your VG has been created successfully by running sudo vgs
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/56b943f7-e0dc-4aa9-85c8-d8d46027d01c)
Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.
sudo lvcreate -n apps-lv -L 14G webdata-vg----Apps
sudo lvcreate -n logs-lv -L 14G webdata-vg---Logs
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/e3278319-0314-48b5-961d-841408108900)
If we exhaust the size for our app, we can just go and add more disk and pvcreate and create and another group that will now group all the pvcreated successfully and we are goo
Verify that your Logical Volume has been created successfully by running sudo lvs
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/db45e241-8330-49bf-9f9f-fe87bf92016f)
Verify the entire setup-----sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/b2e3d67d-c636-4b90-a4e6-d65b8706f144)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/3e4a3986-8d5d-421b-bdc0-80ff96ee4a36)
Now we have created a device we need to add a file system to it--Use mkfs.ext4 to format the logical volumes with ext4 filesystem
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv--Apps
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv----Logs
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/a30d30da-9015-46b2-9d3d-b902e0c30466)
Create /var/www/html directory to store website files-----sudo mkdir -p /var/www/html
Create /home/recovery/logs to store backup of log data----sudo mkdir -p /home/recovery/logs
Mount /var/www/html on apps-lv logical volume-----sudo mount /dev/webdata-vg/apps-lv /var/www/html/
Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)----------sudo rsync -av /var/log/. /home/recovery/logs/
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/92b885e4-4230-42ae-a60a-318cc1c2b353)--back up taken
Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15 above is very important)------sudo mount /dev/webdata-vg/logs-lv /var/log
Restore log files back into /var/log directory--------------sudo rsync -av /home/recovery/logs/log/. /var/log
when you want to mount check---mounting a directory means you want to delete what is in the directotry
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/562e34f3-af83-40a1-aa03-cb6ba4d057d5)---See back up.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/396077db-9434-430f-9dbf-6687758cc21e)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/f0bf63a0-8e2f-4ead-a46f-9f91f667a5fd)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/6c81df7c-1e36-4b43-8747-05ec11067422)---if i restart the instance the last one will disappear but i dont have to
Update /etc/fstab file so that the mount configuration will persist after restart of the server.
The UUID of the device will be used to update the /etc/fstab file;
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/a8195a62-6556-4678-a1d9-0b0780f0625c)
sudo blkid
sudo vi /etc/fstab
I need the app and logs to configure my fstab---from the /dev/mapper/webdata--vg 
Update /etc/fstab in this format using your own UUID (see below) and rememeber to remove the leading and ending quotes.
UUID=4865869e-794e-401d-babe-2937fc5774ba /var/www/html ext4 defaults,nofail 0 0
UUID=2775fc71-e5d3-4a09-8c3c-34553a8feb9f /var/log ext4 defaults 0 0
Test the configuration and reload the daemon
sudo mount -a
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/5732588a-fc6b-4370-bbc2-52336406cbe7)
sudo systemctl daemon-reload
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/a0f431d1-1587-451e-9478-fdfe5e589d39)
Verify your setup by running df -h, output must look like this:
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/9a78dd6b-ee60-487b-a107-834d1060d9af)
we are done with the websolution 
## NOW THE DB
Now lets attach 3 volumes to it 
## Step 2 — Prepare the Database Server
Launch a second RedHat EC2 instance that will have a role – ‘DB Server’
Repeat the same steps as for the Web Server, but instead of apps-lv create db-lv and mount it to /db directory instead of /var/www/html/.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/b5a11286-b34c-4fc4-87eb-9024d01855ff)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/e3eb90bb-4a0f-45a5-8214-734759d392a1)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/9e6edb1d-41dc-4067-9e27-7a1ecd8a9d2d)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/46c2badd-bc61-4508-8c9f-dbdbc7a3d0f7)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/d74d5394-129b-4bbd-af77-d78418974763)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/7697eaab-ded6-4917-8bb6-f9c93eb35722)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/dd65a9aa-3792-4894-a1ac-3fea3c8f2344)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/80d7e6f0-1704-4fa2-936d-98e57b19c102)
copy this out from the screenshot above
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/301b4992-a0d9-412b-af89-c79ad7bd5ca3)
use this in your FSTAB in the dbserver UUID=ebd3ff65-f1d2-49f5-b5aa-e47001463340 /db ext4 defaults 0 0
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/361a0869-49ce-49dd-be76-3ad3c38cc443)
Now we are good 
lets install word press
## Step 3 — Install WordPress on your Web Server EC2
Update the repository---sudo yum -y update
Install wget, Apache and it’s dependencies-----sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
Start Apache---sudo systemctl enable httpd, sudo systemctl start httpd
## To install PHP and it’s depemdencies
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo yum module list php
sudo yum module reset php
sudo yum module enable php:remi-7.4
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
sudo setsebool -P httpd_execmem 1
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/14f51d70-9c8e-49c3-a92b-7fcd6f40704d)
sudo Restart Apache
sudo systemctl restart httpd
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/a67875af-b3f7-45ca-88ba-bc94a782061b)
## Download wordpress and copy wordpress to var/www/html
  mkdir wordpress
  cd   wordpress
  sudo wget http://wordpress.org/latest.tar.gz
  sudo tar xzvf latest.tar.gz
  sudo rm -rf latest.tar.gz
  cp wordpress/wp-config-sample.php wordpress/wp-config.php
  cp -R wordpress /var/www/html/
## Configure SELinux Policies
  sudo chown -R apache:apache /var/www/html/wordpress
  sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
  sudo setsebool -P httpd_can_network_connect=1
  sudo setsebool -P httpd_can_network_connect_db 1
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/01ea3e19-d58f-4355-b9b7-2db95ea73c24)
## Step 4 — Install MySQL on your DB Server EC2
sudo yum update
sudo yum install mysql-server
Verify that the service is up and running by using sudo systemctl status mysqld, if it is not running, restart the service and enable it so it will be running even after reboot:
sudo systemctl restart mysqld
sudo systemctl enable mysqld
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/dd0edafc-1d4c-435f-a370-43f4d63aa3d4)
After enabling your mysql server,do installation command-----sudo mysql_secure_installation
And follow the prompt 
n first then set password ---eze then y all through 
sudo mysql -u root -p ----next run this command 
password is eze
## Step 5 — Configure DB to work with WordPress
sudo mysql
CREATE DATABASE wordpress;
CREATE USER `myuser`@`<Web-Server-Private-IP-Address>` IDENTIFIED BY 'mypass';-------(Use this)CREATE USER 'wordpress'@'%' IDENTIFIED BY 'wordpress'; this means create user wordpress that can connect anywhere with a secured password as word press to protect your database
GRANT ALL ON wordpress.* TO 'myuser'@'<Web-Server-Private-IP-Address>';----(use this)GRANT ALL ON *.* TO 'wordpress'@'%' WITH GRANT OPTION; The first star means database while the second means tables Word press as your user,The next is percent, meaning anywhere can connect
FLUSH PRIVILEGES;
SHOW DATABASES;
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/40dd5325-4c8e-4b8a-8086-aa127778ef95)
select user, host from mysql.user;----------to check for users
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/87a0cee3-49da-427e-a44c-d38e96572426)
exit
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/fb16eec4-abc3-481a-8ce0-dc7e47251250)
drop user 'wordpress'@'172.31.36.237';----command to drop user
sudo vi /etc/my.cnf----Run this command after you have exited the database--we want to include our bind addrss
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/94a127ca-92cb-484d-8446-2979484343a6)
put the buind address in the configuration file in the db server 
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/f308f18f-eb6f-4c46-9db8-c1a76ab5ef5d)---restart the service
No on your webserver go to word press directory cd /var/www/html/wordpress
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/877b7769-547c-44c4-85f3-d6735a2a2975)
Open the php file ----- sudo vi wp-config.php
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/96eff759-f019-45e5-b1aa-4c9512715b80)
so you will see local host, then i have to format using the private IP of the DB server because both server are amazon servers the will connect. i copy the database private ip and change local host 
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/6e803eaf-c7b2-42d8-acfe-77a02d925af4)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/ed84d0c5-2d48-44f2-9c6c-264677c007ef)
Next we need to disable the default page of our apache
Change permissions and configuration so Apache could use WordPress: 
sudo mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.conf_backup------do this
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/400284af-45c2-4825-bdce-0dc8e86d0962)
Step 6 — Configure WordPress to connect to remote database.
Hint: Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, you shall allow access to the DB server ONLY from your Web Server’s IP address, so in the Inbound Rule configuration specify source as /32
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/0ad7f84e-a207-4cc6-95ad-7a806497c2f8)---make 3306 open
mysql -u wordpress -p -h 172.31.47.225 run this command to test that you can connect to your DB server from your webserver but do cd first to take to the home directory
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/636709f6-0a84-4e65-847c-12a186b16b69)
Now use your public ip and test it on the browser  while your DB is conneted 
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/9197bf57-8dad-4fb6-9308-818c7fd67247)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/c82c97b4-5b10-4e96-86ad-78b2c38a13bc)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/066475ac-3171-4789-ac16-c1a317d7150f)


