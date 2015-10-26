#Contents

* Introduction
* Background
* Solution Overview
* Component Design 

#Introduction

The objective of this document is to describe at both a high level and detailed level the solution design currently employed with DigitalTulip. This not only covers the key software components, but also describes the development and deployment methodologies and should act as a one stop shop to enable people to pick up key concepts quickly and be productive from a development perspective in a short space of time.

#Background

Digital Tulip was initiated as a Proof of Concept off the back of the successful myCloud and service optimisations projects at Pearson. The primary motivation for the Pearson project initially was to move the organisation away from the traditional on-premise Microsoft Messaging and Collaboration services, but to also revolutionise the way their organisation worked. A phrase coined by Pearson was "Martini working". This was the idea of working anywhere, any device and at any time.

The initial discovery phase of the project identified some key themes. These themes comprise the cornerstone of the DigitalTulip proposition.

Identity and Access Management is paramout to successful Cloud adoption. Single identity and therefore single sign on, user provisioning, password resets and deprovisioning are an absolute must. Without these expect spiralling license costs, poor consumer adoption, poor user experience and consequently poor productivity

There needs to be a common entry point to access all cloud services. This needs to be available through any device and at any time. It is simple in nature, but necessary when providing access to multiple cloud services. 

Cloud Services are wide and varied, expect them to change frequently and sometimes without warning. All integration therefore could become redundant overnight. This is an important caveat of everything that is done and when offering integration services should be explained to the customer

Finally and most importantly, the business change process for cloud adoption is massive. The technology can reduce some of these barriers, but the way people work is fundamentally shifting. Without relevant communication, support and service management strategies employed, it will be a painful and resistant migration.
 
#Solution Overview

Given the background as well as lessons learnt from Pearson. The initial objective was to try and demonstrate a mechanism of consuming cloud services such as Google, Office 365 and ServiceNow in a manner that would promote consumer adoption. To that end, a web portal was designed presenting a user with their available applications. This was backed with an Identity and Access Management product that delivered Single Sign On to the Portal as well as the external Cloud Services (Google Apps for Works, Office 365, ServiceNow). The illustration depicts those components and their interactions. 

![alt text](/Images/HighLevelInteractionDiagram.png)
 
The intent here is to ensure the consumer experiences cloud services in the least disruptive manner possible. Each cloud service needs its own identity, that is a given and necessary element of consuming SaaS products. However, what DigitalTulip is trying to do here is keep that hidden from the end user. Although there is a lot of engineering being performed in the background, the end user will only be prompted for one set of credentials, irrespective of which cloud service they choose to consume.

#Component Design

This sections take the components in turn describing additional detail around their implementation and composition.
Consumer Portal

##Consumer Portal

As the Enterprise changes and migrates to becoming cloud native, it is difficult to imagine an end user managing and remembering all the URLs she must connect to through their day. The objective of the portal is to provide a way the end consumer can access a central point to access other cloud services. This is analogous to a Mobile Phone home screen. It is of course possible to enrich the content of the Portal to become Enterprise specific and within the DigitalTulip demonstration there is the consumption of external data sources. The expectation is for this to be bespoke development for each client, but with a core principal of providing access to relevant applications.

The portal is a single page web application. It is an angularjs front that communicates via Rest HTTP calls to an Express back end running within NodeJS process. This is a mongodb solution, although the information stored within Mongo is very small. Namely some information about each of the application that needs displaying, plus some information about the user, namely which graphic to display. There are two more points worthy of a mention. Each user is presented with a different set of applications based on Authorisation rules. The Authorisation detail can be found in the IAM section, but suffice to say the Authorisation information is retrieved via an external REST call. The final point to touch on is that the Portal itself is secured using passport-saml

![alt text](/Images/PortalComponetry.png)

The sequence diagram below confirms the data flow when a user first requests the portal page, to further illustrate the interactions between the various components.

1. User requests the portal page
2. The passport-saml validates if a user is authenticated or not
3. This user is not authenticated and therefore they get redirected to the IAM Logon page
4. The user submits their Active Directory credentials and becomes authenticated redirecting them back to the Portal home page
5. The passport-saml now allows the user to through as it has an authentication token
6. Express returns the single page app and this renders on the users screen.
7. Angular makes two calls in parallel. 
8. The first call is to retrieve the applications a user has access to.
9. A call is made to the Authorisation service to retrieve authorised applications
10. The application details for the authorised applications are retrieved from the mongo data store and rendered on the screen by angular
11. Simultaneously a call is made to retrieve the News items. 
12. The express server side code calls ServiceNow for news items
13. This is returned to angular and rendered on the screen.

![alt text](/Images/PortalSequenceDiagram.png)
  
