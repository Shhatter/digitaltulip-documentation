# Create TeamCity Sever on AWS

# Creating Users

1) Navigate to https://atosdigital.signin.aws.amazon.com/console

2) Go into services and click the IAM section then select users on the left sidebar

3) Click create new users

4) Type user name and click add users

5) To add a password for the user, view the users and click on the user that has just been created

6) Click manage passwords on this page and select the option to give the user a password

7) You may also tick the checkbox to get the user to create a new password when they sign in (Recommended)

8) To give a user admin rights select the user and click add user to groups

9) Select admin and add to group

# Assign Elastic IP

1) From the services tab select console home. Verify in the top right corner of the page that your region is Ireland

2) From the services tab select EC2 then select Instances from the left sidebar

3) Select Launch Instance and configure with these settings:
* select ubuntu
* select m3.medium 
* configure instance details and add storage should be default settings 
* on the tags stage add a tag named 'TeamCity Instance'
* configure security group: add custom TCP rule  port 8111 source anywhere

4) Under instances, select Elastic IP and create an elastic IP address for the TeamCity Instance

5) ssh onto the server using “ubuntu@52.31.33.173” as the user. Full command example: ssh -i /Users/Xaoilin/.ssh/TeamCityKey.pem ubuntu@52.31.33.173

# BACKUP TEAMCITY

1) Log onto TeamCity and go to Administration on the top right

2) Backup the TeamCity build and download the zip file it was backed up as

3) Copy that file from local machine to remote TeamCity server you just created in /home/ubuntu

# INSTALL TEAMCITY

1) Run the following command in the ubuntu server you have connected to: wget http://download.jetbrains.com/teamcity/TeamCity-9.0.3.tar.gz

2) Extract teamcity to /var with the following command: tar zxvf file.tar.gz -C /path/to/somedirectory

3) Give the TeamCity folder ownership: sudo chown ubuntu:ubuntu ./TeamCity -R

4) Start TeamCity by running the command "runAll.sh start" in the bin folder; for example: /var/TeamCity/bin/runAll.sh start

5) Verify TeamCity is running at port 8111 with :netstat —-listen

DEBUGGING:

Check TeamCity services are not running:
ps aux|grep TeamCity

Kill any TeamCity services that are: 
sudo kill -9 11474

Check server is running on your browser:
52.31.33.173:8111

# Restore DB

1) Edit the environment with: 
* sudo nano /etc/environment
* add this to the environment file: export JAVA_HOME=/usr/lib/jvm/default-java

2) Check that with: 
* source /etc/environment
* echo $JAVA_HOME

3) Stop TeamCity with: /var/TeamCity/bin/runAll.sh stop

4) Check TeamCity services are not running:
* ps aux|grep TeamCity

5) In .BuildServer/config, move database.hsqldb.properties.dist to /home/ubuntu (If this is not available run step 9 + 10 then repeat this)

6) Delete .BuildServer/config and .BuildServer/system folders completely

7) Navigate to /var/TeamCity/bin and run: ./maintainDB.sh restore -F /home/ubuntu/TeamCity_Backup_20151027_115448.zip -T /home/ubuntu/database.hsqldb.properties.dist

8) Start TeamCity with: /var/TeamCity/bin/runAll.sh start

9) Open browser and connect to teamcity 52.31.33.173:8111

10) Proceed with defaults, the users from the backup TeamCity server will be imported now so you can sign in with the credentials of those users

# TeamCity and GitHub

1) Log into github as teamcity user, (user: digitaltulipdeploy, password: The Normal Secure Password)

2) We need to allocate a private key, go to settings on the top right of the page, then ssh keys tab on the left

3) Add an SSH Key

4) On the server, type "ssh-keygen", stick to defaults just press enter. This will generate an ssh key

5) Write "cat ~./ssh/id_rsa.pub" To view the key

6) Copy the key into github's Key field then add key

7) In edit VCS configs in TeamCity for the project, under default private key, in the username write "digitaltulipdeploy"

8) Go to development > parameters change BUILD_ENV to "devdigitalatos"

# Step 1 AWS Provisioning

1) Run the following commmands to install dependencies
* sudo apt-get install software-properties-common	
* sudo apt-add-repository ppa:ansible/ansible
* sudo apt-get update
* sudo apt-get install ansible
* sudo apt-get install python-pip
* sudo apt-get install boto
* sudo pip install -U boto

2) We need to create a vault pass txt with a password. Navigate to /home/ubuntu and create file .vault_pass.txt and write "The Normal Secure Password" without quotations and save it 
Note: This is the password we use in the dev team and not the literal words "The Normal Secure Password"

3) boto executes a series of APIs on Amazon Web Servers, it uses a set of environment variables to connect to AWS. We need to set these parameters. Go to AWS and create a new user called "TeamCity" and download the credentials using the button at the bottom of the page. Add user to groups "admin".

4) Add these two environment variables in TeamCity linux server:
* export AWS_ACCESS_KEY_ID='AK123'
* export AWS_SECRET_ACCESS_KEY='abc123'
* Replacing the id and key with the credentials you downloaded.

5) Set the configuration parameter to atosdev

6) Import key pair "TeamCityDefaultKey" which will be the key you created on putty earlier for github

7) Open awsprovisioning > deploy > group_vars > all and change the aws_key: TeamCityDefaultKey

8) Run First TeamCity build step awsprovisioning

9) Go to EC2 website and allocate new elastic IP Addresses to:
* HAProxy

# Step 2 Package code

1) Go to fip_banking_node config file and add a new configuration for the newly built server, editing the target url, domain and callbackURL
2) Run the following commands on TeamCity Server:
* sudo apt-get install npm
* sudo apt-get install node
* sudo apt-get install git


# Step 3 Deploy

1) We need to ignore host key verification

2) Edit the ansible file with: nano /etc/ansible/ansible.cfg

3) Uncomment "enable host_key_checking = false" by removing the hash tag and save then install these packages:
* sudo apt-get install firefox
* sudo apt-get install xvfb
* sudo npm install --global protractor

# Step 5 Backbase

* sudo apt-get install maven
* sudo nano ~/.m2/settings.xml
* Copy and paste the code in settings.xml in this folder to the contents of that file