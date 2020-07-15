# AWS SAA-C02 Notes
> These are my personal notes while studing for the AWS SAA-C02 exam. My primary preparation tool was Adrian Cantrill's (SAA-C02) course. These notes approximately follow that course. There may be errors. To get the course, visit [https://learn.cantrill.io](https://learn.cantrill.io.).

## Table of Contents
 
- [Cloud Computing Fundamentals](#Cloud-Computing-Fundamentals)
- [AWS Fundamentals](#AWS-Fundamentals)

---

## Cloud Computing Fundamentals 

### What is Cloud Computing?

According to the National Institute of Standards and Technology (NIST), there are 5 essential characteristics of cloud computing: 

1. **On-Demand Self-Service**: Can provision capabilities as needed without requiring human interaction.
2. **Broad Network Access**: Capabilities are available over the network and accessed through standard mechanisms. 
3. **Resource Pooling**: There is a sense of location independence with no control or knowledge over the exact location of the resources. Resources are pooled to serve multiple consumers using a multi-tenant model.
4. **Rapid Elasticity**: Capabilities can be elastically provisioned and released to scale rapidly outward and inward with demand. To the consumer, the capabilities available for provisioning often appear to be unlimited. 
5. **Measured Service**: Resource usage can be monitored, controlled, reported AND BILLED.

[Back to Top](#AWS-SAA-C02-Notes)

### Public vs Private vs Multi vs Hybrid Cloud

- **Public**: Using 1 public cloud (e.g. AWS, Azure, Google Cloud)
- **Private**: Using on-prem cloud that meets all 5 requirements. Generally legacy on-prem infrastructure doesn't meet all the requirements. (e.g. AWS Outposts, Azure Stack, and Google Anthos)
- **Multi**: Using more than one private cloud. 
- **Hybrid**: Using public and private clouds. *"Hybrid cloud "* IS NOT public cloud plus legacy on-prem. That's known as *"Hybrid Environment" or "Hybrid Networking"*. 

[Back to Top](#AWS-SAA-C02-Notes)

### Cloud Service Models

Cloud Service models define which parts of the infrastructure stack *you* manage and which parts are managed by the *vendor*. The portions the vendor manages and you are charged for is the **unit of consumption**.

- On-Premises (not cloud) - You manage all parts of the infrastructure stack from facilities to application. It's expensive and carries the most risk, but it's also very flexible.
- Infrastructure as a Service (IaaS) - The vendor provides a virtual machine and you consume the OS on up. You pay when you use the virtual machine and don't pay when you don't use it. You lose a bit of flexibility, but also take on a lot less risk. (i.e. EC2)
- Platform as a Service (PaaS) - The vendor provides the runtime and you manage the application on up. Generally aimed at developers. (i.e. Heroku)
- Software as a Service (SaaS) -  The vendor provides the application itself. You pay to use the application but there are very few risks. (i.e. Outlook) 

[Back to Top](#AWS-SAA-C02-Notes)

## AWS Fundamentals

### AWS Private vs Public Services 

Private vs Public services refers to the networking, because they have no permissions by default. 

There are 3 (not 2) zones to be aware of: 

1. Public Internet 
2. AWS Public Zone - accessible via the Public Internet but not on the Public Internet. 
3. AWS Private Zone - isolated by default from the Public Internet and the AWS Public Zone but can be accessed from the with private network. They can sometimes be configured to be accessed via the AWS Public Zone or from an on-premises network. 

For example, an S3 bucket is by default a public service but an EC2 instance in a VPC is not public by default. However, you can configure the EC2 instance to have public connection but this does not make is a private service. 

[Back to Top](#AWS-SAA-C02-Notes)

### AWS Global Networking

**AWS Regions** - physical locations with full deployment of the AWS infrastructure.  Each region is isolated but connected together by high speed global networking.

> :bulb: When you interact with most AWS services, you interact with that service in the specific region. 

**AWS Edge Locations** - many more locations than regions but are generally only content distribution points. 

**Availability Zones** - isolated infrastructure (compute, storage, power, facilities, etc) within a region connected to each other by high speed redundant networks. AWS will provide between 2-6 AZs per region.


**Region Benefits**

1. Geographical Separation
	- Regions are 100% isolated
	- Useful for natural disasters or events that affect an entire area
2. Geopolitical Separation
	- The laws of the region apply to your infrastructure
	- Allows stability from political events
	- You control where your data is stored
3. Location Control
	- Tune architecture for performance
	- Duplicate infrastructure closer to customers

**Service Resilience**

1. Globally Resilient: IAM or Route 53. No way for them to go down. Data is replicated throughout multiple regions.
2. Region Resilient: Operate as separate services in each region. Generally replicate data to multiple AZs in that region.
3. AZ Resilient: Run from a single AZ. It is possible for hardware to fail in an AZ and the service to keep running because of redundant equipment, but should not be relied on.

[Back to Top](#AWS-SAA-C02-Notes)

### Virtual Private Cloud (VPC) Basics

 A Virtual Private Cloud (VPC):
 
 - is a virtual network inside AWS
 - exists within 1 account and 1 region which makes it regionally resilient
 - is private and logically isolated from other virtual networks in AWS by default
 
You can launch other AWS services, such as EC2 instances, into your VPC.

2 types: 

1. Default - only 1 per region and come pre-configured by AWS 
2. Custom - many per region, defined by you, and much more flexible

When you create a custom VPC, you must specify a range of IPv4 addresses for the VPC in the form of a Classless Inter-Domain Routing (CIDR) block. 

A VPC spans all of the AZs in the Region. After creating a VPC, you can add one or more subnets in each AZ. When you create a subnet, you specify the CIDR block for the subnet, which is a subset of the VPC CIDR block. Each subnet must reside entirely within one AZ and cannot span zones. This is done automatically for the Default VPC.

**Default VPC Facts:**

- One per region 
- CIDR range is always: 172.31.0.0/16.
- /20 Subnet in each AZ in the region
- Internet Gateway (IGL), Security Group (SG), and Network ACL (NACL) are preconfigured as well
- Subnets assigned public IPv4 addresses

> :bulb: The higher the / number is, the smaller the grouping. For example, a /17 is half the size of a /16, so two /17's will fit into a /16. Sixteen /20 subnets can fit into one /16.

You can delete a Default VPC, but some services assume that you have the Default VPC. 

Generally, it's best practice to use Custom VPCs.

[Back to Top](#AWS-SAA-C02-Notes)

### Elastic Compute Cloud (EC2) Basics

EC2 is the default compute service in AWS. 

- IaaS that provides virtual machines know as instances.
- Private service by default launched into a single VPC subnet
- AZ resilient (b/c it's launched into an AZ)
- Various sizes and capabilities
- Generally On-Demand billing (per second or per hour). Pricing based on:
	* CPU
	* Memory
	* Storage (provided by either in instance store or EBS)
	* Networking

**Instance Lifecycle**

1. Running State 
	- Running on a physical host using CPU even if idle
	- Using memory even with no processing
	- OS and other data is stored on disk allocated
	- Networking is always ready to transfer information
2. Stopped State 
	- No CPU resources are being consumed
	- No memory is being used
	- No networking is being generated
	- BUT storage is still allocated to the instance and therefore incurs charges
3. Terminated State 
	- Deletes the disk (including EBS) and prevents all future charges 

**AMI**

Amazon Machine Image - used to create an instance or created from an instance. However, AMIs in one region are not available from other regions.

Each AMI contains:

- Permissions: who can use the image
	* Public - everyone allowed
	* Owner - implicitly allowed, only owner can use it
	* Explicit - specific AWS accounts allowed 
- Root Volume: contain the boot volume (C drive in Windows, root in Linux) 
- Block Device Mapping: determines which volume is a boot volume and which volumes is a data volume. 

**Connecting to EC2**

- Windows using RDP (Remote Desktop Protocol), Port 3389
- Linux SSH protocol, Port 22

[Back to Top](#AWS-SAA-C02-Notes)

### Simple Storage Service (S3) Basics 

* The default storage platform for AWS.
* Global service but regional based/regionally resilient
* Public service --> unlimited data, users, scale 
* Made up of objects and buckets
* Great for large scale data storage, distribution or upload. Basically anything where you need to access the whole object at once.
* Ideal storage input or output for other AWS services.

> :bulb: S3 is object storage, not block storage or network file storage. You cannot mount it as a drive.

**Objects**

* Conceptually the same as files
* Key = file name
* Value = Content being stored
* :rotating_light: **Size of objects can be 0kb to 5TB size.**

**Buckets**

* Primary home region (i.e. you can control where the data is stored and what laws and rules apply to that data)
* Blast radius of a region.
* Bucket names are **globally unique**. Bucket names can have 3-63 characters -- all lowercase, no underscores -- and must start with a lowercase letter number. 
* Stores unlimited objects (and thus can be of unlimited size)
* Flat structure (i.e. all files are stored at the root)
	* It may *look like* there is a structure in the UI, S3 does this if the key is like `/old/Koala1.jpg`. But this is just a UI nicety.
* :rotating_light: **100 bucket soft limit, 1000 hard limit per account**

[Back to Top](#AWS-SAA-C02-Notes)

### CloudFormation (CFN) Basics

**Infrastructure as Code** (IaC) product in AWS.

At it's base, CFN uses templates written in YAML or JSON. 

Template parts: 

* Resources - All templates must have a list of resources. The resources section tells CFN what to do. 
* Description - Free text field to provide details of what the template does. If you use a description, it must immediately follow the AWSTemplateFormatVersion. 
* AWSTemplateFormatVersion - version of CFN you want to use
* Metadata - Control how resources are displayed in the console UI. 
* Parameters - Add fields which prompt the user for more information (e.g. the name of a resource)
* Mappings - Creates lookup tables
* Conditions - Define conditional logic which can be used within the resources section. 
* Outputs - Output info once the template is finished running.

CFN allows you to automate deployments, store them in VCS, etc.  

* Template - YAML or JSON file containing, at bare minimum, a list of logical resources. 
* Stack - Living representation of a template containing all the logical resources.
* Logical Resources - Resources in a template file.
* Physical Resources - AWS products or services.

CFN keeps the logical and physical resources in sync.

:rotating_light: **For any Logical Resources in the stack, CF will make a corresponding Physical Resources in your AWS account.**

1. A template can be used to create a stack (or multiple stacks), which then creates physical resources.
2. When a template is update, you can use that to update the stack, which then updates the physical resources.
3. If you delete a stack, it's logic resources are deleted, which then updates the physical resources.

[Back to Top](#AWS-SAA-C02-Notes)

### CloudWatch Basics

CloudWatch has 3 Main Jobs:

1. Metrics (collection, monitoring, & actions) - data for AWS products, apps, or other cloud resources.
2. Logs (collection, monitoring, & actions) - logging data for AWS products, app, or any other cloud resources.
3. Events - AWS service event hub, can trigger an action based on an event or a schedule. 

Some metrics are handled natively w/o any additional configuration, such as CPU Utilization on an EC2 server. Otherwise you'll need to install the CloudWatch agent. 

**Namespaces** 

- Think of  namespace is a container for monitoring data and they contain related metrics. 
- All AWS data goes into an AWS namespace like so `AWS/service`. For example, for EC2 the namespace would be `AWS/EC2`.

**Metrics**

- A metric is a collection of related datapoints in a time-ordered structure.
- CPU Utilization, Network In/Out, Disk IO are all examples of metrics.
- A metric could come from one server or many. Since a **datapoint** is made up of a timestamp an a value, CW uses **dimensions** to separate datapoints for different things or perspectives within the same metric. Dimensions are stored as key/value pair.s 
- For example, when sending CPU Utilization data for EC2 servers, sends the InstanceId and InstanceType as dimensions.

**Alarms**

- Created and linked to a specific metric. Then based on the alarm configuration, you take an action based on that metric. 
- There are 3 states: "OK", "ALARM", "INSUFFICIENT_DATA".
- Usually when the Alarm goes into an "ALARM" state,  an action is taken.

[Back to Top](#AWS-SAA-C02-Notes)

### Shared Responsibility Model

AWS is responsible for security *of* the cloud, the customer is responsible for security *in* the cloud. 

[Back to Top](#AWS-SAA-C02-Notes)

### High-Availability (HA) vs Fault-Tolerance (FT) vs Disaster Recovery (DR)


**High-Availability** 

- Aims to ensure an agreed level of operational performance, usually uptime, for a higher than normal period. Basically, maximize a systems time operational.
- User disruption, though not ideal, is OK. 
- High availability has a cost and requires planning.
- An example of high availability is a car with a spare tire. If the tire blows out, you would need to spend time changing the tire (which is a disruption) but it ensures that you don't have to call for assistance and minimizes the time you are out of action.



**Fault-Tolerance** *(similar but different than high availability)*

- The property that enables a system to continue operating in the event of the failure of some of it's components. Basically, operate through a failure with no customer impact. 
- Much more complex (and expensive) to implement than high availability.
- An example of fault tolerance is an airplane engine failure. If an engine fails the plane cannot stop and make repairs, so it comes with more engines than it needs. 
 
**Disaster-Recovery**

- A set of policies, tools and procedures to enable the recovery or continuation of vital technology infrastructure and systems following a natural or human-induced disaster. 

[Back to Top](#AWS-SAA-C02-Notes)
