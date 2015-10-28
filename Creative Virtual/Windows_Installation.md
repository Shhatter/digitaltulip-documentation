# Prequisites
* Have a windows server 
* Git Clone this repository https://github.com/atosorigin/fip_cv_demo

# Connect to windows server
Access this zip file from the cloned repository under Windows Installation.
Copy the zip file over from local machine to remote server and unzip.
Run the V-Engine Installer to install all dependencies.

# V-Engine Setup

1) Fundamentals:
* Server's name: AtosDemo
* Server's IP: The IP of your server

2) Database Server:
* MySQL

3) MySQL Server:
* Check "Add new Database Server"
* Host IP: localhost
* Port: 3306

* Check "Auto"
* Admin acc: root

4) Engine Selector:
* Check "Live"
* You can optionally choose to receive emails about errors

* VA's name: ATOSDEMO1
* Port: 9990
* Set Password with 1 uppercase 1 lowercase 1 number 1 special character

Install!

# Final setup

1) Open command prompt and stop services / processes Naming service, VAEngine, tomcat

2) Open C:/ and rename webapps, VEngineFactory and VAEngines to the suffix "_old" e.g. "webapps_old"

3) Copy and paste the relevant folders to the C:/ directory from the zip file