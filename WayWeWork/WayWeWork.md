#Introduction
The digital tulip ecosystem was developed in many ways to try and demonstrate some of the best practices and techniques used by Atos to deliver software in a modern cloud based world.

The components available are wide and varied, some addressing specific business needs in the form of fip_banking_node and some demonstrating capability in digitaltulip_iam, digital_tulip_portal. 

The objective with all these project, although starting with fip_banking_node, is to make them Open Source within Atos. That means we actively encourage improvments or enhancements to what is already there.

#Getting Started
You're on the bench, you're looking to expand your knowledge and you been asked to develop a new feature for fip_banking_node.

First things first, is to look at the [Readme File](https://github.com/atosorigin/fip_banking_node/blob/master/README.md). This should be enough to get things started. Assuming you're able to get fip_banking_node running locally. How do you make a contribution.

##Making a Contribution
You've seen the code, you like the code, you've thought of an enhancement or you've seen an enhancement. You want to change something. How do you do it?

###1 - Branch the master
`git checkout -b <<my_new_branch>>`

###2 - Make your changes and test
`gulp test`

###3 - If the tests and you've added new tests
`git add .`
`git commit -m 'my amazing new feature'`
`git push`

###4 - Create a pull request
https://help.github.com/articles/creating-a-pull-request/

###5 - Ninja Time
At this point your code will go through an automated test phase. The outcome will be reported to the available Ninjas. Assuming success, the Ninjas will be presented with a set of outcomes.

This includes
- Regression Test Success
- Test Coverage
- Lint Results
- jsHint Results

Depending on the findings of these tests, the merge request will either be accepted or commented on.

###6 - Repeat
