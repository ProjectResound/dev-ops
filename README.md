# Setup

The Resound suite includes a number of Docker containers that communicate through the [resound-api](https://github.com/ProjectResound/resound-api) container.

![Resound architecture diagram](assets/architecture.png)

The Store + Manage container (what users hit when they go to port 80) sends requests to the API container.  The API container is the only one that has access to the database. Note that the database does *not* run in a container and must be set up separately. By default, a Redis container is also deployed, for the use of the API container.

The easiest way to deploy the containers is to use the [Cloudformation template](cloudformation/README.md). If you currently do not have any VPCs set up for AWS, please read the **VPC Setup** section below.


# VPC Setup
Resound containers are designed to run in a VPC in AWS.

Network requirements for Resound VPC creation:

* VPC
    * Enable DNS Hostnames

* Public Subnets (2 or more)
    * 1 for each Availability Zone
    * break up cidr block evenly
    * Security Groups must allow all traffic from the DB

* Route Table
    * Must have 0.0.0.0/0 Internet Gateway
    * Should be associated with all subnets in VPC

* Network ACL
    * Should be associated with all subnets in VPC

* Security Group
    * Must allow all traffic from Resound DB instance IP
    * Allow all traffic from SSH port 22 and HTTP port 80
