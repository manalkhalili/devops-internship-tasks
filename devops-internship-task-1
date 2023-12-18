# devops-internship-tasks
COURSE NAME
Trainees TASK [linux + shell]


# 1-LVM
Create a volume group, and set 16M as extends. And divided a volume group containing 50 extends on volume group lv, make it as ext4 file system, and mounted automatically under /mnt/data. Please note that this should be implemented on the second disk 
**solution:**
-Use the pvcreate command to initialize the disk /dev/sdb as a physical volume.
pvcreate /dev/sdb
-Create a volume group named myvg with 16MB extents using the physical volume /dev/sdb.
vgcreate -s 16M myvg /dev/sdb
-Format the logical volume lv as an ext4 file system.
lvcreate -n lv -l 50 myvg
-Format the logical volume lv as an ext4 file system.
mkfs.ext4 /dev/myvg/lv
-Mount the logical volume lv automatically at /mnt/data.
mkdir -p /mnt/data
mount /dev/myvg/lv /mnt/data
-Check if the logical volume is mounted correctly by listing the contents of /mnt/data.
ls /mnt/data

 # 2-users, groups and permissions
1- Add user: user1, set uid=601 Password: redhat. The user's login shell should be non-interactive. (no ssh access to server)
 2- Add user1 to group TrainingGroup. 
3- Add users: user2, user3. The Additional group of the two users: user2, user3 is the admin group Password: redhat, user 3 with root permissions 
**solution:**
sudo useradd -u 601 -s /usr/sbin/nologin user1
sudo passwd user1
sudo usermod -aG TrainingGroup user1
Create user2 and user3, set additional group as admin, and grant root permissions to user3:
sudo useradd -G admin user2
sudo useradd user3
sudo usermod -aG admin user3
sudo passwd user2
sudo passwd user3
sudo usermod -aG sudo user3

 
# 3-SSH
Generate SSH key and connect to different VM without password. 
**solution:**
generate ssh key
ssh-keygen -t rsa
Copy Public Key to Remote VM
ssh-copy-id manalkh2@10.0.2.7
ssh manalkh2@10.0.2.7
and using exit to  logout from the remote vm
# 4-Permissions
Copy /etc/fstab to /var/tmp name admin, the user1 could read, write and modify it, while user2 can’t do any permission
**Solution:**
copy the /etc/fstab file to /var/tmp:
 sudo cp /etc/fstab /var/tmp/admin
 sudo chown user2 /var/tmp/admin
 sudo chown u=rw  /var/tmp/admin
# Deny all permissions for user3
sudo chown user3 /var/tmp/admin
sudo chmod u=  /var/tmp/admin
to see the permition
ls -l /var/tmp/admin

# 5-permissions 
SELinux must be running in the Enforcing mode (permanent even after reboot). 
**Solution:**
opend the file 
sudo vi /etc/selinux/config
and change this line to this 
SELINUX=enforcing






# 6-bash script and processes

Write a shell script that will keep running for 10 mins in the background and check the process that it's created and try to kill using commands. 
**Solution:**
the file name kill.sh
#!/bin/bash

# Function to check and kill the process
check_and_kill_process() {
  if pgrep -x "md" > /dev/null; then
    echo "Process 'md' found, killing"
    pkill -x "md"
    echo "Process 'md' killed"
  else
    echo "Process 'md' not found"
  fi
}

runtime=$((10*60))  # 10 minutes in seconds
endtime=$((SECONDS+runtime))

while [ $SECONDS -lt $endtime ]; do
  check_and_kill_process
  sleep 30  # Check every 30 seconds
done


- to see all the processes ps aux command
-make it executable (chmod +x kill.sh).
-after that i will run the terminal using ./kill.sh



# 7-Yum Repo
1- Install tmux on your machine
 2- Install apache server on your machine(httpd) and Install mysql. 
