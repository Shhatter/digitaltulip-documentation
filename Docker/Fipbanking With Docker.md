#Fipbanking with Docker
##Prerequisites

Before beginning this stage, please ensure you have completed either the "Docker installation" or (if you have completed installation previously, but returned to set up fipbanking with docker) the "On Logon" guide.

##The Process

In your existing Gulp (from the previous stage) Navigate to the digital tulip repository, and ensure you are on the dockerise branch - navigate to the client folder and run npm install.
When this is complete navigate to the server folder and run npm install here too. This fetches all the javascript libraries required to run my finance pal.

in gitbash navigate to the database folder, and run the command "docker build -t fipbankingdb . " followed by "docker run -d -P --name db fipbankingdb" This creates & runs the mongo process. Note that if the output looks odd you can type "clear" to tidy it.

If you run "docker images" you can see that the db image yis present.

to build our server instance - navigate to the server folder, and run the command "docker build -t fipbankingapi ." followed by 
"docker run -d -P --name api --link db:db fipbankingapi" to start the process.

Now we need to populate the database, do this by running "docker exec -it bash", this will connect you to the instance, run "cd src" followed by 
"node ./node_modules/mongo-migrate/index.js --runmm up" to run the data migration. Type "exit" when it is complete.

You can use robomongo here to view your database (192.158.56.101:32768) and see that its now populated. 

Ppen up a second gitbash, navigate to the repository again, and this time run gulp. This is likely to open a website that will display an error, and loop infinately - press CTRL + C to end the process. This will have generated the dist folder required by the fipbankingweb stage.

Next, we build the web instance, involving nginx. Navigate to the client folder, and run "docker build -t fipbankingweb .", "docker run -d -P --name web --link db:db fipbankingweb" in order to start the process.

Run "docker ps", get the port for the web instance. then use your web browser, and go to: "192.168.56.101:[Port]". the fipbanking website should be here.
