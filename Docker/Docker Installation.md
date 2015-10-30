#Installing Docker onto Windows Machine

##Introduction

This documents provides step to step instructions on how to install and conigure Docker using the command line.

##Prerequisites

Before proceeding, ensure that the following have been installed

* Oracle Virtual Box 4.3 *IMPORTANT*. Can be found by going to https://www.virtualbox.org/wiki/Download_Old_Builds_4_3 There have been issues with configuring Docker with version 5. 
* Docker toolbox. Install from https://www.docker.com/docker-toolbox. NOTE untick the Virtual Box option & git option when prompted.
* Gitbash 1.9.5 - Newer versions use mintty, which isn't compatable with some elements of docker.

##The Process

Open git bash, and run "docker-machine rm default", followed by "docker-machine create -d virtualbox default"

You may get an IP address conflict notification, this is fine. in gitbash, you may notice it seems to hang (usually after 'Starting VM...' is displayed). Press CTRL + C to end the process.

Now go to virtual box, for the virtual machine 'default', go to settings, network and ensure adapter 1 is set to NAT, and adapter 2 is set to host-only adapter, the adapter name should be "Virtualbox host-only ethernet adapter". 

Restart the machine by right clicking, then selecting close, and power off, followed by start.

Return to git bash and type "export DOCKER_TLS_VERIFY=1" - press enter
Then "export DOCKER_HOST=tcp://192.168.56.101:2376".

Now, still in gitbash SSH onto the default machine using the "docker-machine SSH Default" command.
 run "sudo cat /var/lib/boot2docker/tls/ca.pem".

Copy the output into a text editor, and save it under C://Users//[DASID]//.docker//machine//machines//default.

Do the same for server.pem, and for serverkey.pem, note that you should save the serverkey as server-key.pem.

in gitbash, type exit to end the SSH session, and type "export DOCKER_CERT_PATH=C://Users//[DASID]//.docker//machine//machines//default".

Now run "docker-machine env default". If it returns an eval, run it, Noting that you won't need the path - it should look like "eval $(docker-machine env default). If not, ensure you run all of the above export statements correctly.
If you run "docker ps" you shouldn't get an error message - this means your installation is complete.

See Fipbanking With Docker for a continuation of this document that will help you use docker with fipbanking.




