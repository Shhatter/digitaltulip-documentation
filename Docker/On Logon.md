Please ensure you have completed the "Docker Installation" Guide previously.
This guide is to be used when logging in, in order to set the machine up ready to be used again.

Go to virtual box and start the default machine.

Open gitbash 1.9.5 and run the command: "docker-machine env default" - this should return an eval statement.

Type "eval $(docker-machine env default)" and press enter to set the environment variables.

If you do the "docker-machine ls" command, you should see that the default machine is active.