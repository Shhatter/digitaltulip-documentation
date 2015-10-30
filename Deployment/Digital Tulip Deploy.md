#To Deploy Digital Tulip

Go to our team city instance - http://ci.devatosdigital.digitaltulip.net:8111/

Run Each of the processes:
01 - AWS Provisioning - This deploys the amazon servers.
02 - AWS Setup - This adds the hosting entries so communication can happen between servers using DNS resolution.
03 - Deploy IAM Windows - This creates active directory instance that connects to the iam-linux.
04 - Deploy IAM Linux - This initiates the forgerock digital tulip deployment.
05 - Deploy Portal - This deploys the consumer portal for digital tulip.


Notes:
There is environment specific Code in the config file - Each time you create a new environment you need to update these to match the new environment.