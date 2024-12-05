# AWS Proof of Concept

This project will consist of basic infrastructure on AWS to host a __Proof of Concept__ environment. The architecture will include both public and private subnets and span multiple Availability Zones to test failover and disaster recovery scenarios. This project will host Internet-facing applications and other applications that need to access the Internet to retrive security and operating system updates.

![Desired Infrastructure](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/PoCdiagram.png)

# 1. Log in to AWS Console and create a VPC 

Onece Logged in to the AWS Console I will navigate to the VPC console and click on __'Create VPC'__.

![Create VPC](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/Create%20VPC.png)

Next I will give the VPC a name __'demo-vpc'__ and set the IPv4 CIDR block to use 10.0.0.0/16. Leave all of the
other settings as default and select Create VPC at the bottom of the Create VPC screen.

![name VPC](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/name%20VPC.png)

# 2. Create public and private subnets in three different Availability Zones.

In the VPC console, select *_Subnets_* from the left navigation panel. Click __'Create Subnet'__.

![subnet and create](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/subnet%20and%20create.png) 

I will select the VPC created in Step 1 from the dropdown list. Give the subnet the name __'private-subnet
-1'__ and select __'us-east-1a'__ from the dropdown list for the Availability Zone. Enter the IPv4 CIDR block
of 10.0.0.0/24.

![Private subnet name](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/Private%20subnet%20name.png)

I will repeat this for the 2 additional private subnets and the 3 public subnets. With the following information: 


| Subnet Name        | Availability Zone           | CIDR Block  |
| -------------      |:-------------:              | -----:|
| private-subnet-2   | us-east-1b      | 10.0.1.0/24 |
| private-subnet-3   | us-east-1c      | 10.0.2.0/24 |
| public-subnet-1    | us-east-1a      | 10.0.100.0/24 |
| public-subnet-2    | us-east-1b      | 10.0.101.0/24 |
| public-subnet-3    | us-east-1c      | 10.0.102.0/24 |

# 3. Deploy an Internet Gateway and attach it to the VPC.

In the VPC console, I will select Internet Gateways on the left navigation panel in the VPC console. Click
the Create Gateway button in the top right of the AWS console.

![Internet gateway and create](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/Internet%20gateway%20and%20create.png)

I will give the new Internet Gateway a name of *_'demo-igw'_* and click the Create internet gateway button.

![name internet gateway](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/name%20internet%20gateway.png)

In the Internet Gateway console, I will select the Actions menu and choose __'Attach to VPC'__ from the dropdown
list.

![attach to VPC](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/attach%20to%20VPC.png)

I will select the VPC created in Step 1 by clicking the text box and choosing the VPC. Click the *_'Attach internet
gateway'_* button to complete the task.

![select and attach to vpc](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/select%20and%20attach%20to%20vpc.png)

# 4. Provision a NAT Gateway for outbound connectivity.

I will go back to the VPC Console, select __'NAT Gateways'__ from the left navigation panel. Click the __'Create NAT
Gateway'__ button on the top right of the AWS console.

![create NAT gateway](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/create%20NAT%20gateway.png)

I will Provide the name __'demo-nat-gateway'__ for the NAT Gateway. Select a subnet by choosing *_'public subnet-
2'_* from the dropdown list. Keep the Connectivity Type as __'Public'__. Click the *_'Allocate Elastic IP'_*
button to automatically create and assign a new Elastic IP address for the NAT Gateway. Scroll down
and click the __'Create NAT Gateway'__ button to complete the task.

![select and createNAT](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/select%20and%20createNAT.png)

# 5. Ensure that route tables are configured to properly route traffic based on the
requirements.

In the VPC console, select __'Route Tables'__ from the left navigation panel. Click the *_'Create route table'_*
button on the top right of the AWS console.

Provide the name __'public-rtb'__ for the route table and select the VPC created in Step 1. Click the
*_"Create route table'_* button to create your first route table.

![Create Route table](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/Create%20Route%20table.png)

I will repeat the above task to create a second route table. Name the second route table __'private-rtb'__.
Select the same VPC created in Step 1. Click the *_'Create route table'_* button to create the second route
table.

Then from Route Tables console, select the tick box next to the route table named __'public-rtb'__. In the
bottom panel, select the *_'Subnet Associations'_* tab. Click the *_'Edit subnet associations'_* button.

![start association](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/start%20association.png)

I will select the three *_'public'_* subnets from the list of available subnets by checking the tick box next to the
subnets. These are the same three subnets that you created in Step 2. Once you have selected the
three subnets, click on the *_'Save associations'_* button to save your configuration.
Repeat this step for the __'private-rtb'__ and selecting the 3 private subnets that were created in Step
2.

![edit subnet associations](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/edit%20subnet%20association.png)

Now that the subnets have been associated with the proper route table, we need to add the routes
to ensure network traffic is routed correctly. From the Route Tables console, select the public-rtb
again. In the bottom pane, select the __'Routes'__ tab and click *_'Edit Routes'_*.

![click on routes](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/click%20on%20routes.png)

In the Edit Routes window, click the *_'Add route'_* button. Enter 0.0.0.0/0 in the __'Destination'__ text box
to define our new route destination. Click the text box for __'Target'__, select *_'Internet Gateway'_*, and select
the Internet Gateway that was created in Step 3. It should be the only one listed. Click *_'Save changes'_*
to save the new route configuration.

![edit routes](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/edit%20routes.png)

Lastly I will repeat this step to add a route to the __'private-rtb'__. The Destination should be 0.0.0.0/0. Click the
text box for __'Target'__, select *_"NAT Gateway'_*, and choose the NAT Gateway that was created in Step 4. It
should be the only one listed. Click *_'Save changes'_* to save the new route configuration.