To cement further the component parts and the composition of the page, the next image is that of an annotated view of the main portal page.

![alt text](/Images/mainportal.png)

Identity and Access Management
There are 3 main topics covered in this section: -.
Authentication - Is a user allowed onto a System
Authorisation - A user may be authenticated, but are they authorised to access an application
Identity Synchronisation - How is a single identity maintained across multiple identity sources
Authentication
The centre-piece of this offering is Identity and Access Management. Single Sign On for enterprises is an absolute must when consuming multiple cloud providers. Imagine an organisation that has decided to consume cloud services and have 5 core products used on a day to day basis. GoogleMail, Salesforce, ServiceNow, Box and BlueKiwi. If no consideration had been made to Identity and Access Management. For a user to access these services she would need to remember 5 usernames and 5 passwords and this would be in addition to the existing passwords already in play. The chance of a password reset has increased 5 fold. Not only that, to reset or unlock this account would require the Service Desk to access each cloud platform's administration panel and perform the necessary updates. In essence, there will be an increase in Service Desk demand and an increase in resolution time. 
Aside from the increased burden on the Support Desks, it is also a very clunky user experience that given an organisation change to move from traditional IT to cloud native could act as a considerable inhibitor.
To prevent this scenario, Identity and Access Management has been placed at the centre of the platform. The main objective is to deliver Single Sign On. This move alone will reduce the need for consumers to remember 5 passwords or 5 identities. There is only 1 password the Support Desk need to manage too and of course navigating between different service providers without the need to sign in multiple times is a much more seamless user experience and likely to promote uptake of cloud services.
 
The salient point here is that Single Sign On does not mean Single Identity. What it means it that identities have been synchronised across providers and trusts have been established between services to delegate authentication. This is of course, transparent to the end user, but there is a complicated set of interconnections making this all possible as depicted below.
 
![alt text](/Images/IAMSync.png)
 
SAML2 is an open standards based protocol at the heart of the Single Sign On (SSO) solution. Using openAM as an Identity Data Provider (IDP), it is possible to configure trusts between IDP and SAML2 compliant Relying Parties (RP). However, the trust alone is not sufficient. Once the trust is established and consumer attempts to access a RP. The user will be prompted for their logon credentials. The user is then redirected back to the RP with a SAML Assertion. That assertion contains information specific to that user, it could be an email address, sAmAccountName or anything else the RP deems unique. The RP will then query their identity to store to match the user against the assertion. Assuming the RP can find a match, then the user will be presented with the RPs application. If the RP is unable to locate the identity it will error. The two key carry aways here is that Identity synchronisation is an essential component in SSO. Secondly there are no connections between IDP and the RPs. All connections are performed via the user's browser. The sequence diagram shows the SAML2 dance for a user accessing GMail.

![alt text](/Images/SAML2Seq.png)

##Authorisation

The implementation of Authorisation within DigitalTulip is relatively lightweight, but at least demonstrates the principles. These can no doubt be extended and tuned depending on local variances, but very simplistically the initial use case was to only present users with tiles on the portal page depending on their permissions. A lot is made of Role Based Access Control (RBAC). The reality is that organisations very seldom have a very clear definition of a role and also the type of applications that role requires. Managing 57 varieties of a Project Manager is not a scalable proposition. The 'role' as defined within the DigitalTulip context really refers to whether a user has access to an application or not. 
The solution implements the openIDM product from ForgeRock. This is more than just an authorisation store and some of the extra capabilities are mentioned in the further development section. For the purpose of DigitalTulip we are making use of the openIDM roles and the REST service exposed to query a given user's roles. To achieve this however the product needs to be configured accordingly. The diagram below gives a simple overview.
 
![alt text](/Images/Authz.png)
 
OpenIDM has a very extensive REST API which is not only available to client applications, but used by its own web front end when managing users. When openIDM is initially configured, a live synchronisation is established between its embedded OrientDB database and our ActiveDirectory database. Once the roles have been defined using the REST API, specifically 
POST /openidm/managed/role?_action=create HTTP/1.1
Host: iam.digitaltulip.net
Content-Type: application/json
X-OpenIDM-Username: openidm-admin
X-OpenIDM-Password: D1g1talTul1p
Cache-Control: no-cache

{ "properties": { "description": "Cost Optimisation Users" }, "name": "Cost Optimisation Users", "_id": "costoptimisationusers", "assignments": { "ldap": { "attributes": [ { "name": "ldapGroups", "assignmentOperation": "mergeWithTarget", "unassignmentOperation": "removeFromTarget", "value": [ "CN=costoptimisationusers,CN=Users,DC=digitaltulip,DC=net" ] } ], "onAssignment": { "file": "roles\/onAssignment_ldap.js", "type": "text\/javascript" }, "onUnassignment": { "file": "roles\/onUnassignment_ldap.js", "type": "text\/javascript" } } } }
The request above will create a role called Cost Optimisation Users. This has a direct mapping in the Consumer Portal to a specific tile. Allocation and De-Allocation of roles to users is maintained through Administration UI as show below

