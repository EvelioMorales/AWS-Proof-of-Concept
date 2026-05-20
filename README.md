# AWS Proof of Concept: Multi-AZ VPC Infrastructure

## Project Overview

This project builds a basic AWS Proof of Concept environment using a custom Virtual Private Cloud.

The architecture includes public and private subnets across multiple Availability Zones to support high availability, failover testing, and disaster recovery planning.

The environment is designed to support:

- Internet-facing applications in public subnets
- Internal applications in private subnets
- Secure outbound internet access for private resources
- Multi-AZ network segmentation
- Route table configuration for controlled traffic flow

This project demonstrates foundational AWS networking skills used in cloud engineering, cloud support, DevOps, and cybersecurity roles.

---

## Architecture Diagram

![Desired Infrastructure](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/PoCdiagram.png)

---

## Real-World Scenario

A company needs a secure AWS proof-of-concept environment to test cloud infrastructure before deploying production workloads.

The environment must include:

- A dedicated VPC
- Public subnets for internet-facing resources
- Private subnets for backend/internal resources
- Multiple Availability Zones for resiliency
- Internet access for public resources
- Outbound internet access for private resources through a NAT Gateway
- Route tables to control network traffic

This design allows teams to test application hosting, failover, disaster recovery, and secure network segmentation.

---

## Technologies Used

- Amazon Web Services
- Amazon VPC
- Public Subnets
- Private Subnets
- Internet Gateway
- NAT Gateway
- Elastic IP
- Route Tables
- Availability Zones
- AWS Management Console

---

## AWS Services Used

### Amazon VPC

Amazon VPC provides an isolated virtual network where AWS resources can be deployed securely.

### Public Subnets

Public subnets are used for resources that need direct internet access, such as load balancers, bastion hosts, or internet-facing applications.

### Private Subnets

Private subnets are used for internal resources that should not be directly reachable from the internet, such as application servers or databases.

### Internet Gateway

An Internet Gateway allows resources in public subnets to communicate with the internet.

### NAT Gateway

A NAT Gateway allows resources in private subnets to access the internet for updates, patches, and outbound requests while preventing direct inbound internet access.

### Route Tables

Route tables control where network traffic is sent inside the VPC.

---

## Skills Demonstrated

- Creating a custom AWS VPC
- Designing public and private subnet architecture
- Deploying resources across multiple Availability Zones
- Configuring an Internet Gateway
- Configuring a NAT Gateway
- Allocating an Elastic IP address
- Creating and associating route tables
- Routing public traffic to an Internet Gateway
- Routing private outbound traffic to a NAT Gateway
- Understanding high availability and disaster recovery concepts
- Documenting AWS infrastructure for GitHub

---

## Architecture Summary

