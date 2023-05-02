### Physical hardware
- Self-managed systems deployed either on-premises or in a co-location (COLO)
- Equipment purchased and fully or partially managed by the companies IT staff which adds operational overhead as the company grows

### Virtualization
- Hardward resoureces of a single physical device are divided into multiple virtual devices, called virtual machines (VMs)
- VMs are containers that have virtualized resources like vCPU, vNICs, memory, and storage
- Hypervisors provide the hardware abstraction layer that VMs run on.
	- Hypervisor is what divides the computers resources
Examples:
- VMware
- Virtual box
- vsphere
[picture of his drawing]
[picture of virtualization]

### What is cloud Computing
- The on-demand delivery of IT resources over the internet with pay-as-you-go pricing.
- Utilizing resources and services from a Cloud Provider on an as-needed basis instead of using your own.

- different company services
	- AWS -> Amazon Web Service
	- GCP -> Google Cloud Platform
	- AZURE

### Advantages of Cloud Computing
- Variable expense instead of Captital Expense
- Economies of Scale
- Elasticity
- Deployment time
- No Physical hardware to manage
- Globally available
- Automation

[picture of slide "common cloud automation tasks"]

### Disadvantages of Cloud Computing
- Security
	- Less control over your data
	- Always online
- Cost
	- Unexpected Cost
	- it can be more expensive
- Requires internet connectivity to access data

### Types of Cloud Computing
- Software as a service (SaaS)
	- A complete product - ran and managed by the service provider
- Plateform as a Service (PaaS)
	- Serivices provider maintains the infrastructure while the customer builds and maintains their applications
- Infrastructure as a Service (IaaS)
	- The service provider maintains the hardware, while the customer controls the resources utilization, apps, etc.
	- This is the portion a network engineer will be involved.
	- comes with licenses for the devices being used that they provide.
	- any applications not provided by the cloud provider may require extra license costs.
[picture of types of cloud computing]

### Deployment Types
- Public Cloud
	- Fully utilizing cloud computing
- Hybrid Cloud
	- Interconnected private and public cloud infrastructure
- Private Cloud
	- Deployment resources on-prem (on premises) using virtualization and resource managment tools.
	- not using cloud at all but using the cloud providers equipment.
[pictures showing deployment types]


### Deployment models
- Public Cloud
	- A cloud on the internet that anyone can access
	- cost effective, but least secure
- Hybrid Cloud
	- Combination of private and public clouds
	- Security of private cloud with cost savings of public cloud
	- More difficult to manage
- Private Cloud
	- A one-on-one environment for a customer
	- No shared hardware with other customers
	- Most secure, but most costly
- Community Cloud
	- Select organizations may access the systems in service 
	- More secure than public cloud
	- less scalable
- Multi-cloud
	- Utilizes multiple public clouds
	- Provides highest availability
	- Highest complexity

[visual for cloud types ]

### Global Infrastructures
- Regions
	- A physical location in the world where the data centers are located
	- Geographically distinct
	- Each region is completely independent
	- Not all services available in all regions
	- You can host your data and services in multiple regions
	- Each region has at least two Availavility Zones
	- you would not transfer data between regions
		- it is cheaper to send that data needed in the back of a truck then to send it accross the wire. US -> Some arabic country 
[visual representation of regions]
- Availability Zones
	- One or more data centers owned by the cloud provider
	- each have redundant power, networking, and connectivity
	- physically seperated from other availability zones
	- independant failure zones
	- connected via low-latency links to other availability zones
[global infastructure picutre]

### Getting started with AWS
- Open a web browser and navigate to aws.amazon.com
- Create an account
- Select a support plan and log in
[Piture of login]

### Cloud services - DNS
- AWS Route 53
	- Provides domain registration services
	- DNS services
	- Health checking
	- 53 is the port DNS uses
[picture of dns]
[second pic of route 53]

### Cloud Services - CDN
- AWS CloudFront
	- A content delivery network (CDN)
	- Benefits
		- Cached content closer to users
		- built-in Distributed Denial of Service (DDoS) protection
		- Integrates with many AWS services
[picture of cloud front]

# Cloud Services 
----
### Storage
- AWS S3
	- Use for storage in AWS
	- A persistent, highly durable data store
	- Can store any file type
	- Unlimited storage available
	- Often used in conjunction with CloudFront
[picture of storage]

### Load Balancers
- AWS Elastic Load Balancer (ELB)
	- Automatically distributes traffic across multiple targets like applications, VMs, IP addressed, etc.
	- Can load balance within a single Availibility Zone or across multiple 
	- Features high availability and auto scaling
[pics]
[pictures of load balancing]

### Servers
- AWS Elastic Compute Cloud (EC2)
	- Virtual Servers
	- Many sizes available
	- Pay for only what you use
	- Can automatically scale up or down depending on usage
[picture of "Launch and instance"]
----

### Security in the Cloud
- Security in the Cloud is shared responsibility
- The Cloud provider is responsible for protecting the infrastructure
- The customer is responsible for protecting their data.
[picture of security responsibilities]

### Pricing
- Price will be baded on usage
	- Data usage, compute usage, services used, etc.
- Most cost savings are from immediately removing any resources when not in-use.
- EC2 Instances have specialized pricing
	- On - Demand -> Most expensive
	- Reserved -> discounted (Pay up front for x amount of time)
	- Spot -> ?
- Use a Cloud Providers calculator to estimate your cost
[picture of pricing]


#### LIVE DEMO