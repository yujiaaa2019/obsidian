## Deployment 
- CI/CD: Continuous Integration / Continuous Delivery 

## Maintenance 
- Maintaining widely used APIs 

## System Design Common Fallacies 
1. Network is Reliable
	1. timeout 
	2. idempotency 
2. Latency is Zero
	1. caching 
	2. server resources 
	3. request-response vs. event-driven 
3. bandwidth is infinite 
	1. Exponential backoff
	2. limit the amount of data sent over the network 
	3. efficient transport protocols (gRPC)
4. Network is secure 
5. Topology doesn't change 
6. There's one administrator 
	1. document auth rules 
7. Transport cost is zero 
	1. choose tradeoffs accordingly 
	2. don't be frugal but be aware 
8. The network is homogeneous 

## Domain Model
The domain is the *Problem*
The model is a *Solution*

- It is a living document as the project evolves 
- shapes your system design 



