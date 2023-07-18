# AWS Solutions Architect End Course - Project 02

## Problem Statement:: Set Up and Monitor a WordPress Instance for Your Organization

### Description: Launch a WordPress instance using AWS CloudFormation and monitor the instance using Amazon Route 53.

## Real World Scenario:: 
Your organization publishes blogs and provides documentation services for other businesses and technologies. You have been asked to:

    • Set up a live WordPress instance to publish blogs

    • Set up a WordPress instance that can be used for development and testing purposes so that any work done on this instance will not impact the live blog

    • Configure the WordPress instance for development and testing purposes, which will be available only for business hours (9 AM–6 PM)
    
    • Monitor the health of the WordPress instance 

## Expected Output::

![alt text](https://github.com/jangraviren/sac03-project02/blob/main/images/expected-output.png?raw=true)


## Solution Architecture::

![alt text](https://github.com/jangraviren/sac03-project02/blob/main/images/solution-architecture.jpg?raw=true)


Based on the requirements, we'll use AWS services and serverless technology to set up and monitor the WordPress instance. Here's a high-level architecture for the project:

1. AWS CloudFormation:
    Use AWS CloudFormation to create and manage the necessary AWS resources for the project. This includes setting up the EC2 instances, RDS database, IAM roles, and other required resources. You can define the infrastructure as code using a CloudFormation template.

2. Amazon EC2 Instances: 
    Create two EC2 instances, one for the live WordPress instance and the other for development/testing purposes. These instances will run the WordPress application and host the blogs.

3. Amazon RDS (Relational Database Service): 
    Set up an RDS instance to store the WordPress database. This will provide a scalable and managed database solution for the application.

4. Route 53: 
    Integrate Route 53, the DNS service provided by AWS, into the architecture.

5. DNS Configuration: 
    Create a Route 53 hosted zone for your domain and configure the necessary DNS records. Set up an "A" recordto point to the Live and DEV Ec2 instances IP addresses.

6. Health Checks: 
    Configure Route 53 health checks to monitor the health of the ELB and the EC2 instances. Define health check settings to periodically send requests to the ec2 instances and evaluate its responses. If the health checks fail for an instance, Route 53 can automatically stop routing traffic to the unhealthy resource.

13. Routing Policies: 
    Define routing policies in Route 53 to control the distribution of traffic among healthy resources. There are several routing policies available, such as simple, weighted, latency-based, geolocation-based, and failover. Choose the appropriate routing policy based on your requirements.

14. Simple Routing Policy: 
    In the context of this solution, you can use the "Simple" routing policy in Route 53. With the Simple routing policy, Route 53 responds to DNS queries with all the IP addresses associated with the ELB. The client's DNS resolver chooses one of the IP addresses randomly. This provides a basic load balancing capability across the available EC2 instances.

5. Serverless Technology (AWS Lambda):
    To control the availability of the development and testing instance during business hours, you can use AWS Lambda functions with CloudWatch Events. 
    
    Create a Lambda function that starts the development/testing instance at 9 AM and stops it at 6 PM on business days. Use a CloudWatch Event schedule to trigger the Lambda function at the specified times.

6. Monitoring the WordPress Instance:
    For monitoring the health of the WordPress instances, you can use Amazon CloudWatch. CloudWatch provides various metrics that you can monitor, such as CPU utilization, network traffic, and disk space usage.
    
    Set up CloudWatch Alarms to send notifications or trigger actions if certain thresholds are breached. For example, you can create an alarm to notify you if the CPU utilization of the live WordPress instance exceeds a specific limit.

7. Security and Access Control:
    Implement proper security measures, such as setting up security groups for EC2 instances and database, configuring IAM roles with the least privilege principle, and enabling encryption where required.

    For better security, consider using AWS Identity and Access Management (IAM) to manage access to AWS resources.

8. Backup and Disaster Recovery:
    Implement regular backups of the WordPress database to ensure data safety. You can use Amazon RDS automated backups and snapshots for this purpose.

    Consider implementing disaster recovery measures in addition to ensure high availability of the WordPress instance, such as setting up multi-Availability Zone (AZ) deployments. This is optional for the project. You can build ec2 AMI images for updated/upgraded tested versions of DEV instance to move the things to production. Apart from that we can use the Wordpress data export and import tools available to move/roll out new features to the production. 

# How to Implement the above architecture
    
To implement the above architecture, we need to follow the steps outlined below:

1. Create a IAM user with programmatic access and generate "Access Key" and "Access Secret" of your AWS Account if you want to use the AWS Cli or you can directly upload the launch template (yaml) file while making new CloudFormation stack from the AWS Console.

2. Create a IAM role permissions which is required for the above architecture creation. Above architecture requires admin access of RDS, Ec2, Secrets Manager, Route53, Lambda, Cloudwatch, SNS 

3. Create an AWS CloudFormation template in YAML or JSON format that defines all the necessary AWS resources, including EC2 instances for the live and development/test WordPress environments, Amazon RDS for the database, Auto Scaling Group for the development instance (if needed), security groups, CloudWatch Alarms, and other components. Parameterize the template to allow customization of instance types, security group rules, database settings, and other configurable options.

# ami-03826e81533d65fcb