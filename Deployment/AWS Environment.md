- navigate to https://atosdigital.signin.aws.amazon.com/console
CREATING USERS
- go into services and click the IAM section then select users on the left sidebar
- click create new users
- type user name and click add users
- to add a password for the user view the users and click on the user that has just been created.
- click manage passwords on this page and select the option to give the user a password
- you may also tick to get the user to create a new password when they sign in.
- to give a user admin rights select the user and click add user to groups.
- select admin and add to group

- from the services tab select console home. check in the top right corner that your region is Ireland

CREATE TEAMCITY SERVER WITH ELASTIC IP
- from the services tab select EC2
- select Instances from the left sidebar
- click Launch Instance
- select ubuntu
- select m3.medium 
- configure instance details and add storage should be default settings 
- on the tags stage add a tag named 'TeamCity Instance'
- configure security group
- add custom TCP rule  port 8111 source anywhere

-Under instances, select Elastic IP and create an elastic IP address for the TeamCity Instance
-ssh onto the server using “ubuntu@52.31.33.173” as the user
-full command example: ssh -i /Users/Xaoilin/.ssh/TeamCityKey.pem ubuntu@52.31.33.173

- ssh onto old server stop service teamcity
- teamcity/bin
- run (./runAll.sh stop) to stop

- install java by running (sudo apt-get update) and (sudo apt-get install default-jre)


BACKUP AND RESTORE TEAMCITY
- navigate to http://ci.digitaltulip.net:8111/ (teamcity)
- go to administration at the top right
- select backup
- start backup
- download backup file
- scp download file onto new server (scp -i /HOME/.ssh/TeamCityKey.pem TeamCity_Backup_20151027_115448.zip ubuntu@52.31.33.173:/home/ubuntu)
- download teamcity files (wget http://download.jetbrains.com/teamcity/TeamCity-8.1.4.tar.gz)
- install unzip library to allow you to unzip .zip files(sudo apt-get install unzip)
- run command to unzip the tar.gz to access the teamcity folder. (tar zxvf TeamCity-8.1.4.tar.gz)


- unzip

