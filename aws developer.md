# AWS Associate Developer - Cloud Guru
## IAM

## Lambda & Serverless
Serverless? <br/>
1. data centers: now other people maintain the infrastructure.
	But, that wasn't easy, we still need to set networks, install OSs, etc.
	So, it takes a LOT of time, like months.
2. EC2: provision macines with API calls, etc.
	MUCH faster, and so far ahead of the game.
	But, there's still infrastructure to run...
3. Microsoft Azure and Elastic Beanstalk: give me your code, I'll run the machine for you and deploy your code into it and run it.
	But still, we need to control a server.
4. Containers: lightweight alternative to virtualization, deployed on a server.
	Still have to worry about them running, scaling, response to load.
5. Serverelss and Lambda: take your code and run it without ANY provisioning.

### API Gateway
Amazon API Gateway: basically a service that makes it easy to develop and monitor APIs.
These APIs can then be the "front door" of your code running on EC2, Lambda, DynamoDB, RDS, etc.

It basically exposes HTTPS endpoints to define a REST API.

API Caching: Cache an endpoint's response if it's requested a lot, to:
- improve latency.
- reduce number of calls made.

How to configure API Gateway:
- Define an API
- Define resources i.e. URL paths, for each:
	- Define supported HTTP methods
	- Set security
	- Choose the target of the API
	- Set transformations for req and res.
- Deploy it to stages (dev, test, production).
- Can use custom domain.

### Lambda
What is lambda?
- We upload our code and create a lambda function.
- Then, AWS lambda service takes care of provisioning, monitoring, scaling, OS, patching, etc.

We can use lambda as:
- event-driven, where AWS Lambda runs our code in response to events.
- a compute service that runs in response to HTTP requests.

Lambda supports nodejs, java, python, c#, go.

Lambda pricing.


moamensfirstdomain.com
	
	
We use:
- S3: create a bucket with a name that we'll use for the domain. (You may need to grant permissions to the bucket or allow other resources to access it). Here we'll store our html files that will make the GET request we'll configure later.
- Route 53: create the domain using the s3 bucket name (this will cost money).
- Lambda: we'll create a lambda function that will include our backend code.
- API Gateway: to make the GET request after we make the lambda.
- IAM: to create roles for Lambda to make it able to talk to other stuff like S3, SNS, RDS, DynamoDB, etc.

### AWS Polly Alexa Project - Tips on Lambda
- You need to create a policy to make lambda execute & talk to SNS, DynamoDB, S3 and store objects in it. Then create a role using that policy. That is from IAM.
	- You also need to add a policy to the S3 to make things publicly available.
	- SNS: create a topic.
- You can:
	- Pass environment variables into a lambda function.
	- Test a lambda function: create a test event, and pass data if you need.


### AWS Serverless Application Model - SAM
- A framework to develop and deploy serverless apps.
- All configuration is written in a YAML subset called SAMYAML.
- Supports all CloudFormation stuff.
- Easily deployed.
- Can use CodeDeploy to deploy Lambdas.
- Can help us run Lambda, API Gateway, DynamoDB locally :o (That's a life saver)

A SAM app consists of:
- Transform Header: indicates that it's a SAM template.
- The Code: Which can use three resources:
	- AWS::Serverless::Function: i.e. Lambda.
	- AWS::Serverless::Api: i.e. API Gateway.
	- AWS::Serverless::SimpleTable: i.e. DynamoDB.
- Packaging and Deployment: 
``` sh
aws cloudformation package / sam package
aws cloudformation deploy / sam deploy
```

How does it work?
- When we do the ```package``` command,
	- our code and stuff gets zipped and uploaded to an S3 bucket.
	- the SAM template is transformed into a CloudFormation template.
	- the S3 bucket will be referenced the generated CloudFormation template.
- Now, when we do the ```deploy``` command,
	- create an execute *change sets*. A *change set* defines how CloudFormation takes its existing state and change it based on the modifications we generated in the template.
	- that is applied on our stack, our stack is the resources we'll use, like IAM, DynamoDB, etc.

Installation:
https://github.com/awslabs/aws-sam-cli

