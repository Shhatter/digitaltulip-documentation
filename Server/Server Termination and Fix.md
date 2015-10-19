# Simulating a server termination and fixing it. 

# A Step by step guide

1. Show (http://retail.devops-demo.digitaltulip.net/#/mobile/summary) Works
2. Log in to AWS (https://eu-west-1.console.aws.amazon.com/console/home?region=eu-west-1) - Show 15 servers up and running, devops are the important ones here
3. Terminate the three devops-demo servers
4. Log In To Team City (http://ci.digitaltulip.net:8111)
5. Trigger step 1. - Building Servers 
6. Show AWS Servers building
7. Assign Elastic IP Address 52.18.13.91 to devops demo HAProxy on AWS
8. Trigger step 2 - Deploy Our Code
9. Trigger step 3 - Test Our Code

[View screencast](https://digitaltulip.atlassian.net/wiki/pages/viewpage.action?pageId=13074437)