  <h1 align="center">Udacity project</h1>

  <h3 align="center">
    Deploying a highly available web app using AWS Cloudfront</h3>

<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary><h2 style="display: inline-block">Table of Contents</h2></summary>
  <ol>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li>
      <a href="#About the project">About The Project</a>
      <ul>
        <li><a href="#solution diagram">Solution Diagram</a></li>
        <li><a href="#assumptions">Assumptions</a></li>
        <li><a href="#cloudformation stacks">Cloudformation Stacks</a></li>
        <li><a href="#rubric">Rubric Checklist</a></li>
        <li><a href="#networking">Networking Infrastructure</a></li>
        <li><a href="#servers">Servers</a></li>
        <li><a href="#routing">Routing</a></li>
        <li><a href="#security groups">Security and groups</a></li>
        <li><a href="#bastion server">Bastion server evidence</a></li>
      </ul>
    </li>
  </ol>
</details>

<!-- GETTING STARTED -->
## Getting Started

Clone the repo, setup the cloud network and servers.


### Installation

1. Clone the repo
   ```sh
   git clone https://github.com/dwilliblob/udacity_iac_project.git
   ```
2. Setup the network by running this command
   ```sh
    ./create.sh UdagramNetwork udagram-network.yml udagram-network-parameters.json
   ```
3. Setup the servers by running this command
   ```
    ./create.sh UdagramServers udagram-servers.yml udagram-server-parameters.json
   ```
4. Visit the LoadBalancerPublicURL listed in UdagramServers outputs
5. Discard and clean-up the environment by running
   ```
    ./delete.sh UdagramServers
   ``` 
   and
      ```
    ./delete.sh UdagramNetwork
   ```

<!-- ABOUT THE PROJECT -->
### About the project

Deploying a highly available web app - Udagram
Udacity project demonstrating deplying a webapp using infrastructure as code on AWS Cloudformation.

#### Scenario
Developers push latest version of code in a zip file located in a public S3 Bucket.

Using AWS Cloudfront, deploy application, along with the necessary supporting software into its matching infrastructure.

This is automated (using Cloudfront stacks) so that the infrastructure can be built and discarded as soon as the testing team finishes their tests and gathers their results.


### Solution diagram
![AWS Cloudformation network and servers](DeployHighlyAvailableWebApp.png?raw=true "Solution Diagram")

### Assumptions
1. This project is optimsed for high availability, therefore I have assumed deployment should be across two availability zones (rather than optimising for cost and deploying in a single availability zone).
2. I have assumed S3 bucket exists as referred to in project brief and includes objects (web app zip file). For this reason I have not created an S3 bucket.

### Cloudformation Stacks
* UdagramNetwork
* UdagramServers

### Rubric

>#### Basics
|Criteria|Meets Specifications|Completed|
|---|---|---|
|Parameters|Parameter files should contain 1 or more params|YES|
|Resources|Must include a LoadBalancer, LaunchConfig, AutoscalingGroup health check, security groups, Listener and Target Group|YES|
|Outputs|Application URL i.e. Load Balancer DNS with 'http'|YES|
|Working Test| Display Apache2 Ubuntu Server Default Page |YES|
>#### Load Balancer
|Criteria|Meets Specifications|Completed|
|---|---|---|
|Target Group|The auto-scaling group needs to have a property that associates it with a target group. The Load Balancer will have a Listener rule associated with the same target group|YES|
|Health Check and Listener|Port 80 should be used in Security groups, health checks and listeners associated with the load balancer|YES|
>#### Auto-Scaling
|Criteria|Meets Specifications|Completed|
|---|---|---|
|Subnets|Use PRIV-NET ( private subnets ) for auto-scaling instances|YES|
|Machine Specs| The machine should have 10 GB or more of disk and should be a t3.small or better|YES|
|SSH Key|There shouldn???t be a ???keyname??? property in the launch config|YES|
>#### Bonus
|Criteria|Meets Specifications|Completed|
|---|---|---|
|Output| Create values for output area|YES|
|Bastion Host|Create Bastion Host and log into Private Server|YES|

### Networking
* [Internet Gateway] - Provides access to users (testers) from the internet
* [Internet Gateway Attachment] - Enables connectivity between internet gateway and VPC
* [ElasticIP 1 & 2] - Control allocation of IP's ensuring resources are provided with a consistent IP (Depends on Internet Gateway Attachment)
* [Public Subnets 1 & 2] - Enable communications with outside world 
* [Private Subnets 1 & 2] - Enables secure and controlled traffic to main application 
* [Elastic IP 1 & 2] - Allows assiging consistent IP's for traffic routing
* [NAT Gateways 1 & 2)] - Provides outbound internet access to resources in private subnets - translates incoming public traffic into private traffic - requires public access (locate in public subnet)
* [Public Route Table] - Stores routing rules for our public subnets
* [Default Public Route] - Provides a default route for internal VPC traffic to the outside world (via internet gateway)
* [Public Subnet Route Table Association 1 & 2] - Associates public subnets 1 & 2 with public route table
* [Private Route Table] - Stores routing rules for our private subnets
* [Default Private Route] - Routes private traffic for 0.0.0.0/0 to our NAT Gateway preventing communication with outside world (keeps private traffic within VPC)
* [Private Subnet Route Table Association 1 & 2] - Associates private subnets 1 & 2 with private route table

### Servers

* [Launch Configuration] - Manages, monitors and maintains a specified number of compute resources providing resilience and reliability of our application
* [Autoscaling Group] - Requires at least 2 subnets. A logical group of our EC2 instances
* [Load Balancer] - Provides a single entry point handling requests and distributing them equally across target group of servers providing common service. AWS Load balancers require at least 2 subnets located in different availability zones
* [EC2] - AMI Ubuntu 18 - 2vCPU/4GB RAM 10GB Disk Space
* [Security Group] - Manages inbound and outbound traffic for server instances
* [Route Table] - Rules for routing traffic to specified address ranges (IPs)
* [S3 Bucket] - AWS service outside VPC for storing assets and log files

### Routing

* PublicRouteTable - This route table will have a default rule (AWS::EC2::Route) to allow all outbound traffic routed to the internet gateway. Next, we will attach this route table (AWS::EC2::SubnetRouteTableAssociation) to both our public subnets
* PrivateRouteTable1 - This route table will have a default rule (AWS::EC2::Route) to route all outbound traffic to the NAT gateway (NatGateway1). We will associate this route table to the PrivateSubnet1
* PrivateRouteTable2 - This route table is similar in nature to PrivateRouteTable1, except that it is routing the traffic to the NatGateway2, and will be attached to the PrivateSubnet2

### Security Groups
* Web Server Security Group - Defines firewall rules and protocols open to network traffic
* Load Balancer Security Group - Defines firewall rules and protocols open to network traffic


### Bastion Server

Used to access private EC2 instances within the VPC

Screenshot evidence of setting up bastion server and accessing via ssh (using terminal zsh on MacOS)
![Bastion server screenshot](Website_Browser_screenshot.png?raw=true "Bastion Server Terminal Screenshot")
