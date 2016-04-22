# Simulating a server termination and fixing it

A sever can shut down for a variety of reason. Previously a manual rebuild would be reqiuired to overcome this. This is not only an extremely laborious process, but not feasible in an Agile working enviornment. Automation has become a huge player in the Technical world, and is enouraged throughout the delivery of any IT solution. 

Automation allows for the rebuilding of servers over and over again at the click of a button. The following walkthrough discusses the process required to terminate servers on AWS and rebuild them using a Team City task. A screencast is also provided to demo this in action.   

1. Show (http://retail.devops-demo.digitaltulip.net/#/mobile/summary) Works
2. Log in to AWS (https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1) - Show 15 servers up and running, devops are the important ones here
3. Terminate the three devops-demo servers
4. Log In To Team City (http://ci.digitaltulip.net:8111)
5. Trigger step 1. - Building Servers 
6. Show AWS Servers building
7. Assign Elastic IP Address 52.18.13.91 to devops demo HAProxy on AWS
8. Trigger step 2 - Deploy Our Code
9. Trigger step 3 - Test Our Code

[View screencast 4 on MHub](http://link.mhub.tv/?t=RjYbla) to see a working demo.

# Further detail - 22/04/2016

At this time servers were shutdown and the website was not live. The following are the steps taken to get the website live again.

1. Log into AWS (https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1) - We are looking for the server called "Team City Instance". Start this server if it's not already up and running.
2. Once it is running, with the given IP address, SSH onto that server using PuTTy or Unix, however you need to do so by presenting a private Key which should be given to all relevant developers.
3. When you have accessed the server, navigate to /home/ubuntu/TeamCity/bin and type './runAll.sh stop' to shut down the server and then './runAll.sh start' to start up the server.
4. This should then run tomcat and have the server up and running, typing this in the URL should bring up the TeamCity login screen: '<Insert IP Address For TeamCity>:8111'
5. Log into TeamCity with the format first name and initial of last name for the username, e.g. the user John Smith logs in with username: Johns
6. You should see Three sections, "Demo", "Development" and "Retail-Demo", expand the "Development" section and run the "AWS Provisioning" step, this will rebuild the relevant servers for the website.
7. Once this is successfully run, run the "FIP Banking Node" step which will packge the code and automatically run the "FIP Banking Node Deploy" step.
8. A common error you may run into is when running "FIP Banking Node" is needing to install npm gulp. In this case you need to SSH into the TeamCity server as in step 2 and navigate to the directory as shown in the TeamCity build error and run "npm install gulp" and "npm install"
9. If the "FIP Banking Node Deploy" has successfully finished (Bar the tests failing), navigate back to AWS and assign a "vpc elastic IP" to "awsdev - FIP HAProxy" by navigating to the Elastic IP section.
10. Navigating to this IP address should now show you FIP banking node web application.
