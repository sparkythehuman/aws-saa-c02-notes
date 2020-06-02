# AWS SAA-C02 Notes
> These are my personal notes while studing for the AWS SAA-C02 exam. My primary preparation tool was Adrian Cantrill's (SAA-C02) course. These notes approximately follow that course. There may be errors. To get the course, visit [https://learn.cantrill.io](https://learn.cantrill.io.).

## Table of Contents
 
- [Cloud Computing Fundamentals ](#Cloud-Computing-Fundamentals )

---

## Cloud Computing Fundamentals 

### What is Cloud Computing?

According to the National Institute of Standards and Technology (NIST), there are 5 essential characteristics of cloud computing: 

1. **On-Demand Self-Service**: Can provision capabilities as needed without requiring human interaction.
2. **Broad Network Access**: Capabilities are available over the network and accessed through standard mechanisms. 
3. **Resource Pooling**: There is a sense of location independence with no control or knowledge over the exact location of the resources. Resources are pooled to serve multiple consumers using a multi-tenant model.
4. **Rapid Elasticity**: Capabilities can be elastically provisioned and released to scale rapidly outward and inward with demand. To the consumer, the capabilities available for provisioning often appear to be unlimited. 
5. **Measured Service**: Resource usage can be monitored, controlled, reported AND BILLED.

### Public vs Private vs Multi vs Hybrid Cloud

- **Public**: Using 1 public cloud (e.g. AWS, Azure, Google Cloud)
- **Private**: Using on-prem cloud that meets all 5 requirements. Generally legacy on-prem infrastructure doesn't meet all the requirements. (e.g. AWS Outposts, Azure Stack, and Google Anthos)
- **Multi**: Using more than one private cloud. 
- **Hybrid**: Using public and private clouds. *"Hybrid cloud "* IS NOT public cloud plus legacy on-prem. That's known as *"Hybrid Environment" or "Hybrid Networking"*. 

### Cloud Service Models

Cloud Service models define which parts of the infrastructure stack *you* manage and which parts are managed by the *vendor*. The portions the vendor manages and you are charged for is the **unit of consumption**.

- On-Premises (not cloud) - You manage all parts of the infrastructure stack from facilities to application. It's expensive and carries the most risk, but it's also very flexible.
- Infrastructure as a Service (IaaS) - The vendor provides a virtual machine and you consume the OS on up. You pay when you use the virtual machine and don't pay when you don't use it. You lose a bit of flexibility, but also take on a lot less risk. (i.e. EC2)
- Platform as a Service (PaaS) - The vendor provides the runtime and you manage the application on up. Generally aimed at developers. (i.e. Heroku)
- Software as a Service (SaaS) -  The vendor provides the application itself. You pay to use the application but there are very few risks. (i.e. Outlook) 

[Back to Top](#AWS-SAA-C02-Notes)