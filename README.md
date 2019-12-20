# Deploy a Highly Available Web Service in AWS.

In this project, you will:

* Design and deploy a fault tolerant and resilient web service architecture.
* Monitor availability, simulate and test failure scenarios and recovery.
* Finally, you will create a reference architecture that will encompass the concepts used in the project.

## Dependancies and Prerequisites

### Access to AWS account  
Students will need to use their personal AWS accounts.  Udacity will provide a $100 credit for any usage costs. If project instructions are followed we do not anticipate usage costs to exceed this amount.

### Installation of the AWS CLI and Local Setup of AWS API keys
Instructions and examples in this project will make use of the AWS CLI in order to automate and reduce time and complexity.
Refer to the below links to get the AWS CLI installed and configured in your local environment.

[Installing the CL](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html)

[Configuring the CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)

### Local setup of git and GitHub Repository
You will need to clone / fork / download this GitHub repo in order to work on and submit this project.

### Access to a Diagramming Tool
Some suggested diagramming tools are draw.io or lucidchart.com. If you prefer, you can start from scratch and use your own tools and submit the diagram as an image.

## Exercise 1 - Deploy Web Service Application Infrastructure

**_Deliverables for Exercise 1:**
* There are no deliverables for Exercise 1._

### Task 1:  Review Architecture Diagram
In this task, the objective is to familiarize yourself with the starting architecture diagram. An architecture diagram has been provided which reflects the resources that will be deployed in your AWS account.

The diagram file can be found in the _starter_ directory in this repo.

You may open the “AWS-WebServiceDiagram-v1-noHA.drawio” file in **draw.io** or import the diagram into **lucidchart.com** as a starting point for the architecture portion of this project. 

**You will be updating your architecture diagram as you change your environment.** The instructions will tell you when you need to update your diagram.

**Note:** the starter environment architecture diagram is also available to view in this document as an image.

### Task 2: Review CloudFormation Template
In this task, the objective is to get you up and running quickly - we have provided a CloudFormation template which will deploy the following resources in AWS:

* A VPC with 2 public subnets, one in each availability zone.
* An instance that will be running a simple web service.
* An RDS database that will be queried by the web service application.

Spend a few minutes going through the .yml files in the _starter_ folder to get a feel for how parts of the code will map to the components in the architecture diagram.

## Task 3: Deployment of Initial Infrastructure
In this task, the objective is to deploy the cloudformation stack that will create the below environment.

![base environment](/starter/diagrams/AWS-WebServiceDiagram-v1-noHA.png)

This is a starting point environment which does _not_ have redundancy or fault tolerance.  It has the basic components needed to make the application work.

We will utilize the AWS CLI in this guide, however you are welcome to use the AWS console to deploy the CloudFormation template.


1. From the root directory of the repository - execute the below command to deploy the template.
```
aws cloudformation create-stack --region us-east-1 --stack-name s3-code-repo --template-body file://starter/c1-s3code.yml
```

Expected example output:
```
{
    "StackId": "arn:aws:cloudformation:us-east-1:4363053XXXXXX:stack/s3-code-repo/70dfd370-2118-11ea-aea4-12d607a4fd1c"
}
```

Expected example AWS Console status: https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks


### Verify the infrastructure
Login to the AWS CloudFormation service console and check the progress of the deployment - it will take 5-10 minutes to deploy.

Take a look at the resources that have been deployed and go to the service console (e.g. EC2 or RDS) to see what components have been deployed.

You will need the Application Load Balancer endpoint to test the web service.


## Testing the web service

You can verify the application availability by loading the following URL into your browser.
We will use this through out the project to verify the health and availability of the application.

*provide URL here*

You should see the following response returned by the API call.
*expected content*

## Monitoring for availability and failure
Now that you have a successfully deployed and running environment,  you need some visibility into the health of your application and infrastructure.

To achieve this you will setup a CloudWatch dashboard that will display key metrics.

### Endpoint Health
* Setup a Route53 health check that will simulate an external user making a request to our application.
* Add the resulting metrics of this healthcheck to a dashboard in CloudWatch

### Load balancer metrics
* Client requests will be sent from the load balancer to the application server.  
* Load balancer target groups will also ascertain the application server health
* Load balancer target groups will maintain response code counts etc.

Create a dashboard with key metrics discussed above from the Application Load Balancer

### Infrastructure metrics
* Explore cloudwatch metrics for the EC2 instance(s) and RDS instance
* Create a dashboard which will display provide insight into the availability of your environment


## Failure simulation 1 - instance failure
Run the following commands to simulate a failure scenario where the application server instance crashes.

```
```

Observe the dashboards that you have created above.
What changes do we see?
Which metrics reveal an that there is an outage?

Bring the application back up with the following procedure.
```
```


## Update infrastructure - make it fault tolerant
We initially deployed our environment with a single applications server node and a single AZ database.  
The VPC that we deployed actually has a second subnet in a secondary availability zone.

### Add redudancy and fault tolerance
#### Task 1 - Update the architecture design
Make changes to the initial architecture design so that it will have a secondary application server node and a database server that can handle an outages occurring at the physical location where they are running (availability zone)

#### Task 2 - Make changes to the environment
Make changes to the environment so that it has a secondary application server node and a database server that can handle an outages occurring at the physical location where they are running (availability zone)

As a bonus you will benefit greatly by adding the below design constraints:
##### Task 2 Constraint 1
Perform all changes in the CloudFormation template and research how to update the CloudFormation stack.  Being handy with infrastructure as code is a key responsibility of a CloudArchitect.

##### Task 2 Constraint 2
Make all changes without impacting the availability and uptime of the application.  

#### Task 3
Provide a break down of additional (before vs after) monthly costs for the changes that were made.

## Failure simulation 2 - Availability zone outage.
Run the following commands to simulate a scenario where multiple components and network connectivity within a physical availability zone are undergoing failure.

```
```

Observe the dashboards that you have created above.
What changes do we see?
Which metrics reveal an that there is an outage?
Would the outage impact customers ?
What is the impact on the application?

Bring the application back up with the following procedure.
```
```
### Add self healing to the mix
The initial failure scenarios previously presented required some intervention to bring the system back to a healthy and fully functioning state.

Research and provide 2-3 strategies to allow for quicker detection of an issue and automated remediation.  

## Backups and Disaster recovery
Thus far you have experimented with failures or loss of individual components and observed how availability is impacted.

In some cases it is critical to plan for larger scale outages of AWS services or regions which would cause our entire environment be down without the ability to recover the existing environment.

In this scenario we would need to ensure that data and servers are being backed up or replicated to a separate region.

It is your task to choose a strategy and architect the infrastructure in order to ensure that the environment can be restored to a separate region.

### Document strategy
Provide a brief outline for how you will ensure that your application can be recovered in a different region inclding:
* the approach you will use (e.g. warm standby, pilot light, active-active, etc)
* methods used to backup or replicate servers and data to a different region.
* methods used to recover and bring the application up in a secondary region
* Estimated recovery time objective (RTO)
* Estimated recovery point objective (RPO)
* Cost impacts


### Update the Architecture Diagram
Update the architecture to reflect your chosen strategy

### Deploy to a secondary "DR" region
*this step may be removed if the above steps end up taking too much time to complete*

Deploy your updated design to a secondary environment.  
Attempt to launch the application from backups taken in the primary environment.  This would be similar to a tabletop DR validation exercise.

## Built With

* AWS Cloudformation templates
* AWS CLI and bash scripts
* Python flask web service
* MySQL database (RDS)
* draw.io architecture design diagram


## License

This project is licensed under the MIT License - see the [LICENSE.md]()