3- Create a local yum repository on your local machine(available publicly) with the zabbix rpms: https://repo.zabbix.com/zabbix/4.4/rhel/7/x86_64/
4- Disable all other repositories and keep only the new repo 
5- Install zabbix rpms from the new repo (Download zabbix, zabbix-web,php, zabbix-server, zabbix-agent rpm’s and their dependencies
**Solution:**
-vi /etc/yum.repos.d/zabbix-local.repo
I put inside of it tese lines 
[zabbix-local]
name=local Zabbix Repository
baseurl=file:///var/www/html/zabbix-localrepo
enabled=1
gpgcheck=0




-/var/www/html/zabbix-localrepo
Put the download rpms inside this directory 
After that i install  zabbix rpms from the new repo


 # 8-Network management
1- Open port 443 , 80 
2- Make the changes permanent 
3- Block ssh connection for your colleague ip to the VM.
**Solution:**
-sudo firewall-cmd --zone=public --add-port=443/tcp --permanent
-sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
//For systems using Firewalld, this command lists all allowed ports.
sudo firewall-cmd --list-ports
sudo iptables -A INPUT -p tcp --dport 22 -s <colleague's-IP> -j DROP






# 9-Cronjob
Create a cronjob that will run at 1:30 AM every day and collect the users logged in and save them in a file Format : timestamp – users Note: the cronjob can run a script. 
**Solution:**
users.sh 
#!/bin/bash
 timestamp=$(date+"%y-%m-%d %H:%M:%S")

logged_in_users=$(who)

echo "$timestamp - $logged_in_users" >> /root/users.txt


Grant execute permission to the script:
chmod +x collect_users.sh 
i added this to run and edit the cron jobs 
crontab -e
added this to the file 
30 1 * * * /root/users.sh

# 10-Mariadb
1- install mariadb from the local repo that was created earlier. 
2- open ports in iptables from mariadb.
 3- create database , user(note: handle permissions).
 4- connect to the database created in step 3 using the new user (with password) 
Example schema : 
Create a MariaDB database called studentdb on rhce1 and add ten records each containing “student firstname” (Allen, David, Mary, Dennis, Joseph, Dennis, Ritchie, Robert, David, and Mary), “student lastname” (Brown,Brown, Green, Green, Black, Black, Salt, Salt, Suzuki, and Chen), program enrolled in (3 x mechanical, 3 x electrical,and 4 x computer science), expected graduation year (2 x 2017, 3 x 2018, 5 x 2020), and a student number (110-001 to 110-010)




**Solution:**
-Open Ports in iptables for MariaDB:
sudo iptables -I INPUT -p tcp --dport 3306 -j ACCEPT

-Save iptables Rules:
sudo iptables-save > /etc/sysconfig/iptables

-Start and Enable iptables Service:
sudo systemctl start iptables
sudo systemctl enable iptables

-Create Database, User, and Set Permissions
1.Access MariaDB console:

sudo mysql -u root -
2.Create the database:
CREATE DATABASE studentdb ;
CREATE USER 'manaldb'@'localhost' IDENTIFIED BY 'm11241815m';
to check:
SELECT User FROM mysql.user;

To grant all privileges to manaldb:

GRANT ALL PRIVILEGES ON *.* TO 'user1'@localhost IDENTIFIED BY 'password1';


-after i exit the mariadb session 
i will coneect to the database
after that i made a table that have the provided schema 

CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    program VARCHAR(50),
    graduation_year INT,
    student_number VARCHAR(20)
);
after that i fill the table with these values 



INSERT INTO students (first_name, last_name, program, graduation_year, student_number)
VALUES
    ('Allen', 'Brown', 'mechanical', 2017, '110-001'),
    ('David', 'Brown', 'mechanical', 2017, '110-002'),
    ('Mary', 'Green', 'mechanical', 2017, '110-003'),
    ('Dennis', 'Green', 'electrical', 2018, '110-004'),
    ('Joseph', 'Black', 'electrical', 2018, '110-005'),
    ('Dennis', 'Black', 'electrical', 2018, '110-006'),
    ('Ritchie', 'Salt', 'computer science', 2020, '110-007'),
    ('Robert', 'Salt', 'computer science', 2020, '110-008'),
    ('David', 'Suzuki', 'computer science', 2020, '110-009'),
    ('Mary', 'Chen', 'computer science', 2020, '110-010');

-to check if everything goes right
 SELECT * FROM students;