![alt text](/Images/JennyBates.png)

Jenny Bates's screen looks like the following based on the authorisations above, only showing the tiles pertinent to her.

![alt text](/Images/JennysScreen.png)

##Identity Synchronisation

The final piece in the jigsaw with regards to Identity and Access Management for Digital Tulip relates to Identity synchronisation. Earlier in this section we established that without synchronised Identity, Single Sign On would be impossible. The synchronisation implemented for Digital Tulip is tactical to establish a working demonstration for 3 cloud providers namely Google Apps for Works, Office 365 and ServiceNow. Firstly we shall cover what is currently deployed. The next section will cover a potential strategic implementation that covers some of the onboarding and offboarding requirements. 
The diagram below illustrates how the 3 cloud providers are currently kept in synchronisation.
 
![alt text](/Images/Sync.png)
 
ServiceNow is a cloud based product that has no means to communicate with the on premise infrastructure. The way they achieve synchronisation is to have a Mid Server that periodically polls for workflows to execute. Once such workflow is to query Active Directory. The results are returned to ServiceNow and synchronisation is complete. Google and Microsoft Dir Sync operate in a similar fashion. Both run on a schedule and will query AD and post any changes back to their relevant end points. In the Google case it is the User Provisioning API. In the MS instance, it will be to update Windows Azure Active Directory.
The glaring problem here is that for 3 service providers there are 3 separate means to synchronise data. As more and more services are onboard the complexity of this integration increase significantly. An alternative is proposed in the next section.

##Architectural Considerations

The DigitalTulip project was initiated to see if a demonstration could be brought together quickly to use as a tool to take to clients both internal and external to explain Atos's cloud offerings. This has been developed in an interative fashion, taking onboard changes from stakeholders accordingly. As a result to take the solution to the next stage, now would be the time to revisit some of the areas where technical debt is starting to accrue. This section identifies those at the very least and proposes a solution where possible.

##Identity Synchronisation
As identified previously, the synchronisation performed for the demo, does not represent a sustainable architectural pattern for future growth. One of the unique selling points this platform could offer is quick and easy onboarding and offboarding. That can relate not only to Users, but also to Cloud Services. Extending some of the inbuilt functionality within openIDM, I believe it is possible to demonstrate a scalable architectural pattern to facilitate quick and easy onboarding/offboarding for both users and Services. Giving our clients flexibility with control.

![alt text](/Images/FutureSync.png)
 
The future proposal is to make use of the synchronisation adapters available in openIDM to maintain synchronisation across multiple cloud services. The ingress point would be for either a provisioning or deprovisioning request being made from an external system such as SAP or Oracle, alternatively it could be an action performed by the Identity Administrators such as a role change. Both events would update the Local Identity Store. The synchronisation task will then kick in on a configured interval. This will determine any deltas and then based on the user's roles, will synchronise the identity accordingly. 
The benefit to this solution is that all changes are logged centrally and monitoring usage or licenses of each cloud service is a simple report. There is also a simple pattern for managing synchronisation and onboarding/offboarding of users. In addition to the core supported adapters, the expectation is that Atos would deliver custom adapters that could be reused across multiple clients.
 
##Development Approach and Deployment

The previous sections should give a reasonable introduction into what has been designed for DigitalTulip. The objective of the following section is to detail the development and deployment approaches along with areas that can be approved given sufficient attention.

##DevOps

The approach adopted by the project was very DevOps based. Given the nature of what was happening, it was important to be able to deploy this solution into multiple environments. Rather than using a traditional approach of development software locally, then worrying about how it is deployed and configured later. We put the deployment first and ensured there was a reliable deployment pipeline before pressing on with too much functionality. The upshot is that a new deployed environment can be created in AWS within an hour. A technology has been used which means it can be ported to other IaaS providers without only a small amount of change.
The technology set used to facilitate this is ansible. This is an agentless automation tool that works across linux and windows based environments. As the project required infrastructure quickly, it made sense to consume Amazon Web Services. The ansible scripts that have been developed will create an environment, deploy the core components and then configure them to deliver a workable environment.
All source code be it application code or infrastructure build code reside inside a github repository. The specific repositories are detailed below

