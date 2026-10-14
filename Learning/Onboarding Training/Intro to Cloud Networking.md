## IP Address
network reachability 
- [go/sonar]:  to find what an IP address is or was 

![[Screenshot 2023-07-18 at 2.14.25 PM.png]]
public IP: globally routable address; allows connectivity from the Internet 

## Network Placement 

## AWS Cloud 
![[Screenshot 2023-07-18 at 2.20.50 PM.png]]
Account boundary

### VPC
virtual private cloud; a private network that puts inside AWS 
3 consumer-facing services regions (Washington, Portland, one in Europe)

### Availability Zones
sets of data centers that are within a certain geographical regions. 

### Subnets 
subnet determines connectivity 

## Service Discovery
- NIWS
	- provides a list of all instances registered 
	- instance to instance communication 
	- only in-regional 
- Eureka DNS - Dynamic Response 
	- app/cluster/VIP bucket 
	- subset of all registered instances (8 IPs)
	- instance to instance communication 
	- DNS load balancers (round robin)
- DNS Hostnames - Static 
	- used with load balancers 
	- statically configures 

## Security Groups 
Elastic Network Interface (ENI)
- support multiple security groups 
	- ingress rule sets
		- currently use
		- IP Rules (reference IP Address blocks)
		- SG Reference ![[Screenshot 2023-07-18 at 3.07.50 PM.png]]
	- egress rule sets
		- 
## Application Permission 
IAM Role: A role that's assigned to the application that has credentials to enable connection to the AWS services 
	- grant access to services like S3, SQS, SES
	- credentials are short lived 
## Load Balancer 
Classic LB (to be subset...)
Application LB 
Network LB