```text
Internet
   |
   v
Internet Gateway
   |
   v
Public Route Table
   |
   |-- Public Subnet 1 - us-east-1a
   |-- Public Subnet 2 - us-east-1b
   |-- Public Subnet 3 - us-east-1c
            |
            v
        NAT Gateway
            |
            v
Private Route Table
   |
   |-- Private Subnet 1 - us-east-1a
   |-- Private Subnet 2 - us-east-1b
   |-- Private Subnet 3 - us-east-1c
````

---

## Network Design

### VPC CIDR Block

| Resource | CIDR Block    |
| -------- | ------------- |
| VPC      | `10.0.0.0/16` |

### Private Subnets

| Subnet Name        | Availability Zone | CIDR Block    |
| ------------------ | ----------------- | ------------- |
| `private-subnet-1` | `us-east-1a`      | `10.0.0.0/24` |
| `private-subnet-2` | `us-east-1b`      | `10.0.1.0/24` |
| `private-subnet-3` | `us-east-1c`      | `10.0.2.0/24` |

### Public Subnets

| Subnet Name       | Availability Zone | CIDR Block      |
| ----------------- | ----------------- | --------------- |
| `public-subnet-1` | `us-east-1a`      | `10.0.100.0/24` |
| `public-subnet-2` | `us-east-1b`      | `10.0.101.0/24` |
| `public-subnet-3` | `us-east-1c`      | `10.0.102.0/24` |

---

# Project Steps

## 1. Create a Custom VPC

After logging in to the AWS Management Console, I navigated to the VPC console and selected **Create VPC**.

![Create VPC](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/Create%20VPC.png)

The VPC was configured with the following settings:

| Setting         | Value         |
| --------------- | ------------- |
| Name            | `demo-vpc`    |
| IPv4 CIDR Block | `10.0.0.0/16` |
| Tenancy         | Default       |

The `10.0.0.0/16` CIDR block provides enough IP address space to create multiple public and private subnets across Availability Zones.

![Name VPC](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/name%20VPC.png)

---

## 2. Create Public and Private Subnets

Next, I created public and private subnets across three Availability Zones.

Using multiple Availability Zones improves availability and allows the environment to support failover and disaster recovery testing.

In the VPC console:

1. Select **Subnets**.
2. Click **Create subnet**.
3. Select the VPC created earlier.
4. Enter the subnet name.
5. Select the Availability Zone.
6. Enter the subnet CIDR block.
7. Click **Create subnet**.

![Subnet and Create](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/subnet%20and%20create.png)

Example private subnet configuration:

| Setting           | Value              |
| ----------------- | ------------------ |
| VPC               | `demo-vpc`         |
| Subnet Name       | `private-subnet-1` |
| Availability Zone | `us-east-1a`       |
| CIDR Block        | `10.0.0.0/24`      |

![Private Subnet Name](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/Private%20subnet%20name.png)

The same process was repeated for the remaining public and private subnets.

---

## 3. Create and Attach an Internet Gateway

An Internet Gateway allows public subnet resources to communicate with the internet.

In the VPC console:

1. Select **Internet Gateways**.
2. Click **Create internet gateway**.
3. Name the Internet Gateway:

```text
demo-igw
```

4. Click **Create internet gateway**.

![Internet Gateway and Create](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/Internet%20gateway%20and%20create.png)

![Name Internet Gateway](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/name%20internet%20gateway.png)

After creating the Internet Gateway, I attached it to the VPC.

Steps:

1. Select the Internet Gateway.
2. Click **Actions**.
3. Select **Attach to VPC**.
4. Choose `demo-vpc`.
5. Click **Attach internet gateway**.

![Attach to VPC](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/attach%20to%20VPC.png)

![Select and Attach to VPC](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/select%20and%20attach%20to%20vpc.png)

---

## 4. Create a NAT Gateway

A NAT Gateway allows resources in private subnets to access the internet for outbound traffic.

This is useful for:

* Downloading operating system updates
* Retrieving security patches
* Accessing package repositories
* Sending outbound application requests

Private resources can initiate outbound traffic through the NAT Gateway, but external users cannot directly initiate inbound connections to those private resources.

In the VPC console:

1. Select **NAT Gateways**.
2. Click **Create NAT Gateway**.
3. Name the NAT Gateway:

```text
demo-nat-gateway
```

4. Select a public subnet:

```text
public-subnet-2
```

5. Keep the connectivity type as **Public**.
6. Allocate an Elastic IP address.
7. Click **Create NAT Gateway**.

![Create NAT Gateway](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/create%20NAT%20gateway.png)

![Select and Create NAT](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/select%20and%20createNAT.png)

---

## 5. Create Route Tables

Route tables control how traffic moves through the VPC.

This project uses two route tables:

| Route Table   | Purpose                                                   |
| ------------- | --------------------------------------------------------- |
| `public-rtb`  | Routes public subnet traffic to the Internet Gateway      |
| `private-rtb` | Routes private subnet outbound traffic to the NAT Gateway |

---

## 6. Create the Public Route Table

In the VPC console:

1. Select **Route Tables**.
2. Click **Create route table**.
3. Name the route table:

```text
public-rtb
```

4. Select the VPC:

```text
demo-vpc
```

5. Click **Create route table**.

![Create Route Table](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/Create%20Route%20table.png)

---

## 7. Create the Private Route Table

Repeat the same process to create the private route table.

Use the following name:

```text
private-rtb
```

This route table will be used for private subnet traffic.

---

## 8. Associate Public Subnets with the Public Route Table

To ensure public subnet traffic uses the public route table:

1. Select `public-rtb`.
2. Open the **Subnet Associations** tab.
3. Click **Edit subnet associations**.
4. Select the three public subnets.
5. Click **Save associations**.

Public subnets associated with this route table will be able to route internet-bound traffic to the Internet Gateway once the route is added.

![Start Association](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/start%20association.png)

![Edit Subnet Associations](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/edit%20subnet%20association.png)

---

## 9. Associate Private Subnets with the Private Route Table

Repeat the same process for `private-rtb`.

Associate the following subnets:

* `private-subnet-1`
* `private-subnet-2`
* `private-subnet-3`

Private subnets associated with this route table will use the NAT Gateway for outbound internet traffic.

---

## 10. Add Internet Gateway Route to Public Route Table

Next, I added a default route to the public route table.

In the VPC console:

1. Select `public-rtb`.
2. Open the **Routes** tab.
3. Click **Edit routes**.
4. Click **Add route**.
5. Add the following route:

| Destination | Target     |
| ----------- | ---------- |
| `0.0.0.0/0` | `demo-igw` |

6. Click **Save changes**.

This route sends all internet-bound traffic from the public subnets to the Internet Gateway.

![Click on Routes](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/click%20on%20routes.png)

![Edit Routes](https://github.com/EvelioMorales/AWS-Proof-of-Concept/blob/main/AWSimages/edit%20routes.png)

---

## 11. Add NAT Gateway Route to Private Route Table

Finally, I added a default route to the private route table.

In the VPC console:

1. Select `private-rtb`.
2. Open the **Routes** tab.
3. Click **Edit routes**.
4. Click **Add route**.
5. Add the following route:

| Destination | Target             |
| ----------- | ------------------ |
| `0.0.0.0/0` | `demo-nat-gateway` |

6. Click **Save changes**.

This route allows private subnet resources to access the internet through the NAT Gateway while keeping those resources private.

---

## Final Routing Design

### Public Route Table

| Destination   | Target           | Purpose                            |
| ------------- | ---------------- | ---------------------------------- |
| `10.0.0.0/16` | Local            | Internal VPC communication         |
| `0.0.0.0/0`   | Internet Gateway | Internet access for public subnets |

### Private Route Table

| Destination   | Target      | Purpose                                      |
| ------------- | ----------- | -------------------------------------------- |
| `10.0.0.0/16` | Local       | Internal VPC communication                   |
| `0.0.0.0/0`   | NAT Gateway | Outbound internet access for private subnets |

---

## Validation

The project was validated using the following checks:

| Test                          | Expected Result                                   | Status |
| ----------------------------- | ------------------------------------------------- | ------ |
| Create VPC                    | VPC is created with CIDR `10.0.0.0/16`            | Passed |
| Create public subnets         | Three public subnets exist across three AZs       | Passed |
| Create private subnets        | Three private subnets exist across three AZs      | Passed |
| Attach Internet Gateway       | Internet Gateway is attached to the VPC           | Passed |
| Create NAT Gateway            | NAT Gateway is created in a public subnet         | Passed |
| Associate public route table  | Public subnets are associated with `public-rtb`   | Passed |
| Associate private route table | Private subnets are associated with `private-rtb` | Passed |
| Public route                  | `0.0.0.0/0` routes to Internet Gateway            | Passed |
| Private route                 | `0.0.0.0/0` routes to NAT Gateway                 | Passed |

---

## Security Considerations

* Public subnets should only contain resources that need direct internet access.
* Private subnets should be used for backend services, databases, and internal workloads.
* Security Groups should be used to restrict inbound and outbound traffic.
* Network ACLs can be added for additional subnet-level control.
* NAT Gateway allows outbound internet access without exposing private resources to direct inbound internet traffic.
* IAM permissions should follow least-privilege access.
* VPC Flow Logs can be enabled to monitor network traffic.
* AWS CloudTrail can be enabled to audit changes made to the environment.

---

## High Availability Considerations

This architecture spans three Availability Zones:

* `us-east-1a`
* `us-east-1b`
* `us-east-1c`

Using multiple Availability Zones improves resiliency and allows the environment to support failover testing.

For production environments, consider using:

* Multiple NAT Gateways, one per Availability Zone
* Application Load Balancer across public subnets
* Auto Scaling Groups across private subnets
* Multi-AZ RDS deployments
* Route 53 health checks
* CloudWatch alarms

---

## Cost Management

This project may create AWS charges, especially from:

* NAT Gateway hourly usage
* NAT Gateway data processing
* Elastic IP address usage
* Data transfer

To avoid unnecessary charges:

1. Delete the NAT Gateway when finished.
2. Release unused Elastic IP addresses.
3. Delete unused VPC resources.
4. Monitor AWS Billing.
5. Set up AWS Budgets alerts.

---

## Cleanup

To delete the environment:

1. Delete the NAT Gateway.
2. Release the Elastic IP address.
3. Delete route table associations if needed.
4. Delete custom route tables.
5. Detach and delete the Internet Gateway.
6. Delete the subnets.
7. Delete the VPC.

> Note: Some resources cannot be deleted until dependent resources are removed first.

---

## Lessons Learned

Through this project, I learned how to:

* Create a custom AWS VPC
* Design a multi-AZ subnet architecture
* Separate public and private network resources
* Attach an Internet Gateway
* Create a NAT Gateway for private subnet outbound access
* Configure public and private route tables
* Associate subnets with route tables
* Understand the difference between public and private routing
* Build a foundational AWS proof-of-concept network

---

## Future Improvements

This project can be improved by adding:

* EC2 instances in public and private subnets
* Security Groups for application-level access control
* Network ACLs for subnet-level security
* VPC Flow Logs for traffic monitoring
* AWS CloudTrail for auditing
* Application Load Balancer across public subnets
* Auto Scaling Group across private subnets
* Bastion host or AWS Systems Manager Session Manager
* Multi-AZ NAT Gateway design
* Terraform or CloudFormation automation
* AWS Config compliance rules
* CloudWatch alarms and dashboards

---

## Recommended Production Architecture Upgrade

A more production-ready version of this project could include:

```text
Users
  |
  v
Route 53
  |
  v
Application Load Balancer
  |
  v
Public Subnets across Multiple AZs
  |
  v
Private Application Subnets
  |
  v
Private Database Subnets
  |
  v
Amazon RDS Multi-AZ
```

Additional security and monitoring services:

```text
AWS WAF
AWS Shield
CloudTrail
VPC Flow Logs
CloudWatch
AWS Config
Systems Manager Session Manager
```

---

## Conclusion

This project demonstrates how to build a foundational AWS proof-of-concept network using a custom VPC, public and private subnets, Internet Gateway, NAT Gateway, and route tables.

By separating public and private resources across multiple Availability Zones, this design supports secure cloud networking, outbound internet access for private workloads, and basic high availability planning.

