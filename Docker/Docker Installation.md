# Installing Docker onto Windows Machine

#Introduction

This documents provides step to step instructions on how to install and conigure Docker using the command line.

#Prerequirements

Before proceeding, ensure that the following have been installed

* Oracle Virtual Box 4.3 *IMPORTANT*. Can be found by going to https://www.virtualbox.org/wiki/Download_Old_Builds_4_3 There have been issues with configuring Docker with version 5. 
* Docker toolbox. Install from https://www.docker.com/docker-toolbox. NOTE untick the Vitural Box option when prompted.

Open git bash, and run "docker-machine rm default", followed by "docker-machine create -d virtualbox default"

You may get an IP address conflict notification, this is fine. in gitbash, you may notice it seems to hang (usually after 'Starting VM...' is displayed). Press CTRL + C to end the process.

Now go to virtual box, for the virtual machine 'default', go to settings, network and ensure adapter 1 is set to NAT, and adapter 2 is set to host-only adapter, the adapter name should be "Virtualbox host-only ethernet adapter". 

Restart the machine by right clicking, then selecting close, and power off, followed by start.

Return to git bash and type "export DOCKER_TLS_VERIFY=1" - press enter
Then "export DOCKER_HOST=tcp://192.168.56.101:2376".

Now, still in gitbash SSH onto the default machine using the "docker-machine SSH Default" command.
navigate to /var/lib/boot2docker/tls and run "cat ca.pem".

Copy the output into a text editor, and save it under C://Users//[DASID]//.docker//machine//machines//default.

Do the same for server.pem, and for serverkey.pem, note that you should save the serverkey as server-key.pem.

in gitbash, type exit to end the SSH session, and type "DOCKER_CERT_PATH=C://Users//[DASID]//.docker//machine//machines//default".

Now run "docker-machine env default".

Navigate to the digital tulip repository, and ensure you are on the dockerise branch - navigate to the client folder and run npm install.
when this is complete navigate to the server folder and run npm install.

open up a second gitbash, and run mongod.

open up a third gitbash, navigate to the repository again, and this time run gulp. This is likely to open a website that will display an error, loop infinately - CTRL + C to end the process.

in the original gitbash navigate to client, and run the command "docker build -t fip_banking_web .".


In your browser type 192.168.56.101:32769 - this should load the FipBanking page




