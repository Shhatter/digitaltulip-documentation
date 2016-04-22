#TeamCity

The TeamCity instance is located on https://eu-west-1.console.aws.amazon.com/ec2/v2/home?region=eu-west-1#Instances:sort=instanceState after logging in

With the IP address handy, navigate to the URL: <IP Address>:8111

You will be able to login to TeamCity with username in the format of: <first name><initial of last name> e.g. The user John Smith has the username: Johns

Once you do you will see "Demo", "Development" and "Retail-Demo".

#Development

This is what we need to get the live website up and running. 

1) AWS Provisioning runs an ansible script that automates the server creation on EC2 AWS website. If you shut the servers down (excluding the Team City Instance) and run this step, it will build the servers again and have them running.

2) FIP Banking Node will package the code and clean the dist folder in the project, in addition to automatically triggering step 5 - FIP Banking Node Deploy

3) FIP Banking Backbase Deploy will create and deploy the environment for backbase

4) FIP Banking CV will create the windows server for Creative Virtual and install Creative Virtual into it

5) FIP Banking Deploy will deploy the code to the server and additionally run a series of tests to make sure the website is functional