| Respository Name         | Location                                               | Purpose            |
| ------------------------ |:------------------------------------------------------:| ------------------:|
| digitaltulip-aws         | https://github.com/atosorigin/digitaltulip-aws | Contains ansible scripts to provision a brand new digital tulip environment within Amazon Web Services
| digitaltulip-iam-linux   | https://github.com/atosorigin/digitaltulip-iam-linux   | Holds ansible scripts to deploy Forgerock's IAM solution, specifically concentrating on the Linux components openAM, openDJ, openIG and openIDM as well as HAProxy
| digitaltulip-iam-windows | https://github.com/atosorigin/digitaltulip-iam-windows | Contains the ansible scripts to deploy the windows Active Directory server as well as the ServiceNow Mid Server|
| digitaltulip-portal      | https://github.com/atosorigin/digitaltulip-portal      | This holds the application code for the consumer portal as well as the deployment code for it too. |
| digitaltulip-test        | https://github.com/atosorigin/digitaltulip-test        | Here are a number of selenium test scripts that exercise the functionality of the portal and its constituent parts |

## Deployment

Deployment of this environment is all achieved through TeamCity which is predominately a Continuous Integration server. Essentially however it is a way of controlling scripts and their triggers and reduces the need for sysadmins to connect to remote servers and perform admin tasks. The jobs for each environment looks like this. Each job represents an ansible command similar to this

*ansible-playbook -i plugins/inventory main.yml --extra-vars "env=dev" --vault-password-file ~/.vault_pass.txt*

It is beyond the scope of this document to explain each playbook as that is best done by looking at the source. However, it is worth a mention that each playbook takes an argument of type env which will mean the environment will be referred to as env.digitaltulip.net also there is a supplied password file on the TeamCity server. This is used to decrypt the enc.yml files within the playbooks that store password related information.

![alt text](/Images/TeamCity.png)

The order above is alphabetical and does not represent the logical order in which to execute tasks. That is described below, but before describing that, it is worth spending a moment detailing what the infrastructure looks like within AWS.

![alt text](/Images/Infastructure.png)
 
What the diagram is trying to illustrate is that all deployment and management work is performed by the TeamCity server. The TeamCity server sits on the public internet but when an environment is created is granted network access to a Jump Server or a Bastion Host. It then executes all ansible commands via that host, thereby reducing the risk vector. For completeness, the Teamcity jobs perform the following tasks
TeamCity Job
Task
TeamCity Job
Task
AWS Dependency Setup    Once an environment has been established, will deploy ssh authorised keys onto each server as well as ensure the localhost file is populated to allow name resolution in the absence of a DNS solution
AWS Provisioning    This builds the whole environment on AWS. It creates the servers, creates the virtual private network and configures the network access rule
Create Test User    Creates a set of 6 users for test purposes.
Deploy IAM Linux    Configures the HAProxy Server as well as all the components on the IAM Server.
Deploy IAM Windows  Creates a new AD Forest on the Windows Domain Controller and also creates a set of test users
Deploy Portal   Installs the node and mongo components on the Portal Server. Also performs the data migration needed for the portal database

| TeamCity Job         | Task                                                   | 
| ---------------------|:------------------------------------------------------:|
| AWS Dependency Setup | Once an environment has been established, will deploy ssh authorised keys onto each server as well as ensure the localhost file is populated to allow name resolution in the absence of a DNS solution.         | 
| AWS Provisioning     | This builds the whole environment on AWS. It creates the servers, creates the virtual private network and configures the network access rule.   |  
| Create Test User     | Creates a set of 6 users for test purposes. | 
| Deploy IAM Linux     | Configures the HAProxy Server as well as all the components on the IAM Server.      | 
| Deploy IAM Windows   | Creates a new AD Forest on the Windows Domain Controller and also creates a set of test users.       | 
| Deploy Portal        | Installs the node and mongo components on the Portal Server. Also performs the data migration needed for the portal database.        | 




Given that end state, to achieve it the following Teamcity tasks need to be run in the order below. At the end of which should be a functioning DigitalTulip environment

![alt text](/Images/DeploymentFlow.png)
 
This is predominately an automated process. However, there needs to be a single manual step after deployment for SSO to function. The administrator needs to log onto http://iam.{{env}}.digitaltulip.net/openam and navigate to the User Data store for Active Directory, the page should look like this

![alt text](/Images/ActiveDirectory.png)
 
Ticking the load schema box and pressing save with update ActiveDirectory with a schema change and enable SSO.

##Continuous Delivery

The strong focus on ansible and making the software components deployable means that Continuous Delivery is within reach. The time constraints of the project did not permit this to be realised, but the component parts are most definitely available. The effort would be to try and stitch them together in a way that makes sense. 
Gliffy DiagramEditFullscreenCDPipeline 

![alt text](/Images/CDPipline.png)

The flow above would be a starting point. The boxes marked in green are currently implemented. The orange box have been partially implemented and the red boxes need understanding and implementing. 
