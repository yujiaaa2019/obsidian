- prevent familiar failure mode 
- recover from novel failure mode 

## Some Situations
- Request Latency
	- limit latency with timeouts 
	- spiky or steady (kind of like EC2 scaling pattern)
	- concurrency = RPS (Request per sec) * Latency
- System Load 
	- performance degrades with load 
- Capacity Management 
- Under-scaled Systems 
	- headroom (?)
		- makes things safer
	- shed load until capacity is online 
- Resiliency Tools
	- Chaos Monkey (injecting failure mode into systems to test how it reacts to failure)
	- Squeeze Testing (load testing) --> expensive 
	- Failure Injection Test (FIT)
- Change Management 
	- Canaries 
	- Staggered rollouts 
	- could limit blast radius 
	
## Recovery 
